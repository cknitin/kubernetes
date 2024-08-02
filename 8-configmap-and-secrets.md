# Kubernetes configmap and secrets

- While performing application deployments on K8s Cluster, sometimes we need to change the application Configuration file depending on environments like dev, QA, stage or Prod.

- Changing this application Configuration file means we need to change source Code, Commit the Change, Creating a new image and then go through the complete deployment process.
  
- Hence these configurations should be decoupled from image Content in order to keep Containerised application portable.

- This is where Kubernetes Configmap comes handy. It allows us to handle Configuration files much more efficiently.

- Configmaps are useful for storing and sharing non-sensitive, unencrypted Configuration information. Use Secret Otherwise.

- Configmap can be used to store fine-grained information like individual properties or entire Configfiles.

- Configmap are not intended to act as a replacement for a properties file.



### Configmap can be accessed in following ways:

- 1. As environment variables
- 2. As volumes in the pod Kubectl Create Configmap <mapname> --from-file=<file to read> abc conf

# SECRETS

You don't want sensitive information such as a database password or an API key kept around in clear text.

- Secrets provide you with a mechanism to use such information in a safe and reliable way with the following properties: -> Secrets are namespaced objects, that is exist in the context of a namespace.

- You can access them via a volume or an environment variable from a container running in a pod. The secret data on nodes is stored in tmpfs volumes (tmpfs is a file system which keeps all files in virtual memory. Everything in tmpfs is temporary in the sense that no files will be created on your hard drive. -> A per-secret size limit of 1MB exul -> The API server stores secrets as plaintext in etcd. Secrets can be created

- 1. from a text file
- 2. from a yaml file


# Lab
> Install Docker
```
$  sudo apt update && apt -y install docker.io
```

>  Install kubectl
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

## configmap

```
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
```


```
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

```

```
echo "root" > username.txt; echo "password" > password.txt
```

```
kubectl create secret generic mysecret --from-file=username.txt --from-file=password.txt
```


```
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
```
