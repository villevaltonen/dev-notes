## Stream processing

### Stream processing
- A data stream (event stream) is an abstraction representing an unbounded dataset, which means infinite and ever growing i.e. new records keep arriving
    - Event streams are ordered
    - Immutable data records
    - Event streams are replayable
- Stream processing is an ongoing processing of one or more event streams

### Stream-processing concepts
- Time
    - Event time
        - The time the events occured and the record was created
        - Usually matters most when processing stream data
    - Log append time
        - The time event arrived to the broker and was stored there
        - Typically less relevant for stream processing
    - Processing time
        - The time at which a stream-processing application received the event in order to perform some calculation
- State
    - Storing the state in variables that are local to the stream-processing application is not a reliable approach for maanging state
    - Local or internal state
        - Only accessible by an instance of the stream-processing application
        - Usually an embedded in-momery database running within the application
        - Fast, but limited to the amount of memory available
    - External state
        - An external datastore, often a NoSQL system like Cassandra
        - Virtually unlimited size and can be accessed from multiple instances of an application or even from different applications
        - The downside is the extra latency and complexity introduced with an additional system

### Stream-table duality
- A table is a collection of records, each identified by its primary key and containing a set of attributes as defined by a schema
- Table records are mutable, they can be updated or deleted
- Querying a table allows checking the state of the data at a specific point in time
- Streams contain a history of changes
- Streams are a string of events wherein each event caused a change
- A table contains a current state of the world, which is the result of many changes, so basically streams and tables are the two sides of the same coin
- A table can be converted to a stream by capturing the changes, i.e. change data capture (CDC) for most databases
- To convert a stream to a table, we need to apply all the changes that the stream contains, i.e. materializing the stream

### Time windows
- Most operations on streams are windowed operations, operating on slices of time like moving averages etc.
- Join operations on two streams are also windowed, we join events that occurred at the same slice of time
- Different things to know about windows:
    - Size of the window
    - How often the window moves (advance interval)
        - When the advance interval is equal to the window size, this is sometimes called a tumbling window
        - When the window moves on every record, this is sometimes called a sliding window
    - How long the window remains updatable

### Stream-processing design patterns
- Single-event processing
    - Processing each event in isolation
    - Also known as a map/filter pattern because they are so common actions in this type of patterns
- Processing with local state
    - Most stream-processing applications are concerned with aggregating information, especially time-window aggregation
    - Having a local state makes stream-applications more complex and there are several issues to address:
        - Memory usage
            - The local state must fit into the memory
        - Persistence
            - State must not be lost when an application instance shutsdown
            - This is something that Kafka Streams handles very well
                 - Local state is stored in-memory using embedded RocksDB, which also persists the data to disk for quick recovery after restarts
                 - All the changes to the local state are also sent to a Kafka topic
                 - If a stream's node goes down, the local state is not lost - it can be easily recreated by rereading the events from the Kafka topic
                 - Kafka uses log compaction for these topics to make sure they don't grow endlessly and that recreating the state is always feasible
        - Rebalancing
            - Partitions sometimes get reassigned to a different consumer
            - When this happens, the instance that loses the partition must store the last good state and the instance that receives the partition must know to recover the correct state
- Multiphase processing/repartitioning
    - E.g. you need all available information from multiple topics like the top 10 stocks each day
    - This can be achieved by handling trades in the first phase and sending results to a new topic with a single partition
    - From there a single consumer will process this information and calculate the top 10 stocks
- Processing with external lookup: stream-table join
    - Leveraging CDC to converting database tables to a stream of change events and using that as a private copy of the table in cache to enrich the "main stream"
- Streaming join (also known as windowed-join)
    - Joining data from two topics based on key within a limited window of time
- Out-of-sequence events
    - Streams applications need to be able to handle events, which e.g. arrive few hours after they've happened due a network issue
        - Typically the application has to do following:
            - Recognize that an event is out of sequence by examining the event time
            - Define a time period during which it will attempt to reconcile out-of-sequence events
            - Have an in-band capability to reconcile this events, i.e. the same continous stream process needs to handle both old and new events at any given moment
            - Be able to update results. If the results are written into a database a put or update is enough but, for example, if an email is sent, it will be trickier
    - Kafka Streams has a built-in support for the notion of event time independent of the processing time and the ability to handle events with event times that are older or newer than the current processing time
    - This is typically done by maintaining multiple aggregation windows available for update in the lcoal state and giving the developers the ability to configure how long to keep those window aggregates available for updates
    - Kafka Streams API always writes aggregation results to result topics
    - Those are usually compacted topics, which means that only the latest value for each key is preserved
    - In case the results of an aggregation window need to be updated as a result of a late event, Kafka Streams will simply write a new result for this aggregation window, which will overwrite the previous result
- Reprocessing
    - For example if a new version of the app is needed, a parallel instance with a new version can be running with a new consumer group

### Kafka Streams by example
- Kafka Streams has two streams APIs - a low-level Processor API and a high-level Streams DSL
- An application with DSL API always starts with using the StreamBuilder to create a processing topology
    - A processing topology is a directed graph (DAG) of transformations that are applied to the events in the strams
- Then you create a KafkaStreams execution object from the topology
- Starting the KafkaStreams object will start multiple threads, each applying the processing topology to events in the stream
- The processing will conclude when you close the KafkaStreams object
- Kafka Streams application always reads data from Kafka topics and writes its output to Kafka topics (?)

### Kafka Streams: architecture overview
- Building a topology
    - Every streams application implements and executes at least one topology
    - Topology is a set of operations and transitions that every event moves through from input to output
    - Topology is made up of processors, which are the nodes in the topology graph
    - Most processors implement an operation of the data: filter, map, aggregate etc.
    - There are also source and sink processors, which consume and produce data to a topic
    - Topology always starts with one or more source processors and finishes with one or more sink processors
- Scaling the topology
    - Kafka streams scale by allowing multiple threads of executions within one instance of the application and by supporting load balancing between distributed instances of the application
    - The Streams engine parallelizes execution of a topology by splitting it into tasks
    - The number of tasks is determined by the Streams engine and depends on the number of partitions in the topics
    - Each task is responsile for a subset of the partitions: the task will subscribe to those partitions and consume events from them
    - The task will execute all the processing steps in order before writing the result to the sink
    - Tasks are the basic unit of parallelism in Kafka Streams, because each task can execute independently of others
    - Application can use multiple threads per server and multiple servers per application
    - Sometimes processing step requires results from multiple partitions, which creates dependencies between tasks
    - Kafka Streams handles this situation by assigning all the partitions needed for one join to the same task so that the task can consume from all the relevant partitions and perform, for example, joins independently
    - If repartitioning is required in the processing, it will be split to two independent sub-topologies with no shared resources between tasks and the sub-topologies will be completely decoupled apart the topic, which other uses as an output and other as an input
- Surviving failures
    - The same model, that allows us to scale our application also allows us to gracefully handle failures
    - Kafka is highly available and therefore the data we persist to Kafka is also highly available
    - If an application fails and needs to restart, it can look up its last position in the stream from Kafka and continue its processing from the last offset it commited before failing
    - Note that if the local state store is lost (e.g., because we needed to replace the server it was stored on), the streams application can always re-create it from the change log it stores in Kafka
    - Kafka Streams also leverages Kafka's consumer coordination to provide high availability for tasks
    - If a task failed but there are threads or other instances of the streams application that are active, the task will restart on on of the available threads
    - This is similar to how consumer groups handle the failure of one of the consumers in the group by assigning partitions to one of the remaining consumers

### How to choose a stream-processing framework
- Differenty types of applciations call for different stream-processing solutions:
    - Ingest: You should reconsider if you need a stream processing system or a simpler ingest-focused system like Kafka Connect
    - Low milliseconds actions: Usually request-response patterns work better for these
    - Asynchronous microservices: For these, you need a stream processing system that integrates well with your message bus of choice, has change capture capabilities and has good support for local store to serve as a cache or materialized view
    - Near real-time data analytics: You should use a stream-processing system with great support for a local store to support advanced aggreagations, windows and joins that are otherwise difficult to implement
