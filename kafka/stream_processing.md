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

