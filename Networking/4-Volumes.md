# Volumes

- Containers are short lived in Nature.
- All data stored inside a Container is deleted if the Container Groshes. However the Kubelet will restart it with a Clean state, which means that it will not have any of the Old data.
- To Overcome this problem, Kubernetes Uses Volumes. A Volume is essentially a directory backed by a Storage medium. The storage medium and its Content are determined by the Volume type
- In Kubernetes, a volume is attached to a Pod and shared among the Containers of that Pod.
- The Volume has the same life span as the Pod, and it Outlives the Containers of the Pod this allows data to be preserved across Container Restarts.

## Volume Types 
A Volume type decides the properties of the directory, like Size, Content etc. 
Some eg of Volume types are: 
- node-local type such as emptydir and hostpath 
- File sharing types such as nfs 
- Cloud provider-specific types like awselasticblockstore, azuredisk 
- distributed file system types, for example glusterfs or Cephts 
- Special purpose types like seeret, gitrepo

### EmptyDir 
- Use this when we want to share contents between multiple Containers on the Same pod & not to the host machine.
- An emptydir volume is first Created when a pod is assigned to a node, and exist as long as that pod is running on that node.
- As the name says, it is initially empty
- Containers in the pod Can all read and write the same files in the emptydir Volume, though that Volume Can be mounted at the same or different paths in each Containers.
- when a pod is removed from a node for any reason, the data in the emptydir is deleted forever. 
- A Container crashing does not remove a Pod from a node, so the data in an emptyDir volume is safe across Container Crashes.
