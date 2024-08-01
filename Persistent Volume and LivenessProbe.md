# Persistent Volume and LivenessProbe in Kubernetes

- In a typical IT environment, storage is managed by the storage/system administrator. The end user will just get instructions to Use the storage, but does not have to worry about the underlying storage management.
- In the Containerized World, We would like to fallow Similar rules, but it becomes Challenging, given the many volume types. We have seen earlier. Kubernetes resolves this problem with the Persistent Volume (PV) Subsystem
- The PV is not backed by locally attached Storage on a worked node but by networked storage system Such as EBS or NFS or Ja distributed filesystem like Ceph.
- A persistent Volume (PV) is a Cluster-wide resource that you can use to store data in a way that it persists beyond the lifetime of a pod.
- K8s provides APIs for users. and administrator to manage and Consume Storage To manage the Volume, it uses the Persistent Volume API resource type and to Consume it, uses the Persistent Volume- Claim API resource type. 
