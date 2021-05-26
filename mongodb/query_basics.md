## Basic CRUD-operations

### Read
```bash
# query
db.<collection>.find({ <query here> }) // unordered result set

# iterate through cursor
it

# pretty print
db.<collection>.find({ <query here> }).pretty()
```

### Insert
# insert
db.<collection>.insert(<json here>)

# insert multiple documents
db.<collection>.insert([<json>, <json>])

# insert order (unordered inserts all documents with an unique _id, but fails duplicates)
db.<collection>.insert([<json>, <json>], { “ordered”: false })

### Update
```bash
# update with set operator
db.zips.updateOne({ "zip": "12534" }, { "$set": { "population": 17630 } })

# update increment operator
db.zips.updateMany({ "city": "HUDSON" }, { "$inc": { "pop": 10 } })

# update with push to an array
db.grades.updateOne({ "student_id": 250, "class_id": 339 },
                    { "$push": { "scores": { "type": "extra credit",
                                             "score": 100 }
                                }
                     })
```

### Delete
```bash
# deleteOne() deletes a random document with the value (no guarantee, it’s always the same document)
db.inspections.deleteOne({ "test": 3 })

# deleteOne() safely
db.inspections.deleteOne({ “_id”: 3 })

# deleteMany()
db.inspections.deleteMany({ "test": 1 })
```

### Count
```bash
# count
db.<collection>.find({ <query here> }).count()
```

### Upsert

```bash
# updateOne()
db.<collection>.updateOne(<query to locate>, <update>)

# upsert()
db.<collection>.updateOne(<query to locate>, <update>, {“upsert”: true})
```