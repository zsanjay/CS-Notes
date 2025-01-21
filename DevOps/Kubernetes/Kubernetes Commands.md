
### Commands to install Minikube

1. curl -LO [https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-arm64](https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-arm64) 
2. sudo install minikube-darwin-arm64 /usr/local/bin/minikube
3. minikube start --driver=docker --alsologtostderr

### kubectl commands

1. To list the nodes

```shell
kubectl get nodes

NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   78s   v1.32.0
```

2. Minikube status

```shell
minikube status

minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

3. Kubernetes Version

```shell
kubectl version

WARNING: This version information is deprecated and will be replaced with the output from kubectl version --short.  Use --output=yaml|json to get the full version.

Client Version: version.Info{Major:"1", Minor:"27", GitVersion:"v1.27.2", GitCommit:"7f6f68fdabc4df88cfea2dcf9a19b2b830f1e647", GitTreeState:"clean", BuildDate:"2023-05-17T14:20:07Z", GoVersion:"go1.20.4", Compiler:"gc", Platform:"darwin/arm64"}

Kustomize Version: v5.0.1

Server Version: version.Info{Major:"1", Minor:"32", GitVersion:"v1.32.0", GitCommit:"70d3cc986aa8221cd1dfb1121852688902d3bf53", GitTreeState:"clean", BuildDate:"2024-12-11T17:59:15Z", GoVersion:"go1.23.3", Compiler:"gc", Platform:"linux/arm64"}

WARNING: version difference between client (1.27) and server (1.32) exceeds the supported minor version skew of +/-1
```

4. To get the services

```shell
kubectl get services

NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   19m
```

5. To get help from create command.

```shell
kubectl create -h
```

There is no command to create pods instead we create deployment which is the abstraction over Pods.

![kub20250121140214](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250121140214.png)

6. Create Deployment of nginx

```shell
kubectl create deployment nginx-depl --image=nginx
```

7. To check deployments

```shell
kubectl get deployment

NAME         READY   UP-TO-DATE   AVAILABLE   AGE
nginx-depl   1/1     1            1           87s
```

8. To check the pods.

```shell
kubectl get pod     

NAME                          READY   STATUS    RESTARTS   AGE
nginx-depl-68c944fcbc-xzjs9   1/1     Running   0          17s
```

9. To get the replicaset

Replicaset is used to manage replicas of a pod.

```shell
kubectl get replicaset

NAME                    DESIRED   CURRENT   READY   AGE
nginx-depl-68c944fcbc   1         1         1       4m4s
```

![kub20250121141005](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250121141005.png)

#### Pod name consists of three things:

1. Deployment name
2. replicaset name
3. pod name

Format - deployment-replicaset-pod
Example. : nginx-depl-68c944fcbc-xzjs9

![kub20250121141544](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250121141544.png)

Everything below the deployment should be managed by kubernetes.

10. Command for edit the deployment.

```shell
kubectl edit deployment nginx-depl
```

After editing the new replicaset and pod will be created and old pod will be deleted.

```shell
kubectl get deployment

NAME         READY   UP-TO-DATE   AVAILABLE   AGE
nginx-depl   1/1     1            1           16m

kubectl get replicaset

NAME                    DESIRED   CURRENT   READY   AGE
nginx-depl-54bd6589c    1         1         1       34s - New Replicaset
nginx-depl-68c944fcbc   0         0         0       16m - Old

kubectl get pod       

NAME                         READY   STATUS    RESTARTS   AGE
nginx-depl-54bd6589c-c4l6g   1/1     Running   0          44s - New Pod
```

11. Command to get additional information of a pod.

```shell
kubectl describe pod mongo-depl-85ffbc9879-p8tp4

Name:             mongo-depl-85ffbc9879-p8tp4
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Tue, 21 Jan 2025 14:27:42 +0400
Labels:           app=mongo-depl
                  pod-template-hash=85ffbc9879
Annotations:      <none>
Status:           Running
IP:               10.244.0.5
IPs:
  IP:           10.244.0.5

Controlled By:  ReplicaSet/mongo-depl-85ffbc9879

Containers:
  mongo:

    Container ID:   docker://9bedb5de2d9c58cde82e4c0d47d20e8af37a9ffc6c15da0a894d36c204fbee5d
    Image:          mongo
    Image ID:       docker-pullable://mongo@sha256:33d17eb9a1c27c6635d08f890cd075d0f9392c598eaad8c4461344ac5fbc7140

    Port:           <none>
    Host Port:      <none>
    State:          Running
    Started:      Tue, 21 Jan 2025 14:28:15 +0400
    Ready:          True
    Restart Count:  0
    Environment:    <none>

    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-rgtkw (ro)

Conditions:

  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       True 
  ContainersReady             True 
  PodScheduled                True 

Volumes:

  kube-api-access-rgtkw:
  
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s

Events:

  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  75s   default-scheduler  Successfully assigned default/mongo-depl-85ffbc9879-p8tp4 to minikube
  Normal  Pulling    75s   kubelet            Pulling image "mongo"
  Normal  Pulled     42s   kubelet            Successfully pulled image "mongo" in 33.044s (33.044s including waiting). Image size: 845653788 bytes.
  Normal  Created    42s   kubelet            Created container: mongo
  Normal  Started    42s   kubelet            Started container mongo
```

12. To get the logs of the pod.

```shell
kubectl logs pod-name

kubectl logs mongo-depl-85ffbc9879-p8tp4
```

13. Command to debug the pod.

```shell
kubectl exec -it mongo-depl-85ffbc9879-p8tp4 -- bin/bash
```

14. Command to delete deployment and all the resources underneath like replicaset and pod belongs to the deployment.

```shell
kubectl delete deployment mongo-depl

deployment.apps "mongo-depl" deleted

kubectl get pod                                         

NAME                         READY   STATUS    RESTARTS   AGE
nginx-depl-54bd6589c-c4l6g   1/1     Running   0          23m

kubectl get replicaset                                  

NAME                    DESIRED   CURRENT   READY   AGE
nginx-depl-54bd6589c    1         1         1       24m
nginx-depl-68c944fcbc   0         0         0       39m
```

If you want to configure more option while creating deployment we need to provide a json/yml file.

15. Command to create a deployment using file.

```shell
kubectl apply -f [file name]
```

### nginx-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: nginx-deployment
	labels:
		app: nginx
spec:
	replicas: 1
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
			  - containerPort: 80
```

To create deployment 

```shell
kubectl apply -f nginx-deployment.yaml
deployment.apps/nginx-deployment created
```

Editing the deployment and changing replicas from 1 to 2.

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
			  - containerPort: 80
```

Now the deployment is edited or configured. When we create the deployment first time the message will be created and when we edit the same deployment then the message will be configured.

```shell
kubectl apply -f nginx-deployment.yaml
deployment.apps/nginx-deployment configured

kubectl get replicaset                

NAME                          DESIRED   CURRENT   READY   AGE
nginx-deployment-7f65fcf556   2         2         2       10m
```

### Summary

![kub20250121150938](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250121150938.png)


![kub20250121151026](https://github.com/zsanjay/Obsidian-Notes/blob/main/assets%2Fimages%2F20250121151026.png)

### References

https://medium.com/@seohee.sophie.kwon/how-to-run-a-minikube-on-apple-silicon-m1-8373c248d669

https://gitlab.com/nanuchi/youtube-tutorial-series/-/blob/master/basic-kubectl-commands/cli-commands.md
