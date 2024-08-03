LEC 56 NOTES TECHNICAL GUFTGU
====================
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
===========Job============================================================
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
        command: ["bin/bash", "-c", "echo Technical-Guftgu; sleep 5"]
      restartPolicy: Never
--------------
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
-------------
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
=================Init Containers==
apiVersion: v1
kind: Pod
metadata:
  name: initcontainer
spec:
  initContainers:
  - name: c1
    image: centos
    command: ["/bin/sh", "-c", "echo LIKE AND SUBSCRIBE TECHNICAL GUFTGU > /tmp/xchange/testfile; sleep 30"]
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
=================

