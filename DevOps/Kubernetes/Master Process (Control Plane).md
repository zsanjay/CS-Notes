
A Kubernetes cluster consists of a control plane and one or more worker nodes. Here's a brief overview of the main components:

### [Control Plane Components](https://kubernetes.io/docs/concepts/overview/components/#control-plane-components)

Manage the overall state of the cluster:
### Api Server

[kube-apiserver](https://kubernetes.io/docs/concepts/architecture/#kube-apiserver)

The core component server that exposes the Kubernetes HTTP API.

The Kubernetes API server validates and configures data for the api objects which include pods, services, replication controllers, and others. The API Server services REST operations and provides the frontend to the cluster's shared state through which all other components interact.

The API server is a component of the Kubernetes [control plane](https://kubernetes.io/docs/reference/glossary/?all=true#term-control-plane) that exposes the Kubernetes API. The API server is the front end for the Kubernetes control plane.

The main implementation of a Kubernetes API server is [kube-apiserver](https://kubernetes.io/docs/reference/generated/kube-apiserver/). kube-apiserver is designed to scale horizontally—that is, it scales by deploying more instances. You can run several instances of kube-apiserver and balance traffic between those instances.

![[20250121105609.png]]

![[20250121105717.png]]

### [kube-scheduler](https://kubernetes.io/docs/concepts/architecture/#kube-scheduler)

Control plane component that watches for newly created [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) with no assigned [node](https://kubernetes.io/docs/concepts/architecture/nodes/), and selects a node for them to run on.

Factors taken into account for scheduling decisions include: individual and collective resource requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, inter-workload interference, and deadlines.

Looks for Pods not yet bound to a node, and assigns each Pod to a suitable node.

![[20250121105941.png]]

### [kube-controller-manager](https://kubernetes.io/docs/concepts/architecture/#kube-controller-manager)

Control plane component that runs [controller](https://kubernetes.io/docs/concepts/architecture/controller/) processes.

In robotics and automation, a _control loop_ is a non-terminating loop that regulates the state of a system.

Here is one example of a control loop: a thermostat in a room.

When you set the temperature, that's telling the thermostat about your _desired state_. The actual room temperature is the _current state_. The thermostat acts to bring the current state closer to the desired state, by turning equipment on or off.

In Kubernetes, controllers are control loops that watch the state of your [cluster](https://kubernetes.io/docs/reference/glossary/?all=true#term-cluster), then make or request changes where needed. Each controller tries to move the current cluster state closer to the desired state.

Logically, each [controller](https://kubernetes.io/docs/concepts/architecture/controller/) is a separate process, but to reduce complexity, they are all compiled into a single binary and run in a single process.

There are many different types of controllers. Some examples of them are:

- Node controller: Responsible for noticing and responding when nodes go down.
- Job controller: Watches for Job objects that represent one-off tasks, then creates Pods to run those tasks to completion.
- EndpointSlice controller: Populates EndpointSlice objects (to provide a link between Services and Pods).
- ServiceAccount controller: Create default ServiceAccounts for new namespaces.

Runs [controllers](https://kubernetes.io/docs/concepts/architecture/controller/) to implement Kubernetes API behavior.

![[20250121110120.png]]

![[20250121110457.png]]

### [etcd](https://kubernetes.io/docs/concepts/architecture/#etcd)

Consistent and highly-available key value store used as Kubernetes' backing store for all cluster data.
If your Kubernetes cluster uses etcd as its backing store, make sure you have a [back up](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster) plan for the data.

Note - Application data is not stored in etcd.

![[20250121110600.png]]

![[20250121110654.png]]

![[20250121110751.png]]

### Multi Master Process

![[20250121111021.png]]

![[20250121111051.png]]


### Cluster Setup

![[20250121111230.png]]

#### Add new Master Node/ Server:

![[20250121111354.png]]

