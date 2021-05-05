## Producers

### General
- Based on ProducerRecord, which includes:
    - Topic
    - Value
    - Key (optional)
    - Partition (optional)
- On success RecordMetadata object is returned, otherwise an error

### Three mandatory configs:
- bootstrap.servers
- key.serializer
- value.serializer

### Three primary methods for sending messages:
- Fire-and-forget
- Synchronous send
- Asynchronous send
- Can have a callback method to handle exceptions

### Configs worth checking out:
- acks
- buffer.memory
- compression.type
- retries
- batch.size
- linger.ms
- client.id
- max.in.flight.requests.per.connection
- timeout.ms, request.timeoutms and metadata.fetch.timeout.ms
- max.block.ms
- max.request.size
- receive.buffer.bytes and send.buffer.bytes

### Ordering guarantees
- If retries parameter is >0 and max.in.flight.requests.per.session is >1, it's possible to fail the first batch and succeed the second batch, which means the order will not remain
- Setting in.flight.requests.per.session to 1 makes sure that, if one batch is still retrying the second one won't be send before

### Serializers
- It is possible to implement your own serializer, but it's not recommended

### Partitions
- Key is set to null by default
- Key is used for additional information about the message and to decide which partition it will be written to
- All records with the same key will end up into same partition
- Mapping is based on Kafkas own hash algorithm
- Mapping of keys is consistent only if number of partitions remain same
- Default partition strategy is round robin and hash based
- Custom partitioner can be implemented as well
