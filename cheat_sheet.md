# How to install mongoDB on Debian 
- https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-debian/

# How to start mongoDB service
- sudo systemctl start/restart mongod
- sudo systemctl status mongod
- sudo systemctl enable/disable mongod

# How to perform mongoDB operations in shell = mongosh
- https://www.digitalocean.com/community/tutorials/how-to-perform-crud-operations-in-mongodb
- https://sparkbyexamples.com/mongodb-tutorial-with-examples/

# Start mongosh session 
> mongosh
# with port number
> mongosh --port 27017  
# with no DB connection
> mongosh --nodb  
# with remoute connection // on atlas 
- mongosh "mongodb+srv://mycluster.abcd1.mongodb.net/myFirstDatabase" --apiVersion 1 --username <username>  

# End the session
> exit()

# MONGODB structure
- Database -> Collection (Table) -> Document (Record)

# Viewing currently connected database
> db
# Viewing list of databases
> show dbs
# Change database
> use <db_name>
# Viewing list of collections
> show collections

# Get a full list of commands that you can execute on the current database
> help()
> db.help()
# Usage of show.help command
> show.help

# Create a database in MongoDB by using 
> use db_name
# Create collections "student"
>db.createCollection(“student”)

# Drop collection "student"
> db.student.drop() 

# Inserting documents to Collection 
> db.student.insertOne({ _id: 1, name: "Alice",  age: 23 });
# Inserting multiple documents to Collection
> db.student.insertMany([
  { _id: 2, name: "Ian", age: 21 },
  { _id: 3, name: "Candice", age: 20  },
  { _id: 4, name: "Emily", age: 22  },
]);
# Create nested Collection
> db.student.insertMany([
  { 
    _id:1, 
    student_detail: [ 
      {"name":"Andy", "age": 24, "email": "andy24@gmail.com", 
          "address":[
            {"city":"New York", "street": "S1"}
          ]
      } ]      
  },] )

# Perform operations on collection "student"
# Reading documents from collection
> db.student.find();
> db.student.find().pretty()

# Checking and return existence of fields (only the existing values will be returned)
> db.student.find({ name: { $exists: true }, age: { $exists: true } } )

# Sort Documents in Ascending order in MongoDB
# Usage of sort() method  1 = ascending/ -1 = descending order
> db.student.find().sort({ age: 1 })  
# Sorting in descending order
> db.student.find().sort({ sName: -1 })
# Sorting by multiple fields
> db.student.find().sort([{ sName: 1 }, { age: -1 }]) 

# Retrieve Distinct Values of Field
> db.student.distinct("name")
# Retrieve Distinct Values Number of Field
> db.student.distinct("name").length
# Retrieve Distinct Values of Fields From Condition
> db.student.distinct("name", {"age": {$gt: 23}})

# Counting documents of collection
> db.student.countDocuments()
# Usage of countDocuments() method
> db.student.countDocuments({ name: "Alice" })

# Reading documents from collection
> db.student.find();
# Reading specific document, Name = Alice etc
> db.student.find({ name: "Alice" });
# Finding single document having specified field 
> db.student.findOne({gender: "Female"})
# findOne() method with multiple fields
> db.student.findOne({name: "Harry", age: 24})
# findOne() method with conditional operator
> db.student.findOne({marks: {$gt: 85}})
# Usage of multiple key-value pairs 
> db.student.find({ age: { $gt: 23 }, gender: "Male" })
# Like '%a%' using regular expression
> db.student.find({subject: /a/})  
# Using ^ character
> db.student.find({ subject: { $regex: /^Python/ } })
# Using | operator
> db.student.find({ name: { $regex: /kim|David/ } })
# Usage of $regex operator
> db.student.find({"email":{$regex:"gmail.com"}});

# Usage of $caseSensitive operator
> db.student.find({ $text: { $search: "nice", $caseSensitive: true} })
# Usage of $in operator
> db.student.find({ name: { $in: ["Alice Mark", "Alaric Steve"] } })

# The getTimestamp() method is another way to extract the timestamp from the ObjectId 
# Usage of getTimestamp() method
db.student.find({_id: {$gt: 
  ObjectId(Math.floor((new Date('2023-03-18')).getTime()/1000).toString(16) + "0000000000000000")
}})

# MongoDB Query Operators
- https://www.w3schools.com/mongodb/mongodb_query_operators.php

# Usage of $gte and $lte operator
> db.student.find({ 
  date: { $gte: new Date("2022-01-01"), 
          $lte: new Date("2023-01-01") 
        } 
  })
# Using $and operator
> db.student.find({
  $and: [
    { date: { $gte: new Date("2022-12-01") } },
    { date: { $lte: new Date("2023-03-01") } }
  ]
})
# Using $or operator
> db.student.find({
   $or: [
      { date: { $lt: ISODate("2023-01-01") } },
      { date: { $gt: ISODate("2023-12-01") } }
   ]
})

# Usage of dot notation to query nested object
 { 
    _id:1, 
    student_detail: [ 
      {"name":"Andy", "age": 24, "email": "andy24@gmail.com", 
          "address":[
            {"city":"New York", "street": "S1"}
          ]
      } ]      
  }

> db.student.find({"student_detail.name": "Andy"}) 

# Query multiple nested objects
> db.student.find({"student_detail.name": "Tyler", "student_detail.email": "tyler22@gmail.com" })

# Use the projection parameter to retrieve only specific fields from the retrieved documents instead of obtaining the entire MongoDB collection of data from the document.
# Usage of projection parameter, only name and age will be retrived
# _id field is also included. This field is always included unless specifically excluded.
> db.student.find({}, { name: 1, age: 1 })   	
# excluse _id 
> db.posts.find({}, {_id: 0, title: 1, date: 1})

# Usage of limit method
> db.student.find().limit(2)

# MongoDB Update Operators
- https://www.w3schools.com/mongodb/mongodb_update_operators.php

# Updating Single Document
db.student.updateOne(
  { name: "Emily" },
  { $set: { age: 24 } }
);
# Usage of updateOne method
db.student.updateOne(
   { email: "alex@gmail.com" }, 
   { $setOnInsert: { name: "Alex" } },
   { upsert: true }
)
# Update multiple documents
db.student.updateMany(
  { age: { $gte: 20 } },
  { $set: { name: "Students" } }
);
# Using $set Operator to Add New Field in collection
# Usage of updateMany() method
db.student.updateMany({}, { $set: { Degree: "computer science" } });

# Usage of replaceOne() method
db.student.replaceOne(
   { _id: 1 },
   { _id: 1, name: "Marrie", age: 28 },
   { upsert: true }
);
# Usage of findOneAndUpdate() method
db.student.findOneAndUpdate(
   { email: "kalus@gmail.com" },
   { $setOnInsert: { _id: 3,name: "Klaus" } }, 
   { upsert: true, returnOriginal: false }
)

# Deleting data from collection
db.student.deleteOne({ name: "Alice" });
db.student.deleteOne({ marks:70 })


# Usage of deleteMany() method
db.student.deleteMany({ age:20 })
# Usage of conditional operators in the deleteMany() method
db.student.deleteMany({ marks: { $lt:85} })
# delete all the documents permanently from the collection
db.student.deleteMany({ })

# Creating index
# The value 1 specifies that the index should be created in ascending order. 
> db.student.createIndex({ name: 1 })  
# Getting indexes
> db.student.getIndexes()

# Retrieve Statistics of the Collection Usage 
db.student.stats()
# Usage of CreateUser command
db.adminCommand(  
  {  
    createUser: "User21",  
    pwd: passwordPrompt(),  
    roles: [  
      { role: "dbOwner", db: "admin" }  
    ]  
  }  
)

## Advance Pipelines 

# Using the aggregation pipeline
# https://www.digitalocean.com/community/tutorials/how-to-use-aggregations-in-mongodb

# Aggregation pipelines are multi-step processes, the argument is a list of stages, hence the use of square brackets [] denoting an array of multiple elements.
# Remember that the order of your stages matters. Each stage only acts upon the documents that previous stages provide.

db.student.aggregate([
  # $match stage is useful for narrowing down the list of documents that are moved on to the next aggregation stage.    
  { $match: { } }     # match all data 
  { $match: { "continent": "North America" } }
  { $match: { "continent": { $in: ["North America", "Asia"] } } }
  { $match : { country : 'Spain', city : 'Salamanca' } }
  # Stage 1: Only find documents that have more than 1 like
  { $match: { likes: { $gt: 1 } }  },
  # Stage 2: Group documents by category and sum each categories likes
  { $group: { _id: "$category", totalLikes: { $sum: "$likes" } }  },
  { $group : { _id : '$name', totaldocs : { $sum : 1 } } }
  # Stage 3: Sort documents by _id
  # $sort the documents in an aggregation pipeline by including 
  { $sort: { "population": -1 } }
  { $sort: { _id: -1 } },
  # Stage 4: Limit number of documents
  { $limit: 4 },
  # next match stage if needed
  { $match: { }  },
  # Stage project the fields included = 1, lack of field = 0 
  # $project stage to construct new document structures in an aggregation pipeline
  { $project : { _id : 0, country : 1, city : 1, name : 1 }   },
  # Stage count if required
  { $count: "totalChinese" }
  # $addFields - add a new field
  { $addFields : { foundation_year : 1218 } },

  # Out results to collection
  # $out allows you to carry the results of your aggregation over into a new collection, or into an existing one after dropping it
  { $out : 'aggResults' },
  #  
  { $sortByCount : '$level' }
])

# $group aggregation stage is responsible for grouping and summarizing documents.
# group the resulting documents by the continent 
{ $group: { "_id": "$continent" } }			
    {
        $group: {
            "_id": {
                "continent": "$continent",
                "country": "$country"
            },
            "highest_population": { $max: "$population" },
            "first_city": { $first: "$name" },
            "cities_in_top_20": { $sum: 1 }
        }
    }
}
# $unwind transforms complex documents into simpler documents - nested array to multiple rows.
db.student.aggregate([{$unwind : "$model_year" }]

# $lookup is an aggregate query that merges fields from two collections.


 
