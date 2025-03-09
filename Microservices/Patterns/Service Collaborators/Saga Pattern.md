## Context

You have applied the [Database per Service](https://microservices.io/patterns/data/database-per-service.html) pattern. Each service has its own database. Some business transactions, however, span multiple service so you need a mechanism to implement transactions that span services. For example, let’s imagine that you are building an e-commerce store where customers have a credit limit. The application must ensure that a new order will not exceed the customer’s credit limit. Since Orders and Customers are in different databases owned by different services the application cannot simply use a local ACID transaction.

## Problem

How to implement transactions that span services?

### One Possible Solution

[[Two Phase Commit]]

## Solution

Implement each business transaction that spans multiple services as a saga. A saga is a sequence of local transactions. Each local transaction updates the database and publishes a message or event to trigger the next local transaction in the saga. If a local transaction fails because it violates a business rule then the saga executes a series of compensating transactions that undo the changes that were made by the preceding local transactions.

![](https://microservices.io/i/sagas/From_2PC_To_Saga.png)

There are two ways of coordination sagas:

- Choreography - each local transaction publishes domain events that trigger local transactions in other services.
- Orchestration - an orchestrator (object) tells the participants what local transactions to execute.

## Example: Choreography-based saga

![](https://microservices.io/i/sagas/Create_Order_Saga.png)

An e-commerce application that uses this approach would create an order using a choreography-based saga that consists of the following steps:

1. The `Order Service` receives the `POST /orders` request and creates an `Order` in a `PENDING` state
2. It then emits an `Order Created` event
3. The `Customer Service`’s event handler attempts to reserve credit
4. It then emits an event indicating the outcome
5. The `OrderService`’s event handler either approves or rejects the `Order`

[Take a tour of an example saga](https://microservices.io/post/architecture/2024/03/20/tour-of-two-sagas.html)

## Example: Orchestration-based saga

![](https://microservices.io/i/sagas/Create_Order_Saga_Orchestration.png)

An e-commerce application that uses this approach would create an order using an orchestration-based saga that consists of the following steps:

6. The `Order Service` receives the `POST /orders` request and creates the `Create Order` saga orchestrator
7. The saga orchestrator creates an `Order` in the `PENDING` state
8. It then sends a `Reserve Credit` command to the `Customer Service`
9. The `Customer Service` attempts to reserve credit
10. It then sends back a reply message indicating the outcome
11. The saga orchestrator either approves or rejects the `Order`

[Take a tour of an example saga](https://microservices.io/post/architecture/2024/03/20/tour-of-two-sagas.html)

## Resulting context

This pattern has the following benefits:

- It enables an application to maintain data consistency across multiple services without using distributed transactions.

This solution has the following drawbacks:

- Lack of automatic rollback - a developer must design compensating transactions that explicitly undo changes made earlier in a saga rather than relying on the automatic rollback feature of ACID transactions.

- Lack of isolation (the “I” in ACID) - the lack of isolation means that there’s risk that the concurrent execution of multiple sagas and transactions can use data anomalies. consequently, a saga developer must typical use countermeasures, which are design techniques that implement isolation. Moreover, careful analysis is needed to select and correctly implement the countermeasures. See [Chapter 4/section 4.3 of my book Microservices Patterns](https://livebook.manning.com/book/microservices-patterns/chapter-4/143) for more information.


There are also the following issues to address:

- In order to be reliable, a service must atomically update its database _and_ publish a message/event. It cannot use the traditional mechanism of a distributed transaction that spans the database and the message broker. Instead, it must use one of the patterns listed below.

- A client that initiates the saga, which an asynchronous flow, using a synchronous request (e.g. HTTP `POST /orders`) needs to be able to determine its outcome. There are several options, each with different trade-offs:
    
    - The service sends back a response once the saga completes, e.g. once it receives an `OrderApproved` or `OrderRejected` event.
    - The service sends back a response (e.g. containing the `orderID`) after initiating the saga and the client periodically polls (e.g. `GET /orders/{orderID}`) to determine the outcome
    - The service sends back a response (e.g. containing the `orderID`) after initiating the saga, and then sends an event (e.g. websocket, web hook, etc) to the client once the saga completes.

