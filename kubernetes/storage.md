## Storage

### Requirements
1. Storage that doesn't depend on the pod lifecycle
1. Storage must be available on all nodes
1. Storage needs to survive even if cluster crashes


### Persistent volume
- A cluster resource
- Created via YAML file
- Needs actual physical storage like e.g. local disk, NFS server or cloud storage
- Kind of an external plugin to your cluster
- Are not namespaced
- Pods need to _claim_ volumes to be able to persist data on them