
## Context

A service often needs to publish events when it updates its data. These events might be needed, for example, to update a [CQRS view](https://microservices.io/patterns/data/cqrs.html). Alternatively, the service might participate in an choreography-based [saga](https://microservices.io/patterns/data/saga.html), which uses events for coordination.

## Problem

How does a service publish an event when it updates its data?

## Solution

Organize the business logic of a service as a collection of DDD [aggregates](https://microservices.io/patterns/data/aggregate.html) that emit domain events when they created or updated. The service publishes these domain events so that they can be consumed by other services.