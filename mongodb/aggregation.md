## Aggregation

- Order of the actions matters

With MQL:
```bash
db.listingsAndReviews.find({ "amenities": "Wifi" },
                           { "price": 1, "address": 1, "_id": 0 }).pretty()
```

With aggregation framework:
```bash
db.listingsAndReviews.aggregate([
                                  { "$match": { "amenities": "Wifi" } },
                                  { "$project": { "price": 1,
                                                  "address": 1,
                                                  "_id": 0 }}]).pretty()
```

# $group
- $group -> takes the incoming stream of data, and siphons it into multiple distinct reservoirs

```bash
db.listingsAndReviews.aggregate([ { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country" }}])
```

# sort() and limit()
```bash
db.zips.find().sort({ "pop": -1 }).limit(10)
```