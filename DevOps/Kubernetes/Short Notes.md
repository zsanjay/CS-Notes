### What is Kubernetes?

1. Open source container orchestration tool.
2. Developed by Google.
3. Helps you manage containerized applications in different environment like physical, virtual, cloud and hybrid.

#### What problems does kubernetes solve and what are the tasks of an orchestration tool?

The need for a container orchestration tool.

1. Trend from Monolithic to Microservices.
2. Increase usage of containers.

#### What features do orchestration tools offer?

1. High Availability or no downtime.
2. Scalability or high performance.
3. Disaster recovery - backup and restore.

### Kubernetes Components

1. Pod
2. Service
3. Ingress
4. Volumes
5. Secrets
6. ConfigMap
7. StatefulSet 
8. Deployment

### Node and Pod

### Pod

1. Smallest unit of K8s.
2. Abstraction over container.
3. Usually 1 application per Pod.
4. Each Pod gets its own IP address.
5. Pods are ephemeral (temporary or short-lived) if the application crashes inside the container then the new pod will be re-created.
6. New IP address on re-creation.

Note - You only interact with the Kubernetes layer, don't need to manage containers.

### Service (Like Elastic Ip in AWS)

1. Permanent IP address.
2. Lifecycle of Pod and Service NOT connected.

#### You want your application should be accessible through browser.

There are two types of services.

1. Internal Service for internal applications like databases.
2. External Service for applications which is accessible through browsers.


IP format = http://NODE_IP_ADDRESS:SERVICE_PORT_NUMBER good for the test purposes.

### Ingress

In Kubernetes, **Ingress** is an API object that manages external access to services within a cluster, typically HTTP/HTTPS traffic. It provides **routing rules** that control how requests reach different services based on domain names, paths, or other conditions.

### 🔹 **Key Features of Ingress**:

1. **Path-Based Routing** – Routes traffic to different services based on URL paths.
2. **Host-Based Routing** – Routes traffic based on the requested domain name.
3. **TLS Termination** – Supports HTTPS by terminating SSL/TLS at the Ingress controller.
4. **Load Balancing** – Distributes traffic across multiple pods of a service.
5. **Rewrite & Redirect** – Can modify request URLs before forwarding them.

### 🔹 **Ingress vs. Other Networking Options**

- **NodePort**: Exposes a service on each node's IP at a static port.
- **LoadBalancer**: Provisions an external load balancer (cloud provider dependent).
- **Ingress**: Provides more advanced routing and is usually used with an **Ingress Controller**.

### 🔹 **Example Ingress Resource**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /service1
        pathType: Prefix
        backend:
          service:
            name: service1
            port:
              number: 80
      - path: /service2
        pathType: Prefix
        backend:
          service:
            name: service2
            port:
              number: 80
```

### 🔹 **Ingress Controller**

An **Ingress resource** alone is not enough; you need an **Ingress Controller** to process and enforce rules. Popular controllers include:

- **NGINX Ingress Controller** (most common)
- **Traefik**
- **HAProxy**
- **AWS ALB Ingress Controller** (for AWS environments)


![[20250320102851.png]]

### ConfigMap 

If the database URL changes and it usually configured in the built application using application.properties file. We need to rebuild the docker image and push it to the repo and kubernetes pull it in your pod and restarts the application.

ConfigMap is the external configuration of your application like db url etc.

DB_URL = mongo-db

Don't put credentials into ConfigMap.

### Secret

1. Used to store secret data.
2. Base64 encoded.

DB_USER = mongo-user
DB_PWD = mongo_pwd

The build-in security mechanism is not enabled by default!

Use ConfigMap or Secret as environment variables or as a properties file.

### Volumes

#### Data Storage

1. Storage on local machine.
2. or remote, outside of the k8s cluster.

The volume component attaches a physical storage on a hard drive to your pod and that storage could be either on a local machine meaning on the same server node where the Pod is running or it could be on a remote storage which outside of the kubernetes cluster.

Remote Storage could be cloud storage or on premises storage.

So, when the database pod or container gets restarted all the data will be there persisted.

Note - K8s doesn't manage data persistance!

### How to achieve high availability ?

If an application pod dies in the production environment, user will face a downtime. To avoid this, we need to replicate the pod over multiple nodes.

There is no single point of failure and the replica is connected to the same Service.

Service has two functionalities:

1. permanent IP
2. load balancer.

To specify the number of replicas for the pod, define blueprints for pods.

#### Deployment for Stateless apps

1. blueprint for my-app pods.
2. you create Deployments.
3. abstraction of Pods.

Note - DB can't be replicated via Deployment, because it has state.

Problem with database, if db pod is replicated then multiple db pods will access the same db and pods will read and write from the same database. So, if two or more pods writing to the same data then it will create inconsistencies. To avoid data inconsistencies, we need to use another component called StatefulSet.

### StatefulSet for stateful apps or databases.

1. Application like mongodb, mysql, elastic search etc.

Note - Deploying StatefulSet is not easy and DB are often hosted outside of K8s cluster.

![[20250320113928.png]]

### Kubernets Architecture

#### Node processes

#### Worker machine

1. Each node has multiple pods on it.
2. 3 processes must be installed on every Node.
3. Worker Node do the actual work.


#### 3 processes

1. Container Runtime - docker
2. Kubelet - It interacts with both - the container and node and it also starts the pod with a container inside.
3. Kube Proxy forwards the request from the service to the pod.

### How do you interact with this cluster?

#### How to:

1. Schedule pod?
2. monitor?
3. re-schedule/re-start pod?
4. how a new node joins the cluster?

All the above problems, solved by master node.

### Master processes or node

4 processes run on every master node.

1. Api Server (Like API Gateway in AWS)
	1. Cluster Gateway
	2. Acts as a gatekeeper for authentication!
	3. Client(sends request) -> API Server -> validates request -> forward the request to Scheduler ->  schedule the pod.
	4. Only 1 entrypoint into the cluster.
2. Scheduler
	1. It decides where to put the pod on the basis of resource availability on the node.
	2. It just decides on which Node new Pod should be scheduled.
	3. Scheduler forward the request to kubelet to start the pod with the container on the decided node.
3. Controller Manager
	1. It detects cluster state changes and check if any pod dies.
	2. It request the scheduler to re-schedule the pods and scheduler does the same process of making decision on which node it has to schedule the pod and kubelet restarts the pod.
4.  etcd - key value store
	1. It is the cluster brain
	2. Cluster changes get stored in the key value store.
	3. It answers all the information like:
		1. Is the cluster healthy?
		2. What resources are available?
		3. Did the cluster state change?
	4. Application data is NOT stored in etcd!


#### Kubernetes cluster made up of multiple master nodes:
	1. Multiple Api server on master nodes is load balanced.
	2. etcd provides distributed storage across all master nodes.

### Cluster Setup with 2 master node and 3 worker nodes

![[20250320123350.png]]

### Add new Master/Node server:

![[20250320123514.png]]


### Minikube 

It is the one node cluster, where master and worker processes run on the same node.
Docker runtime is pre-installed. 

1. It creates Virtual Box on your laptop.
2. Node runs in that Virtual Box.
3. 1 Node K8s cluster
4. for testing purposes.

### Kubectl

It is a command line tool for K8s cluster. It interacts with the cluster using commands to create multiple components in the node like Pod, Service, Secret and ConfigMap.

There are various ways through you can interact with API server.

1. UI
2. API (SDK)
3. CLI - Kubectl - the most powerful among 3 clients 

Note - Kubectl can be used to work with any cluster whether it is minikube or cloud cluster.

To run K8s cluster, virtualization on your machine needed.
### CLI Commands

### install hyperhit and minikube

`brew update`

`brew install hyperkit`

`brew install minikube`

`kubectl`

`minikube`

### [create minikube cluster](#create-minikube-cluster)

`minikube start --driver=docker --alsologtostderr` 

`kubectl get nodes`

`minikube status`

`kubectl version`

### [delete cluster and restart in debug mode](#delete-cluster-and-restart-in-debug-mode)

`minikube delete`

`minikube start --driver=docker --v=7 --alsologtostderr`

`minikube status`

### [kubectl commands](#kubectl-commands)

`kubectl get nodes`

`kubectl get pod`

`kubectl get services`

`kubectl create deployment nginx-depl --image=nginx`

`kubectl get deployment`

`kubectl get replicaset`

`kubectl edit deployment nginx-depl`

### [debugging](#debugging)

`kubectl logs {pod-name}`

`kubectl exec -it {pod-name} -- bin/bash`

### [create mongo deployment](#create-mongo-deployment)

`kubectl create deployment mongo-depl --image=mongo`

`kubectl logs mongo-depl-{pod-name}`

`kubectl describe pod mongo-depl-{pod-name}`

### [delete deployment](#delete-deployment)

`kubectl delete deployment mongo-depl`

`kubectl delete deployment nginx-depl`

### [create or edit config file](#create-or-edit-config-file)

`vim nginx-deployment.yaml`

`kubectl apply -f nginx-deployment.yaml`

`kubectl get pod`

`kubectl get deployment`

### [delete with config](#delete-with-config)

`kubectl delete -f nginx-deployment.yaml`

#Metrics

`kubectl top` The kubectl top command returns current CPU and memory usage for a cluster’s pods or nodes, or for a particular pod or node if specified.

Note - Minikube has kubectl as dependency, no separate installation needed.

### YAML Configuration file in Kubernetes

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: nginx-deployment
	labels:
		app: nginx
spec: // specification for deployment
	replicas: 1
	selector:
		matchLabels:
			app: nginx
	template:
		metadata:
			labels:
				app: nginx
		spec: // specification for pod
			containers:
			- name: nginx
			  image: nginx:1.16
			  ports:
			  - containerPort: 80
```

### Overview:

- The 3 parts of configuration file.
- Connecting Deployments to Service to Pods.

Each configuration file has 3 parts.

1) metadata
2) specification
3) status - Automatically generated and added by Kubernetes!
#### Deployment

nginx-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: nginx-deployment
	labels:
		app: nginx
spec: // specification for deployment
	replicas: 2
	selector:
		matchLabels:
			app: nginx
```

#### Service

nginx-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
	name: nginx-service
spec: // specification for service
	selector:
	ports:
```

`Attributes of "spec" are specific to the kind!`

### Status

K8s maintain the status of the component if the desired configuration is not equals to actual configuration then it takes the action to make it same.

Desired == Actual

K8s updates state continuously.

Where does K8s get this status data?

Etcd holds the current status of any K8s component!

### YAML configuration files

	- "Human friendly data serialization standard for all programming languages"
	- syntax : strict indentation!
	- store the config file with your code or own git repository


### Blueprint for pods (Template)

```YAML
template:
		metadata:
			labels:
				app: nginx
		spec: // specification for pod
			containers:
			- name: nginx
			  image: nginx:1.16
			  ports:
			  - containerPort: 80
```

	- has it's own metadata and spec section
	- applies to Pod
	- blueprint for a Pod

### Connecting components (Labels and Selectors and Ports)

```yaml
apiVersion: v1
kind: Service
metadata:
	name: nginx-service
spec: // specification for service
	selector:
	ports:
```

- Metadata parts contain the labels and specification parts contain selectors.
- Any key-value pair for component.
- Pods get the label through the template blueprint.
- This label is matched by the selector.

### Connecting Services to Deployments

![[20250326103629.png]]

### Ports in Service and Pod

```yaml
ports:
	- protocol: TCP
	  port: 80
	  targetPort: 8080
```

nginx Service has a port, where service itself is accessible at.

DB Service -> port: 80 ->  nginx Service -> targetPort:8080 -> Pod 

In this DB service is accessing the nginx service on port 80 and nginx service is accessing other service pod using targetPort.

#### Create a deployment
```yaml
kubectl apply -f nginx-deployment.yaml
```
#### Create a service

```yaml
kubectl apply -f nginx-deployment.yaml
```

#### Command to get the running Pod.

```yaml
kubectl get pod
```
#### Command to get the service.

```yaml
kubectl get service
```

#### To check the target pod and endpoints of the service.

```yaml
kubectl describe service nginx-service (service name) 
```

### To get the IP address of the pod.

```yaml
kubectl get pod -o wide
```

#### To get the deployment status from the ectd.

```yaml
kubectl get deployment nginx-deployment -o yaml > nginx-deployment-result.yaml
```

#### To delete the deployment

```yaml
kubectl delete -f nginx-deployment.yaml
```


#### Demo Project : MongoDB and MongoExpress

![[20250326113824.png]]

2 Deployment / Pod
2 Service 
1 ConfigMap
1 Secret

1. Mongo db Pod  and Internal Service 
2. ConfigMap to store the DB url.
3. Secret to store the db username and password.
4. Mongo Express (UI for the MongoDB) with the external service to connect with the browser.

#### Request Flow through the K8s components.

Browser -> Mongo Express (External Service) -> Mongo Express (Pod) -> MongoDB Internal Service -> MongoDB Pod.

Internal Service using the secret like DB User and DB Password.

To get all the components.

```yaml
kubectl get all
```

### All the values in secret must be in Base64.

```
echo -n 'username' | base64
```

### Filter the components with name mongodb 

```yaml
kubectl get all | grep mongodb
```

### To check the logs of the pod.

```yaml
kubectl logs pod-name
```

### How to make it an External Service?

1. type : "Loadbalancer"


It assigns service an external IP address and so accepts external requests.
But internal service also acts as a load balancer.

1. nodePort - must be between 30000 - 32767

Port for external IP address.
Port you need to put into browser.



```
kubectl get service

NAME                    TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE

kubernetes              ClusterIP      10.96.0.1      <none>        443/TCP          64d

mongo-express-service   LoadBalancer   10.99.188.73   <pending>     8081:30000/TCP   62d

mongodb-service         ClusterIP      10.98.83.38    <none>        27017/TCP        62d
```


Internal Service or Cluster IP is DEFAULT.
Load balancer assigns in addition an External-IP

For Internal Service we have only one port  27017/TCP but for external service we have two ports - 8081:30000/TCP

### Command to run external service:

```
minikube service mongo-express-service

|-----------|-----------------------|-------------|---------------------------|

| NAMESPACE |         NAME          | TARGET PORT |            URL            |

|-----------|-----------------------|-------------|---------------------------|

| default   | mongo-express-service |        8081 | http://192.168.49.2:30000 |

|-----------|-----------------------|-------------|---------------------------|

🏃  Starting tunnel for service mongo-express-service.

|-----------|-----------------------|-------------|------------------------|

| NAMESPACE |         NAME          | TARGET PORT |          URL           |

|-----------|-----------------------|-------------|------------------------|

| default   | mongo-express-service |             | http://127.0.0.1:63749 |

|-----------|-----------------------|-------------|------------------------|
```


### Namespace 

1. Organize resources in namespaces.
2. You can have multiple namespaces in the cluster.
3. Virtual cluster inside a cluster.
4. There are 4 namespaces per default.

```shell
kubectl get namespace

NAME              STATUS   AGE

default           Active   64d
kube-node-lease   Active   64d
kube-public       Active   64d
kube-system       Active   64d
```

#### kube-system namespace

1. Do NOT create or modify in kube-system.
2. System processes
3. Master and Kubectl processes.

#### kube-public namespace

1. It contains publicly accessible data.
2. A configmap, which contains cluster information.

```shell
kubectl cluster-info

Kubernetes control plane is running at https://127.0.0.1:62974

CoreDNS is running at https://127.0.0.1:62974/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

#### kube-node-lease namespace

1. heartbeats of nodes
2. each node has associated lease object in namespace.
3. determines the availability of a node.

### default namespace

Resources you created are located here.


#### Create your own namespace.

```shell
kubectl create namespace my-namespace

namespace/my-namespace created

kubectl get namespaces               

NAME              STATUS   AGE

default           Active   64d
kube-node-lease   Active   64d
kube-public       Active   64d
kube-system       Active   64d
my-namespace      Active   5s
```

#### Create a namespace with a configuration file.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
	name: mysql-configmap
	namespace : my-namespace
data:
	db_url: mysql-service.database
```


What are Namespaces?

1. Cluster inside a cluster.
2. Default Namespaces.

#### Why to use Namespaces - same like microservices (High cohesion and loose coupling)?

1. Everything in one default or custom namespace.

Problem is all the components will be created in one namespace like deployments, replicasets, services and configmaps. There is no separation and overview, what are the components are there and the purpose of it.

![[20250327104142.png]]

We can have various namespaces for different layers of an application like database, monitoring, elastic stack and nginx-ingress.

We shouldn't use namespaces for smaller projects.

1. Conflicts : Many teams, same application.

If multiple teams are using the same namespace, one team can override the deployment of other team by deploying the component with the same name, by different configuration.

Solution is to create different namespaces for different project or teams.

1. Project A Namespace
2. Project B Namespace

3. Resource Sharing:  

a) Staging and Development

Suppose you can have different environment like development and staging, and you want to deploy components for elastic stack for monitoring, you need to deploy it in both the namespaces instead create a new namespace for elastic stack and re-use those components in both the environments.

b) Blue/Green Deployment:

The versions of application can differ, suppose you have old version is already deployed and new version of application is going to be deployed and both want to use the elastic stack in that scenario you may encounter the problem and reuse the components.

4. Access and Resource Limits on Namespaces.

	a) Each team has own, isolated environment.
	b) Limit: CPU, RAM, Storage per NS like Resource Quota A for namespace 1 and Resource Quota B for namespace 2.


#### Use Cases when to use Namespaces

1. Structure your components.
2. Avoid conflicts between teams.
3. Share services between different environments.
4. Access and Resource Limits on Namespaces Level.

### Characteristics of Namespaces?

You can't access most resources from another Namespace.

Each NS must define own ConfigMap and Secret.

Access Service in another Namespace.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
	name: mysql-configmap
data:
	db_url: mysql-service.database // service-name.namespace_name
```

#### Components, which can't be created within a Namespace.

1. live globally in a cluster.
2. you can't isolate them
3. Examples - volume and node.

#### Check the components which are not name spaced.

```
kubectl api-resources --namespaced=false

NAME                                SHORTNAMES   APIVERSION                        NAMESPACED   KIND

componentstatuses                   cs           v1                                false        ComponentStatus

namespaces                          ns           v1                                false        Namespace

nodes                               no           v1                                false        Node

persistentvolumes                   pv           v1                                false        PersistentVolume

mutatingwebhookconfigurations                    admissionregistration.k8s.io/v1   false        MutatingWebhookConfiguration

validatingadmissionpolicies                      admissionregistration.k8s.io/v1   false        ValidatingAdmissionPolicy

validatingadmissionpolicybindings                admissionregistration.k8s.io/v1   false        ValidatingAdmissionPolicyBinding

validatingwebhookconfigurations                  admissionregistration.k8s.io/v1   false        ValidatingWebhookConfiguration

customresourcedefinitions           crd,crds     apiextensions.k8s.io/v1           false        CustomResourceDefinition

apiservices                                      apiregistration.k8s.io/v1         false        APIService

selfsubjectreviews                               authentication.k8s.io/v1          false        SelfSubjectReview

tokenreviews                                     authentication.k8s.io/v1          false        TokenReview

selfsubjectaccessreviews                         authorization.k8s.io/v1           false        SelfSubjectAccessReview

selfsubjectrulesreviews                          authorization.k8s.io/v1           false        SelfSubjectRulesReview

subjectaccessreviews                             authorization.k8s.io/v1           false        SubjectAccessReview

certificatesigningrequests          csr          certificates.k8s.io/v1            false        CertificateSigningRequest

flowschemas                                      flowcontrol.apiserver.k8s.io/v1   false        FlowSchema

prioritylevelconfigurations                      flowcontrol.apiserver.k8s.io/v1   false        PriorityLevelConfiguration

ingressclasses                                   networking.k8s.io/v1              false        IngressClass

runtimeclasses                                   node.k8s.io/v1                    false        RuntimeClass

clusterrolebindings                              rbac.authorization.k8s.io/v1      false        ClusterRoleBinding

clusterroles                                     rbac.authorization.k8s.io/v1      false        ClusterRole

priorityclasses                     pc           scheduling.k8s.io/v1              false        PriorityClass

csidrivers                                       storage.k8s.io/v1                 false        CSIDriver

csinodes                                         storage.k8s.io/v1                 false        CSINode

storageclasses                      sc           storage.k8s.io/v1                 false        StorageClass

volumeattachments                                storage.k8s.io/v1                 false        VolumeAttachment
```


#### Check the components which are name spaced.

```yaml
kubectl api-resources --namespaced=true 

NAME                        SHORTNAMES   APIVERSION                     NAMESPACED   KIND

bindings                                 v1                             true         Binding

configmaps                  cm           v1                             true         ConfigMap

endpoints                   ep           v1                             true         Endpoints

events                      ev           v1                             true         Event

limitranges                 limits       v1                             true         LimitRange

persistentvolumeclaims      pvc          v1                             true         PersistentVolumeClaim

pods                        po           v1                             true         Pod

podtemplates                             v1                             true         PodTemplate

replicationcontrollers      rc           v1                             true         ReplicationController

resourcequotas              quota        v1                             true         ResourceQuota

secrets                                  v1                             true         Secret

serviceaccounts             sa           v1                             true         ServiceAccount

services                    svc          v1                             true         Service

controllerrevisions                      apps/v1                        true         ControllerRevision

daemonsets                  ds           apps/v1                        true         DaemonSet

deployments                 deploy       apps/v1                        true         Deployment

replicasets                 rs           apps/v1                        true         ReplicaSet

statefulsets                sts          apps/v1                        true         StatefulSet

localsubjectaccessreviews                authorization.k8s.io/v1        true         LocalSubjectAccessReview

horizontalpodautoscalers    hpa          autoscaling/v2                 true         HorizontalPodAutoscaler

cronjobs                    cj           batch/v1                       true         CronJob

jobs                                     batch/v1                       true         Job

leases                                   coordination.k8s.io/v1         true         Lease

endpointslices                           discovery.k8s.io/v1            true         EndpointSlice

events                      ev           events.k8s.io/v1               true         Event

ingresses                   ing          networking.k8s.io/v1           true         Ingress

networkpolicies             netpol       networking.k8s.io/v1           true         NetworkPolicy

poddisruptionbudgets        pdb          policy/v1                      true         PodDisruptionBudget

rolebindings                             rbac.authorization.k8s.io/v1   true         RoleBinding

roles                                    rbac.authorization.k8s.io/v1   true         Role

csistoragecapacities                     storage.k8s.io/v1              true         CSIStorageCapacity
```

### Get Config Map

By default, if you don' t provide namespace in the configuration it will create the component into the default namespace.

```shell
kubectl get configmap           

NAME                DATA   AGE

kube-root-ca.crt    1      64d
mongodb-configmap   1      63d
```

```shell
kubectl get configmap -n default       

NAME                DATA   AGE

kube-root-ca.crt    1      64d
mongodb-configmap   1      63d
```


### To get the detailed output in yaml.

Use -o yaml flag.

```
kubectl get configmap -o yaml

apiVersion: v1

items:

- apiVersion: v1

  data:

    ca.crt: |

      -----BEGIN CERTIFICATE-----

      MIIDBjCCAe6gAwIBAgIBATANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwptaW5p

      a3ViZUNBMB4XDTI1MDEyMDA5MzY0OFoXDTM1MDExOTA5MzY0OFowFTETMBEGA1UE

      AxMKbWluaWt1YmVDQTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAJ9e

      G0zoB0paAX2zdzBHpudlP/tQ0iCJqcjYp/ytSiPHdTP9AsSewnz//onF3ubDBqBl

      3MNp9MKYLRqhbCoVuBTvQfawkVPSTppsY11T6Nt/1dKEB6KmAUEercuZy3V8CHSe

      2LqrDxQaa1ZMct7AeTH0f+ZWIBM4Lo6Ds1K/3lndLIBxAlrke1da2Nknn+877sqh

      R8vupOnHB4fGDZb47GHXLj6mp8hRlnVSSsyN6DGvjFoCLyF63ntyphXGXsQxwDUO

      7yn/f40Bw1f+jq7QNnKWDQnEguj/QXiHmOM3ty8JcybHzM51yXggvj/P1tVzBnee

      3d0G2V5SmSWf2ZnpgwsCAwEAAaNhMF8wDgYDVR0PAQH/BAQDAgKkMB0GA1UdJQQW

      MBQGCCsGAQUFBwMCBggrBgEFBQcDATAPBgNVHRMBAf8EBTADAQH/MB0GA1UdDgQW

      BBQv/DlkJlb/W9otDw6R9UY4UMCRkTANBgkqhkiG9w0BAQsFAAOCAQEAeQQHn1tt

      gfcW4nO58zlOE3KETiwlIjiPCv2D1MAJ697k93eqrRemV7krQGszx9zyke8gdDSs

      rQfCEjdGuql1cKgw0PTsJzTJhm9mPjwkLcX5OFPzRCPCdttdvS8fBrJASpVWiFov

      1Ip3EtmutuaIeOhceAE4eVeJ8FULS3qKRmLbIQMkXX4t+RyGRG8w0WY8CcmyqJpI

      Vmz5P9T9P7pcty3LPz8XBe2WSnv3C7TCuL8liPZlZzSefv3isdx9LeRmdVqSD3j8

      09tlFtPSC51BT2J7elPZHa+4Br+Tw2HaSQEuJALcoiHwToi1Q+gmb9J2OwKowDOG

      rw0YEnBIuQN+2Q==

      -----END CERTIFICATE-----

  kind: ConfigMap

  metadata:

    annotations:

      kubernetes.io/description: Contains a CA bundle that can be used to verify the

        kube-apiserver when using internal endpoints such as the internal service

        IP or kubernetes.default.svc. No other usage is guaranteed across distributions

        of Kubernetes clusters.

    creationTimestamp: "2025-01-21T09:37:01Z"

    name: kube-root-ca.crt

    namespace: default

    resourceVersion: "313"

    uid: 435ec5c6-606e-4409-99e7-7da43fa488dc

- apiVersion: v1

  data:

    database_url: mongodb-service

  kind: ConfigMap

  metadata:

    annotations:

      kubectl.kubernetes.io/last-applied-configuration: |

        {"apiVersion":"v1","data":{"database_url":"mongodb-service"},"kind":"ConfigMap","metadata":{"annotations":{},"name":"mongodb-configmap","namespace":"default"}}

    creationTimestamp: "2025-01-22T10:13:07Z"

    name: mongodb-configmap

    namespace: default

    resourceVersion: "26377"

    uid: 9aeb09b0-1fcd-4edd-8771-998b53c0cca5

kind: List

metadata:

  resourceVersion: ""
```

## Create component in a Namespace.
##### We can provide the namespace using cli:

--namespace = namespace name.

```shell
kubectl apply -f mysql-configmap.yaml --namespace=my-namespace
```

### Provide namespace in config file.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
	name: mysql-configmap
	namespace: my-namespace
data:
	db_url: mysql-service.database // service-name.namespace_name

```

### To get the component from the specific namespace.

```bash
kubectl get configmap -n my-namespace
```

By default, K8s check in the default namespace only.

	- Configuration file over kubectl cmd
	- Better documented
	- More convenient

#### Change Active Namespace

Change the active namespace with kubens!

To install kubens, use the below command:

```bash
brew install kubectx
```

### To check the active namespace

```bash
kubens

default - Highlighted
kube-node-lease
kube-public
kube-system
my-namespace
```

### To change the active namespace

```shell
kubens my-namespace

Context "minikube" modified.
Active namespace is "my-namespace".
```

### K8s Ingress explained

#### External Service vs Ingress

In external service, we need to provide the IP address of the node and the port, but it is not the recommended practice for the end product.

In Ingress, we can use the URL or Domain Name instead of the IP and then the request route to the internal service.

#### YAML Configuration Files

External Service 

```yaml
apiVersion: v1
kind: Service
metadata:
	name: myapp-external-service
spec:
	selector:
		app: myapp
	type: LoadBalancer
	ports:
		- protocol: TCP
		  port: 8080
		  targetPort: 8080
		  nodePort: 35010
```


This service can be accessible on node-IP-address:nodePort

http://124.89.101.2:35010

Ingress

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
	name: myapp-ingress
spec:
	rules:
	- host: my-app.com
	  http:
		paths:
		- backend:
			serviceName: myapp-internal-service
			servicePort : 8080
```

http://my-app.com

![[20250328111301.png]]
##### Routing rules : Forward request to the internal service.

Note here for security nothing is configured yet.

http : Step 2 Incoming Request gets forwarded to the internal service.

Request from browser to ingress.

![[20250328112146.png]]

Host:

- valid domain address
- map domain name to Node's IP address, which is the entrypoint.

### How to configure Ingress in your Cluster?

You need an implementation for Ingress!

Which is Ingress Controller?

![[20250328113014.png]]


1. Evaluates all the rules.
2. Manages redirections
3. Entrypoint to cluster.






















