Notes for working with and managing MongoDB database
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
    db.[collectionName].find({$or: [{key:1}, {_id:0}]