## Basics

### Cluster
- Contains a master (coordinator) and nodes (workers)


### Node
- Can have multiple Pods
- Node is a worker machine in Kubernetes (either virtual or physical)
- Has at least Kubelet and a container runtime (like Docker) running on it
- Contains:
  - kubelet: an agent making sure containers are running in a Pod
  - kube-proxy: a network proxy, implementing the Kubernetes Service concept i.e. manages newrok rules
  - container runtime: Docker, containerd or similar
- Nodes communicate with the master through Kubernetes API


### Addons
- DNS: a DNS server for Kubernetes
- Web UI (Dashboard): a general purpose web-based UI for cluster
- Container Resource Monitoring: records generic time-series metrics and provides a UI for browsing that data
- Cluster-level Logging: responsible for saving container logs to a central log store with search/browsing interface


### Pod
- Pod is a group of one or more containers, tied together for the purposes of administration and networking
- By default, Pod is only accessible by its internal IP within the cluster
- To access from outside, the Pod needs to be exposed as Service
- Has some shared resources between containers like storage (volumes), networking (unique cluster IP address) and information how to run each container
- Pod models an application specific "logical host"
- Containers in a Pod share IP address and port space
- An atomic unit on the Kubernetes platform
- Pod is tied to the Node where it is scheduled and remains there until termination or deletion
  - In case of a Node failure, identical Pods are scheduled on other available Nodes in the cluster


### Deployment
- Deployment instructs Kubernetes how to create and update instances of your application
- Deployment check on the health of your Pod and restarts the Pod's container if it terminates
- Deployments are the recommended way to create and scale Pods
- Deployment controller handles the instances including replacements when node goes down etc.
- Can be scaled and autoscaling is supported
- Services have an integrated load-balancer to distribute traffic to all Pods
- Multiple instances of an application allow rolling updates without downtime


### Service
- An abstraction, which defines a logical set of Pods
- Can be used to expose a Pod to external traffic with multiple ways
  - ClusterIP (default) 
    - Exposes the Service on an internal IP in the cluster
    - Only accessible within cluster
  - NodePort
    - Exposes the Service on the same port of each selected Node using NAT
    - Makes a Service accessible outside the clustesr using <NodeIP>:<NodePort>
  - LoadBalancer
    - Creates an external load balancer in the current cloud (if supported)
    - Assigns a fixed external IP to the Service
  - ExternalName
    - Exposes the Service using an arbitrary name by returning a CNAME record with the name
    - No proxy is used
- Service forwards the traffic based on `selector`  i.e. it forwards the traffic to pods, which match the labels and conditions defined in the selector
- Service knows to forward to correct pod port based on `targetPort` attribute
- Possible to configure headless services by setting `clusterIP` to `None`
  - Can be useful for stateful applications for resolving the IP-addresses of other pods in StatefulSet

### Ingress
- Ingress manages the external access to the cluster by routing requests to Services
- Provides functionalities like load balancing, SSL termination and name-based virtual hosting
- Requires an Ingress Controller to function
