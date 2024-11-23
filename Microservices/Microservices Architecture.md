[[#https://microservices.io/post/microservices/general/2019/02/27/microservice-canvas.html]]

[[https://microservices.io/post/architecture/2023/02/09/assemblage-architecture-definition-process.html]]

Design an architecture that structures the application as a set of two or more [independently deployable](https://microservices.io/post/architecture/2022/05/04/microservice-architecture-essentials-deployability.html), [loosely coupled](https://microservices.io/post/architecture/2023/03/28/microservice-architecture-essentials-loose-coupling.html), [components, a.k.a. services](https://microservices.io/post/microservices/general/2019/02/16/whats-a-service-part-1.html). Each service consists of one or more subdomains. Each subdomain is part of a single service except for shared library subdomains that are used by multiple services. A service is owned by the team (or teams) that owns the (non-library) subdomains.

![](https://microservices.io/i/posts/microservices-teams-subdomains.png)

An [API gateway](https://microservices.io/patterns/apigateway.html) is typically the application’s entry point. Some system operations will be local to a single service, while others will be distributed across multiple services. A distributed system operation is implemented using the [service collaboration patterns](https://microservices.io/post/patterns/2023/07/29/service-collaboration-patterns.html).

In order to be independently deployable each service typically has its own source code repository and its own deployment pipeline, which builds, tests and deploys the service.

## Examples

### Fictitious e-commerce application

Let’s imagine that you are building an e-commerce application that takes orders from customers, verifies inventory and available credit, and ships them. The application consists of several components including the StoreFrontUI, which implements the user interface, along with some backend services for checking credit, maintaining inventory and shipping orders. The application consists of a set of services.

![](https://microservices.io/i/Microservice_Architecture.png)


### Show me the code

Please see the [example applications developed by Chris Richardson](http://eventuate.io/exampleapps.html). These examples on Github illustrate various aspects of the microservice architecture.


## Resulting context

### Benefits

This solution has a number of benefits:

- Simple services - each service consists of a small number of subdomains - possibly just one - and so is easier to understand and maintain
- Team autonomy - a team can develop, test and deploy their service independently of other teams
- Fast deployment pipeline - each service is fast to test since it’s relatively small, and can be deployed independently
- Support multiple technology stacks - different services can use different technology stacks and can be upgraded independently
- Segregate subdomains by their characteristics - subdomains can be segregated by their characteristics into separate services in order to improve scalability, availability, security etc

### Drawbacks

This solution has a number of (**potential**) drawbacks:

- Some distributed operations might be complex, and difficult to understand and troubleshoot
- Some distributed operations might be potentially inefficient
- Some operations might need to be implemented using complex, eventually consistent (non-ACID) transaction management since loose coupling requires each [service to have its own database](https://microservices.io/patterns/data/database-per-service.html).
- Some distributed operations might involve tight runtime coupling between services, which reduces their availability.
- Risk of tight design-time coupling between services, which requires time consuming lockstep changes.

### Issues

There are many issues that you must address when designing an architecture.
#### Designing a good (monolithic or microservice) architecture with Assemblage

There are two key issues that you must address. The first issue is whether to use the monolithic or microservice architecture. And then, if you choose to use the microservice architecture, the next key challenge is to define a good [service architecture](https://microservices.io/post/architecture/2023/09/19/assemblage-part-3-whats-a-service-architecture.html). You must avoid (or at least minimize) the potential drawbacks: complex, inefficient interactions; complex eventually consistent transactions; and tight runtime coupling.

[Assemblage](https://microservices.io/post/architecture/2023/02/09/assemblage-architecture-definition-process.html), is an architecture definition process that uses the [dark energy and dark matter forces](https://microservices.io/post/architecture/2023/03/26/dark-energy-dark-matter-force-descriptions.html) to group the subdomains in a way that results in good microservice architecture.

[![](https://microservices.io/i/posts/assemblage-overview/Defining_Microservice_Architecture_V2.png)](https://microservices.io/post/architecture/2023/02/09/assemblage-architecture-definition-process.html)

The result of applying Assemblage is either a monolithic architecture or a microservice architecture.

The [dark energy and dark matter forces](https://microservices.io/post/architecture/2023/03/26/dark-energy-dark-matter-force-descriptions.html) play a major role in shaping the service architecture and also heavily influence the design of the distributed operations mentioned below.

[![](https://microservices.io/i/posts/dark-energy-dark-matter/Dark_Energy_Dark_Matter_overview.png)](https://microservices.io/post/architecture/2023/03/26/dark-energy-dark-matter-force-descriptions.html)

#### Designing distributed operations

Another key design challenge when using microservices, is implementing distributed operations, which span multiple services. This is especially challenging since each [service has its own database](https://microservices.io/patterns/data/database-per-service.html). The solution is to use the [service collaboration patterns](https://microservices.io/post/patterns/2023/07/29/service-collaboration-patterns.html), which implement distributed operations as a series of local transactions:

[![](https://microservices.io/i/patterns/data/service-collaboration-patterns.png)](https://microservices.io/post/patterns/2023/07/29/service-collaboration-patterns.html)

There are four service collaboration patterns:

- [Saga](https://microservices.io/patterns/data/saga.html), which implements a distributed command as a series of local transactions
- [Command-side replica](https://microservices.io/patterns/data/cqrs.html), which replicas read-only data to the service that implements a command
- [API composition](https://microservices.io/patterns/data/api-composition.html), which implements a distributed query as a series of local queries
- [CQRS](https://microservices.io/patterns/data/cqrs.html), which implements a distributed query as a series of local queries

