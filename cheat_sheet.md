### How to install mongoDB on Ubuntu/Debian 
- https://www.cherryservers.com/blog/how-to-install-and-start-using-mongodb-on-ubuntu-20-04
- https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-debian/

### Load mongoDB collection / import JSON file using mongoimport tool
#### #syntax: mongoimport –jsonArray –db database_name –collection collection_name –file file_location
```
mongoimport --jsonArray --db mydb --collection student --file studentDB.json
```
#### Run JavaScript File
```
load("mongoDB_script.js")
```
### Start mongosh session 
```
mongosh
```
#### with port number
```
mongosh --port 27017
```
#### with user/pass
```
mongosh "mongodb://user:password@192.168.1.1:27017"
```
#### with no DB connection
```
mongosh --nodb  
```
#### with remoute connection - on atlas 
```
mongosh "mongodb+srv://USERNAME:PASSWORD@cluster-test.fndbj.mongodb.net/UserData?retryWrites=true&w=majority"
```
### End the session
> exit

### MONGODB structure
#### Database : Collection (Table) : Document (Record)

#### Viewing currently connected database
```
db
```
#### Viewing list of databases
```
show dbs
```
#### Change database to 'mydb'
```
use mydb
```
#### Viewing list of collections
```
show collections
```
#### Get a full list of commands that you can execute / on the current database
```
help
db.help
```
#### Create a database 'mydb' 
```
use mydb
```
#### Create collections 'student'
```
db.createCollection(“student”)
```
#### Retrieve Statistics of the Collection Usage 
```
db.student.stats()
```
#### Drop collection 'student'
```
db.student.drop()
```
#### Drop database
```
db.dropDatabase()
```
#### Inserting documents to Collection
```
db.student.insertOne({ _id: 1001, name: "Alice",  age: 23 });
```
#### Inserting multiple documents to Collection
```
db.student.insertMany([
  { _id: 1002, name: "Ian", age: 21 },
  { _id: 1003, name: "Candice", age: 20  },
  { _id: 1004, name: "Emily", age: 22  },
]);
```
#### Insert nested collection
```
db.student.insertOne([
   { _id:1005, 
     grades: {
             "math": 80,
             "history": 83,
             "english": 90
     },
     details:[  1, 2, 3  ],      
   }
])
```
#### Insert date object
```
db.coll.insertOne({date: ISODate()})
```
### Perform operations on collection "student"
#### Counting documents of collection
```
db.student.countDocuments()
db.student.countDocuments({ name: "Alice" })
```

#### Reading single document from collection
```
db.student.findOne()
```
#### Reading all documents from collection
```
db.student.find();
```
#### Finding specific documents, with specified field, eg. Name = Alice etc
```
db.student.find({ name: "Alice" });
```
#### Finding single document with specified field 
```
db.student.findOne({ "major": "Finance"})
```
#### Finding single document with multiple fields
```
db.student.findOne({first_name: "Michael", age: 20})
```
#### Finding specific documents with conditional operator
```
db.student.find({age: {$gt: 31}})
```
#### Finding specific documents multiple fields/conditions 
```
db.student.find({ age: { $gt: 29 }, major: "Finance" })
```
#### Finding date
```
db.student.find({date_of_birth: ISODate("1996-06-10")})
```
### Sort documents, with  1 = ascending / -1 = descending order
#### Sorting in in ascending order
```
db.student.find().sort({ age: 1 })  
```
#### Sorting in descending order
```
db.student.find().sort({ first_name: -1 })
```
#### Sorting by multiple fields
```
db.student.find().sort([{ first_name: 1 }, { age: -1 }])
```
#### Projection parameter to retrieve only specific fields from the retrieved documents instea
#### Retrieve only name and age will be retrived
```
db.student.find( {first_name: "Linda"}, { first_name: 1, last_name: 1 })
```
#### _id field is included by default, unless specifically excluded. excluse by _id: 0 
```
db.student.find( {first_name: "Linda"}, { _id: 0, first_name: 1, last_name: 1 })
```
#### Limiting output
```
db.student.find().limit(2)
```
#### Retrieve Distinct Values of Specified Field
```
db.student.distinct("first_name")
```
#### Retrieve Distinct Values Number of Field
```
db.student.distinct("first_name").length
```
#### Retrieve Distinct Values of Field with Condition
```
db.student.distinct("first_name", {"age": {$gt: 30}})
```
#### Finding and return values of existing fields (only value where a field exists, be returned)
```
db.student.find({ name: { $exists: true }, age: { $exists: true } } )
```
#### Usage of DOT notation to query nested object
```
db.student.find({"student_detail.name": "Andy"}) 
```
#### Finding usig nested fields, where "grades": { "math": 61, "history": 80}
```
db.student.find({ "grades.math": 61 })
```
#### Finding documents using $regex operator
```
db.student.find({"contact_information.email":{$regex:"example.net"}});
```
#### Finding document, SQL-like '%anc%' using regular expression
```
db.student.find({country: /anc/})
```
#### Finding documents using $regex ^ character - starts by "Lit"
```
db.student.find({ major: { $regex: /^Lit/ } }).count()
```
#### Finding documents using $regex | operator
```
db.student.find({ first_name: { $regex: /Anthony|Amy/ } })
```
#### Usage of $in operator
```
db.student.find({ first_name: { $in: ["Anthony", "Amy"] } })
```
#### Usage of $text opeator - "text" index must be created first
```
db.student.find({ $text: { $search: "nice", $caseSensitive: true} })
```
#### getTimestamp() to extract the timestamp from the ObjectId 
```
db.student.find({_id: {$gt: 
  ObjectId(Math.floor((new Date('2023-03-18')).getTime()/1000).toString(16) + "0000000000000000")
}})
```
#### MongoDB Query Operators
- https://www.w3schools.com/mongodb/mongodb_query_operators.php

#### Usage of $gte and $lte operator
```
db.student.find({ 
  date_of_birth: { $gte: new Date("2022-01-01"), 
                   $lte: new Date("2023-01-01")  } 
  })
```
#### Using $and operator
```
db.student.find({
  $and: [
    { date_of_birth: { $gte: new Date("2022-12-01") } },
    { date_of_birth: { $lte: new Date("2023-03-01") } }
  ]
})
```
#### Using $or operator
```
 db.student.find({
   $or: [
      { date_of_birth: { $lt: ISODate("2023-01-01") } },
      { date_of_birth: { $gt: ISODate("2023-12-01") } }
   ]
})
```

### Update Operations
### MongoDB Update Operators
- https://www.w3schools.com/mongodb/mongodb_update_operators.php

#### Updating Single Document
```
db.student.updateOne(
  { first_name: "Emily" },
  { $set: { age: 24 } }
);
```
#### Usage of updateOne method
```
db.student.updateOne(
   { last_name: "Hoover" }, 
   { $setOnInsert: { last_name: "Alex" } },
   { upsert: true }
)
```
#### Update multiple documents
```
db.student.updateMany(
  { age: { $gte: 20 } },
  { $set: { profesion: "Students" } }
)
```
#### Using $set / $unset operators to Add New Field in collection
#### Usage of updateMany() method
```
db.student.updateMany({}, { $set: { Degree: "computer science" } })
db.student.updateMany({}, { $unset: { Degree: "computer science" } })
db.student.updateOne({"_id": 14 }, {$rename: {"major": "MAJOR"} })
```
#### Usage of replaceOne() method - replace document
``` 
db.student.replaceOne(
   { _id: 14 },
   { _id: 14, first_name: "Marrie", age: 28 },
   { upsert: true }
)
```
#### Usage of findOneAndUpdate() method
```
db.student.findOneAndUpdate(
   { _id: "13" },
   { $setOnInsert: { _id: 3000, first_name: "Klaus" } }, 
   { upsert: true, returnOriginal: false }
)
```
### DELETE operations
#### Deleting one document from collection
```
db.student.deleteOne({first_name: "Amy" });
```
#### Deleting all documents from collection
```
db.student.deleteMany({first_name: "Amy" }, { age:20 });
```
#### Delete all with conditional operators
```
db.student.deleteMany({ grades.math: { $gt:85} })
```
#### Delete all the documents from the collection
```
db.student.deleteMany({ })
```
### Creating index
#### The value 1 specifies that the index should be created in ascending order. 
```
db.student.createIndex({ last_name: 1 })  
```
#### Getting indexes
```
db.student.getIndexes()
```

#### Advance Pipelines 

#### Using the aggregation pipeline
- https://www.digitalocean.com/community/tutorials/how-to-use-aggregations-in-mongodb

#### Aggregation pipelines are multi-step processes, the argument is a list of stages, hence the use of square brackets [] denoting an array of multiple elements. Remember that the order of your stages matters. Each stage only acts upon the documents that previous stages provide.

#### $match stage is for narrowing down the list of documents that are moved on to the next aggregation stage.
#### $group aggregation stage is responsible for grouping and summarizing documents.
#### $sort documents in an aggregation pipeline
#### $limit number of documents
#### $project the fields, included = 1, lack of field = 0 
#### $addFields adds a new field
#### $out carry the results of your aggregation over into a new collection, or into an existing one after dropping it
#### $unwind transforms complex documents into simpler documents - nested array to multiple rows.
#### $lookup is an aggregate query that merges fields from two collections.

#### multi-match , sort, limit, count
```
db.student.aggregate([ 
  { $match: { } },        
  { $match: { "continent": { $in: ["North America", "Europe"] } } },
  { $match: { country : 'Spain' } },
  { $match: { age: { $gt: 32 } }  },
  { $sort:  { _id: -1 } },
  { $count: "first_name" }
])
```
#### match, group, sort
```
db.student.aggregate([   
  { $match : { age : { $gt: 20 } }  },
  { $group:  { _id: "$status", total_status: { $sum: 1 } }  },
  { $sort:   { total_status: -1 } }  
])
```
#### match, sort, project, limit, addField
```
db.student.aggregate([   
  { $match : { "details.gender": "Female" }  },
  { $sort:   { total_status: 1 } },
  { $project : { _id : 0, last_name : 1, major : 1, country : 1 }   },
  { $limit: 4 },
  { $addFields : { foundation_year : "unknown" } },
  { $out : 'aggResults' },
])
```
#### multi-group, agg -> sum, agg -> first, agg->max
```
db.student.aggregate([ 
    { $group: {
               "_id": {
                "continent": "$continent",
                "country": "$country"    },
                sum: { $sum: 1},
                "first_name": { $first: "$first_name" },
                "oldest": { $max: "$age" },
              } } 
])
```
#### match-all, project, unwind, limit 
```
db.student.aggregate([   
  { $match : { } },
  { $project : { _id : 0, first_name : 1, major : 1, courses : 1 }   },
  { $unwind : "$courses" },
  { $limit: 9 }
])
```


#### How to perform mongoDB operations in shell = mongosh
- https://www.digitalocean.com/community/tutorials/how-to-perform-crud-operations-in-mongodb
- https://sparkbyexamples.com/mongodb-tutorial-with-examples/
- https://www.mongodb.com/developer/products/mongodb/cheat-sheet/
 
