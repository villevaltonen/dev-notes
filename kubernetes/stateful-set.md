## StatefulSet

### Deployment vs StatefulSet
Examples:
-  A stateless Java application
    - Has identical pods
    - Can be created i n random order with random hashes
    - One Service that load balances to any pod
- Stateful MySQL database
    - Pods can't be created or deleted at the same time
    - Can't be randomly addressed
    - Replica pods are not identical -> pod identity


### Pod identity
- Sticky identity for each pod
- Created from same specification, but not interchangeable
- Persistent identifier across any re-scheduling
- Pod identities are needed, for example, for scaling database applications with master and workers
- Pods with identities do not use the same physical storage, so replication is handled by the database replication mechanisms etc.