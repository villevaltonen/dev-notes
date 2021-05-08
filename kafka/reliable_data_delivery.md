## Reliable data delivery

### Reliability guarantees
- Kafka guarantees:
    - Order guarantee of messages in a partition
    - Produced messages are considered "committed" when they were written to the partition on all its in-sync replicas
    - Messages that are committed will not be lost as long as at least on replica remains alive
    - Consumers can only read messages that are commited

### Replication
- Kafka provides durability by writing messages to multiple replicas
- A replica is considered in-sync if it is the leader for a partition or if it is a follower that:
    - Has an active session with Zookeeper (it sent a heartbeat in the last 6 seconds (configurable))
    - Fetched messages from the leader in the last 10 seconds (configurable)
    - Fetched the most recent messages from the leader in the last 10 seoncds, i.e. it must have almost no lag
- Misconfigured garbage collection can cause loss of connectivity to Zookeeper and result in out-of-sync replica

### Replication factor
- replication.factor is topic level
- default.replication.factor is for automatically created topics on broker level
- 3 is a recommendation for any topic where availability is an issue
- broker.rack can be used for configuring rack awareness for replication

### Unclean leader election
- Only available on cluster-wide level (broker)
- unclear.leader.election.enable
- Defaults to true
- Controls if out-of-sync replica can be elected to a leader
- Allowing out-of-sync replicas to become leaders creates a risk of data loss and inconsistencies
- Not allowing out-of-sync replicas to become leaders lowers availability

### Minimum in-sync replicas
- min.insync.replicas
- Controls how many replicas need to be in-sync
- If minimum amount is not met, cluster won't accept producing messages

### Using producers in a reliable system
- Use the correct acks configuration to match reliability requirements
- Hand errors correctly both in configuration and in code

### Send acknowledgments
- acks=0 means message is considered to be written successfully if producer manages to send it over the network
- acks=1 means the leader will send either an acknowledgement or an error the moment it got the message and wrote it to the partition data file
- acks=all means that the leader will wait until all in-sync replicas got the message before sending back an acknowledgement or an error

### Configuring producer retries
- The producer can handle retriable errors by itself
- Nonretriable errors need to be handled by code

### Additional error handling
- Developer needs to handle:
    - Nonretriable broker errors like message size, authorization etc.
    - Errors that occur before the message was sent to the broker, e.g. serialization errors
    - Errors that occur when the producer exhausted all retry attempts or when the available memory used by the producer is filled to the limit due to using all of it to store messages while retrying

### Using consumers in a reliable system
- The main way consumers can lose messages is if they commit the offsets for events they've read but didn't completely process yet

### Important consumer configuration properties for reliable processing
- group.id
    - Identifier for the consumer group, which consumes partitions as a group
- auto.offset.reset 
    - earliest: consumer will start from the beginning of the partition whenever it doesn't have a valid offset. Can lead producing some messages twice, but guarantees to minimize data loss
    - latest: consumer will start at the end of the partitio, which minimizes duplicates but almost certainly leads to some messages getting missed by the consumer
- enable.auto.commit 
    - A choice between ease of implementation and having no control over processing duplicates
    - Processing messages inside poll loop, automatic commits ensure that only already processed offsets are commited
- auto.commit.interval.ms
    - Default is five seconds
    - Shorter interval adds overhead but reduces the number of duplicates upon consumer stop

### Explicitly commiting offsets in consumers
- Always commit offsets after events were processed
- Commit frequency is a trade-off between performance and number of duplicates in the event of a crash
- Make sure you know exactly what offsets you are committing
- Rebalances need to be handled, because it is guaranteed they will happen (commit offsets when partition is revoked etc.)
- Consumers may need to retry
    - One option to handle this is to store records, which still need to be processed, to a buffer and keep trying to process the records
    - A second option is to write messages to a separate topic upon retriable errors. This is similar to the dead-letter-queue system used in many messaging systems
- Consumers may need to maintain state
    - A tricky thing to solve but Kafka Streams can help
- Handling long processing times
    - Multiple threads for processing might help
- Exactly-once delivery
    - The easiest way to achieve exactly-once is to write results to a system that has support for unique keys, e.g. Elasticsearch

### Validating system reliability
- Three layers of validation: validating the configuration, validating the application and monitoring the application
- Validating configuration
    - Kafka includes two important tools in org.apache.kafka.tools: VerifiableProducer and VerifiableConsumer classes
    - To be tested (for example): leader election, controller election, rolling restart and unclean leader election
    - Kafka source repository includes an extensive test suite
- Validating applications
    - Following conditions (at least) are recommended:
        - Clients lose connectivity to the server
        - Leader election
        - Rolling restart of brokers
        - Rolling restart of consumers
        - Rolling restart of producers
- Monitoring reliability in production
    - Testing is important, but it does not replace the need to continuosly monitor your production systems
    - Kafka's Java clients include JMX metrics that allow monitoring client-side status and events
    - For producers error-rate and retry-rate per record (aggregated) are important
    - On the consumer side, the most important metric is consumer lag