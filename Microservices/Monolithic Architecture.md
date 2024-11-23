#Monolithic 
## Context

Your business-critical enterprise application needs rapid, frequent, and reliable changes as measured by [DORA metrics](https://microservices.io/articles/glossary#dora-metrics). The engineering organization uses small, cross-functional teams following [Team Topologies](https://microservices.io/tags/team%20topologies). Teams implement [DevOps practices](https://microservices.io/tags/devops) with continuous deployment, delivering frequent changes through automated pipelines.

![https://microservices.io/i/posts/teams-own-subdomains.png](https://microservices.io/i/posts/teams-own-subdomains.png)

Each team manages subdomains - implementable models of business functionality. A subdomain contains business entities implementing business rules and adapters for external communication. For example, Java-based subdomains use packages compiled into JAR files.

Subdomains handle application behavior through system operations, triggered by: client requests (sync/async), external events, or time-based triggers. These operations interact with business entities across subdomains.

## Problem

How to organize the subdomains into one or more deployable/executable components?

There are five [dark energy forces](https://microservices.io/post/microservices/2021/11/30/dark-matter-dark-energy.html):

- [Simple components](https://microservices.io/articles/dark-energy-dark-matter/dark-energy/simple-components.html) - simple components consisting of few subdomains are easier to understand and maintain than complex components
- [Team autonomy](https://microservices.io/articles/dark-energy-dark-matter/dark-energy/team-autonomy.html) - a team needs to be able to develop, test and deploy their software independently of other teams
- [Fast deployment pipeline](https://microservices.io/articles/dark-energy-dark-matter/dark-energy/fast-deployment-pipeline.html) - fast feedback and high deployment frequency are essential and are enabled by a fast deployment pipeline, which in turn requires components that are fast to build and test.
- [Support multiple technology stacks](https://microservices.io/articles/dark-energy-dark-matter/dark-energy/multiple-technology-stacks.html) - subdomains are sometimes implemented using a variety of technologies; and developers need to evolve the application’s technology stack, e.g. use current versions of languages and frameworks
- [Segregate by characteristics](https://microservices.io/articles/dark-energy-dark-matter/dark-energy/segregate-by-characteristics.html) - e.g. resource requirements to improve scalability, their availability requirements to improve availability, their security requirements to improve security, etc.

There are five [dark matter forces](https://microservices.io/post/microservices/2021/11/30/dark-matter-dark-energy.html):

- [Simple interactions](https://microservices.io/articles/dark-energy-dark-matter/dark-matter/simple-interactions.html) - an operation that’s local to a component or consists of a few simple interactions between components is easier to understand and troubleshoot than a distributed operation, especially one consisting of complex interactions
- [Efficient interactions](https://microservices.io/articles/dark-energy-dark-matter/dark-matter/efficient-interactions.html) - a distributed operation that involves lots of network round trips and large data transfers can be too inefficient
- [Prefer ACID over BASE](https://microservices.io/articles/dark-energy-dark-matter/dark-matter/prefer-acid-over-base.html) - it’s easier to implement an operation as an ACID transaction rather than, for example, eventually consistent sagas
- [Minimize runtime coupling](https://microservices.io/articles/dark-energy-dark-matter/dark-matter/minimize-runtime-coupling.html) - to maximize the availability and reduce the latency of an operation
- [Minimize design time coupling](https://microservices.io/articles/dark-energy-dark-matter/dark-matter/minimize-design-time-coupling.html) - reduce the likelihood of changing services in lockstep, which reduces productivity

## Solution

Design an architecture that structures the application as a single deployable/executable component that uses a single database. The component contains all of the application’s subdomains. Since there’s a single component, all operations are local.

![[Pasted image 20241123120457.png]]


## Example

Let’s imagine that you are building an e-commerce application that takes orders from customers, verifies inventory and available credit, and ships them. The application consists of several components including the StoreFrontUI, which implements the user interface, along with some backend services for checking credit, maintaining inventory and shipping orders.

The application is deployed as a single monolithic application. For example, a Java web application consists of a single WAR file that runs on a web container such as Tomcat. A Rails application consists of a single directory hierarchy deployed using either, for example, Phusion Passenger on Apache/Nginx or JRuby on Tomcat. You can run multiple instances of the application behind a load balancer in order to scale and improve availability.

![[Pasted image 20241123120920.png]]

## Resulting context

### Benefits

Since the application consists of a single component and all operations are local this solution has a number of benefits. Specifically, the dark matter forces are resolved as follows:

- Simple interactions - all interactions are generally easier to understand and troubleshoot. However, interactions between subdomains could potentially be complex.
- Efficient interactions - interactions are typically more efficient since all communication is local.
- Prefer ACID over Base - operations can be implemented as ACID transactions (usually)
- Minimize runtime coupling - there’s no runtime coupling between components
- Minimize design time coupling - these’s no design time coupling between components. There can, however, be design time coupling between subdomains within the monolith.

### Drawbacks

This solution has a number of (**potential**) drawbacks. Specifically, an architecture might fail to resolve the dark energy forces:

- Simple components - since there’s only a single component it is potentially difficult to understand and maintain due its size and complexity
- Team autonomy - there’s potentially less team autonomy since all teams are contributing to the same code base and they need to coordinate their work more often
- Fast deployment pipeline - the deployment pipeline is potentially slow since there’s a single large application that needs to be built and tested
- Multiple technology stacks - the application uses a single technology stack, which might not be ideal for all subdomains. Also, if the application is large upgrading the technology stack might be very time consuming
- Segregate by characteristics - there’s no possibility of segregating subdomains by their characteristics, which might reduce scalability, availability, security etc

