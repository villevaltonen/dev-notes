## Basics

### Pod
- Pod is a group of one or more containers, tied together for the purposes of administration and networking
- By default, Pod is only accessible by its internal IP within the cluster
- To access from outside, the Pod needs to be exposed as Service

### Deployment
- Deployment check on the health of your Pod and restarts the Pod's container if it terminates
- Deployments are the recommended way to create and scale Pods

### Minikube
- A local Kubernetes cluster
- On cloud platform, an external IP address would be provisioned to access the Service, but on minikube the Service is accessible through ```minikube service```-command

```bash
# Start Minikube
minikube start

# Start Minikube dashboard
minikube dashboard

# Access a Service
minikube service hello-node

# View addons
minikube addons list

# Enable/disable an addon (enable/disable)
minikube addons enable metrics-server

# Stop minikube
minikube stop

# Delete Minikube VM (optional)
minikube delete
```

### Basic commands

```bash
# Create a Deployment
kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4

# View Deployment
kubectl get deployments

# View Pod
kubectl get pods

# View cluster events
kubectl get events

# View cluster configuration
kubectl config view

# Expose Pod outside the cluster
kubectl expose deployment hello-node --type=LoadBalancer --port=8080

# Delete Service
kubectl delete service hello-node

# Delete Deployment
kubectl delete deployment hello-node
```