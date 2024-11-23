## Context

You have applied the [Microservices architecture pattern](https://microservices.io/patterns/microservices.html) and the [Database per service pattern](https://microservices.io/patterns/data/database-per-service.html). As a result, it is no longer straightforward to implement queries that join data from multiple services.
## Problem

How to implement queries in a microservice architecture?
## Solution

Implement a query by defining an _API Composer_, which invoking the services that own the data and performs an in-memory join of the results.

![](https://microservices.io/i/data/ApiBasedQueryBigPicture.png)

## Example

An [API Gateway](https://microservices.io/patterns/apigateway.html) often does API composition.

## Resulting context

This pattern has the following benefits:

- It a simple way to query data in a microservice architecture

This pattern has the following drawbacks:

- Some queries would result in inefficient, in-memory joins of large datasets.