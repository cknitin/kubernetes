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

![alt text](https://github.com/cknitin/kubernetes/blob/main/images/Deployment%20and%20rollback%20-5.png))

### The following are typical use Cases of Deployments. Deployment & Rollback 
#### 1 Create a deployment to rollout a Replicaset →
  - The replicaset Creates pods in the Background. Check the status of the Rollout to see if it succeeds or not
#### 2 Declare the new state of the Pods 
  - By updating the PodTemplateSpec of the deployment. A new Replicaset is Created and the Deployment manages moving the pods from the Old Replicaset to the new one at a Controlled rate. Each new Repicaret updates the revision of the Deployment. 
#### 3 Rollback to an earlier Deployment Revision 
  - If the Current state of the Deployment is not stable Each rollback Updates the revision of the Deployment. 
#### 4 Rollback to an earlier Deployment Revision 
  - If the Current state of the deployment is not stable. Each Rollback updates the revision of the deployment.
#### 5 Scale up the Deployment to facilitates more load. 
#### 6 Pause the Deployment to apply multiple fixes to its PodTemplate Spec and then resume it to start a new Rollout 
#### 7 Cleanup older Replicasets that dont need anymore


