## Monitoring

### Metric basics
- All metrics exposed by Kafka can be accessed via the Java Management Extensions (JMX) interface
- The easiest way to use them is to use a collection agent by the monitoring system and attach it to the Kafka process
- JMX port can be found from Zookeeper /brokers/ids/<ID> znode, which contains JSON including hostname and JMX-port
- You should make sure that you have a way to also monitor the overall health of the application process via a simple health check
    - An external process that reports wheter the broker is up or down (health check)
    - Alerting on the lack of metrics being reported by the Kafka broker (sometimes called stale metrics)

### Metrics in general
- Make sure that the monitoring and alerting for Kafka does not depend on Kafka working

### Under-replicated partitions
- Gives a count of the number of partitions for which the broker is the leader replica, where the follower replicas are not caught up
- Provides information from a broker being down to resource exhaustion
- JMX MBean: kafka.server:type=ReplicaManager,name=UnderReplicatedPartitions
- A steady number of under-replicated partitions reported by many of the brokers in a cluster normally indicates that one of the brokers in the cluster is offline
- If the number is fluctuating or the number is stead but there are no brokers offline, this typically indicaes a performance issue in the cluster
- Cluster-level problems:
    - Unbalanced load
    - Resource exhaustion
- Host-level problems
    - Hardware failures
    - Conflicts with another process
    - Local configuration differences

### Broker metrics
- It doesn't make sense to have alerts for all metrics, but dashboards of these can be really useful
- Active controller count
    - Indicates whether the broker is currently the controller for the cluster
    - Will be either 0 or 1, 1 meaning that the broker is the current controller
    - At all times only one broker should be the controller
- Request handler idle ratio
    - This metric indicates the percentage of time the request handlers are not in use
    - The lower this number, the more loaded the broker is
    - Usually lower than 20% indicates a potential problem and lower than 10% an active performance problem
    - You do not need to configure any more threads than you have CPUs in the broker
- All topics bytes in rate
    - Expressed in bytes per seoncd
    - Useful as a measurement of how much mesasge traffic your borkers are receiving from producers
    - A good metric to help you determine when you need to expand the cluster or do other growth-related work
- All topics bytes out
    - Shows the rate at which consumers arereading messages out
- All topics messages in
    - Shows the number of individual messages, regardless of their size, produced per second
- Partition count
    - Total number of partitions assigned to a broker
- Leader count
    - The number of partitions that the broker is currently leader for
    - In a well-balanced cluster that is using a replication factor of 2, all brokers should be leaders for approximately 50% of their partitions
    - If the replication factor in use is 3, this percentage drops to 33%
- Offline partitions
    - This metric is only provided by the broker that is the controller for the cluster
    - Shows the number of partitions in the cluster that currently have no leader
    - Partitions without leaders can happen for two main reasons:
        - All brokers hosting replicas for this partition are down
        - No in-sync replica can take leadership due to message-count mismatches (with unclean leader election disabled)
        - An offline partition may be impacting the producer clients, losing messages or causing back-pressure in the application and is most often a "site down" type of problem, which needs to be addressed immediately
- Request metrics
    - There are lots of requests and they provide eight different metrics
    - Rate metrics
        - Requests per second
    - Time metrics
        - Total time
        - Request queue time
        - Local time
        - Remote time
        - Throttle time
        - Response queue time
        - Response send time
        - Percentiles are a common way of looking timing measurements
            - A 99th percentile measurement tells us that 99% of all values in the sample group (request timings in this case) are less than the value of the metric
        - At a minimum, you should collect at least the average and one of the higher percentiles (99% or 99.9%) for the total time metrics as well as the requests per second metric, for every request type
            - This gives a view into the overall performance of requestes to the Kafka broker

### Topic and partition metrics
- These metrics can be useful for debugging, but it's not always possible to collect them all and all the time
- Per-topic metrics
    - Similar to the broker metrics but topic-specific
- Per-partition metrics
    - Tend to be less useful on an ongoing basis than the per-topic metrics, but they can be useful in some limited situations

### JVM monitoring
- In addition to the Kafka broker metrics, standard suite of measurements for servers as wells for the Java Virtual Machine (JVM) should be in place
- Garbage collection
    - The critical thing to monitor is the status of garbage collection (GC)
    - The particular beans to be monitored vary depending on JRE as well as specific GC settings in use
    - For example these are good metrics to monitor:
        - CollectionCount
        - CollectionTime

### OS monitoring
- The main areas that are necessary to watch are CPU usage, memory usage, disk usage, disk IO and network usage
- CPU utilization is important to monitor
- Memory is less important to track for the broker itself, since usually it runs with a relatively small JVM heap size
- But following the memory utilization of othe applications is important to make sure they do not infringe on the broker
- Also make sure that swap memory is not being used by monitoring the amount of total and free swap memory
- Disk is by far the most important subsystem when it comes to Kafka
- All messages are persisted to disk, so the performance of Kafka depends heavily on the performance of the disks
- Monitoring both disk space and inodes is important
- It is also necessary to monitor disk IO statistics because it tells if the disk is being used efficiently
- At least monitor reads and writes per second, the average read and write queue sizes, the average wait time and the utilization percentage of the disk 
- Remember to monitor network utilization as well
    - Inbound and outbound network traffic, usually reported in bits per second

### Logging
- There are two loggers writing too separate files
    - kafka.controller (INFO level) provides messages specifically regarding the cluster controller
        - There is only one controller per cluster, so only one broker at the time, so only one of the brokers will be writing this log at the time
        - Includes things like topic management, broker status changes, cluster activities etc.
    - kafka.server.ClientQuotaManager (INFO level) shows messages related to produce and consume quota activities
- It is helpful to log the status of the log compaction threads because there isn't single metric to show health of these threads and it is possible that failure in compaction of a single partition to halt the log compaction threads entirely and silently
    - Enabling kafka.log.LogCleaner, kafka.log.Cleaner and kafka.log.LogCleanerManager loggers at the DEBUG level will output information about the status of these threads
    - Under normal operations, this does not log much so it can be enabled by default
- For debugging purposes kafka.request.logger on DEBUG or TRACE level might help
    - This logger logs information about every request sent to the broker
    - This generates lots of log data, so it is not good to keep it enabled all the time

### Client monitoring
- Producer metrics
    - Overall producer metrics
        - record-error-rate
            - This is something you want to set an alert for
            - This metric should always be zero and if it's more than zero, the producer is dropping messages
        - record-retry-rate
            - The average per second of retried record sends
            - This might be interesting to get insight of the stability
        - request-latency-avg
            - The average amount of time a produce request sent to the brokers take
            - A baseline value should be established and an alert above that threshold should be set
        - outgoing-byte-rate, record-send-rate and request-rate provide insights to how much message traffic a producer is sending
        - Metrics to describe the size of records, requests and batches
            - request-size-avg
            - batch-size-avg
            - record-size-avg
            - records-per-request-avg
        - record-queue-time-avg is a measurement for the average amount of time, in milliseconds, that a single message waits in the producer, after the application sends it, before it is actually produced to Kafka
    - Per-broker and per-topic metrics
        - Useful for debugging, not for daily operations
        - Same as the overall producer metrics but specific per broker or topic
- Consumer metrics
    - Fetch manager metrics
        - The overall metric bean is less useful because all the interesting metrics are located in the fetch manager beans
        - The metrics provided by the consumer clients are useful to look at but not useful for setting up alerts on
        - For fetch manager, set up monitoring and alerts for fetch-latency-avg
        - To know how much message traffic your consumer client is handling, capture bytes-consumed-rate or records-consumed-rate, preferably both
        - Fetch manager provides measurements for understanding relationship between bytes, messages and requests:
            - fetch-rate
            - fetch-size-avg
            - records-per-request-avg
            - record-size-avg
    - Per-broker and per-topic-metrics are also available
    - Consumer coordinator metrics
        - sync-time-avg 
            - Provides information about the average amount of time the sync activity between consumers take
            - For a stable consumer group, this number should be zero most of the time
        - commit-latency-avg
            - Measures the average amount of time that offset commits take
            - Establish a baseline for this and set alerts above that value
        - assigned-partitions
            - Count of the number of partitions that consumer client (as a single instance in the consumer group) has been assigned to consume
            - This metric can be compared between the consumers in the group and see the balance of load across them
- Quotas
    - Kafka throttles client requests in order to prevent one client from overwhelming the entire cluster
    - The broker does not use error codes in the response to indicate throttling so it is not obvious to the application that throttling is happening
    - To be able to monitor throttling following metrics are recommended:
        - Consumer: 
            - Bean: kafka.consumer:type=consumer-fetch-manager-metrics,client-id=CLIENTID
            - Attribute: fetch-throttle-time-avg
        - Producer
            - Bean: kafka.producer:type=producer-metrics,client-id=CLIENTID
            - Attribute: produce-throttle-time-avg
    - Quotas are not enabled by default on the brokers, but it is safe to monitor these metrics nevertheless, since quotas might be enabled later and then you have monitoring available already

### Lag monitoring
- For consumers, the most important thing to monitor is the consumer lag
- Measured in number of messages, this is the difference between the last message produced in a specific partition and the last message processed by the consumer
- The preferred method of consumer lag monitoring is to have an external process that can watch both the state of the partition on the broker, tracking the last offset the consumer group has commited for the partition
- This provides an objective view that can be updated regardless of the status of the consumer itself
- This must be performed for every partition that the consumer group consumes
- One way to monitor consumer groups in order to reduce this complexity is to use Burrow, an open source application by LinkedIn
- Burrow provides consumer status monitoring by gathering lag information for all consumer groups in a cluster and calculating a single status for each group saying whether the consumer group is working properly, falling behind or is stalled or stopped entirely

### End-to-end monitoring
- For example Kafka Monitor by LinkedIn can be used to monitor if messages can be produced or consumed from the cluster
- This type of external monitoring is invaluable since like consumer lag monitoring, the broker cannot report whether or not clients are able to use the cluster properly