# Volumes

- Containers are short lived in Nature.
- All data stored inside a Container is deleted if the Container Groshes. However the Kubelet will restart it with a Clean state, which means that it will not have any of the Old data.
- To Overcome this problem, Kubernetes Uses Volumes. A Volume is essentially a directory backed by a Storage medium. The storage medium and its Content are determined by the Volume type
- In Kubernetes, a volume is attached to a Pod and shared among the Containers of that Pod.
- The Volume has the same life span as the Pod, and it Outlives the Containers of the Pod this allows data to be preserved across Container Restarts.
