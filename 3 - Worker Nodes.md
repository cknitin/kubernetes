## Nodes (kublet and container engine.)
Node is going to run 3 important pieces of software/ process.

  1. Kublet
  2. Container engine
  3. Kubeproxy

## 1. Kublet:
 - Agent running on the node.
 - Listens to Kubernetes master. (e.g-POD creation request)
 - Use port 10255.
 - Send success/fail reports to master.

## 2. Container Engine:
 - Works with kublet.
 - Pilling images.
 - Start/ stop containers.
 - Exposing containers on ports specified in manifest.

## 3. Kubeproxy:
 - Assign IP to each POD.
 - It is required to assign IP address to PODs (dymanic).
 - Kube-proxy runs on each node and this make sure that each POD will get its own unique IP address. These 3 components collectively called NODE.

## POD:
- Smallest unit in Kubernetes.
- POD is a group of one or more containers that are deployed together on the same host.
- A cluster is a group of nodes.
- A cluster has at least one worker node and master node.
- In Kubernetes the control unit is the POD, not containers.
- It consists of one or more tightly coupled containers.
- POD runs on node which is control by master.
- Kubernetes only knows about PODs (does not know about individual container).
- Cannot start containers without a POD.
- One POD usually contains one container.

## Multi container PODs:
- Share access to memory space.
- Connect to each other using local host <container port>.
- Share access to the same volume.
- Containers within POD are deployed in an all-or-nothing manner.
- Entire POD is hosted on the same node (scheduler will decide about which node).

## POD limitations:
- No auto healing or auto scaling.
- POD crashes.

## Higher level Kubernetes objects:
 a. Replication set: auto scaling and auto healing
 b. Deployment: versioning and rollback
 c. Service:
 d. Static (non-ephemeral) IP and networking.
 e. Volume: non-ephemeral storage.

## Important:
 - Kubectl- single cloud
 - Kubeadm- on premise
 - Kubefed- federated
