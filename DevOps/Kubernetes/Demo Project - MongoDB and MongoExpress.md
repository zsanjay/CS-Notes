

![[20250122113437.png]]

### Request Flow

![[20250122113744.png]]

### To check all the components in the K8s cluster.

```shell
kubectl get all

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   22h
```

### Create a mongodb deployment.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: mongodb-deployment
	labels:
		app: mongodb
spec:
	replicas: 1
	selector:
		matchLabels:
			app: mongodb
	template:
		metadata:
			labels:
				app: mongodb
		spec:
			containers:
			- name: mongodb
			  image: mongo
			  ports:
			  - containerPort: 27017
			  env:
			  - name: MONGO_INITDB_ROOT_USERNAME
				valueFrom:
					secretKeyRef:
						name: mongodb-secret
						key: mongo-root-username
				- name: MONGO_INITDB_ROOT_PASSWORD
					valueFrom:
						secretKeyRef:
							name: mongodb-secret
							key: mongo-root-password
---
apiVersion: v1
kind: Service
metadata:
	name: mongodb-service
spec:
	selector:
		app: mongodb
	ports:
		- protocol: TCP
		  port: 27017
          targetPort: 27017
```

### Secret Configuration File

![[20250122115506.png]]


Storing the data in a Secret component doesn't automatically make it secure. There are built-in mechanism (like encryption) for basic security, which are not enabled by default.

To encrypt the data, we can use this command.

```shell
Username : 'sanjay'

echo -n 'sanjay' | base64
c2FuamF5


Password : 'Dubai@6140'

echo -n 'Dubai@6140' | base64
RHViYWlANjE0MA==

```

Secret must be created before the deployment if you want to refer it in the deployment configuration.

```yaml
apiVersion: v1
kind: Secret
metadata:
	name: mongodb-secret
type: Opaque
data:
	mongo-root-username: c2FuamF5
	mongo-root-password: RHViYWlANjE0MA==
```

#### Create a secret.

```shell
kubectl apply -f mongodb-secret.yaml                         
secret/mongodb-secret created
```

#### To get the secret

```shell
kubectl get secret

NAME             TYPE     DATA   AGE
mongodb-secret   Opaque   2      62s
```

### Create the deployment

```shell
kubectl apply -f mongodb-deployment.yaml
deployment.apps/mongodb-deployment created
```

#### To check all the components

```shell
kubectl get all       

NAME                                      READY   STATUS    RESTARTS   AGE
pod/mongodb-deployment-6d9d7c68f6-zrk95   1/1     Running   0          96s


NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   22h


NAME                                 READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mongodb-deployment   1/1     1            1           96s


NAME                                            DESIRED   CURRENT   READY   AGE
replicaset.apps/mongodb-deployment-6d9d7c68f6   1         1         1       96s
```

We can put multiple documents in one yaml file. Deployment & Service in 1 file, because they belong together and it is separated by 3 dash

```
Deployment
---
Service
```

### Service Configuration File

![[20250122123219.png]]

Reapply the deployment

```shell
kubectl apply -f mongodb-deployment.yaml

deployment.apps/mongodb-deployment unchanged
service/mongodb-service created
```

### Get the service

```shell
kubectl get service

NAME              TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)     AGE
kubernetes        ClusterIP   10.96.0.1      <none>        443/TCP     23h
mongodb-service   ClusterIP   10.96.61.132   <none>        27017/TCP   6m28s
```

### Describe the service.

```shell
kubectl describe service mongodb-service

Name:              mongodb-service
Namespace:         default
Labels:            <none>
Annotations:       <none>
Selector:          app=mongodb
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.96.61.132
IPs:               10.96.61.132
Port:              <unset>  27017/TCP
TargetPort:        27017/TCP
Endpoints:         10.244.0.10:27017
Session Affinity:  None
Events:            <none>
```

### Validate if service connected to the right pod.

```shell
kubectl get pod -o wide

NAME                                  READY   STATUS    RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES
mongodb-deployment-6d9d7c68f6-zrk95   1/1     Running   0          21m   10.244.0.10   minikube   <none>           <none>
```

### Filter out all the components using selector name:

```shell
kubectl get all | grep mongodb

pod/mongodb-deployment-6d9d7c68f6-zrk95   1/1     Running   0          23m
service/mongodb-service   ClusterIP   10.96.61.132   <none>        27017/TCP   12m
deployment.apps/mongodb-deployment   1/1     1            1           23m
replicaset.apps/mongodb-deployment-6d9d7c68f6   1         1         1       23m
```

### Mongo express deployment

![[20250122135920.png]]

### Config Map

![[20250122140459.png]]

![[20250122140636.png]]

#### mongodb-configmap.yaml

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
	name: mongodb-configmap
data:
	database_url: mongodb-service
```
### Create a ConfigMap

```shell
kubectl apply -f mongodb-configmap.yaml
configmap/mongodb-configmap created
```

### Get the ConfigMap

```shell
kubectl get configmap

NAME                DATA   AGE
kube-root-ca.crt    1      24h - default created by kubernetes
mongodb-configmap   1      56s
```

#### mongo-express-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: mongo-express
	labels:
		app: mongo-express
spec:
	replicas: 1
	selector:
		matchLabels:
			app: mongo-express
	template:
		metadata:
			labels:
				app: mongo-express
		spec:
			containers:
			- name: mongo-express
			  image: mongo-express
			  ports:
			  - containerPort: 8081
		      env:
			  - name: ME_CONFIG_MONGODB_ADMINUSERNAME
				valueFrom:
					secretKeyRef:
						name: mongodb-secret
						key: mongo-root-username
			  - name: ME_CONFIG_MONGODB_ADMINPASSWORD
				valueFrom:
					secretKeyRef:
						name: mongodb-secret
						key: mongo-root-password
			  - name: ME_CONFIG_MONGODB_SERVER
				valueFrom:
					configMapKeyRef:
						name: mongodb-configmap
						key: database_url
```

#### Create mongo express deployment

```shell
kubectl apply -f mongo-express-deployment.yaml 
deployment.apps/mongo-express created
```

### How to create an external service?

![[20250122153003.png]]

![[20250122153100.png]]
