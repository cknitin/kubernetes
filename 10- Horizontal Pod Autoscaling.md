# Horizontal Pod Autoscaler

- Kubernetes has the possibility to automatically scale pods based on Observed CPU Utilization, which is horizontal Pod Autoscaling.

- Scaling Can be done only for Scalable Objects like Controller, deployment or Replica Set

- HPA is implemented as a Kubernetes API resource and a Controller

- The Controller periodically adjust the number of replicas in a replication Controller or deployment to match the Observed average CPU Utilization to the target specified by user

- The HPA is implemented as a Controller loop with a period Controlled by the Controller manager's-horizontal-pod-autoscaler sync-period flag (Default Value of 30 Seconds)

- During each period, the Controller manager Queries the resource Utilization against the metrics specified in each horizontalPod Autoscaler definition


More points

- For per-pod resource metrics (like CPU), the Controller fetches the metrics from the resource metrics API for each pod targeted by the Horizontal Pod Autoscaler.

-  Then if a target utilization Value is set, the Controller Calculates the Utilization value as a percentage of the Equivalent resource request on the Containers in each pod. 

- If a target raw-value is set, the raw metric Values are used directly. The Controller then takes the mean of the Utilization on the raw value across all targeted pods, and produces a ratio used to scale the number of desired replicas. 

- Cooldown period to wait before another downscale Operation Can be performed Controlled by 

```
--horizontal-pod-autoscaler-downtime-stabilization flag (Default Value of 5 min) 
```

→ Metric Server needs to be deployed in the Cluster to provide metrics via the resource metrics API. 

```
---kubelet-insecure-tts Kubecht autoscale deployment mydeploy --cpu-percent=20 --min=1 --max=10
```


# Lab
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

> Install metric server

```
$ wget -O metricserver.yml https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

open the metricserver.yml file

```
vi metricserver.yml
```

now edit it

```
  labels: 
    k8s-app: metrics-server 
spec: 
  containers: 
    args: 
    --cert-dir=/tmp 
    --secure-port=4443 
    --kubelet-insecure-tls   <------------Add this line here 🅰️ 🥇 
    --kubelet-preferred-address-types=InternalIP, ExternalIP, Hostname 
    --kubelet-use-node-status-port 
    --metric-resolution=15s 
    image: k8s.gcr.io/metrics-server/metrics-server:v0.5.2 
    imagePullPolicy: IfNotPresent
```

```
kubectl apply -f metricserver.yml
```

```
kudectl get namespaces
```

there is a namespace kube-system

```
kubectl get pods -n kube-system
```

after running above command you will metrics server i.e. mertics-server-53dsg-sdf4523

```
kubectl logs -f  mertics-server-53dsg-sdf4523 -n kube-system
```
Now create POD


```
vi autoscale.yml
```

```
kind: Deployment
apiVersion: apps/v1
metadata:
   name: mydeploy
spec:
   replicas: 1
   selector:
    matchLabels:
     name: deployment
   template:
     metadata:
       name: testpod8
       labels:
         name: deployment
     spec:
      containers:
        - name: c00
          image: httpd
          ports:
          - containerPort: 80
          resources:
            limits:
              cpu: 500m
            requests:
              cpu: 200m
```

```
kubectl apply -f autoscale.yml
```

```
kubectl get all
```

> To apply HPA 

```
$ kubectl autoscale deployment mydeploy --cpu-percent=20 --min=1 --max=10
```

Now start a new instance of this linux machine

and go inside the POD

```
kubectl exec --podename-- --it /bin/bash
```

Terminal 1

```
watch kubectl get all 
```
 

Now Terminal 2

```
apt-update
```

you will see new pods in Terminal 2
