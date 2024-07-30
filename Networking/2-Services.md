# Object - Services 

- Each pod gets its own IP address, however in a deployment, the set of Pods running in one moment in time could be different from the Set of pods running that application a moment later. 

Problem - This leads to a problem: if some set of Pods (Call them backends) provides functionality to other pods ('call them frontend") inside your cluster, how do the frontends find out and keep track of which IP address to connect to, So that the frontend Can use the backend part of the workload?


- When using RC, pods are terminated and Created during Scaling or replication Operations.
- When using deployments, while updating the image version the pods are terminated and new pods take the place of other pods.
- Pods are very dynamic ie they come & go on the K8s Cluster and on any of the available nodes & it would be difficult to access the pods as the pods IP changes once its recreated
- Service Object is an logical bridge between pods and end users, which provides virtual IP (VIP). 
- Service allows Clients to reliably Connect to the Containers running in the Pod using the VIP. 
- The VIP is not an actual IP Connected to a network interface, but its purpose is purely to forward traffic to One or more pods. 
- Kube proxy is the One which keeps the mapping between the VIP and the pods upto date, which Queries the API Server to learn about new Services in the Cluster

- Although each pod has a unique IP address, those IP's are not exposed outside the Cluster.
- Services helps to expose the VIP mapped to the pods & allows application to receive traffic. 10.1.1.1
- Labels are used to select which are the pods to be put under a service.
- Creating a service will create an endpoint to access the pods/application in it.

- Services Can be exposed in different ways by specifying a type in the Service spec:
    - Cluster IP
    - NodePort
    - Load Balancer: Created by cloud Providers that will route external traffic to every node on the Nodeport. (eg. ELB on AWS) 
    - Headless → Creates several endpoints that are used to produce DNS Records. Each DNS record is bound to a Pod. 
- By default It Service Can run only between ports 30,000 - 32767
- The set of pods targeted by a service is usually determined by a selector.

## Cluster IP 
- Exposes VIP only reachable from within the Cluster.
- Mainly used to communicate between Components of Microservices.
