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


### Lab

## Lab 4

> volume labs

```
vi emptydir.yml
```

```
apiVersion: v1
kind: Pod
metadata:
  name: myvolemptydir
spec:
  containers:
  - name: c1
    image: centos
    command: ["/bin/bash", "-c", "sleep 15000"]
    volumeMounts:                                    # Mount definition inside the container
      - name: xchange
        mountPath: "/tmp/xchange"          
  - name: c2
    image: centos
    command: ["/bin/bash", "-c", "sleep 10000"]
    volumeMounts:
      - name: xchange
        mountPath: "/tmp/data"
  volumes:                                                   
  - name: xchange
    emptyDir: {}
```

```
kubectl apply -f emptydir.yml
```

```
kubectrl get pods
```

To enter inside POD and container one i.e. c1
```
kubectl exec myvolemptydir -c c1 -it -- /bin/bash
```

```
cd /tmp

ls
```
you will see a volume with name xchange

```
cd xchange

vi abc.yml

```

add some text

```
exit
```

Now enter in second container

To enter inside POD and container c2
```
kubectl exec myvolemptydir -c c2 -it -- /bin/bash
```

```
cd /tmp
ls

cd data

ls
```

you will see abc.yml from container two c2

now edit abc.yml, add some text 
```
vi abc.yml
```
To exit vi
```
:wq
```
Go inside the c1 again
```
kubectl exec myvolemptydir -c c1 -it -- /bin/bash
```

```
cat /tmp/xchange/abc.yml
```

and see the updated output
