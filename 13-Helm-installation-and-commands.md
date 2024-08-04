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


```
kubectl install  testjenkins stable/jenkins
```

```
helm delete testjenkins
```

```
helm install --dry-run testchart stable/tomcat
```

> To wait after install

```
helm install --wait --timeout 20s testcomcat2 stable/tomcat

helm delete testtomcat2
```

> To install any specific version

```
helm install testchart stable/tomcat --version 0.4.0

helm list  
```

# More helm commands

There are two ways to pass Configuration data during install. 

--set: Specify Overrides on the command line 

--values (or-f): Specify a YAML file with Overrides 
    
  Foreg:- 
  
  ```
  helm install mychart Stable/tomcat -- set service type = 1 NodePort 
  ```
  
  ```
  helm install testchart Stable/Jenkins --set master servicetype = NodePort 
  ```

  'helm get': Information about from a Named Release 
   
    helm get <all / manifest/values> <Rel name> 
  
    For eg - 
    ```
     helm get all mychart
    ```

  - helm list 
     -  List all the Named Releases

  - helm status
     - Display the Status of the Named Release

  - helm status <Release Name>
    eg:-
    
    ```
    helm status mychart
    ```

# Lab -  I want to change the service type from loadBalancer to nodPort

```
helm show values stable/tomcat
```

you will see in yml (manifest) 

service:
    type: LoadBalancer


now change LoadBalancer to NodePort

```
helm install testchart2 stable/tomcat --set service.type=NodePort
```

To check changes value

```
helm get values testchart2
```

```
helm get manifest testchart2
```

```
hel status testchart2
```

# More HELM commands

- helm history: Fetch Release history helm history <Release Name>

- helm delete: Uninstall the deployed Release helm delete < ReleaseName >

- helm upgrade. “ Upgrade a Release" helm upgrade <ReleaseName><chartName> 
    for eg

    ```  
    helm upgrade mychart stable/tomcat
    ```  
- helm rollback : Rollback a Release to any previous Version helm rollback <Release Name > < Revision > 

  for eg 
  
    ```
    helm rollback mychart
    ```
