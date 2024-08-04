# HELM  (Packaging Manager) (version 3)
- Introduced first time in 2015 HELM 2 Client-Servers
- Helm helps you manage K8s applications with helm charts which helps you define, install and Upgrade even the most complex Kubernetes application.
- Helm is the K8s equivalent of yum or apt.
- Helm is now an official K8s Project and is part of CNCF
- The main Building Block of Helm Based deployments are Helm charts: these Charts describe a Configurable Set of dynamically generated K8s resources.
- The Chart can either be stored locally or fetched from remote chart Repositories  


# Why Use Helm ? 
Writing and maintaining Kubernetes YAML manifest for all the required Kubernetes Objects Can be a time consuming and tedious task. For the simplest of deployments, you would need atleast 3 YAML manifest with duplicated and Hardcoded Values. Helm simplifies this process and Create a Single package that can be advertised to your cluster. 

→ HELM 3 was released in Nov. 2019. Helm K8s automatically maintains a database of all Versions of your releases. So whenever something goes wrong during deployment, raling back to the previou Version is just one command away.
