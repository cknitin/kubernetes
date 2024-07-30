# Object - Services 

- Each pod gets its own IP address, however in a deployment, the set of Pods running in one moment in time could be different from the Set of pods running that application a moment later. 

Problem - This leads to a problem: if some set of Pods (Call them backends) provides functionality to other pods ('call them frontend") inside your cluster, how do the frontends find out and keep track of which IP address to connect to, So that the frontend Can use the backend part of the workload?
