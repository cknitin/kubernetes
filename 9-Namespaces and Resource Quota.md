# Namespaces and Resource Quota (Namespace, limits & request )

- You Can name your object, but if many are using the cluster then it would be difficult for Managing 
- A namespace is a group of related Elements that each have a unique name or identifier. Namespace is used to uniquely identify one or more names
  from other similar names of different Objects, groups or the namespace in general
-  Kubernetes namespaces help different projects, teams or Customers to share a Kubernetes Cluster & provides ÷

A scope for every names.
A mechanism to attach authorization and policy to a subsection of the cluster.


- By default, a Kubernetes Cluster will instantiate a default namespace when provisioning the Cluster to hold the default set of pods, 
  Services and Deployments used by the Cluster. 
- We Can use resource Quota on specifying how many resources each namespace can use.
- Most Kubernetes resources (eg pods, services, replication Controllers and Others) are in Some namespaces-and low-level resources
  such as nodes And perwsistent Volumes, are not in any namespace. 
→ Namespaces are intended for use in environments with many users spread across multiple teams, or projects! for cluster with a few to tens of users,
  you should not need to Create or think about namespaces and all.

## Create namespace
- Let us assume we have shared K8s Cluster for Dev and production use Cases. 
- The dev team would like to maintain a space in the cluster where they can get a view on the list of Pods, Services 
  and Deployments they use to build and run their application. In this, no restrictions are put on who can or Cannot modify resources to enable agile development. 
- for prod team we can enforce Strict procedure on who Can or Cannot manipulate the Set of Pods, Services and deploymente 

Cmd 
```
Kubech get namespaces
```
