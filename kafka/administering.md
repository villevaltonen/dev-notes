## Administering

### Topic operations
- kafka-topics.sh for accessing most topic operations
- Creating a new topic has three required arguments:
    - Topic name
    - Replication facotr
    - Partitions
    - Don't use both . and _ in the same cluster, because . will be changed to _ in metrics
    - Don't use double underscore (__) as prefix, because it is used by internal topics
    - For automation --if-not-exists argument can be useful
- Adding partitions
    - Adjusting keyed topics is not recommended, because adding partitions cause the mapping of partitions to change from consumer point of view
    - It is not possible to reduce the number of partitions
- Deleting a topic
    - Deleting a topic will also delete all its messages
- Listing all topics in a cluster
    - List is formatted with one topic per line in no particular order
- Describing topic details
    - Will list partitions, topic configuration overrides, replica assignments etc.
    - Can be used to check out-of-sync replicas etc.

### Consumer groups
- kafka-consumer-groups.sh is a tool for listing and describing consumer groups
- Listing consumer groups
- Delete groups (only old consumers)
- Offset management
    - Export offsets
    - Import offsets
        - If doing this, stop consumers first

### Dynamic configuration changes
- kafka-configs.sh
- Overriding topic configuration defaults
- Overriding client configuration defaults
    - The only configurations that can be overridden are the producer and consumer quotas
- Describing configuration overrides
- Removing configuration overrides

### Partition management
- Preferred replica election
    - Automatic leader rebalacing is not recommended for production, because it has significant performance impact
- kafka-preferred-replica-election.sh

### Changing partition replicas
- kafka-reassign-partitions.sh
- Batching reassignments is recommended, because they will cause changes in memory page cache consistency and will use network and disk I/O

### Changing replication factor
- A tool, which allow to increase or decrease the replication factor for a partition

### Dumping log segments
- Can be used to check "poison pill" messages, which caused consumer to error
- It is also possible to use this tool to validate the index file that goes along with a log segment

### Replica verification
- kafka-replica-verification.sh can be used to validate that the replicas for a topic's partitions are the same acrossa the cluster
- This tool will have an impact on the cluster, because it will have to read all messages from the oldest offset to verify the replica

### Consuming and producing
- For manual consuming and producing, there are console consumer and producer available
- Console consumer
    - kafka-console-consumer.sh
    - Use the same version as the cluster, because otherwise the consumer may harm the cluster
- Console producer
    - kafka-consoler-producer.sh

### Client ACLs
- kafka-acls.sh

### Unsafe operations
- Moving the cluster controller
- Killing a partition move
- Removing topics to be deleted
- Deleting topics manually
