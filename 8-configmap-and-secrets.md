# Kubernetes configmap and secrets

- While performing application deployments on K8s Cluster, sometimes we need to change the application Configuration file depending on environments like dev, QA, stage or Prod.

- Changing this application Configuration file means we need to change source Code, Commit the Change, Creating a new image and then go through the complete deployment process.
  
- Hence these configurations should be decoupled from image Content in order to keep Containerised application portable.

- This is where Kubernetes Configmap comes handy. It allows us to handle Configuration files much more efficiently.


LEC 53 NOTES TECHNICAL GUFTGU
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
====================================================================
configmap
====================================================================
 apiVersion: v1
kind: Pod
metadata:
  name: myvolconfig
spec:
  containers:
  - name: c1
    image: centos
    command: ["/bin/bash", "-c", "while true; do echo Technical-Guftgu; sleep 5 ; done"]
    volumeMounts:
      - name: testconfigmap
        mountPath: "/tmp/config"   # the config files will be mounted as ReadOnly by default here
  volumes:
  - name: testconfigmap
    configMap:
       name: mymap   # this should match the config map name created in the first step
       items:
       - key: sample.conf
         path: sample.conf

==============================================================================

apiVersion: v1
kind: Pod
metadata:
  name: myenvconfig
spec:
  containers:
  - name: c1
    image: centos
    command: ["/bin/bash", "-c", "while true; do echo Technical-Guftgu; sleep 5 ; done"]
    env:
    - name: MYENV         # env name in which value of the key is stored
      valueFrom:
        configMapKeyRef:
          name: mymap      # name of the config created
          key: sample.conf            
-----------------=========================================================
echo "root" > username.txt; echo "password" > password.txt

kubectl create secret generic mysecret --from-file=username.txt --from-file=password.txt




apiVersion: v1
kind: Pod
metadata:
  name: myvolsecret
spec:
  containers:
  - name: c1
    image: centos
    command: ["/bin/bash", "-c", "while true; do echo Technical-guftgu; sleep 5 ; done"]
    volumeMounts:
      - name: testsecret
        mountPath: "/tmp/mysecrets"   # the secret files will be mounted as ReadOnly by default here
  volumes:
  - name: testsecret
    secret:
       secretName: mysecret  
