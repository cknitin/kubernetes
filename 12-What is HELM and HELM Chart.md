# HELM  (Packaging Manager) (version 3)
- Introduced first time in 2015 HELM 2 Client-Servers
- Helm helps you manage K8s applications with helm charts which helps you define, install and Upgrade even the most complex Kubernetes application.
- Helm is the K8s equivalent of yum or apt.
- Helm is now an official K8s Project and is part of CNCF
- The main Building Block of Helm Based deployments are Helm charts: these Charts describe a Configurable Set of dynamically generated K8s resources.
- The Chart can either be stored locally or fetched from remote chart Repositories  


# Why Use Helm ? 
Writing and maintaining Kubernetes YAML manifest for all the required Kubernetes Objects Can be a time consuming and tedious task. For the simplest of deployments, you would need atleast 3 YAML manifest with duplicated and Hardcoded Values. Helm simplifies this process and Create a Single package that can be advertised to your cluster. 

- HELM 3 was released in Nov. 2019. Helm K8s automatically maintains a database of all Versions of your releases. So whenever something goes wrong during deployment, raling back to the previou Version is just one command away.

## Some Keyword to Understand HELM 

> CHART
 
A Chart is a Helm package. It contains all of the resource definitions necessary to run an application, tool or service inside of a K8s Cluster. Think of it like the Kubernetes equivalent of a Homebrew formula, apt, yum or RPM file. 

OR 

Helm Charts are simply K8s YAML manifests Combined into a single package that Can be advertised to your K8s Cluster.


> RELEASE 

A release is an instance of a chart running in a K8s Cluster. One Chart Can often be installed many times into the Same Cluster and each time it is installed, a new release is Created. 

Consider a MySQL chart, if you can install that Chart twice. Each one will have its own release, which will in turn have its own release name.


|Revision |	Request         |
| ------- | ----------------|
| 1	      | Installed Chart |
| 2	      | Upgrade to 1.1  |

Helm keep tracks of all Chart execution (Install/Upgrade/Rollback)	


> REPOSITORY 

Location where packaged Charts Can be stored and Shared.



DEV ==> QA ==> UAT ==> Prod


#### Normal Kubernetes deployment YAML

```
apiversion: apps/v1 
  Kind: Deployment 
  metadata name: Release
    name-springboot 
    Spec: 
    replicas: 2 
  Selector: 
  matchlabels 
app.Kubernetes io/name: springboot
```

#### Helm deployment YAML 

```
apiversion: apps/v1
  kind :Deployment 
  metadata: 
    name: Release-name-springboot 
    spec: 
    replicas: {{.Values replicaCount }} 
    Selector: 
     matchlabels: 
     app. Kubernetes.io/name : Springboot
```

# HELM-3 ARCHITECTURE 

- HELM-3 is a single-service architecture. One executable is responsible for implementing HELM. There is no Client-server split, nor is the Core processing logic distributed among Components. 

- Implementation of HELM-3 is a Single command line, Client with no in-cluster server, or Controller. This tool exposes Command line Operations and Unilaterally handles the package management process.

![HELM-3 ARCHITECTURE](https://github.com/cknitin/kubernetes/blob/main/images/HLM-ar-1.png)

