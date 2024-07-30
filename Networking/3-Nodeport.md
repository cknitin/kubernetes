# NODEPORT

- Makes a service accessible from outside the cluster 
- Expose the service on the same port of each selected node in the cluster using NAT

## Lab

```
vi nodport.yml
```

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
kudectl apply -f nodeport.yml

kudectl get pods
```
Now create a service.yml

```
kind: Service                            # Defines to create Service type Object
apiVersion: v1
metadata:
  name: demoservice
spec:
  ports:
    - port: 80                           # Containers port exposed
      targetPort: 80                     # Pods port
  selector:
    name: deployment                     # Apply this service to any pods which has the specific label
  type: NodePort                        # Specifies the service type i.e ClusterIP or NodePort
```

```
kubectl apply -f service.yml
```

```
kubectl get svc
```

```
kubectl describe svc demoservice
```

Allow the port in AWS if not allowed, if all traffic is allowed then no need to allow this port



