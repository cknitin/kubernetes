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






