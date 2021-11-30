## Cheat sheet

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

# Port forward a local port to the cluster
kubectl port-forward service/my-awesome-service 8081:80

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