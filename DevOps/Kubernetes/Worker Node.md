The Worker nodes are responsible for running containerized applications. The worker Node has the following components.

1. kubelet
2. kube-proxy
3. Container runtime

## Kubernetes Worker Node Components

Now let’s look at each of the worker node components.

### 1. Kubelet

Kubelet is an agent component that runs on every node in the cluster. t does not run as a container instead runs as a daemon, managed by systemd.

It is responsible for registering worker nodes with the API server and working with the podSpec (Pod specification – YAML or JSON) primarily from the API server. podSpec defines the containers that should run inside the pod, their resources (e.g. CPU and memory limits), and other settings such as environment variables, volumes, and labels.

It then brings the podSpec to the desired state by creating containers.

To put it simply, kubelet is responsible for the following.

1. Creating, modifying, and deleting containers for the pod.
2. Responsible for handling liveliness, readiness, and startup probes.
3. Responsible for Mounting volumes by reading pod configuration and creating respective directories on the host for the volume mount.
4. Collecting and reporting Node and pod status via calls to the API server with implementations like `cAdvisor` and `CRI`.

Kubelet is also a controller that watches for pod changes and utilizes the node’s container runtime to pull images, run containers, etc.

Other than PodSpecs from the API server, kubelet can accept podSpec from a file, HTTP endpoint, and HTTP server. A good example of “podSpec from a file” is Kubernetes static pods.

Static pods are controlled by kubelet, not the API servers.

This means you can create pods by providing a pod YAML location to the Kubelet component. However, static pods created by Kubelet are not managed by the API server.

Here is a real-world example use case of the static pod.

While bootstrapping the control plane, kubelet starts the api-server, scheduler, and controller manager as static pods from podSpecs located at `/etc/kubernetes/manifests`

Following are some of the key things about kubelet.

1. Kubelet uses the CRI (container runtime interface) gRPC interface to talk to the container runtime.
2. It also exposes an HTTP endpoint to stream logs and provides exec sessions for clients.
3. Uses the CSI (container storage interface) gRPC to configure block volumes.
4. It uses the CNI plugin configured in the cluster to allocate the pod IP address and set up any necessary network routes and firewall rules for the pod.

[![Kubernetes component kubelet explained](https://devopscube.com/wp-content/uploads/2023/01/kubelet-architecture-830x1024.png)](https://devopscube.com/wp-content/uploads/2023/01/kubelet-architecture.png)

- Kubelet is the primary node agent that runs on each node.
- It registers the node with the API Server.
- Kubelet is also a controller for pods.

![[20250120153254.png]]

### 2. Kube proxy

To understand Kube proxy, you need to have a basic knowledge of Kubernetes Service & endpoint objects.

Service in Kubernetes is a way to expose a set of pods internally or to external traffic. When you create the service object, it gets a virtual IP assigned to it. It is called clusterIP. It is only accessible within the Kubernetes cluster.

The Endpoint object contains all the IP addresses and ports of pod groups under a Service object. The endpoints controller is responsible for maintaining a list of pod IP addresses (endpoints). The service controller is responsible for configuring endpoints to a service.

You cannot ping the ClusterIP because it is only used for service discovery, unlike pod IPs which are pingable.

Now let’s understand Kube Proxy.

Kube-proxy is a daemon that runs on every node as a [daemonset](https://devopscube.com/kubernetes-daemonset/). It is a proxy component that implements the Kubernetes Services concept for pods. (single DNS for a set of pods with load balancing). It primarily proxies UDP, TCP, and SCTP and does not understand HTTP.

When you expose pods using a Service (ClusterIP), Kube-proxy creates network rules to send traffic to the backend pods (endpoints) grouped under the Service object. Meaning, all the load balancing, and service discovery are handled by the Kube proxy.

**So how does Kube-proxy work?**

Kube proxy talks to the API server to get the details about the Service (ClusterIP) and respective pod IPs & ports (endpoints). It also monitors for changes in service and endpoints.

Kube-proxy then uses any one of the following modes to create/update rules for routing traffic to pods behind a Service

1. **[IPTables](https://wiki.centos.org/HowTos/Network/IPTables)**: It is the default mode. In IPTables mode, the traffic is handled by IPtable rules. This means that for each service, IPtable rules are created. These rules capture the traffic coming to the ClusterIP and then forward it to the backend pods. Also, In this mode, kube-proxy chooses the backend pod random for load balancing. Once the connection is established, the requests go to the same pod until the connection is terminated.

2. **[IPVS](https://en.wikipedia.org/wiki/IP_Virtual_Server):** For clusters with services exceeding 1000, IPVS offers performance improvement. It supports the following load-balancing algorithms for the backend.
    1. `rr`: round-robin : It is the default mode.
    2. `lc`: least connection (smallest number of open connections)
    3. `dh`: destination hashing
    4. `sh`: source hashing
    5. `sed`: shortest expected delay
    6. `nq`: never queue

3. **Userspace** (legacy & not recommended)
4. **Kernelspace**: This mode is only for Windows systems.

[![how does kube proxy work - Workflow](https://devopscube.com/wp-content/uploads/2023/01/image-14-748x1024.png)](https://devopscube.com/wp-content/uploads/2023/01/image-14.png)

If you would like to understand the performance difference between kube-proxy IPtables and IPVS mode, [read this article](https://www.tigera.io/blog/comparing-kube-proxy-modes-iptables-or-ipvs/).

Also, you can run a Kubernetes cluster without kube-proxy by replacing it with [Cilium](https://docs.cilium.io/en/v1.9/gettingstarted/kubeproxy-free/).

> **1.29** **Alpha Feature:** Kubeproxy has a new 𝗻𝗳𝘁𝗮𝗯𝗹𝗲𝘀 based backend. nftables is the successor of IPtables that is Designed to be simpler and more efficient.

- Process that runs on each node.
- Implements part of the kubernetes services concept.
- Listens for the creation of new services.
- Forwards traffic using IPtables or IPVS.
	- IPVS preferred with high number of services.
- Manages the rules to load balance traffic to backend pods.

### 3. Container Runtime

You probably know about [Java Runtime (JRE)](https://aws.amazon.com/what-is/java-runtime-environment/). It is the software required to run Java programs on a host. In the same way, container runtime is a software component that is required to run containers.

Container runtime runs on all the nodes in the Kubernetes cluster. It is responsible for pulling images from container registries, running containers, allocating and isolating resources for containers, and managing the entire lifecycle of a container on a host.

To understand this better, let’s take a look at two key concepts:

1. **Container Runtime Interface (CRI):** It is a set of APIs that allows Kubernetes to interact with different container runtimes. It allows different container runtimes to be used interchangeably with Kubernetes. The CRI defines the API for creating, starting, stopping, and deleting containers, as well as for managing images and container networks.
2. **Open Container Initiative (OCI):** It is a set of standards for container formats and runtimes

Kubernetes supports multiple [container runtimes](https://kubernetes.io/docs/setup/production-environment/container-runtimes/) (CRI-O, Docker Engine, containerd, etc) that are compliant with **[Container Runtime Interface (CRI)](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-node/container-runtime-interface.md)**. This means, all these container runtimes implement the CRI interface and expose gRPC [CRI APIs](https://kubernetes.io/docs/concepts/architecture/cri/) (runtime and image service endpoints).

So how does Kubernetes make use of the container runtime?

As we learned in the Kubelet section, the kubelet agent is responsible for interacting with the container runtime using CRI APIs to manage the lifecycle of a container. It also gets all the container information from the container runtime and provides it to the control plane.

Let’s take an example of [CRI-O](https://cri-o.io/) container runtime interface. Here is a high-level overview of how container runtime works with kubernetes.

[![Kubernetes container runtime CRI-O overview](https://devopscube.com/wp-content/uploads/2022/12/image-5-1024x702.png)](https://devopscube.com/wp-content/uploads/2022/12/image-5.png)

1. When there is a new request for a pod from the API server, the kubelet talks to CRI-O daemon to launch the required containers via Kubernetes Container Runtime Interface.
2. CRI-O checks and pulls the required container image from the configured container registry using **`containers/image`** library.
3. CRI-O then generates OCI runtime specification (JSON) for a container.
4. CRI-O then launches an OCI-compatible runtime (runc) to start the container process as per the runtime specification.