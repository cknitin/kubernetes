# Namespaces and Resource Quota (Namespace, limits & request )

- You Can name your object, but if many are using the cluster then it would be difficult for Managing 
- A namespace is a group of related Elements that each have a unique name or identifier. Namespace is used to uniquely identify one or more names
  from other similar names of different Objects, groups or the namespace in general
-  Kubernetes namespaces help different projects, teams or Customers to share a Kubernetes Cluster & provides ÷

A scope for every names.
A mechanism to attach authorization and policy to a subsection of the cluster.


- By default, a Kubernetes Cluster will instantiate a default namespace when provisioning the Cluster to hold the default set of pods, 
  Services and Deployments used by the Cluster. 
- We Can use resource Quota on specifying how many resources each namespace can use.
- Most Kubernetes resources (eg pods, services, replication Controllers and Others) are in Some namespaces-and low-level resources
  such as nodes And perwsistent Volumes, are not in any namespace. 
→ Namespaces are intended for use in environments with many users spread across multiple teams, or projects! for cluster with a few to tens of users,
  you should not need to Create or think about namespaces and all.

## Create namespace
- Let us assume we have shared K8s Cluster for Dev and production use Cases. 
- The dev team would like to maintain a space in the cluster where they can get a view on the list of Pods, Services 
  and Deployments they use to build and run their application. In this, no restrictions are put on who can or Cannot modify resources to enable agile development. 
- for prod team we can enforce Strict procedure on who Can or Cannot manipulate the Set of Pods, Services and deploymente 

Cmd 
```
Kubech get namespaces
```


# Lab

> Set up k8s

```
 Install Docker
$  sudo apt update && apt -y install docker.io

 Install kubectl
$  curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl &&   chmod +x ./kubectl && sudo mv ./kubectl /usr/local/bin/kubectl

 Install Minikube
$  curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

 Start Minikube
$  apt install conntrack
$  minikube start --vm-driver=none
$  minikube status

```

>  To create namespace mynamespace.yml

```
apiVersion: v1
kind: Namespace
metadata:
   name: dev
   labels:
     name: dev
```

```
kubectl apply mynamespace.yml
```

> Create a POD

```
vi pod.yml


kind: Pod                              
apiVersion: v1                     
metadata:                           
  name: testpod                  
spec:                                    
  containers:                      
    - name: c00                     
      image: ubuntu              
      command: ["/bin/bash", "-c", "while true; do echo Technical Guftgu; sleep 5 ; done"]
  restartPolicy: Never       

```

> Create POD in dev namespace

```
kudectl apply -f pod.yml -n dev

kubectl get pods

kubectl get pods -n dev

```

> To delete

```
kubectl delete -f pod.yml-n dev
```

> To set default namespace

```
$ kubectl config set-context $(kubectl config current-context) --namespace=dev
```

> To konw where K8s finding the pod
```
$ kubectl config view | grep namespace:
```

# Resource and Limit

## Managing Compute Resources for Containers 

- A pod in Kubernetes will run with no limits on CPU and memory 
- You can optionally specify how much CPU and memory (RAM) each Container needs.
- Scheduler decides about which nodes to place pods, only if the Node has enough CPU resources available to satisfy the Pod CPU request
- CPU is specified in units of Cores and memory is specified in units of Bytes

Two types of Constraints Can be set for each resource type - Request and Limits 
- A request is the amount of that resources that the system will guarantee for the Container and 
  Kubernetes will use this Value to decide on which node to place the pod 
- A limit is the max amount of resources that Kubernetes will allow the Container to use In the Case that request is not set for a Container, 
  it default to limits If limit is not set, then if default to 0 
- CPU values are specified in 'millicpu' and memory in MiB.


# Resource Quota 
- A Kubernetes Cluster Can be divided into namespaces. If a Container is Created in a namespace that has a default CPU limit, and the Container does not specify its own CPU limit, then the Container is assigned the default CPU limit. 
- Namespaces Can be assigned Resource Quota Objects, this will limit the amount of usage allowed to the Objects in that namespace. You Can limit -
   - 1. Compute
   - 2. Memory
   - 3. Storage

Here are two restrictions that a resource Quota imposes on a namespace:

Every Container that runs in the namespace must have its own CPU limit.
The total amount of CPU used by all Containers in the namespace must not exceed a specified limit.


# Lab  - of resorces

## Set up k8s

>  Install Docker
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


> To create resources

resource.yml

```
apiVersion: v1
kind: Pod
metadata:
  name: resources
spec:
  containers:
  - name: resource
    image: centos
    command: ["/bin/bash", "-c", "while true; do echo Technical-Guftgu; sleep 5 ; done"]
    resources:                                          
      requests:
        memory: "64Mi"
        cpu: "100m"
      limits:
        memory: "128Mi"
        cpu: "200m"
```

```
kubectl apply -f resorce.yml
```

```
kubectl get pods
```

```
kubectl describe pod resources <-- Pode name
```

# Lab - Resource Quota 

```
vi resourcequota.yml
```

```
apiVersion: v1
kind: ResourceQuota
metadata:
   name: myquota
spec:
  hard:
    limits.cpu: "400m"
    limits.memory: "400Mi"
    requests.cpu: "200m"
    requests.memory: "200Mi"
```

```
kubectl apply -f resourcequota.yml
```

```
vi testpod.yml
```

```
kind: Deployment
apiVersion: apps/v1
metadata:
  name: deployments
spec:
  replicas: 3
  selector:      
    matchLabels:
     objtype: deployment
  template:
    metadata:
      name: testpod8
      labels:
        objtype: deployment
    spec:
     containers:
       - name: c00
         image: ubuntu
         command: ["/bin/bash", "-c", "while true; do echo Technical-Guftgu; sleep 5 ; done"]
         resources:
            requests:
              cpu: "200m"
```

```
kubectl apply -f testpod.yml
```

```
kubectl get deploy
```

Note - Pod will not create because limit of namepsace is 200m CPU and in in above yml file we request for creating 3 pod with 200m CPU which will be 3 X 200 = 600
this is exceed limit, reason being POD will not create

```
kubectl get rs   <--- this cmd will return a name use that below
```

```
kubectl describe rs --name--
```

Now set limit range

# Lab -

limitRange.yml

```
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 1
    defaultRequest:
      cpu: 0.5
    type: Container
```


now create pod

pod.yml

```
kind: Pod                              
apiVersion: v1                     
metadata:                           
  name: testpod                  
spec:                                    
  containers:                      
    - name: c00                     
      image: ubuntu              
      command: ["/bin/bash", "-c", "while true; do echo Technical Guftgu; sleep 5 ; done"]
  restartPolicy: Never       
```

Now Pod will be crated

# Lab - Now set the Limit but not define the request

cpu2.yml

```
apiVersion: v1
kind: Pod
metadata:
  name: default-cpu-demo-2
spec:
  containers:
  - name: default-cpu-demo-2-ctr
    image: nginx
    resources:
      limits:
        cpu: "1"

```

```
kubectl apply -f cpu2.yml
```

```
kubectl get pods
```

```
kubectl describe pod --podname--

Note: if request is not mention and limit is mention then Request = Limit
```

# Lab - now request is define but limit is not define

```
vi reqDefineButLimitNot.yml
```

```
apiVersion: v1
kind: Pod
metadata:
  name: default-cpu-demo-3
spec:
  containers:
  - name: default-cpu-demo-3-ctr
    image: nginx
    resources:
      requests:
        cpu: "0.75"
```

```
kubectl apply -f reqDefineButLimitNot.yml
```

```
kucectl get pods
```

```
kubectl describe pod --podname--

Note - if req is define but limit is not define the, pod will get default limit
```

# Lab

prod-namespace 

limits.CPU: "600m' 
limits memory: "800" 

requests.cpu: *200m 
requests memory *500M 

> Default Range 
  cpu 
    - (min) request - 05 
    - (max) limit - 1 
  memory 
    - (min) request - 500M 
    - (max) limit - 1G Quota


```
vi cpu2.yml
```

```
apiVersion: v1
kind: Pod
metadata:
  name: default-cpu-demo-2
spec:
  containers:
  - name: default-cpu-demo-2-ctr
    image: nginx
    resources:
      limits:
        cpu: "1"
```

```
kubecltapply -f cpu2.yml

kubectl get pods

kubectl describe pode --podname--
```

in above ex- we set limit cpu = 1, and in describe cmd result you will see request 1 but we did not set it, system will set request = limit

```
kubectl delete -f cpu2.yml
```


# Lab

```
vi cpu1.yml
```

```
apiVersion: v1
kind: Pod
metadata:
  name: default-cpu-demo-3
spec:
  containers:
  - name: default-cpu-demo-3-ctr
    image: nginx
    resources:
      requests:
        cpu: "0.75"
```

```
kubecltapply -f cpu1.yml

kubectl get pods

kubectl describe pode --podname--
```

Here limit will be set default for cpu i.e. 1

```
kubectl delete -f cpu2.yml
```


# Lab - for memory

```
vi memory-default.yml
```

```
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-min-max-demo-lr
spec:
  limits:
  - max:
      memory: 1Gi
    min:
      memory: 500Mi
    type: Container
```

```
kubeclt apply -f memory-default.yml
```

Now create pod

```
vi mem.yml
```

```
apiVersion: v1
kind: Pod
metadata:
  name: constraints-mem-demo
spec:
  containers:
  - name: constraints-mem-demo-ctr
    image: nginx
    resources:
      limits:
        memory: "800Mi"
      requests:
        memory: "600Mi"
```

```
kubeclt apply -f mem.yml
```

```
kubectl get pods

kubectl describe pode --podname--
```

using descirbe cmd you will req and limit as per out yml file

```
kubectl delete -f mem.yml
```

# Lab

```
vi mem2.yml
```

```
apiVersion: v1
kind: Pod
metadata:
  name: constraints-mem-demo
spec:
  containers:
  - name: constraints-mem-demo-ctr
    image: nginx
    resources:
      limits:
        memory: "1800Mi"
      requests:
        memory: "600Mi"
```

```
kubeclt apply -f mem2.yml
```

Now you will see error becuase limit goes beyond the posible limit i.e. 1 GB per pod

```
kubectl get pods

kubectl describe pode --podname--
```

using descirbe cmd you will req and limit as per out yml file

```
kubectl delete -f mem.yml
```


Note  - If request is not specified & limit is given, then request = limit
