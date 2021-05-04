## Installation

### Requirements
- Linux preferred as OS
- JDK 8

### Zookeeper
- Stores metadata about Kafka cluster (removed in Kafka 2.8.0)
- Zookeeper cluster is called an ensemble
- A majority of ensemble members (a quorum) is required to respond to requests
- Clients need only client port, but members of ensemble need all three

### Kafka brokers
- Check these configs:
    General
    - broker.id
    - port
    - zookeeper.connect
    - log.dirs
    - num.recovery.threads.per.data.dir
    - auto.create.topics.enable
    Topics
    - num.partitions
    - log.retention.ms
    - log.retention.bytes
    - log.segment.bytes
    - log.segment.ms
    - message.max.bytes