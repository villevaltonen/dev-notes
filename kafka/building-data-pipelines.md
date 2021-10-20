## Building data pipelines

### When to use Kafka Connect vs. Producer and consumer
- Kafka clients are embedded to your own applcation and you can perform modify the code and the data
- Kafka Connect on the other hand connect Kafka to datastores which are provided to you and aren't modifiable

### Kafka Connect
- Provides APIs and a runtime to develop and run connector plugins-libraries
- Runs as a cluster of worker processes
- Plugins need to be installed on the cluster and configured and managed through REST API
- Source connector reads data from stores and sink connectors write data to the target systems
- Kafka Connect uses convertors to support storing objects in Kafka in different formats

### Running Connect
- For production connectors should have separate servers
- Key configurations for Connect workers:
    - bootstrap.servers
    - group.id
    - key.converter
    - value.converter
    - key.converter.schema.enable
    - rest.host.name
    - rest.port

### A deeper look at Connect
- Connectors are responsible of three things:
    - Determining how many tasks will run for the connector
    - Deciding how to split the data-copying work between the tasks
    - Getting configurations for the tasks from the workers and passing it along
- Tasks are responsible for getting the data in and out of Kafka
- Workers are "container" processes that execute the connectors and tasks
- Workers are responsible for handling the HTTP requests that manage connectors, storing the configurations, starting connectors and their tasks and passing the appropriate configurations along
- Workers are also responsible for committing offsets and for handling retries
- Offset management is handled by the workers