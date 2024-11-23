## Problem

How do you migrate a legacy monolithic application to a microservice architecture?
## Solution

Modernize an application by incrementally developing a new (strangler) application around the legacy application. In this scenario, the strangler application has a [microservice architecture](https://microservices.io/patterns/microservices.html).

![](https://microservices.io/i/decompose-your-monolith-devnexus-feb-2020.001.jpeg)

The strangler application consists of two types of services. First, there are services that implement functionality that previously resided in the monolith. Second, there are services that implement new features. The latter are particularly useful since they demonstrate to the business the value of using microservices.

## References

https://martinfowler.com/bliki/StranglerFigApplication.html
https://martinfowler.com/bliki/ConwaysLaw.html