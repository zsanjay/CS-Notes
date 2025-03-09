
## [Kubernetes Deployment](https://spacelift.io/blog/kubernetes-deployment-yaml#what-is-a-kubernetes-deployment)

Deployments are a fundamental building block for managing containerized applications in a Kubernetes cluster. A Deployment is an API resource and a higher-level abstraction that provides a declarative way to manage and scale a set of identical pods. You describe a _desired state_ in a Deployment, and the Deployment [Controller](https://kubernetes.io/docs/concepts/architecture/controller/) changes the actual state to the desired state at a controlled rate.

Common scenarios where deployments are used include the following:

- [Create a Deployment to roll out a ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#creating-a-deployment) – A Deployment manages a Replica Set, which, in turn, ensures that a specified number of pod replicas (instances) are running and healthy at all times. It allows you to scale your application by adjusting the desired number of replicas.

- [Declare the new state of the Pods](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment) – Deployments are configured using declarative YAML or JSON files, which specify the desired state of the application. Kubernetes continually works to ensure that the actual state of the application matches this desired state.

- [Scale up the Deployment to facilitate more load](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#scaling-a-deployment) – You can easily scale the number of replicas up or down based on the desired workload. Deployments can automatically adjust the number of replicas to match the specified number.

- [Pause the rollout of a Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#pausing-and-resuming-a-deployment) – Deployments support rolling updates, allowing you to make changes to your application without causing downtime. During an update, new pods are gradually created, and old pods are scaled down, ensuring a smooth transition from one version to another.

- [Rollback to an earlier Deployment revision](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-back-a-deployment) – If a rolling update fails or results in issues, Kubernetes deployments provide automated rollback mechanisms to revert to the previous stable version of the application.

- [Use the status of the Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#deployment-status) – If a pod fails or becomes unresponsive, the Deployment will automatically replace it with a new pod, maintaining the desired number of replicas.

- [Clean up older ReplicaSets](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#clean-up-policy)


A **Kubernetes Deployment YAML** file is a configuration file written in [YAML (YAML Ain’t Markup Language)](https://spacelift.io/blog/yaml) that defines the desired state of a Kubernetes Deployment. This YAML file is used to create, update, or delete Deployments in a Kubernetes cluster. It contains a set of key-value pairs that specify various attributes and settings for the Deployment, such as the number of replicas, pod template specifications, labels, and more.

A typical Kubernetes Deployment YAML file includes the following key components:

- **apiVersion**: Specifies the Kubernetes API version, such as “apps/v1” for Deployments.
- **kind**: Specifies the type of Kubernetes resource, in this case, “Deployment.”
- **metadata**: Provides metadata for the Deployment, including the name, labels, and annotations.
- **spec**: Defines the desired state of the Deployment, including the number of replicas, the pod template, and any other related specifications. It includes:
- **replicas**: Specifies the desired number of identical pod replicas to run.
- **selector**: Specifies the labels that the Replica Set uses to select the pods it should manage.
- **template**: Contains the pod template used for creating new pods, including container specifications, image names, and container ports.

### Overview

1. The 3 parts of configuration file.
2. Connecting Deployment to Service to Pods
3. Demo

### 3 PARTS of a K8s configuration file

![[20250122093727.png]]

![[20250122093800.png]]

![[20250122093908.png]]

### Status

![[20250122094156.png]]

![[20250122094655.png]]

#### Question :  Where does K8s get this status data?

Ans : Etcd holds the current status of any K8s component.

### Format of configuration file.

![[20250122095308.png]]

### Blueprint for pods (Template)

![[20250122101728.png]]

### Connecting components (Labels & Selectors & Ports)

![[20250122102654.png]]

![[20250122102805.png]]

### Ports

![[20250122102938.png]]

### Ports in Service and Pod

![[20250122103118.png]]


![[20250122103150.png]]

### nginx-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.16
        ports:
        - containerPort: 8080
```

### nginx-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

### Create the deployment

```shell
kubectl apply -f nginx-deployment.yaml
deployment.apps/nginx-deployment configured
```

### Create the service

```shell
kubectl apply -f nginx-service.yaml
service/nginx-service created
```

### Get the pod

```shell
kubectl get pod

NAME                                READY   STATUS    RESTARTS   AGE

nginx-deployment-687d5fb844-447d8   1/1     Running   0          2m14s
nginx-deployment-687d5fb844-s6qxw   1/1     Running   0          2m15s
```

As you can see, there are two pods because in the configuration file we mentioned replication = 2.

### Get the service

```shell
kubectl get service

NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE

kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP   21h
nginx-service   ClusterIP   10.109.68.198   <none>        80/TCP    60s
```

kubernetes is the default service, created by kubernetes.

### To Validate service has the right pods to which it forwards request.

```shell
kubectl describe service nginx-service

Name:              nginx-service
Namespace:         default
Labels:            <none>
Annotations:       <none>
Selector:          app=nginx
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.109.68.198
IPs:               10.109.68.198
Port:              <unset>  80/TCP
TargetPort:        8080/TCP
Endpoints:         10.244.0.8:8080,10.244.0.9:8080
Session Affinity:  None
Events:            <none>
```

### See more information about pods

```shell
kubectl get pod -o wide

NAME                                READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES

nginx-deployment-687d5fb844-447d8   1/1     Running   0          12m   10.244.0.9   minikube   <none>           <none>

nginx-deployment-687d5fb844-s6qxw   1/1     Running   0          12m   10.244.0.8   minikube   <none>           <none>
```

### Get the status of the deployment stored in etcd.

```shell
kubectl get deployment nginx-deployment -o yaml > nginx-deployment-result.yaml
```

### nginx-deployment-result.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	annotations:
		deployment.kubernetes.io/revision: "2"
		kubectl.kubernetes.io/last-applied-configuration: |
			{"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"labels":{"app":"nginx"},"name":"nginx-deployment","namespace":"default"},"spec":{"replicas":2,"selector":{"matchLabels":{"app":"nginx"}},"template":{"metadata":{"labels":{"app":"nginx"}},"spec":{"containers":[{"image":"nginx:1.16","name":"nginx","ports":[{"containerPort":8080}]}]}}}}
	creationTimestamp: "2025-01-21T10:57:16Z"
	generation: 3
	labels:
		app: nginx
	name: nginx-deployment
	namespace: default
	resourceVersion: "15105"
	uid: a9924ad3-609d-40cc-93e6-f3449ee8a33f
spec:
	progressDeadlineSeconds: 600
	replicas: 2
	revisionHistoryLimit: 10
	selector:
		matchLabels:
			app: nginx
		strategy:
			rollingUpdate:
				maxSurge: 25%
				maxUnavailable: 25%
			type: RollingUpdate
	template:
		metadata:
			creationTimestamp: null
			labels:
				app: nginx
		spec:
			containers:
			- image: nginx:1.16
			imagePullPolicy: IfNotPresent
			name: nginx
			ports:
			- containerPort: 8080
				protocol: TCP
			resources: {}
			terminationMessagePath: /dev/termination-log
			terminationMessagePolicy: File
		dnsPolicy: ClusterFirst
		restartPolicy: Always
		schedulerName: default-scheduler
		securityContext: {}
		terminationGracePeriodSeconds: 30
status:
	availableReplicas: 2
	conditions:
	- lastTransitionTime: "2025-01-21T11:05:13Z"
		lastUpdateTime: "2025-01-21T11:05:13Z"
		message: Deployment has minimum availability.
		reason: MinimumReplicasAvailable
		status: "True"
		type: Available
	- lastTransitionTime: "2025-01-21T10:57:16Z"
		lastUpdateTime: "2025-01-22T06:38:39Z"
		message: ReplicaSet "nginx-deployment-687d5fb844" has successfully progressed.
		reason: NewReplicaSetAvailable
		status: "True"
		type: Progressing
	observedGeneration: 3
	readyReplicas: 2
	replicas: 2
	updatedReplicas: 2
```

### To delete the deployment and service.

```shell
kubectl delete -f nginx-deployment.yaml 
deployment.apps "nginx-deployment" deleted

kubectl delete -f nginx-service.yaml   
service "nginx-service" deleted
```


### References

https://spacelift.io/blog/kubernetes-deployment-yaml





