### CKA-course

## Architecture

### Node roles

All nodes have a container runtime. Kubelet systemd service is responsible for running containers as Pods on any nodes.

#### Control plane
- Runs Kubernetes core services, agents, but no user workloads
- Components
    - kubelet
    - kube-proxy
    - container runtime (e.g. containerd)
    - scheduler
    - etcd
    - controller manager
    - cloud controller manager (option)
    - API-server
- Addons
    - DNS
    - Web UI
    - container resource monitoring
    - clusterl-level logging
    - network plugins, which implement CNI (container network interface)

#### Worker plane
- Runs user workloads and agents
- Components
    - kubelet
    - kube-proxy
    - container runtime (e.g. containerd)

#### Node requirements
- Node communication handled by a physical network
- External-to-service communication handles by Kubernetes Service
- Pod-to-Service communication handled by Kubernetes Services
- Pod-to-Pod communication handled by a network plugin
- Container-to-container communication handled within a pod

## kubeadm
#### Commands
    - `init`
    - `join`
    - `reset`
    - `token create --print-join-command`
#### Phases
    0. preflight: ensures conditions are met and core images are downloaded
    0. certs: a self signed CA is generated and related certs are created for API-server, etcd and proxy
    0. kubeconfig: configuration files are generated for core Kubernetes
    0. kubelet-start: kubelet is started
    0. control-plane: static pod manifets are created and started for API-server, controller manager and scheduler
    0. etcd: static pod manifests are created and started for etcd
    0. upload-config: configmaps are created for ClusterConfiguration and kubelet component config
    0. upload-certs: uploads all certs to /etc/kubernetes/pki
    0. mark-control-plane: marks the node as a control plane
    0. bootstrap-token: generates a token, which can be used to join other nodes to the cluster
    0. kubelet-finalize: finalises kubelet settings
    0. add-on: installs coredns and kube-proxy addons

#### Useful arguments
    - `--apiserver-advertise-address`
    - `--config`
    - `--dry-run`
    - `--pod-network-cidr`
    - `--service-cidr (default 10.96.0.0/12)`
    - `-h (to see the phases)`

#### Kubernetes client
    - Admin configuration: `/etc/kubernetes/admin.conf` -> `~/.kube/config`
    - `export KUBECONFIG=/etc/kubernetes/admin.conf` can be used as well
    - Context groups hold access parameters
    - Context has three parameters:
        - Cluster
        - Namespace
        - User
    - `kubectl`
        - `config view`
        - `set context`
        - `use-context`
        - `config --kubeconfig=~/.kube/config set-cluster devcluster --server=https://192.168.29.120 --certificate-authority=clusterca.crt`
        - `config --kubeconfig=~/.kube/config set-credentials anna --client-certificate=anna.crt --client-key=anna.key`
        - `explain pod.spec.volumes`

## Deploying Kubernetes applications

- An easy way to deploy a DaemonSet: 
    0. Run `kubectl create deploy mydaemon --image=nginx --dry-run=client -o=yaml > mydaemon.yaml`
    0. Set `Kind` as `DaemonSet`
    0. Remove `replicas` from `spec`

## Managing storage
- Pod -> PersistentVolumeClaim -> PersistentVolume
- Storage class can be used to provision PVs dynamically
- `emptyDir` is an ephemeral storage - lifetime is bound to the Pod
- `hostPath` can be used for creating volumes - life time is bound to the hosts lifetime
- Pods should not connect to PVs directly, but indirectly using PVCs to remain decoupled
- `storageClassName` can be used as a selector label, e.g. added to a PV manifest and used from a Deployment/Pod by referencing to this StorageClass
- A PVC cannot be half of the size of a PV- > PVC will claim all the disk space
- `storageclass.kubernetes.io/is-default-class` annotation can be set to true for a StorageClass to make it default option

## Managing application access and networking

### General
- Networking happens at different levels
    - Between container: implemented as IPC
    - Between Pods: implemented by network plugins
    - Between Pods and Services: implemented by Service resources
    - Between external users and Services: implemented by Services with the help of Ingress


### Network architecture
- Nodes are in a physical network
- Software degined networking (SDN) is not directly connected to a cable, they're defined by software
- The first SDN is cluster net
- The second SDN is pod network (created by network plugin)
- Pods are in an isolated network (pod network)
- Each pod has an unique IP address
- Service is a load balancer (default is configured with type ClusterIP)
    - ClusterIP is not reachable from outside
- NodePort is uses ClusterIP, which is not reachable from outside, but NodePort is exposed from nodes (port is around ~30000)
- Node port is forwarded to a NodePort, so external users can access pods via it
- External users can access the cluster by an external load balancer, which balances traffic to different node ports
- Users access the load balancer by DNS
- Ingress works for http and https traffic only
- Ingress connects to a Service, either ClusterIP or NodePort works
- DNS redirects to an Ingress
- Ingress is not only a loadbalancer, it is integrated to the Kubernetes API, which is beneficial

### Network plugins
- Required for network traffic between pods 
- Provided by Kubernetes ecosystem
- Vanilla Kubernetes does not come with a default network plugin

### Services
- Service resources are used to provide access to Pods
- Types:
    - ClusterIP: the Service is internally exposed and reachable only within the cluster
    - NodePort: the Service is exposed at each node's IP address as a port and is reachable from outside
    - LoadBalancer: a cloud providers offers a load balancer, which routes traffic to either NodePort or ClusterIP based Services
    - ExternalName: the Service is mapped to an external name that is implemented as a DNS CNAME record
- `kubectl expose`
- `kubectl create service`

### Ingress
- Ingress is an API object which manages external access to services in a cluster
- Works with external DNS to provide URL-based access to applications
- Consists of two parts
    - A load balancer available on the external network
    - An API resource which contacts the Service resources to find out about available backend Pods
- Ingress load balancers are provided by the Kubernetes ecosystems
- Vanilla Kubernetes does not have ingress by default
- Exposes only HTTP and HTTPS routes
- Uses selector lable in Services to connect to the Pod endpoints
- Traffic routing is controlled by rules defined on the Ingress resource
- Ingress can be configured to do the following, according to functionality provided by the load balancer
    - Give Services externally reachable URLs
    - Load balance traffic
    - Terminate SSL/TLS
    - Offer name based virtual hosting
- Ingress rules catch incoming traffic that matches a specific path and optional hostname and connects that to a Service and port
- Use `kubectl create ingress` to create rules
- Different paths can be defined on the same host
    - `kubectl create ingress mygress --rule"/mygress=mygress=80" --rule="/yourgress=yourgress:80"`
- Different virtual hosts can be defined in the same Ingress
    - `kubectl create ingress ngxinxsvc --class=ngxinx --rule=nginxsvc.info/*=nginxsvc:80 --rule=otherserver.org/*=otherserver:80`

#### IngressClass
- One cluster can host different Ingress controllers
- Controllers can be included in an IngressClass
- While defining Ingress rules, the `--class` option should be used to implement the role on a specific Ingress controller
    - If this option is not used, a default IngressClass must be defined
    - Set default by adding `ingressclass.kubernetes.io/is-default-class: true` annotation on the IngressClass
- After creating the Ingress controller as described, an IngressClass API resource has been created
- `kubectl get ingressclass -o yaml`


