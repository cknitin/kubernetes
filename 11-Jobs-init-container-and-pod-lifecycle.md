# Jobs 
- We have replicasets, Daemonsets, Statefulsets and deployments they all share one common property: they ensure that their pods are always running. If a pod fails, the Controller restarts it or reschedules it to another node to make sure the application the pods is hosting keeps running.

## Use Cases 
-  Take Backup of a DB
-  Helm Charts uses jobs
-  Running Batch processes
-  Run a task at an Schedule interval
-  Log rotation Fod Lifecycle 

```
apiVersion: batch/vi 
Kind: Job 
metadata: 
    name: myjob 
Spec 
template 
  metadata 
    name: myjob 
 Spec 
  Containers 
    -- name: co1 
      image: Centos7 
      Command: ['bin/bash', '-c', 'echo TG; Sleep 5']
      restartPolicy: Never 
```
      
Job does not get deleted by itself, we have to delete it 

```
Kubett delete -f Job.yml
```

# Lab

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

Create a manifest file batch.yml

```
vi batch.yml
```

```
apiVersion: batch/v1
kind: Job
metadata:
  name: testjob
spec:
  template:
    metadata:
      name: testjob
    spec:
      containers:
      - name: counter
        image: centos:7
        command: ["bin/bash", "-c", "echo Hello Word; sleep 5"]
      restartPolicy: Never
```
:wq

```
kubectl apply -f batch.yml

kubectl get pods

watch !!
```

# Lab : Parallelism

```
vi parallelism.yml
```

```
apiVersion: batch/v1
kind: Job
metadata:
  name: testjob
spec:
  parallelism: 5                           # Runs for pods in parallel
  activeDeadlineSeconds: 10  # Timesout after 30 sec
  template:
    metadata:
      name: testjob
    spec:
      containers:
      - name: counter
        image: centos:7
        command: ["bin/bash", "-c", "echo Technical-Guftgu; sleep 20"]
      restartPolicy: Never
```

:wq

```
kubectl apply -f parallelism.yml

kubectl get pods

watch !!
```

# The Cron Job Pattern

- If we have multiple nodes hosting the application for high availability, which nodes handles Cron? 
- What happens if multiple identical cron jobs run Simultaneously?


```
apiVersion: batch/vibeta1
Kind: CronJob
metadata:
 name: Bhupi
Spec
Schedule: "* * * * *"
jobTemplate:
  Spec:
  template
    Spec
    Containers
    -image: ubuntu
       name: TestCon
       Command: [6/bin/bash", "-C" "echo Hello world; sleep 5"J
restartPolicy : Never"
```

# Lab - Cron Job

```
vi cronjob.ynl
```

```
apiVersion: batch/v1beta1
kind: CronJob
metadata:
 name: bhupi
spec:
 schedule: "* * * * *"
 jobTemplate:
   spec:
     template:
       spec:
         containers:
         - image: ubuntu
           name: bhupi
           command: ["/bin/bash", "-c", "echo Technical-Guftgu; sleep 5"]
         restartPolicy: Never
```

:wq

```
kubectl apply -f cornjob.yml

kubectl get pods
```

using above manifest file pod will create after every 1 min

```
watch !!
```

# Init Containers 
- Init Containers are specialised Containers that run before app Containers in a Pod.
- init Containers always run to Completion
- If a pod's init Container fails, Kubernetes repeatedly restarts the pod until the init Container succeeds
- init Containers do not support readiness Probe

## Use Cases 
- Seeding a Database
- Delaying the application launch until the dependencies are ready 
- Clone a git repository into a Volume
- Generate Configuration file dynamically


# Lab - Init Containers

```
vi init.yml
```

```
apiVersion: v1
kind: Pod
metadata:
  name: initcontainer
spec:
  initContainers:
  - name: c1
    image: centos
    command: ["/bin/sh", "-c", "echo Hello World > /tmp/xchange/testfile; sleep 30"]
    volumeMounts:        
      - name: xchange
        mountPath: "/tmp/xchange"  
  containers:
  - name: c2
    image: centos
    command: ["/bin/bash", "-c", "while true; do echo `cat /tmp/data/testfile`; sleep 5; done"]
    volumeMounts:
      - name: xchange
        mountPath: "/tmp/data"
  volumes:                            
  - name: xchange
    emptyDir: {}
```

```
kubectl apply -f init.yml

watch kubectl get pods

kubectl logs -f pod/initcontainer
```

# Pod Lifecycle

→ Pending → Running → Succeeded → Failed → Completed → Unkeawn | Running 

- The phase of a Pod is a simple, high-level Summary of where the pod is in its lifecycle. 

#### Pending 
    - The pod has been accepted by the K&s system, but its not running. 
    - One or more of the Container images is still downloading. 
    - If the pod Cannot be scheduled because of resource Constraints. 
    -

#### Running
    - The pod has been bound to a node. 
    - All' of the Containers have been Created 
    - Atleast one Container is Still running Or is in the process of starting or restarting. 

#### Succeeded    
    Succeeded All Containers in the lod have terminated in success, and will not be restarted.

#### Failed 
    - All Containers in the pod have terminated and atleast One Container has terminated in failure. 
    - The Container either exited with non-zero Status or was terminated by the system. 
#### Unknown 
    - State of the pod could not be obtained. 
    - Typically due to an error in network or communicating with the host of the Pod.

#### Completed 
    - The pod has run to Completion as there's nothing to keep it running. eg.- Completed jobs


# POD Conditions

A pod has a PodStatus, which has an array of PodConditions through which the pod has or has not passed. 

- Using below command, you can get Condition of a pod 

```
Kubectl describe pod PODNAME'
```


### These are the possible types 

#### Pod Scheduled:
    - The pod has been schedule to a node 

#### Ready 
    - The pod is able to serve request and will be added to the load Balancing pods of all matching services. 

#### Initialized
    - All init Containers have Started Successfully 

#### Unscheduled 
    - The scheduler Cannot Schedule the pod right now, for eg. due to lacking of Resources or other Constraints 

#### Container Ready 
    - All Containers in the pod are ready


```
kubectl describe pod/initcintainer | grep -A 5 Conditions
```
