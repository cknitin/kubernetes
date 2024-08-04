# Helm Commands 

```
helm repo': Interact with charts Repository. Helm-3 No longer ships with a default chart Repository
```

```
helm repo list 
```

```
helm repo add <Name> <URL>
```

```
helm repo remove <Name>
```

```
'helm search': for finding charts for eg:- helm search repo <chart>
```

```
helm Show: Information about a chart
```

for eg

```
helm show <values | chart | readmel all ><Chart Name>
```

```
helm install: Install a package foreg → helm install <Release Name> <chart Name>
```

```
Wait Untill all Subjects are ready foreg → helm install mychart stable/tomcat --wait --timeout 105
```

```
helm create: Create a new Chart with given name For eg - helm Create helloworld
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

> To download helm

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh

```

> To verify the installation use the following command

```
which helm
```

```
helm version
```

```
helm list repo
```

```
helm list
```

```
helm repo add stable https://charts.helm.sh/stable
```

```
helm repo list
```

```
helm repo remove stable
```

```
helm repo list 
```

No repo found

```
helm repo search jenkins

helm repo search tomcat

```

```
helm show values stable/tomcat
```

```
helm show chart stable/tomcat
```

```
helm show all stable/tomcat
```

```
apt-get install tree
```

```
helm create helloworld

ls

tree helloworld/

rm -rf helloworld
```
```
helm install testchart2 stable/tomcat --set service.type=NodePort
```

> To uninstall helm

```
which helm ( to see which folder its installed )
rm -rf /usr/local/bin/helm
kubectl get all
```
