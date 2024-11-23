
## Context

You have applied the [Microservices architecture pattern](https://microservices.io/patterns/microservices.html) and the [Database per service pattern](https://microservices.io/patterns/data/database-per-service.html). As a result, it is no longer straightforward to implement queries that join data from multiple services. Also, if you have applied the [Event sourcing pattern](https://microservices.io/patterns/data/event-sourcing.html) then the data is no longer easily queried.

## Problem

How to implement a query that retrieves data from multiple services in a microservice architecture?

## Solution

Define a view database, which is a read-only ‘replica’ that is designed specifically to support that query, or a group related queries. The application keeps the database up to date by subscribing to [Domain events](https://microservices.io/patterns/data/domain-event.html) published by the service that own the data. The type of database and its schema are optimized for the query or queries. It’s often a NoSQL database, such as a document database or a key-value store.

![](https://microservices.io/i/patterns/data/QuerySideService.png)

## Examples

- My book’s FTGO example application has the [`Order History Service`](https://github.com/microservices-patterns/ftgo-application#chapter-7-implementing-queries-in-a-microservice-architecture), which implements this pattern.
    
- There are [several Eventuate-based example applications](http://eventuate.io/exampleapps.html) that illustrate how to use this pattern.
    

## Resulting context

This pattern has the following benefits:

- Supports multiple denormalized views that are scalable and performant
- Improved separation of concerns = simpler command and query models
- Necessary in an event sourced architecture

This pattern has the following drawbacks:

- Increased complexity
- Potential code duplication
- Replication lag/eventually consistent views