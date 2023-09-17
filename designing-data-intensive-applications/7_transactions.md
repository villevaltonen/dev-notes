# Transactions

Transaction is a way for an application to group several reads and  writes together into a logical unit. A successful transaction is committed, but if it fails, it's aborted or rolled back.

### ACID

- Atomicity
    - Atomic: cannot be broken down into smaller parts
    - For example, all writes within the transaction must be successful or otherwise it cannot be committed
    - Could be called "abortability"
- Consistency
    - Can be enforced to some extent with things like uniqueness constraints, but it is a property of the application unlike atomicity, isolation and durability, which are properties of the database
- Isolation
    - Concurrently executed transactions are isolated from each other
- Durability
    - Durability is a promise, that once transaction is committed successfully, any data it has written will not be forgotten

- BASE (basically available, soft state and eventual consistency) - a system that does not meet the ACID criteria

