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
    
