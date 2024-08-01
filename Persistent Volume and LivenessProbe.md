# Persistent Volume and LivenessProbe in Kubernetes

- In a typical IT environment, storage is managed by the storage/system administrator. The end user will just get instructions to Use the storage, but does not have to worry about the underlying storage management.
- In the Containerized World, We would like to fallow Similar rules, but it becomes Challenging, given the many volume types. We have seen earlier. Kubernetes resolves this problem with the Persistent Volume (PV) Subsystem
- The PV is not backed by locally attached Storage on a worked node but by networked storage system Such as EBS or NFS or Ja distributed filesystem like Ceph.
- A persistent Volume (PV) is a Cluster-wide resource that you can use to store data in a way that it persists beyond the lifetime of a pod.
- K8s provides APIs for users. and administrator to manage and Consume Storage To manage the Volume, it uses the Persistent Volume API resource type and to Consume it, uses the Persistent Volume- Claim API resource type. 

## Persistent Volume Claim 
- In Order to use a PV you need to Claim it first, using a Persistent Volume Claim (PVC) 
- The PVC request a PV with your desired Specification (Size, access modes, Speed etc) from Kubernetes and Once a Suitable Persistent Volume is found, it is bound to a Persistent Volumeclaim.
- After a successful bound to a pod, you can mount it as a volume.
- Once a user finishes its works, the attached Persistent Volume Can be released. The underlying PV Can there be reclaimed and Recycled for future usage.

## Elastic Block Store AWS (EBS 
- An aws EBS volume mounts an AWS (EBS) V'olume into your pod. Unlike empty Dir, which is erased when a pod is removed, the contents of an EBS Volume are preserved and the Volume is merely Unmounted.

  - There are some Restrictions:
    - The nodes on which Pods are running must be aws EC2 instances.
    - Those instances need to be in the Same region and Availability Zone as the EBS Volume.
    - EBS Only Supports a single EC2 instance mounting a Volume.
