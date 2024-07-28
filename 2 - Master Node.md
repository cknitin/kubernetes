## Role of Master Node:
- Kubernetes cluster contains containers running or bare metal/ vm instances. Cloud instances/ all mix.
- Kubernetes designates one or more of these as master and all others as workers.
- The master is now going to run set of k8s processes. These processes will resume smooth functioning of cluster. These processes are called “control plane”.
- Can be multi-master for high availability.
- Master runs control plane to run cluster smoothly.

## Components of Control Plane (master):
- 1. Kube-API server
- 2. Etcd
- 3. Kube-scheduler
- 4. Controller manager

### 1. Kube-API server (for all communications):
- This api-server interacts directly with user (i.e we apply .yml or json manifest
to kube-apiserver).
- This kube-apiserver is meant to scale automatically as per load.
- Kube api-server is front-end of control-plane.

### 2. Etcd:
- It stores metadata and status of cluster.
- Etcd is consistent and high available store (key-value store).
- Source of touch for cluster state (info about state of cluster).

#### Etcd has following features:
 a. Fully replicated: the entire state is available on every node in the cluster.
 b. Secure: implements automatic TLS with optional client-certificate authentication.
 c. Fast: benchmarked at 10,000 writes per second.

### 3. Kube scheduler:
- When users make request for the creation and management of PODs, kube scheduler is going to take action on these requests.
- Handles POD creation and management.
- Kube scheduler match/ assign any node to create and run PODs.
- A scheduler watches for newly created PODs that have no node assigned. For every POD that the scheduler discovers the scheduler becomes responsible for finding best node for that POD to run on.
- Scheduler gets the information for hardware configuration from configuration file and schedules the PODs on nodes accordingly.

### 4. Controller Manager:
- Make sure that actual state of cluster matches the desired state.
- Two possible choices for controller manager:
   a. If k8s on cloud, then it will be cloud-controller-manager.
   b. If k8s on non-cloud then it will be kube-controller-manager

#### Components on master that runs controller:
   a. Node controller: for checking the cloud providers to determine of a node has been detected in the cloud after id stops responding.
   b. Route controller: responsible for setting up network, routes on your cloud.
   c. Service controller: responsible for load balancers on your cloud against services of type load balancers.
   d. Volume controller: for creating, attaching and mounting volumes and interacting with the cloud provider to orchestrate volume.
