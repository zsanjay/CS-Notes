## What is Kubernetes?

As the definition says, [Kubernetes](https://kubernetes.io/) or k8s is an open-source **orchestration and cluster management for container-based applications** maintained by the Cloud Native Computing Foundation.

The official Kubernetes (k8s) website says,

[Kubernetes](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/) is an open-source system for automating deployment, scaling, and management of containerized applications. It groups containers that make up an application into logical units for easy management and discovery. Kubernetes builds upon [15 years of experience of running production workloads at Google](http://queue.acm.org/detail.cfm?id=2898444), combined with best-of-breed ideas and practices from the community.

In simple words, Kubernetes makes it easy to **manage containers on multiple hosts**. Also, it makes the container deployment so easy using a declarative YAML file.

You specify how you want the container to be deployed, and Kubernetes takes care of it by reading the information provided in the YAML.

## Why do we need Kubernetes?

![Kubernetes](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116104651.png)

The first question when it comes to Kubernetes or a container orchestrator is why we need it. Let’s understand it from two real-world examples.

### Container Deployments

Let’s say you have a couple of Java applications. You can package it into a container and run it on a server containing a Docker engine or any container engine. For this scenario, there is no complexity.

You package your application into a [Docker image using Dockerfile](https://devopscube.com/build-docker-image/) and expose a port on a host for the external world to access it.

However, the downside is that it can be a single point of failure, as it is only running on a single server. To handle the single point of failure, you need an efficient mechanism.

This is why you need a [container orchestration tool](https://devopscube.com/docker-container-clustering-tools/) like Kubernetes to scale applications on-demand and withstand single-node failures.

Kubernetes helps in scaling applications, self-healing, and rolling updates, making it well-suited for running containers.

### Micro services Deployment/Orchestration

Now, let’s say, you have a big application that is composed of [microservices](https://opensource.com/resources/what-are-microservices) (APIs, UI, user management, credit card transaction system, etc). All these micro service components have to talk to each other using REST APIs or other protocols.

As the application has many components or micro services, we cannot deploy all the services in one server or a container. The applications have to be decoupled and each micro service should be deployed and scaled on its own. This makes application development and deployment easier and faster.

In this scenario, the complexity lies in **networking, shared file systems, load balancing, and [service discovery](https://devopscube.com/service-discovery-explained/)**. Here is where Kubernetes comes into the picture. It helps in orchestrating complex processes in a manageable way.

Using Kubernetes, you just have to worry about your application development and deployments.

All heavy lifting like networking, service-to-service communication across nodes, load balancing, service discovery, resource scheduling, scalability, and high availability are taken care of by Kubernetes.

![Kub1](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116105016.png)

Overall Kubernetes helps you achieve the following.

1. Self Healing
2. Automatic Container Scheduling
3. Horizontal and Vertical Scaling
4. Rolling application upgrades and downgrades with zero downtime

## [Kubernetes basic terms and concepts](https://spacelift.io/blog/kubernetes-tutorial#kubernetes-basic-terms-and-concepts)

![Kub2](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116114158.png)

### Nodes

Nodes represent the physical machines that form your [Kubernetes cluster](https://spacelift.io/blog/kubernetes-cluster). They run the containers you create. Kubernetes tracks the status of your nodes and exposes each one as an object. You used Kubectl to retrieve a list of nodes in the example above.

While your fresh cluster has only one node, Kubernetes advertises support for [up to 5,000 nodes](https://kubernetes.io/docs/setup/best-practices/cluster-large). It’s theoretically possible to [scale even further](https://openai.com/index/scaling-kubernetes-to-7500-nodes/).

### Pods

[Pods](https://kubernetes.io/docs/concepts/workloads/pods) are the fundamental compute unit in Kubernetes. A Pod is analogous to a container but with some key differences. Pods can contain multiple containers, each of which share a context. The entire Pod will always be scheduled onto the same node. The containers within a Pod are tightly coupled so you should create a new Pod for each distinct part of your application, such as its API and database.

In simple situations, Pods will usually map one-to-one with the containers your application runs. In more advanced cases, Pods can be enhanced with [init containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers) and [ephemeral containers](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers) to customize startup behavior and provide detailed debugging.
### [[Kubernetes Services]]

### What Is Kubectl-Port-Forward & Why Do We Use It in Kubernetes?

kubectl port-forward is a command that enables you to create a secure tunnel between your local computer and a Pod running in a Kubernetes cluster. This allows you to access the application running in the Pod as though it were running locally on your computer.

By using "kubectl port-forward", you can access resources inside the Pod, making it useful for debugging, testing, and accessing internal resources that are not exposed to the outside world.

## How Does Kubernetes Port Forwarding Work?

Here is how Kubernetes port forwarding works:

- **Command execution**: You execute the "kubectl port-forward" command, specifying the target Pod, local port, and target port.
- **Network connection set up**: A network connection is set up between your local computer and the target Pod with the help of the [Kubernetes API server](https://kodekloud.com/blog/kubernetes-architecture-overview/).
- **Forward network traffic**: Once the network connection is established, any request made from the local computer is forwarded to the target Pod. Similarly, any response from the application running inside the Pod is forwarded back to your local computer. This allows you to interact with the application and diagnose any issues that may arise.

## Kubectl Port-Forward Syntax

The syntax of the "kubectl port-forward" command is as follows:

```sh
kubectl port-forward POD_NAME LOCAL_PORT:REMOTE_POD_PORT
```

kubectl port-forward POD_NAME LOCAL_PORT:REMOTE_POD_PORT

Let's break down the different components of this command:

- **kubectl**: This is the command-line tool used to interact with Kubernetes clusters.
- **port-forward**: This is the action that we want to perform with "kubectl".
- **POD_NAME**: This is the name of the Pod that we want to forward traffic to and from.
- **LOCAL_PORT**: This is the port number on the local machine that we want to use to establish the connection.
- **REMOTE_POD_PORT**: This is the port number on the Pod that we want to connect to.

### Secrets and ConfigMaps

[Secrets](https://spacelift.io/blog/kubernetes-secrets) are used to inject sensitive data into your cluster such as API keys, certificates, and other kinds of credential. They can be supplied to Pods as environment variables or files mounted into a volume.

![Kub3](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250117110529.png)

[ConfigMaps](https://spacelift.io/blog/kubernetes-configmap) are a similar concept for non-sensitive information. These objects should store any general settings your app requires.

![Kub4](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250117110330.png)

### [[Volumes]]

### Deployment and Stateful Set

![Kub20250120134924](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250120134924.png)

![Kub20250120135117](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250120135117.png)

### StatefulSet

![Kub20250120135232](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250120135232.png)

![Kub20250120135329](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250120135329.png)

![Kub20250120135919](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250120135919.png)

### Worker machine in K8s cluster

![Kub20250120142331](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250120142331.png)

![Kub20250120142957](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250120142957.png)

![Kub20250120143036](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250120143036.png)

![Kub20250120143226](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250120143226.png)

![Kub20250120143312](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250120143312.png)


There are three processes must be installed on every node.

1. Container Runtime - Docker
2. Kubelet - Process of Kubernetes 
	1. Kubelet interacts with both - the container and node.
	2. Kubelet starts the pod with a container inside.
3. Kube Proxy

### [[Master Process (Control Plane)]]


### References

https://devopscube.com/kubernetes-tutorials-beginners/

https://kodekloud.com/blog/port-forwarding-kubernetes/

https://spacelift.io/blog/kubernetes-tutorial

https://www.youtube.com/watch?v=X48VuDVv0do


