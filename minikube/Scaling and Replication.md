# Scaling and Replication

- Kubernetes was designed to orchestrate multiple containers and replication.
- Need for multiple containers/replication helps us with these:
- Reliability: By having multiple versions of an application, you prevent problems if one or more fails.
- Load Balancing: Having multiple versions of a container enables you to easily send traffic to different instances to prevent overloading of a single instance or node.
- Scaling - When load does become too much for the number of existing instances, Kubernetes enables you to easily scale up your application, adding additional instances as needed. 
- Rolling updates: Updates to a Service by replacing pods one by one.
