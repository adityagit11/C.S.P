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
