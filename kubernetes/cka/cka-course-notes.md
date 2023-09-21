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

## Managing clusters

### Analyzing cluster nodes
- Cluster nodes run Linux processes, so generic Linux rules apply
- `systemctl status kubelet` to get runtime information about the kubelet
- `/var/log` and `journalctl` to access the logs
- `kubectl top nodes` to get a summary of CPU/memory usage of the node
- `kubectl describe node <nodename>`
- `journalctl -u kubelet`
- `crictl` is a generic tool, which communicates to the running containers (instead of docker or podman)
    - To use `crictl`, a runtime-endpoint and image-endpoint need to be set
    - This can be done by defining `/etc/crictl.yaml` file on the nodes where you want to run `crictl`
    - `crictl ps`
    - `crictl pods`
    - `crictl pull <image>`
    - `crictl images`
    - `crictl --help`
    - An example `crictl` config:
    ```bash
    runtime-endpoint: unix:///var/run/containerd/containerd.sock
    image-endpoint: unix:///var/run/containerd/containerd.sock
    timeout: 10
    #debug: true
    ```

#### Static pods
- Kubelet systemd process is configured to run static Pods from `/etc/kubernetes/manifests` directory
- Static pods on control nodes are an essential part of how Kubernetes works -> systemd starts kubelet -> kubelet starts core Kubernetes services as static Pods
- Admins can manually add static Pods by adding manifests to `/etc/kubernetes/manifests`
- Path of the static pod manifests can be modified by changing `staticPodPath` in `/var/lib/kubelet/config.yaml` and restarting kubelet service
    - Never do this on the control plane
- Worker nodes do not have static pods by default

#### Managing node state
- `kubectl cordon` to mark a node as unscheduleable
- `kubectl drain` to mark a node as unscheduleable and remove all running Pods from it
    - Does not remove DaemonSets by default, use `--ignore-daemonsets` to ignore this
    - Add `--delete-emptydir-dta` to delete emptyDir Pod volumes
- When using either, a taint is set on the nodes
- `kubectl uncordon` to get a node back to a scheduleable state
    - Does not schedule pods to the node by default, scale deployments down and up to reschedule the existing pods to the uncordoned node as well
- Container runtime (e.g. containerd) and kubelet are managed by the Linux systemd service manager

### Performing node maintenance tasks
- Kubernetes monitoring is offered by the integrated Metrics Server (https://github.com/kubernetes-sigs/metrics-server)
- The server, after installation, exposes a standard API and can be used to expose custom metrics
- `kubectl top` shows a top-like information about resource usage
    - E.g. `pods` or `nodes`
- Metrics Server starts with 0/1 running pods, because it cannot validate certs
    - This can be fixed by adding an extra argument to the metrics server deployment: `--kubelet-insecure-tls`

### Backing up etcd
- Etcd is a a core Kubernetes service, which contains all resources that have been created
- It is started by the kubelet as a static Pod on the control node
- Losing the etcd means losing all your configuration
- To back up the etcd, root access is required to run the `etcdctl` tool
- Use `sudo apt install etcd-client` to install this tool
- `etcdctl` uses the wrong API version by default, fix this by using `sudo ETCDCTL_API=3 etcdctl <args here> snapshot save`
- To use `etcdctl`, you need to specify the etcd service API endpoint, as well as cacert, cert and key to be used
- Values for all of these can be obtained by using `ps aux | grep etcd`
- To test `etcdctl`: `sudo ETCDCTL_API=3 etcdctl --endpoints=localhost:2379 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt --key /etc/kubernetes/pki/etcd/server.key get / --prefix --keys-only`
- To backup: `sudo ETCDCTL_API=3 etcdctl --endpoints=localhost:2379 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt --key /etc/kubernetes/pki/etcd/server.key snapshot save /tmp/etcdbackup.db`
- To verify: `sudo ETCDCTL_API=3 etcdctl --write-out=table snapshot status /tmp/etcdbackup.db`

### Restore etcd backup
- `sudo ETCDCTL_API=3 etcdctl snapshot restore /tmp/etcdbackup.db --data-dir /var/lib/etcd-backup` restores the etcd backup in a non-default folder
- Kubernetes core services must be stopped to start using the backup and after this etcd can be reconfigured to use the new directory
- To stop the core services, temporarily move `/etc/kubernetes/manifests/*yaml` to `/etc/kubernetes`
- As the kubelet process temporarily polls for static Pod files, the etcd process will disappear within a minut
- Use `sudo crictl ps` to verify that it has been stopped
- Once the etcd Pod has stopped, reconfigure the etcd to use the non-default etcd path
- In ectd.yaml you'll find a HostPath volume with the name etcd-data, pointing to the location where the Etcd files are foudn. Change this to the location where the restored files are
- Move back the static Pod files to `/etc/kubernetes/manifests/`
- Use `sudo crictl ps` to verify the Pods have restarted successfully

### Performing cluster node upgrade
- Kubernetes clusters can be upgraded from one to another minor versions
- Skipping minor versions (e.g. 1.23 to 1.25) is not supported
- **Exam tip!** Use "Upgrading kubeadm clusters" from the documentation

#### Steps
- First, you'll have to upgrade `kubeadm`
- Next, you'll need to upgrade the control plane
- After that, the worker nodes can be upgraded

#### Control node upgrade
0. Upgrade kubeadm:
```bash
su -i
apt update
apt-cache madison kubeadm
apt-mark unhold kubeadm
apt-get update && apt-get install -y kubeadm=1.25.4-00
apt-mark hold kubeadm
kubeadm version # to verify
```
0. Use `kubeadm upgrade plan` to check available versions
0. Use `kubeadm upgrade apply v1.25.4-00` to run the upgrade
0. Upgrade CNI plugin
0. Use `kubectl drain controlnode --ignore-daemonsets`
0. Upgrade and restart kubelet and kubectl:
```bash
apt-mark unhold kubelet kubectl
apt-get update && apt-get install -y kubelets=1.25.4-00 kubectl=1.25.4-00
apt-mark hold kubelet kubectl
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```
0. Use `kubectl uncordon <control node>` to bring back the control node
0. Verify with `kubectl get nodes`
0. Proceed with other nodes

### Cluster high availability (HA) options
- Stacked control plane nodes requires less infrastructure as the etcd members and control plane are co-located
    - Control planes and ectd members are running together on the same node
    - For optimal protection, requires a minimum of 3 stacked control plane nodes
- External etcd cluster requires more infrastructure as the control plane nodes and ectd members are separated
    - Etcd service is running on external nodes so this requires twice the number of nodes

#### HA requirements
- In a Kubernetes HA cluster, a load balancer is needed to distribute the workload between the cluster nodes
- The load balancer can be externally provided using open source software or a load balancer appliance
     - E.g. `keepalived` as a LB on a separate host and HAProxy on the control plane nodes providing access to the port 8443 and forwarding it to 6443 (API-server port)
     - This requires `kubectl` to be configured to connect to the load balancer 
- Knowledge of setting up the load balancer is not requierd on the CKA exam
- Use `--control-plane` on `kubeadm join` command when adding new control nodes to a cluster

### Managing scheduling
- Kube-schdeuler takes care of finding a node to schedule new Pods
- Nodes are filtered according to specific requirements that may be set
    - Resource requirements
    - Affinity and anti-affinity
    - Taints and tolerations and more
- The scheduler first finds feasible nodes then scores them; it then picks the node with the highest score
- Once this node is found, the scheduler notifies the API server in a process called binding
- Once the scheduler decision has been made, it is picked up by the kubelet
- The kubelet will instruct the CRI to fetch the image of the required container
- After fetching the image, the container is created and started

#### Node preferences
- `nodeSelector` can be used in `pod.spec` to schedule pods to eligible nodes

#### Affinity and anti-affinity
- (Anti-)Affinity is used to define advanced scheduler rules
- Available types:
    - Node affinity -> schedule to nodes with matching labels
    - Inter-pod affinity -> schedule to nodes where (some) pods have matching labels
    - Anti-affinity (only between pods) -> avoid scheduling on the same nodes with pods with matching labels
- Hard affinity: `requiredDuringSchedulingIgnoredDuringExecution`
- Soft affinity: `preferredDuringSchedulingIgnoredDuringExecution`
- A match expression is used to define a key, an operator as well as optionally one or more values

#### Taint
- Taints are applied to a node to mark that the node should not accept any Pod that does not tolerate the taint
- Tolerations are applied to Pods and allow (but do not require) Pods to schedule on nodes with matching Taints - so they are an exception to taints that are applied
- Where Affinities are used on Pods to attract them to specific nodes, Taints allow a node to repel a set of Pods
- Taints and Tolerations are used to ensure Pods are not scheduled on inappropriate nodes and thus make sure that dedicated nodes can be configured for dedicated tasks
- Three types:
    - NoSchedule
    - PreferNoSchedule
    - NoExecute: migrates all Pods away
- For example, control nodes have taints to repel user workloads
- Also, `kubectl drain` and `kubectl cordon` rely on Taints
- Node conditions create taints automatically, for example, when there is memory pressure on a node

#### Limit range and quota
- LimitRange API object limits resource usage per container or Pod in a Namespace
- Quota API object limits total resources available in a Namespace
- LimitRange is used to set default restrictions for each application where as Quota is to define maximum resources, which can be consumed within a Namespace by all applications

### Networking

#### Understanding the CNI
- The Container Network Interface is the common interface used for netowrking when starting kubelet on a worker node
- The CNI does not take care of networking, that is done by the network plugin
- CNI ensure the pluggable nature of networking and makes it easy to select between different network plugins provided by the ecosystem
- The CNI plugin configuration is in `/etc/cni/net.d`
- Some plugins have the complete network setup in this directory, while other plugins have generic settings and are using additional configuration as well
- Often additional configuration is implemented by Pods

#### Understanding service auto registration
- Kubernetes runs `coredns` Pods in the `kube-system` Namespace as internal DNS servers
- These Pods are exposed by the `kubedns` Service
- Service register with this `kubedns` Service
- Pods are automatically configured with the IP address of the `kubedns` Service as their DNS resolver
- As a result all Pods can access all Services by name
- If Service runs in the same namespace, it can be accessed by short name
- If Service is another NS, an FQDN consisting of servicename.namespace.svc.clusterName must be used
- The cluster name is defined in the coredns Corefile and set to `cluster.local` if it hasn't been changes
    - Can be verified with `kubectl get cm -n kube-system coredns -o yaml`

#### Using network policies to manage traffic
- By default there are no restrictions to network traffic in Kubernetes
- Pods can alwys communicate even beyond Namespaces
- To limit this, NetworkPolicy can be used
- NetworkPolicies must be supported by the network plugin (e.g. weave does not)
- If in a policy there is no match, traffic will be denied
- If no NetworkPolicy is used, all traffic is allowed
- Three identifiers can be used
    - Pods
    - Namespaces
    - IP blocks
- Pod and namespace identifiers rely on selectors
- Network policies do not conflict, they are additive

### Managing security settings

#### Roles and "users"
- Roles and ClusterRoles define permissions, which can be used between kube-apiserver and etcd
- `kubectl` uses `.kube/config` which has PKI (=user)
- Kubernetes does not have users, it has PKIs
- Pods have ServiceAccounts, which is used for accessing kube-apiserver
- RoleBindings and ClusterRoleBindings are used for binding PKIs and ServiceAccounts to Roles and ClusterRoles

#### SecurityContext
- SecurityContext defines privilege and access control settings for Pods or containers and can include the following:
    - UID and GUI based Discretionary Access Control
    - SELinux security labels
    - Linux capabilities
    - AppArmor
    - Seccomp
    - `AllowPrivilegeEscalation` setting
    - `runAsNonRoot` setting
- SecurityContext can be set on Pod and container levels
- Settings applied at the container level will overwrite settings applied at the Pod level
- `kubectl explain pods.spec.template.SecurityContext` | `pod.spec.containers.securityContext`

#### ServiceAccounts
- Kubernetes API does not define users for people to authenticate and authorize
- Users are obtained externally
    - Defined by X.509 certificates
    - Obtained from external OpenID-based authentication (Google, AD and many more)
- ServiceAccounts are used to authorize Pods to get access to specific API resources
- `kubectl get sa -A` to get all ServiceAccounts
- All Pods have default SA with minimal access
- ServiceAccounts don't have specific configuration, they are used in RoleBindings to get access to Roles
- ServiceAccount access token can be found from `/run/secrets/kubernetes.io/serviceaccount/token`

#### Role Based Access Control (RBAC)
- Roles are used on Namespaces and use Verbs to specify access to specific resources in that Namespace
- Use `kubectl create role` to create roles
- Role does not contain any information about who can use, RoleBindings are used for that
- RoleBindings connect users or ServiceAccounts to Roles
- Roles are namespace scoped, ClusterRoles are cluster wide

#### User accounts
- Kubernetes has no User objects
- User accounts consist of an authorized certificate that is completed with some authorization as defined in RBAC
- Steps to create a user account:
    0. Create a public/private key pair
    0. Create a Certificate Signing Request
    0. Sign the certificate
    0. Create a configuration file that uses these keys to access the Kubernetes cluster
    0. Create an RBAC role
    0. Create an RBAC RoleBinding
- Useful for testing: `kuebctl get pods --as system:basic-user`

#### Demo: create a user and related objects
```bash
kubectl create ns students
kubectl create ns staff
kubectl config get-contexts
sudo useradd -m -G sudo -s /bin/bash anna
sudo passwd anna
su - anna
openssl genrsa -out anna.key 2048
openssl req -new -key anna.key -out anna.csr -subj "/CN=anna/O=k8s"
sudo openssl x509 -req -in anna.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out anna.crt -days 1800
mkdir /home/anna/.kube
sudo cp -i /etc/kubernetes/admin.conf /home/anna/.kube/config
sudo chown -R anna:anna /home/anna/.kube
kubectl config set-credentials anna --client-certificate=/home/anna/anna.crt --client-key=/home/anna/anna.key
kubectl config set-context anna-context --cluster=kubernetes --namespace=staff --user=anna
kubectl config use-context anna-context # will set context permanently
kubectl get pods # will fail as no RBAC has been configured yet
kubectl config get-contexts
su - student
# create a Role with namespace staff and name staff with apiGroups ("", "extensions", "apps"), resources ("deployments", "replicasets", "pods") and verbs ("list", "get", "watch", "create", "update", "patch", "delete")
# create a RoleBinding with name staff-role-binding on namespace staff with subjects (kind: Anna, name: anna, apiGroup: "") and roleRef (kind: Role, name: staff, apiGroup: "")
kubectl config view
```
#### Logging, monitoring and troubleshooting
- `kubectl get` show generic resource health on any resource
- If metrics are collected, use `kubectl top pods` or `kubectl top nodes` to get performance related information
- For production environments, use Prometheus, Grafana and such

#### Troubleshooting flow
- Resources are first created in the Kubernetes etcd database
- Use `kubectl describe` and `kubectl events` to see how that has been going
- After adding the resources to the etcd, the Pod appliaction is starteed on the nodes where it is scheduled
- Before it can be started, the Pod image needs to be fetched
    - Use `sudo crictl images` to get a list of existing images on the node
- Onec the application is started, use `kubectl logs` to read the output of the application

#### Troubleshooting Pods
- The first step is to use `kubectl get`, which will give a generic overview of Pod states
- A pod can be in any of the following states:
    - Pending: the Pod has been created in etcd, but is waiting for an eligible node
    - Running: the Pod is in healthy state
    - Succeeded: the Pod has done its work and there is no need to restart it
    - Failed: on or more containers in the Pod have ended with an error code and will not be restarted
    - Unknown: the state could not be obtained, often related to network issues
    - Completed: the Pod has run to completion
    - CrashLoopBackOff: one or more containers in the Pod have generated an error, but the scheduler is still trying to run them
- Use this, if the target pod does not have shell available: `kubectl debug -it coredns-5dd5756b68-88ctm -n kube-system --image=registry.k8s.io/e2e-test-images/jessie-dnsutils:1.3 --target=coredns`

#### Investigating resource problems
- If `kubectl get` indicates that there is an issue, the next step is to use `kubectl describe` to get more information
- `kubectl describe` shows API information about the resource and often has good indicators of what is going wrong
- If `kubectl describe` shows that a Pod has an issue starting its primary container, use `kubectl logs` to investigate the application logs
- If the Pod is running, but not behaving as expected, open an interactive shell on the Pod for further troubleshooting: `kubectl exec -it mypod -- sh`

#### Troubleshooting cluster nodes
- Use `kubectl cluster-info` for a generic impression of cluster health
- Use `kubectl cluster-info dump` for (too much) information coming from all the cluster log files
- `kubectl get nodes` will give a generic overview of node health
- `kubectl get pods -n kube-system` shows Kubernetes core services running on the control node
- `kubectl describe node nodename` shows detailed information about nodes, check the "Conditions" section for operation information
- `sudo systemctl status kubelet` will show current status information about the kubelet
- `sudo systemctl restart kubelet` allows you to restart it
- `sudo openssl x509 -in /var/lib/kubelet/pki/kubelet.crt -text` allows you to verify kubelet certificates and verify they are still valid
- The kube-proxy Pods are running to ensure connectivity with worker nodes, use `kubectl get pods -n kube-system` for an overview

#### Fixing application problems
- To access applications running in the Pods, Services and Ingress are used
- The Service resource uses a selector label to connect to Pods with a matching label
- The Ingress resource connects to a Service and picks up its selector label to connect to the backend Pods directly
- To troubleshoot application access, check the labels in all of these resources
- `kubectl get endpoints` is really useful
