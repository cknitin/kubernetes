# Labels, Selectors, ReplicationController and ReplicaSet in Kubernetes

- Labels are the mechanism you use to organise Kubernetes objects.
- A label is a key-value pair without any predefined meaning that can be attached to the object.
- Labels are similar to tags in AWS or git where you use a name to quick reference.
- So you are free to choose labels as you need it to refer an environment which is used for dev or testing or production refer a product group like
   Department A, Department B.
- Multiple labels can be added to a single object.

## EXAMPLE OF LABELS

```
vi Pod5.yml
```

```
kind: Pod
apiVersion: v1
metadata:
  name: meerutpod
  labels:                                                   
    env: development
    class: pods
spec:
    containers:
       - name: c00
         image: ubuntu
         command: ["/bin/bash", "-c", "while true; do echo Hello-Word; sleep 5 ; done"]
```

### To exit -

```
:wq
``` 

```
kubectl apply -f Pod5.yml
```

```
kubectl get pods --show-labels
```

### Now if you want to add a label to an existing pod ->

```
kubectl label pods myPod myName=test
```
```
kubectl get pods --show-labels
```

### Now list pods matching a label ->
```
kubectl get pods -l env=development
```

### Now give a list, where "development" label is not present ->
```
kubectl get pods -l env!=development
```

### Now if you want to delete pod using label

```
kubectl delete pod -l env=development
```
```
kubectl get pods
```

# Node Selector

- One use case of selecting lables is to constrain the set of nodes onto which a POD can be schedule.
  i.e. You can tell a POD to only be able to run on particular node.
- Generally such constraints are unnessary as the scheduler will automatically do a responsilbe placement, but on certain circumtances we might need it.
- We can use labels to tag nodes
- You the node are tagged, you can use the label selectors to specifiy the PODs run only of specific nodes
- First we give label to the node
- Then we use node selector to the POD configuration

## NODE SELECTOR EXAMPLE

```
kind: Pod
apiVersion: v1
metadata:
  name: nodelabels
  labels:
    env: development
spec:
    containers:
       - name: c00
         image: ubuntu
         command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]
    nodeSelector:                                         
       hardware: t2-medium 
```


