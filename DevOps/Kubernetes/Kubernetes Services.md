
![Kub20250116121043](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116121043.png)

Kubernetes Services are resources that map network traffic to the Pods in your cluster. You need to create a Service each time you expose a set of Pods over the network, whether within your cluster or externally.

Kubernetes [Services](https://kubernetes.io/docs/concepts/services-networking/service) are API objects that enable network exposure for one or more cluster Pods. Services are integral to the [Kubernetes networking model](https://spacelift.io/blog/kubernetes-networking) and provide important abstractions of lower-level components, which could behave differently between different clouds.

**Note:** The term “service” is often used generically by other tools to refer to an app, workload, or deployment. However, a Kubernetes Service is a specific type of entity that always provides network access to an app or component.

### Why Services are needed in Kubernetes?

Services are necessary because of the distributed architecture of Kubernetes clusters. Apps are routinely deployed as Pods that could have thousands of replicas, spanning hundreds of physical compute Nodes. When a user interacts with your app, their request needs to be routed to any one of the available replicas, regardless of where it’s placed.

Services sit in front of your Pods to achieve this behavior. All network traffic flows into the Service before being redirected to one of the available Pods. Your other apps can then communicate with the service’s IP address or DNS name to reliably access the Pods you’ve exposed.

DNS for Services is enabled automatically through the Kubernetes [service discovery](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service) system. Each Service is assigned a DNS A or AAAA record in the format `` `<service-name>.<namespace-name>.svc.cluster-domain` ``—a service called `demo` in the default namespace will be accessible within a `cluster.local` cluster at `demo.default.svc.cluster.local`, for example. This enables reliable in-cluster networking without having to lookup service IP addresses.

## [Kubernetes Service types](https://spacelift.io/blog/kubernetes-service#kubernetes-service-types)

All Kubernetes Services ultimately forward network traffic to a set of Pods they represent. However, several different types of Service exist with their own characteristics and use cases. Here’s how the five currently available options compare.

![Kub20250116150519](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116150519.png)

### 1. ClusterIP Services

ClusterIP Services assign an IP address that can be used to reach the Service from within your cluster. This type doesn’t expose the Service externally.

ClusterIP is the default service type used when you don’t specify an alternative option. It’s the most common kind of service you’ll use as it enables simple internal networking for your workloads.

![Kub20250116121348](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116121348.png)

![Kub20250116131846](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116131846.png)

![Kub20250116131720](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116131720.png)

#### Multi-Port Services

![Kub20250116132654](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116132654.png)

### Port vs Target Port

![Kub20250116132429](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116132429.png)

### Service Endpoints

![Kub20250116132325](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116132325.png)

### 2. Headless Services

[Headless services](https://kubernetes.io/docs/concepts/services-networking/service/#headless-services) are a special type of Service that don’t provide load balancing or a cluster IP address. They’re “headless” because Kubernetes doesn’t automatically proxy any traffic through them. This allows you to use DNS lookups to discover the individual IP addresses of any pods selected by the service.

![Kub20250116144219](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116144219.png)
A headless service is useful when you want to interface with other service discovery systems without [kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy) interfering. You can create one by specifically setting a Service’s `spec.clusterIP` field to the `None` value.

![20250116145007](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116145007.png)


![20250116145129](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116145129.png)

![20250116145411](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116145411.png)

![20250116145616](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116145616.png)

### NodePort Services

![20250116150558](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116150558.png)

![20250116150651](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116150651.png)

NodePort Services are exposed externally through a specified static port binding on each of your Nodes. Hence, you can access the Service by connecting to the port on any of your cluster’s Nodes. NodePort Services are also assigned a cluster IP address that can be used to reach them within the cluster, just like ClusterIP Services.

![20250116150731](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116150731.png)

![20250116150901](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116150901.png)

![20250116151052](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116151052.png)

![20250116151337](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116151337.png)

![20250116151438](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116151438.png)

![20250116151550](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250116151550.png)

Use of NodePort Services is generally unadvisable. They have functional limitations and can lead to security issues:

- Anyone who can connect to the port on your Nodes can access the Service.
- Each port number can only be used by one NodePort Service at a time to prevent conflicts.
- Every Node in your cluster has to listen to the port by default, even if they’re not running a Pod that’s included in the Service.
- No automatic load-balancing: clients are served by the Node they connect to.

When a NodePort Service is used, it’s generally to facilitate the use of your own load-balancing solution that reroutes traffic from outside the cluster. NodePorts can also be convenient for temporary debugging, development, and troubleshooting scenarios where you need to quickly test different configurations.

### LoadBalancer Services

![20250117094334](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250117094334.png)


![20250117094404](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250117094404.png)


![20250117094449](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250117094449.png)

LoadBalancer Services are exposed outside your cluster using an external load balancer resource. This requires a connection to a load balancer provider, typically achieved by integrating your cluster with your cloud environment. Creating a LoadBalancer service will then automatically provision a new load balancer infrastructure component in your cloud account. This functionality is automatically configured when you use a managed Kubernetes service such as Amazon EKS or Google GKE.

Once you’ve created a LoadBalancer service, you can point your public DNS records to the provisioned load balancer’s IP address. This will then direct traffic to your Kubernetes Service. Therefore, LoadBalancers are the Service type you should normally use when you need an app to be accessible outside Kubernetes.

![20250117094620](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250117094620.png)
### Which Service type should I use?

The correct Kubernetes Service type for a particular workload primarily depends on whether you’ll need external access. If you will interact with a Service from outside Kubernetes, then LoadBalancers should be preferred. A NodePort Service can be useful instead if you can accept its tradeoffs, a load balancer integration is unavailable, or you plan to implement your own load balancing solution.

For workloads that will only be accessed within your cluster—typically including database connections, caches, and other internal system components—ClusterIPs should be used to prevent inadvertently exposing the Service. Developers and operators can still connect to these Services from their workstations to debug problems and manually interact with workloads using the [port-forwarding features](https://spacelift.io/blog/kubectl-port-forward) [available in Kubectl](https://spacelift.io/blog/kubectl-port-forward).

Here’s a quick tabular summary of how each Service type compares.

| **Characteristic**         | **ClusterIP**                                  | **NodePort**                                                            | **LoadBalancer**                                                      | **ExternalName**                                                            | **Headless**                                                                                                      |
| -------------------------- | ---------------------------------------------- | ----------------------------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| **Accessibility**          | Internal                                       | External                                                                | External                                                              | Internal                                                                    | Internal                                                                                                          |
| **Use case**               | Expose Pods to other Pods in your cluster      | Expose Pods on a specific Port of each Node                             | Expose Pods using a cloud load balancer resource                      | Configure a cluster DNS CNAME record that resolves to a specified address   | Interface with external service discovery systems                                                                 |
| **Suitable for**           | Internal communications between workloads      | Accessing workloads outside the cluster, for one-off or development use | Serving publicly accessible web apps and APIs in production           | Decoupling your workloads from direct dependencies on external service URLs | Advanced custom networking that avoids automatic Kubernetes proxying                                              |
| **Client connection type** | Stable cluster-internal IP address or DNS name | Port on Node IP address                                                 | IP address of external load balancer                                  | Stable cluster-internal IP address or DNS name                              | Stable-cluster internal IP address or DNS name that also enables DNS resolution of the Pod IPs behind the Service |
| **External dependencies**  | None                                           | Free port on each Node                                                  | A Load Balancer component (typically billable by your cloud provider) | None                                                                        | None                                                                                                              |

### Ingresses

[Ingresses](https://spacelift.io/blog/kubernetes-ingress) are another type of Kubernetes networking object. They’re often used in conjunction with Services. Whereas Services manage how networking is managed within your cluster, Ingresses are used to control external access, typically based on HTTP and HTTPS routes.

Ingresses make it much easier to run multiple apps in one cluster. Ideally, you want to avoid creating a new LoadBalancer service for every app, as your cloud provider will bill you for each load balancer resource you use. Ingresses allow you to work with one LoadBalancer service that reroutes traffic based on HTTP characteristics such as hostname and port. You can direct requests to app.example.com to your web-app Service, for example, while api.example.com targets the backend Service.

To use Ingresses, you have to run an [Ingress controller](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers) inside your cluster. The controller creates a single LoadBalancer service that you direct all your external traffic to. When requests hit that service, the Ingress controller compares their characteristics to the Ingress rules you’ve created. It then forwards the requests to the Service indicated by the matching Ingress object.

**Note:** Ingress is a mature Kubernetes feature, but work is underway to replace it with the [Gateway API](https://kubernetes.io/docs/concepts/services-networking/gateway), a modern evolution that’s more generic and offers improved separation of concerns. Gateway is installable as an optional add-on in Kubernetes v1.24+, but Ingress remains supported for the foreseeable future.

[Ingresses](https://kubernetes.io/docs/concepts/services-networking/ingress) are closely related objects. These are used to set up HTTP routes to services via a [load balancer](https://spacelift.io/blog/kubernetes-load-balancer). Ingresses also support HTTPS traffic secured by TLS certificates.

![20250117094657](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250117094657.png)


### References

https://spacelift.io/blog/kubernetes-service
