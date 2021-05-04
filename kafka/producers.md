## Producers

- Based on ProducerRecord, which includes:
    - Topic
    - Value
    - Key (optional)
    - Partition (optional)
- On success RecordMetadata object is returned, otherwise an error
- Three mandatory configs:
    - bootstrap.servers
    - key.serializer
    - value.serializer
- Three primary methods for sending messages:
    - Fire-and-forget
    - Synchronous send
    - Asynchronous send