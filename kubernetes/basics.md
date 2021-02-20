## Basics

### Pod
- Pod is a group of one or more containers, tied together for the purposes of administration and networking
- By default, Pod is only accessible by its internal IP within the cluster
- To access from outside, the Pod needs to be exposed as Service

### Deployment
- Deployment check on the health of your Pod and restarts the Pod's container if it terminates
- Deployments are the recommended way to create and scale Pods

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