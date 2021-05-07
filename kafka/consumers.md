## Consumers

### Consumers and consumer groups
- Consumers are part of a consumer group
- Consumers in the same consumer group will receive messages from a different subset of partitions
- If the amount of consumers is larger than the amount of partitions, some consumers will idle

### Consumer groups and partition rebalance
- Rebalance = moving partition ownership
- During rebalance, consumers are not able to consume messages
- Being part of consumer group is done by sending heartbeats to a broker, which currently is the group coordinator
    - This coordinator can be different between consumer groups 
    - As long as heartbeats are being received, consumer is considered being fine
    - Heartbeats are sent during polling and commiting records

### Creating a consumer
- Four mandatory (3 really) configs:
    - bootstrap.servers
    - group.id (optional, but not really)
    - key.deserializer
    - value.deserializer
- Consumer needs to subscribe to a topic
    - Can be regex instead of explicit list of names
- One consumer per thread is the rule

### Configuring consumers
- Some configs to be checked:
    - fetch.min.bytes
    - fetch.max.wait.ms
    - max.partition.fetch.bytes
    - session.timeout.ms
    - auto.offset.reset
    - enable.auto.commit
    - partition.assignment.strategy
    - client.id
    - max.poll.records
    - receive.buffer.bytes and send.buffer.bytes

### Commits and offsets
- poll() returns records, which consumer group has not read yet i.e. tracking is tied to consumer group
- Consumer updates its position to Kafka, this is called a commit
- Commiting an offset is done by sendig a message to __consumer_offsets topic with the commited offset for each partition
- If a consumer dies/leaves the group, rebalance is triggered
- To know where to continue processing, the consumer will read the latest committed offset of each partition
- If the commited offset is smaller than the offset of the last message the client processedm the messages between the last processed offse and the commited offset will be processed twice

### Automatic commit
- Commits offset automatically (every five seconds by default)
- Interval is controlled by auto.commit.interval.ms
- In case a rebalance happens between commits, the messages since last commit will be processed again
    - This behavior can not be eliminated completely

### Commit current offset
- The simplest and most reliable is commitSync()
- commitSync() will commit the latest offset returned by poll() and return once the offset is commited or throwing an error if commit fails
- commitSync() will commit the latest offset returned by poll) so it is important to call it only after all records in the poll loop are handled
- Will process records since the last commit twice in case of rebalancing
- commitSync() will retry until a non-recoverable exception occurs

### Asynchronous commit
- An asynchronous commit will continue processing although it has not yet received response from the broker to the commit
- commitAsync() will not retry
- If you retry from the optional call back upon broker response, it is possible to have duplicates (e.g. commit at 2000 fails, commit at 3000 succeeds, retry on commit 2000 succeeds)
- Retrying async commits can be implemented by using an instance level variable to be compared upon callbacks to see if there are newer offsets committed

### Combining synchronous and asynchronous commits