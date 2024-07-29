# Scaling and Replication

- Kubernetes was designed to orchestrate multiple containers and replication.
- Need for multiple containers/replication helps us with these:
- Reliability: By having multiple versions of an application, you prevent problems if one or more fails.
- Load Balancing: Having multiple versions of a container enables you to easily send traffic to different instances to prevent overloading of a single instance or node.
- Scaling - When load does become too much for the number of existing instances, Kubernetes enables you to easily scale up your application, adding additional instances as needed. 
- Rolling updates: Updates to a Service by replacing pods one by one.

## Replication Controller 
- A replication Controller is a Object that enables you to easily Create multiple pods, then make sure that number of pods always exist.
- If a pod created using RC will be automatically replaced if they does Crash, failed, or terminated
- RC is recommended if you just want to make sure 1 pod is always running, even after system reboots.
- You Can run the RC with 1 replica & the RC will make sure the pod is always running.

```
kind: ReplicationController               
apiVersion: v1
metadata:
  name: myreplica
spec:
  replicas: 2            
  selector:        
    myname: CKNitin                             
  template:                
    metadata:
      name: testpod6
      labels:            
        myname: CKNitin
    spec:
     containers:
       - name: c00
         image: ubuntu
         command: ["/bin/bash", "-c", "while true; do echo Hello-World; sleep 5 ; done"]
```

```
kubectl get rc
```

```
kubectl describe rc myreplica
```

Ctl + l

```
kubectl get pods
```

```
kubectl delete pod pod-name
```

```
kudectl get pods
```
#### To scale up

```
kudectl scale --replicas=8 rc -l myname=CKNitin
```

#### To scale down

```
kudectl scale --replicas=2 rc -l myname=CKNitin
```


# Replica Set 
- Replica Set is a next generation Replication Controller.
- The replication Controller only supports Equality-based Selector whereas the replica set Supports set-based Selector ie filtering according to set of Values
- Replicaset rather than the Replication Controller is used by other objects like deployment.

```
kind: ReplicaSet                                    
apiVersion: apps/v1                            
metadata:
  name: myrs
spec:
  replicas: 2  
  selector:                  
    matchExpressions:                             # these must match the labels
      - {key: myname, operator: In, values: [test, test2, test3]}
      - {key: env, operator: NotIn, values: [production]}
  template:      
    metadata:
      name: testpod7
      labels:              
        myname: test
    spec:
     containers:
       - name: c00
         image: ubuntu
         command: ["/bin/bash", "-c", "while true; do echo Hello World; sleep 5 ; done"]
```

```
kubectl apply -f myrs.yml
kubectl get rs
kubectl get pods

kubectl scale --replicas=1 rs/myrs

kubectl get pods

kubectl get rs

kubectl delete pod <pod-name>

kubectl get pods 
```
