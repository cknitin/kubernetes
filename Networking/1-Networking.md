# Kubernetes Networking, Services, Nodeport & Volumes

## Kubernetes Networking addresses four Concerns:

- 1. Containers within a pod use networking to communicate via loopback
- 2. Cluster Networking provides Communication between different Pods.
- 3. The Service resources lets you expose an application running in Pods to be reachable from Outside your Cluster.
- 4. You can also use services to publish Services only for Consumption inside your Cluster.
- 5. Container to Container communication on Same pod happens through localhost within the Containers.


## Lab 1

> Install Docker
```
$  sudo apt update && apt -y install docker.io
```

> Install kubectl
```
$  curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl &&   chmod +x ./kubectl && sudo mv ./kubectl /usr/local/bin/kubectl
```

> Install Minikube
```
$  curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

> Start Minikube
```
$  apt install conntrack
$  minikube start --vm-driver=none
$  minikube status
```

> KUBERNETES NETWORKING

Objective: Container to Container communication on Same pod happens through localhost within the Containers.

```
vi pod1.yml
```

```
kind: Pod
apiVersion: v1
metadata:
  name: testpod
spec:
  containers:
    - name: c00
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo Hello-Pod; sleep 5 ; done"]
    - name: c01
      image: httpd
      ports:
       - containerPort: 80
```

```
kubectl apply -f pod1.yml

kubectl get pods
```
To enter inside POD and container

```
kubectl exec testpod -it -c c00 -- bin/bash
```
install curl to communicate

```
apt update && apt install curl
```

```
curl localhost:80
```

To exit from pod and container
```
exit
```

TO delete POD

```
kubectl delete -f pod1.yml
```


## Lab 2

Now try to established Communication between two different Pods within same machine.

- Pod to Pod Communication on same worker node happens through Pod IP

- By default Pod's IP will not be accessible Outside the node.

Objective: POD to POD communication

One node can have 2 POSs and each POD has one container, POD to POD communication

```
vi pod2.yml
```

```
kind: Pod
apiVersion: v1
metadata:
  name: testpod1
spec:
  containers:
    - name: c01
      image: nginx
      ports:
       - containerPort: 80
```

```
kubectl apply -f pod1.yml
```

```
vi pod3.yml
```

```
kind: Pod
apiVersion: v1
metadata:
  name: testpod2
spec:
  containers:
    - name: c01
      image: nginx
      ports:
       - containerPort: 80
```

```
kubectl apply -f pod1.yml
```

```
kubectl get pods
```

```
kubectl get pods -o wide
```

```
curl 147.25.68.52:80
```

```
kubectl apply -f pod1.yml

kubectl get pods
```

## Lab 5
> HOST PATH

```
apiVersion: v1
kind: Pod
metadata:
  name: myvolhostpath
spec:
  containers:
  - image: centos
    name: testc
    command: ["/bin/bash", "-c", "sleep 15000"]
    volumeMounts:
    - mountPath: /tmp/hostpath
      name: testvolume
  volumes:
  - name: testvolume
    hostPath:
      path: /tmp/data 
```
