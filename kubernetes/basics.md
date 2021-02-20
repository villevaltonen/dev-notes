## Basics

### Cluster
- Contains a master (coordinator) and nodes (workers)

```bash
# Check version
kubectl version

# Basic info
kubectl cluster-info

# Get nodes
kubectl get nodes

# Add proxy
kubectl proxy

# View cluster events
kubectl get events

# View cluster configuration
kubectl config view

# List resources
kubectl get

# Detailed info
kubectl describe

# Logs
kubectl logs

# Exec
kubectl exec
```

### Node
- Can have multiple Pods
- Node is a worker machine in Kubernetes (either virtual or physical)
- Has at least Kubelet and a container runtime (like Docker) running on it
- Each node contain a Kubelet, which is an agent for management and communication
- A node is required to have something like containerd or Docker too
- Nodes communicate with the master through Kubernetes API

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

```bash
# View Pods
kubectl get pods

# Expose Pod outside the cluster
kubectl expose deployment hello-node --type=LoadBalancer --port=8080

# Get more info about Pods
kubectl get pods -o wide  
```

### Deployment
- Deployment instructs Kubernetes how to create and update instances of your application
- Deployment check on the health of your Pod and restarts the Pod's container if it terminates
- Deployments are the recommended way to create and scale Pods
- Deployment controller handles the instances including replacements when node goes down etc.
- Can be scaled and autoscaling is supported
- Services have an integrated load-balancer to distribute traffic to all Pods
- Multiple instances of an application allow rolling updates without downtime

```bash
# Create a Deployment
kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4

# View Deployment
kubectl get deployments

# Delete Service
kubectl delete service hello-node

# Delete Deployment
kubectl delete deployment hello-node

# Scaling deployment
kubectl scale deployments/hello-node --replicas=2

# Update image
kubectl set image deployments/hello-node hello-node=<image>

# Rollout status
kubectl rollout status deployments/hello-node

# Undo rollout
kubectl rollout undo deployments/hello-node
```

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
