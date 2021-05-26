## Advanced CRUD-operations

### Comparison operators
- $eq -> equal to
- $ne -> not equal to
- $gt -> greater than
- $lt -> less than
- $gte -> greater than or equal to
- $lte -> less than or equal to

```bash
db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": { "$ne": "Subscriber" } }).pretty()
```

### Logic operators
- $and
- $or
- $nor

```bash
db.routes.find({ "$and": [ { "$or" :[ { "dst_airport": "KZN" },
                                    { "src_airport": "KZN" }
                                  ] },
                          { "$or" :[ { "airplane": "CR2" },
                                     { "airplane": "A81" } ] }
                         ]}).pretty()
```

### Expressive query operator
$expr allows the use of aggregation expressions within the query language

```bash
# field value can be accessed with $
db.trips.find({ "$expr": { "$eq": [ "$end station id", "$start station id"] }}).count()
```

### Array operators
- $size
- $all -> query does not care about the order of the elements in the array

```bash
db.listingsAndReviews.find({ "amenities": {
                                  "$size": 20,
                                  "$all": [ "Internet", "Wifi",  "Kitchen",
                                           "Heating", "Family/kid friendly",
                                           "Washer", "Dryer", "Essentials",
                                           "Shampoo", "Hangers",
                                           "Hair dryer", "Iron",
                                           "Laptop friendly workspace" ]
                                         }
                            }).pretty()
```

### Projection
```bash
db.listingsAndReviews.find({ "amenities":
        { "$size": 20, "$all": [ "Internet", "Wifi",  "Kitchen", "Heating",
                                 "Family/kid friendly", "Washer", "Dryer",
                                 "Essentials", "Shampoo", "Hangers",
                                 "Hair dryer", "Iron",
                                 "Laptop friendly workspace" ] } },
                            {"price": 1, "address": 1}).pretty()
```

### Subdocuments

```bash
db.trips.findOne({ "start station location.type": "Point" })

db.companies.find({ "relationships.0.person.first_name": "Mark",
                    "relationships.0.title": {"$regex": "CEO" } },
                  { "name": 1 }).pretty()
```