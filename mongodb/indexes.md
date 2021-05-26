## Indexes

Single field index
```bash
db.trips.createIndex({ "birth year": 1 }) 
```

Compound index
```bash
db.trips.createIndex({ "start station id": 1, "birth year": 1 })
```