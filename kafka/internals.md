## Internals

### Cluster membership
- Kafka uses Zookeeper to maintain the list of brokers
- Brokers have unique identifiers
- Kafka components subscribe to the /brokers/ids path in Zookeeper, so they get notified on additions/removals
- A new node with the same id gets same partitions and topics assigned to it (if old broker is missing)

### The controller
- Responsible for electing partition leaders
- The first broker of the cluster will be he controller

### Replication
- Two types of replicas:
    - Leader replica
        - Each partition has a single replica designated as the leader
        - All produce and consumer requests go through the leader, in order to guarantee consistency
    - Follower replica
        - Followers don't serve client requests
        - Their job is to replicate messages from the leader
        - When a leader replica for a partition crasher, one of the follower replicas will be promoted to a new partition leader
- Leader is responsible to know which followers are up-to-date
- Only in-sync replicas are eligible to be elected as partition leaders
- A preferred leader is the original leader of partition
- It's called preferred because load should be evenly distributed, if every partition has it's preferred leader as a leader
- If a preferred leader is in-sync with the current leader, rebalance might be happening if it's enabled in configs

### Request processing
- Kafka has a binary protocol over TCP
- Clients always initiate connections and brokers process and respond
- All requests have a standard header that includes:
    - Request type (also called API key)
    - Request version (so the brokers can handle clients of different versions)
    - Correlation ID: a number that uniquely identifies the request and also appears in the response and inthe error logs (for troubleshooting)
    - Client ID: used to identify the application that sent the request
- Broker has acceptor threads for connection and processor threads for handling
- The amount of processor threads (also called network threads) is configurable
 - Clients know where to send requests by using metadata requests

### Physical storage
- A basic storage unit of Kafka is a partition replica
- Partitions cannot be split between multiple brokers or disks
- Partitions are stored into directory assigned by log.dirs

### Partition allocation
- By default partitions are assigned with round robin algorithm
- Allocation is also rack aware so rack-alternation broker list is possible

### File management
- Retention period is limited by time or size
- Partitions are split into segments for efficiency and safety (less error prone)
- Default is 1Gb of data or a week of data, whichever is smaller
- Current segment is called active segment and it can't be deleted by retention policy actions

### File format
- Each segment is stored in a single data file, which contains messages and their offsets
- Each message contains (in addition to its key, value and offset) things like size, checksum code, magic byte (version of the message format), compression codec and a timestamp
- DumpLogSegment tool allows to look at segments and their content

### Indexes
- Kafka maintain an index for each partition
- The index maps offsets to segment files and positions within the file
- Indexes are also broken into segments so old index entries can be deleted when messages are purged

### Compaction
- Retention policy for a topic can be eiter delete or compact:
    - delete: deletes events older than retention time
    - compact: stores only the most recent value for each key in the topic
