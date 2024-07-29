# Deployment and Rollback

  - Replication Controller & Replica Set is not able to do Updates & Rollback apps in the Cluster
  - A deployment Object act as a Supervisor for pods, giving you fine-grained Control Over how and when a new pod is Rolled Out, Updated or Rolled Back to a Previous state.
  - When using deployment Object, we first define the state of the app, then K8s Cluster Schedules mentioned app instance onts specific individual nodes.
  - In K8s, if the node hosting an instance goes down or the pod is deleted, the deployment controller replaces it.
  - This provides a self-healing mechanism to address machine failure or maintenance.


Replication Controller & Replica Set is not able to do Updates & Rollback apps in the Cluster 
  - A deployment Object act as a Supervisor for pods, giving you fine-grained Control Over how and when a new pod is Rolled Out, Updated or Rolled Back to a Previous     state.
  -  When using deployment Object, we first define the state of the app, then K8s Cluster Schedules mentioned app instance onts specifie individual nodes.
  -  A deployment provides declarative updates for Pods & Replicaset
  -  K8s then monitors, if the node hosting an instance goes down or pod is deleted the deployment Controller replaces it. 
  -  This provides a self-healing mechanism to address machine failure or maintenance.

![alt text](https://github.com/cknitin/kubernetes/blob/main/images/Deployment%20and%20rollback%20-5.png))

### The following are typical use Cases of Deployments. Deployment & Rollback 
#### 1 Create a deployment to rollout a Replicaset →
  - The replicaset Creates pods in the Background. Check the status of the Rollout to see if it succeeds or not
#### 2 Declare the new state of the Pods 
  - By updating the PodTemplateSpec of the deployment. A new Replicaset is Created and the Deployment manages moving the pods from the Old Replicaset to the new one at a Controlled rate. Each new Repicaret updates the revision of the Deployment. 
#### 3 Rollback to an earlier Deployment Revision 
  - If the Current state of the Deployment is not stable Each rollback Updates the revision of the Deployment. 
#### 4 Scale up the Deployment to facilitates more load. 
#### 5 Pause the Deployment to apply multiple fixes to its PodTemplate Spec and then resume it to start a new Rollout 
#### 6 Cleanup older Replicasets that dont need anymore

### If there are problems in the deployment, Kubernetes will automatically roll back to the previous Version, however you can also explicitly rollback to a     specific revision, as in Our Case to Revision 1 (the Original Pod Version)

### You Can rollback to a specific Version by Specifying it with --to-revision
  - for eg → Kubectl rollout undo Type deploy/mydeployments --to-revision=2

### Note That the name of the Replicaset is always formatted as [Deployment-name]- [Random string]
```
  cmd → Kubectl get cleploy
```

### When you inspect the deployments in your Cluster, the following fields are displayed:

#### NAME 
  - List the names of the deployments in the namespace
#### READY 
  - Display how many replicas of the application are available to your users. If follows the pattern of ready/Desired 
#### UP-TO-DATE 
  - Display the number of replicas that have been updated to achieve the desired state. 
#### AVAILABLE 
  - Displays how many replicas of the application are available to your users. 
#### AGE 
  - Display the amount of time that the application has been Running.


> LAB 

```
sudo su
```

command to install docker is

```
sudo apt update && apt -y install docker.io
```

install Kubectl now with the given link

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x ./kubectl && sudo mv ./kubectl /usr/local/bin/kubectl
```

```
install Minikube with the given link
```

```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

```
apt install conntrack
```

```
minikube start --vm-driver=none
minikube status
kubectl version
kubectl get nodes
```

```
kind: Deployment
apiVersion: apps/v1
metadata:
   name: mydeployments
spec:
   replicas: 2
   selector:     
    matchLabels:
     name: deployment
   template:
     metadata:
       name: testpod
       labels:
         name: deployment
     spec:
      containers:
        - name: c00
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo Hello-World; sleep 5; done"]

```

To check deployment was Greated or not 
```
 Kubectl get deploy
```
 
To Check, how deployment creates RS & pods
```
 Kubectl describe deploy mydeploymen
``` 
```
Kubectl get rs 
```
To scale up or scale down 
```
kubectl scale --replicas=1 deploy mydeployments
```







