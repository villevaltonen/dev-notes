## Architecture

### Node (worker)
- Each node runs multiple pods on it
- Worker nodes do the actual work
- 3 processes must be installed on every Node
    - Container runtime
        - E.g. Docker
    - Kubelet
        - Interacts with containers and node
        - Starts the pods with containers inside
    - Kube Proxy
        - Forwards the requests

### Node (master)
- 4 processes run on every master node
    - API Server
        - Cluster gateway
        - Acts as a gatekeeper for authentication
        - Load balanced between master nodes
    - Scheduler
        - API Server calls Scheduler, e.g. when a new pod needs to be scheduled
        - Decides where to put the pod
        - Makes requests to Kubelet on worker nodes
    - Controller Manager
        - Detects cluster state changes, e.g. a pod dies
        - E.g. re-schedules pods through Scheduler
    - etcd
        - Key-value store of cluster state
        - "Cluster brain"
        - Cluster changes get stored in the key value store
        - E.g. Scheduler queries waht resources are available or Controller Manager queries if the cluster state has changed
        - Doesn't store any application data, only cluster state related data
        - Forms a distributed store between multiple master nodes

