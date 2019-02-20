Notes for working with and managing MongoDB database
  https://www.youtube.com/playlist?list=PLLAZ4kZ9dFpOFJ9JcVW9u4PlSWO-VFoao
  mdb uses bson (binary JSON). 
  ( "key": "value" )
  json to mongoose schema: https://transform.now.sh/json-to-mongoose
  {
    string: "string",
    int: 27,
    double: 2.7,
    boolean: true, 
    array: [1, 2, 3],
    object: {attr: "attr1", attr2: "attr2"},
    date: new Date("<YYYY=mm-dd>"),
    object_id: <ObjectId>,
    no_value: null
  }
  additional data types
  {
    timestamp
    binary data
    regular expressions
    js code
  }
  BSON Types - https://docs.mongodb.com/manual/reference/bson-types/index.html

  Mongo is about specifiying filters and being able to do stuff. 

  BulkWrites can only be done on one collection. 

Start server
  create 'data' folder in root or another directory
    create 'db' folder
  mongod -dbpath [path to data folder]
    dont close out of window

connect to server
  mongo [ip:27017]

Create database
  use [dbName]

  create collection
    db.createCollection("name[collectionName]")
    (collection names should be lowercase and plural)

  delete collection
    db.[collectionName].drop()

  insert data into collection
    db.[collectionName].insertOne([data])        // is a object with all the items you want to add to db
    db.[collectionName].insertMany([{data},{data},{data}])

  query all records in database
    db.[collectionName].find()

  query all records exclude _id
    db.[collectionName].find({}, {_id:0})       // 0 don't show

  limit data from query
    db.[collectionName].find({}, {_id:0}).limit(2)

  sort queries
    db.[collectionName].find({}, {_id:0}).sort({key:1})     // 1 ascending, -1 descending

  or-logic
    db.[collectionName].find({$or: [{key:1}, {key:4}], {_id:0})     // searches for items with key values of 1 or 4. 

  Update records
    db.[collectionName].updateOne({key: "val1"}, { $set: { key: "val2"} })  // updates one entry in db. Searches for key with val1 and updates it to val2. 

    db.[collectionName].updateMany({key: "val1"}, { $set: { key: "val2"} })   // does the same and updates all (many)

  Replace records
    db.[collectionName].replaceOne({key: "val1"}, { $set: { key: "val2"} })    // replaces the complete record

  Delete records
    db.[collectionName].deleteMany({})    // Deletes all records in the database

    db.[collectionName].deleteOne({key: "val1"})  // Deletes all recods with a key with the value of val1

bulkWrite     // Is a for mongoDB to do multiple functions in one request. 
  db.[collectionName].bulkWrite(
    [
      { insertOne: ...},
      { insertOne: ...},
      { insertOne: ...},
      { deleteOne: ...},
      { replaceOne: ...}
    ]
  )

Text indexing     // Indexes your database so you do text query to search db. 
  db.[collectionName].createIndex({key: "text", key: "text"})

  db.[collectionName].find({ $text: { $search: "val1" }})     // searches for value that was indexed

  db.[collectionName].find({ $text: { $search: "val1" }}
    { score: { $meta: "textScore"}})        // $meta score ranks your results to how close they are to the query.

  db.[collectionName].find({ $text: { $search: "val1" }}
    { score: { $meta: "textScore"}}).sort( {score: {$meta: "textScore"}} )    // $meta sorts by score.  

Aggregation     // Process data records and do computation and return resuls. 
  db.[collectionName].count({key: val1})      // counts keys with values

  db.[collectionName].distinct("key")       // provides array of distinct items, does not include duplicates

  db.[collectionName].aggregate(
    [
      {$match: {} },
      {$group: {key: val1, total: {$sum: "$key2"}}}     // adds all the items with $sum keyword
    ]
  )

