
## Minikube

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