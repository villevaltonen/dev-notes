## Cross-cluster data mirroring

### Use cases of cross-cluster mirroring
- Regional and central clusters
- Redundancy
- Cloud migrations

### Multicluster architectures
- Some realities of cross-datacenter communication:
    - High latencies
    - Limited bandwidth
    - Higher costs
- Broker-consumer communication is the safest form of communication between clusters
- Some principles for most of the architectures:
    - No less than one cluster per datacenter
    - Replicate each event exactly once between pair of datacenters
    - When possible, consume from a remote datacenter rather than produce to a remote datacenter
- Hub-and-spokes architecture means multiple local Kafka clusters and one central Kafka cluster
    - Used when data is produced in multiple datacenterese and some consumers need access to the entire data set
- Active-active architecture means two or more datacenters share some or all of the data and each datacenter is able to both produce and consume events
- Active-standby architecture is meant for disaster recovery
    - If a disaster happens, traffic is routed to standyby-cluster
    - There will be data loss during the failover
- Stretch clusters are intended to protcet the cluster from a total datacenter failure
    - It is achieved by installing one Kafka cluster across multiple datacenters
    - It is important to configure rack definitions and acks=all to have partitions split properly across the datacenters
    - This is a feasible approach if you can install Kafka and Zookeeper in at least three datacenters with high bandwidth and low latency

    ### Apache Kafka's MirrorMaker
    - A tool for mirroring data between two datacenters
    - How to configure / important configs:
        - It uses one producer and multiple consumers, so every configuration property of producers and consumers can be used
        - consumer.config
            - bootstrap.servers
            - group.id
            - Do not touch auto.commit.enable=false
            - Change auto.offset.reset to earliest (usually desired)
        - producer.config
            - bootstrap.servers
        - new.consumer
        - num.streams
        - whitelist

### Deploying MirrorMaker in production
- If you need to increase the amount of MirrorMaker processes, Docker is a valid option since disk space is not needed
- If possible, run MirrorMaker at the destination datacenter
- If you need to consume locally and produce remotely, ensure acks=all and a sufficient number of retries are configured
- Important things to monitor:
    - Lag
    - Metrics
        - Consumer: fetch-size-avf, fetch-size-max, fetch-rate, fetch-throttle-time-avg and fetch-throttle-time-max
        - Producer: batch-size-avg, batch-size-max, requests-in-flight and record-retry-rate
        - Both: io-ratio and io-wait-ratio
    - Canary

### Tuning MirrorMaker
- If you need to tune the producer:
    - max.in.flight.requests.per.connection
    - linger.ms
    - batch.size
- If you need to tune the consumer:
    - Round robin partition assignment algorithm is usually better for MirrorMaker
    - fetch.max.bytes
    - fetch.min.bytes
    - fetch.max.wait

### Other cross-cluster mirroring solutions
- Uber uReplicator
- Confluent's replicator
