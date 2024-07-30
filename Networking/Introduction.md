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
      command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]
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



# Lab 2

```
kind: Deployment
apiVersion: apps/v1
metadata:
   name: mydeployments
spec:
   replicas: 1
   selector:      # tells the controller which pods to watch/belong to
    matchLabels:
     name: deployment
   template:
     metadata:
       name: testpod1
       labels:
         name: deployment
     spec:
      containers:
        - name: c00
          image: httpd
          ports:
          - containerPort: 80
```

```
kind: Service                             # Defines to create Service type Object
apiVersion: v1
metadata:
  name: demoservice
spec:
  ports:
    - port: 80                               # Containers port exposed
      targetPort: 80                     # Pods port
  selector:
    name: deployment                    # Apply this service to any pods which has the specific label
  type: ClusterIP                       # Specifies the service type i.e ClusterIP or NodePort

$ kubectl get svc
```

> volume labs

```
apiVersion: v1
kind: Pod
metadata:
  name: myvolemptydir
spec:
  containers:
  - name: c1
    image: centos
    command: ["/bin/bash", "-c", "sleep 15000"]
    volumeMounts:                                    # Mount definition inside the container
      - name: xchange
        mountPath: "/tmp/xchange"          
  - name: c2
    image: centos
    command: ["/bin/bash", "-c", "sleep 10000"]
    volumeMounts:
      - name: xchange
        mountPath: "/tmp/data"
  volumes:                                                   
  - name: xchange
    emptyDir: {}
```

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
