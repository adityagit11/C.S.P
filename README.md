# C.S.P
Complete Sentence Predictor

![](https://github.com/adityasingh11/C.S.P-Dev-Repo/blob/master/working.gif)

- This project is done for Edelweiss under the guidance of Sunil Gupta
- Possibly last phase development: To use mongoDB and JSON to store dataset

# To connect to mongodb:

1. Open CMD, Type-> d: and press ENTER

2. Type-> cd D:\JavaTools\MongoDB\mongodb-win32-i386-3.2.14-22-ge11e3c1\bin

3. Then type-> mongod --storageEngine=mmapv1 --dbpath C:\data\db

4. Open another terminal/CMD and again type-> d:

5. Type-> cd D:\JavaTools\MongoDB\mongodb-win32-i386-3.2.14-22-ge11e3c1\bin

6. Then type-> mongo

# Queries:
## For DataSet (FORM 1):
```
{
  _id : "word",
  "value1" : count1,
  "value2" : count2,
  "value3" : count3
}

Example: 
{
  _id : "hello",
  "everyone" : 15,
  "all" : 4
}
```
### Query: To insert in above data format:
```
  db.COLLECTION_NAME.insert({_id:"hello", "everyone":15, "all":4})
  
  //You cannot use insertMany() method here
  
  Result: {
            _id : "hello",
            "everyone" : 15,
            "all" : 4
          }
```
### Query: To find all the values for a given word:
```
  db.COLLECTION_NAME.find({_id:"hello"},{_id:0})
  
  //Above query finds and returns the whole document with the _id field omitted
  
  Result: {"everyone" : 15, "all" : 4}
```

### Query: To increment count of a particular value:
```
  db.COLLECTION_NAME.update({_id:"hello"},{$inc:{"all":1}})
  
  //Above query will increment the count of value "all" and leaves every other value as it is
  
  Result: {"everyone" : 15, "all" : 5}
```

### Query: To increment count of a particular value which is not present:
If you try to increment count of some variable which is not present at the moment in collection then it automatically gets added to database.
```
  db.COLLECTION_NAME.update({_id:"hello"},{$inc:{"okay":1}})
  
  Result: Result: {"everyone" : 15, "all" : 5, "okay" : 1}
```


# Mongo-Java-Client

## Code : To Connect to a client:
```
  MongoClient mongoClient = new MongoClient();
  or
  MongoClient mongoClient = new MongoClient("localhost");
  or
  MongoClient mongoClient = new MongoClient("localhost", 27017);
  or
  //USING CONNECTION STRING/ URI
  MongoClientURI connectionString = new MongoClientURI("mongodb://localhost:27017");
  MongoClient mongoClient = new MongoClient(connectionString);
```

## Code : To disconnect from a client:
```
  mongoClient.close();
```

## Code : To create/ GET database:
If a database does not exist, MongoDB creates the database when you first store data for that database.
```
  MongoDatabase database = mongoClient.getDatabase("DATABASE_NAME");
```

## Code : To create/ GET collection (inside a database):
If a collection does not exist, MongoDB creates the collection when you first store data for that collection.
```
  MongoCollection<Document> collection = database.getCollection("COLLECTION_NAME");
```
It returns a collection of documents
Each document looks something like this:
```
  {
   "name" : "MongoDB",
   "type" : "database",
   "count" : 1,
   "versions": [ "v3.2", "v3.0", "v2.6" ],
   "info" : { x : 203, y : 102 }
  }
```

## Code: To create an document:
To create an document like above:
```
  Document doc = new Document().append("KEY", "VALUE");
  System.out.println(doc);
  
  Result: Document{{KEY=VALUE}}
```
KEY - String Type
VALUE - Object Type


Our project required automatic updating the document
```
  for(int i = 0, j = 9;i<10&&j>-1;i++,j--)
  {
    doc.append(new Integer(i).toString(), new Integer(j).toString());
  }
  System.out.println(doc);
  
  Result: Document{{KEY=VALUE, 0=9, 1=8, 2=7, 3=6, 4=5, 5=4, 6=3, 7=2, 8=1, 9=0}}

```

## Code: To insert a document into the collection
Once you have the MongoCollection object, you can insert documents into the collection.
If there is no field by key: _id then mongodb adds its own _id element
```
  collection.insertOne(doc);
```

## Code: To count the documents into a collection
```
  System.out.println(collection.count());
```

## Code: To query the collection
You have to use method find() which returns FindIterable<Document> Type
1. Method by for each
```
  for(Document eachDocument : collection.find())
  {
    System.out.println(each.toJson());
  }
  
  Result (Sample) : { "_id" : 11, "KEY" : "VALUE" }
```
2. Method by MongoCursor<Document>
```
    MongoCursor<Document> cursor = collection.find().iterator();
    try
    {
    	while(cursor.hasNext())
    		System.out.println(cursor.next().toJson());
    }
    finally{
    	cursor.close();
    }
```
3. Method by FindIterable<Document> Type
```
  FindIterable<Document> resultDoc = collection.find();
  for(Document each : resultDoc)
  {
    System.out.println(each);
  }
```
## Code: To add filter to query
### Get Documents which matches a filter
It creates a filter matching documents where field name equals specified values.
```
  FindIterable<Document> resultDoc = collection.find(eq("_id",11));
```

### To add projections:
```
  FindIterable<Document> resultDoc =
		collection.find(eq("_id",11)).projection(new Document("_id",0));
    
  is equivalent to:
  db.COLLECTION_NAME.find({"_id":11},{"_id":0})
```

## Code: To update documents
Suppose you have a document:
```
  {
    "_id" : 11,
    "KEY" : "VALUE",
    "count" : 8
  }
```
To update the count field in the above document:
```
  MongoDB Shell CodE: db.COLLECTION_NAME.update({"_id":11},{$inc:{"count":1}})
  
  Java CodE: 
    FindIterable<Document> resultDoc = null;
    resultDoc = collection.find();
    for(Document each : resultDoc)
    {
    	System.out.println(each.toJson());
    }
    
    System.out.println("*****************");
    
    Document fieldToUpdateDoc = new Document("count", 1);
    Document projectDoc = new Document("$inc", fieldToUpdateDoc);
    Document queryDoc = new Document("_id", 11);
    collection.updateOne(queryDoc, projectDoc);
    
    
    resultDoc = collection.find();
    for(Document each : resultDoc)
    {
    	System.out.println(each.toJson());
    }
  Result: 
      { "_id" : 11, "KEY" : "VALUE", "count" : 8.0 }
        *****************
      { "_id" : 11, "KEY" : "VALUE", "count" : 9.0 }
  
```
