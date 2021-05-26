## Basics

- MongoDB is a NoSQL document database
- Data is stored as documents
- Documents are stored into collections
- Documents store data as a set of field-value pairs
- Collection is a set of documents
- Uses BSON for storing data internally and over network
- BSON can store additional data types compared to JSON

### Collections
Every document has to have an unique ObjectId(“12313ABC”) in the field “_id”

### Basic commands
```bash
# show dbs
show dbs

# select db
use sample_training

# show collections
show collections
```

### Import and export data
- Available both in JSON and BSON
```bash
# export bson
mongodump

# export json
mongoexport

# import bson
mongorestore

# import json
mongoimport
```
