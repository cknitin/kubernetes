# Persistent Volume and LivenessProbe in Kubernetes

- In a typical IT environment, storage is managed by the storage/system administrator. The end user will just get instructions to Use the storage, but does not have to worry about the underlying storage management.
- In the Containerized World, We would like to fallow Similar rules, but it becomes Challenging, given the many volume types. We have seen earlier. Kubernetes resolves this problem with the Persistent Volume (PV) Subsystem
- The PV is not backed by locally attached Storage on a worked node but by networked storage system Such as EBS or NFS or Ja distributed filesystem like Ceph.
- A persistent Volume (PV) is a Cluster-wide resource that you can use to store data in a way that it persists beyond the lifetime of a pod.
- K8s provides APIs for users. and administrator to manage and Consume Storage To manage the Volume, it uses the Persistent Volume API resource type and to Consume it, uses the Persistent Volume- Claim API resource type. 

## Persistent Volume Claim 
- In Order to use a PV you need to Claim it first, using a Persistent Volume Claim (PVC) 
- The PVC request a PV with your desired Specification (Size, access modes, Speed etc) from Kubernetes and Once a Suitable Persistent Volume is found, it is bound to a Persistent Volumeclaim.
- After a successful bound to a pod, you can mount it as a volume.
- Once a user finishes its works, the attached Persistent Volume Can be released. The underlying PV Can there be reclaimed and Recycled for future usage.

## Elastic Block Store AWS (EBS 
- An aws EBS volume mounts an AWS (EBS) V'olume into your pod. Unlike empty Dir, which is erased when a pod is removed, the contents of an EBS Volume are preserved and the Volume is merely Unmounted.

  - There are some Restrictions:
    - The nodes on which Pods are running must be aws EC2 instances.
    - Those instances need to be in the Same region and Availability Zone as the EBS Volume.
    - EBS Only Supports a single EC2 instance mounting a Volume.

# Lab 1

> Install Docker
```
$  sudo apt update && apt -y install docker.io
```

> Install kubectl
```
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl &&   chmod +x ./kubectl && sudo mv ./kubectl /usr/local/bin/kubectl
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
Now create volume in AWS and attach it with below yml

```
vi mypv.yml
```

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: myebsvol
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Recycle
  awsElasticBlockStore:
    volumeID:           # YAHAN APNI EBS VOLUME ID DAALO
    fsType: ext4
```

```
kubectl apply -f mypv.yml
```

```
kubectl get pv
```

Now create a volume claim yml file

```
vi mypvc.yml
```

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myebsvolclaim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

```
kubectl apply -f mypvc.yml
```

Create POD

```
vi deployment.yml
```

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pvdeploy
spec:
  replicas: 1
  selector:      # tells the controller which pods to watch/belong to
    matchLabels:
     app: mypv
  template:
    metadata:
      labels:
        app: mypv
    spec:
      containers:
      - name: shell
        image: centos
        command: ["bin/bash", "-c", "sleep 10000"]
        volumeMounts:
        - name: mypd
          mountPath: "/tmp/persistent"
      volumes:
        - name: mypd
          persistentVolumeClaim:
            claimName: myebsvolclaim

```

```
kubectl apply -f deployment.yml
```

> To get deploy object
```
kubectl get deploy
```

> To get ReplicaSet

```
kubectl get rs
```

> To get Pods

```
kubectl get pods
```

```
kubectl exec -pod-name- it -- /bin/bash
```

```
cd /tmp/persistent

ls
```

```
vi text

Hello Volumne

:wq

ls

exist

delete pod -pode-name-
```

```
kubectl get pods
```

```
kubectl exec -new-pod-name- it -- /bin/bash
```

```
cd /tmp/persistent

ls

cat test
```




## Health check/LivenessProbe
- A Pod is considered ready when all of its Containers are ready.
- In Order to Verify if a Container in a Pod is Healthy and ready to serve traffic, Kubernetes provides for a range of healthy checking mechanism.
- Health Checks or probes are Carried Out by the Kubelet to determine When to restart a Container (For liveness) and used by services and deployments to determine if a pod should receive traffic.

  ### for-eg- 
  Liveness Probes could Catch a deadlock, where an application is running, but unable to make progress. Restarting a Container in such a State can help to make the application more available despite bugs. 

- One use of readiness probes is to Control which pods are used as backends for services When a pod is not ready, it is removed from Service load Balancers. 
- for running healthchecks, we would use cmds specific to the application. 
- If the cmd succeeds, it returns O, and the Kubelet Considers the Container to be alive and healthy. If the Command returns a non-zero value, the Kubelet Kills the Pod and recreate it.


# Lab 2

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: mylivenessprobe
spec:
  containers:
  - name: liveness
    image: ubuntu
    args:
    - /bin/sh
    - -c
    - touch /tmp/healthy; sleep 1000
    livenessProbe:                                       # define the health check
      exec:
        command:                                         # command to run periodically
        - cat                
        - /tmp/healthy
      initialDelaySeconds: 5                             # Wait for the specified time before it runs the first probe
      periodSeconds: 5                                   # Run the above command every 5 sec
      timeoutSeconds: 30                              

```
