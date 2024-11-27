
When making a request to a service, the client makes a request via a router (a.k.a load balancer) that runs at a well known location. The router queries a [service registry](https://microservices.io/patterns/service-registry.html), which might be built into the router, and forwards the request to an available service instance.

The following diagram shows the structure of this pattern.

![](https://microservices.io/i/servicediscovery/server-side-discovery.jpg)

## Examples

An AWS Elastic Load Balancer (ELB) is an example of a server-side discovery router. A client makes HTTP(s) requests (or opens TCP connections) to the ELB, which load balances the traffic amongst a set of EC2 instances. An ELB can load balance either external traffic from the Internet or, when deployed in a VPC, load balance internal traffic. An ELB also functions as a [Service Registry](https://microservices.io/patterns/service-registry.html). EC2 instances are registered with the ELB either explicitly via an API call or automatically as part of an auto-scaling group.

Some clustering solutions such as [Kubernetes](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/services.md) and [Marathon](https://mesosphere.github.io/marathon/docs/service-discovery-load-balancing.html) run a proxy on each host that functions as a server-side discovery router. In order to access a service, a client connects to the local proxy using the port assigned to that service. The proxy then forwards the request to a service instance running somewhere in the cluster.

## Resulting context

Server-side service discovery has a number of benefits:

- Compared to [client-side discovery](https://microservices.io/patterns/client-side-discovery.html), the client code is simpler since it does not have to deal with discovery. Instead, a client simply makes a request to the router
- Some cloud environments provide this functionality, e.g. AWS Elastic Load Balancer

It also has the following drawbacks:

- Unless it’s part of the cloud environment, the router must is another system component that must be installed and configured. It will also need to be replicated for availability and capacity.
- The router must support the necessary communication protocols (e.g HTTP, gRPC, Thrift, etc) unless it is TCP-based router
- More network hops are required than when using [Client Side Discovery](https://microservices.io/patterns/client-side-discovery.html)