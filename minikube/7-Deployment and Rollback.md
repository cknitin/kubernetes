# Deployment and Rollback

  - Replication Controller & Replica Set is not able to do Updates & Rollback apps in the Cluster
  - A deployment Object act as a Supervisor for pods, giving you fine-grained Control Over how and when a new pod is Rolled Out, Updated or Rolled Back to a Previous state.
  - When using deployment Object, we first define the state of the app, then K8s Cluster Schedules mentioned app instance onts specific individual nodes.
  - In K8s, if the node hosting an instance goes down or the pod is deleted, the deployment controller replaces it.
  - This provides a self-healing mechanism to address machine failure or maintenance.


Replication Controller & Replica Set is not able to do Updates & Rollback apps in the Cluster 
  - A deployment Object act as a Supervisor for pods, giving you fine-grained Control Over how and when a new pod is Rolled Out, Updated or Rolled Back to a Previous     state.
  -  When using deployment Object, we first define the state of the app, then K8s Cluster Schedules mentioned app instance onts specifie individual nodes.
  -  A deployment provides declarative updates for Pods & Replicaset
  -  K8s then monitors, if the node hosting an instance goes down or pod is deleted the deployment Controller replaces it. 
  -  This provides a self-healing mechanism to address machine failure or maintenance.

![alt text]([http://url/to/img.png](https://github.com/cknitin/kubernetes/blob/main/images/Deployment%20and%20rollback%20-5.png))


