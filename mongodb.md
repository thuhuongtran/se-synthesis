## MongoDB
### What is MongoDB
To manage huge sets of unstructured data like log or IoT data, a NoSQL database is used.
MongoDB is an open-source NoSQL database using JSON-like documents with optional schemas. MongoDB works on the concept of Collection and Document.

#### Document
A Document in MongoDB is an ordered set of keys with associated values. Complex documents will contain multiple key/value pairs:
#### Collection
A collection in MongoDB is a group of documents. If a document is the MongoDB analog of a row in a relational database, then a collection can be thought of as the analog to a table.
#### Database
MongoDB groups collections into databases. MongoDB can host several databases, each grouping together collections.

### MongoDB Shell (mongosh)

The MongoDB Shell, mongosh, is a fully functional JavaScript and Node.js 14.x REPL environment for interacting with MongoDB deployments. You can use the MongoDB Shell to test queries and operations directly with your database.

### Highlight features
* **Indexing**: It supports generic secondary indexes and provides unique, compound, geospatial, and full-text indexing capabilities as well.
* **Aggregation**: It provides an aggregation framework based on the concept of data processing pipelines.
* **Special collection and index types**: It supports time-to-live (TTL) collections for data that should expire at a certain time
* **File storage**: It supports an easy-to-use protocol for storing large files and file metadata.
* **Sharding**: Sharding is the process of splitting data up across machines.
### Transaction
A transaction is a sequence of database operations that will only succeed if every operation within the transaction has been executed correctly.

Because of the way they’re implemented in MongoDB, transactions can only be performed on MongoDB instances that are running as part of a larger cluster.
This could either be a sharded database cluster or a replica set.

```jshelllanguage
session = db.getMongo().startSession(
        { readPreference:
            {mode: "primary"}
        });
productsCollection = session.getDatabase("products").products;
salesCollection = session.getDatabase("products").sales;
session.startTransaction({
    readConcern: { level: "snapshot" }, 
    writeConcern: { w: "majority" }
    });
try {
    //.. insert, update
} catch (error) {
    session.abortTransaction();
    throw error;
}
session.commitTransaction();
session.endSession();
```
`session.startTransaction(`: Start transaction.

`session.commitTransaction();`: Commit transaction.
`session.endSession();`

`session.abortTransaction()`: Means do not commit either transaction.

### Sharding
Sharding is the process of storing data records across multiple machines and it is MongoDB's approach to meeting the demands of data growth. As the size of the data increases, a single machine may not be sufficient to store the data nor provide an acceptable read and write throughput. Sharding solves the problem with horizontal scaling. With sharding, you add more machines to support data growth and the demands of read and write operations.
* In replication, all writes go to master node
* Latency sensitive queries still go to master
* Single replica set has limitation of 12 nodes
* Memory can't be large enough when active dataset is big
* Local disk is not big enough
* Vertical scaling is too expensive

### Queries
#### Drop database
```jshelllanguage
db.dropDatabase()
```
#### Create collection
```jshelllanguage
db.createCollection(name, options)
```
#### Drop collection
```jshelllanguage
db.COLLECTION_NAME.drop()
```
#### Insert document
```jshelllanguage
db.COLLECTION_NAME.insert(document)
```
```jshelllanguage
> db.users.insert({
... _id : ObjectId("507f191e810c19729de860ea"),
... title: "MongoDB Overview",
... description: "MongoDB is no sql database",
... by: "tutorials point",
... url: "http://www.tutorialspoint.com",
... tags: ['mongodb', 'database', 'NoSQL'],
... likes: 100
... })
WriteResult({ "nInserted" : 1 })
>
```
#### Find
```jshelllanguage
db.COLLECTION_NAME.find()
db.COLLECTION_NAME.findOne()
db.COLLECTION_NAME.find().limit(NUMBER)
db.COLLECTION_NAME.find().sort({KEY:1})
```
```jshelllanguage
db.mycol.find({},{"title":1,_id:0}).sort({"title":-1})
```
#### Update
```jshelllanguage
db.COLLECTION_NAME.update(SELECTION_CRITERIA, UPDATED_DATA)
db.COLLECTION_NAME.save({_id:ObjectId(),NEW_DATA})
db.COLLECTION_NAME.findOneAndUpdate(SELECTIOIN_CRITERIA, UPDATED_DATA)
```
```jshelllanguage
db.mycol.update({'title':'MongoDB Overview'},{$set:{'title':'New MongoDB Tutorial'}})
```
#### Delete document
```jshelllanguage
db.COLLECTION_NAME.remove(DELLETION_CRITTERIA)
```
```jshelllanguage
db.mycol.remove({'title':'MongoDB Overview'})
```
#### Index
```jshelllanguage
db.COLLECTION_NAME.createIndex({KEY:1})
db.COLLECTION_NAME.dropIndex({KEY:1})
```
```jshelllanguage
db.mycol.createIndex({"title":1,"description":-1})
```
#### Aggregation
```jshelllanguage
db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)
```
`$sum`, `$avg`, `$min`, `$max`, `$push`, `$addToSet`, `$first`, `$last`

* `$project` − Used to select some specific fields from a collection.
* `$match` − This is a filtering operation and thus this can reduce the amount of documents that are given as input to the next stage.
* `$group` − This does the actual aggregation as discussed above.
* `$sort` − Sorts the documents.
* `$skip` − With this, it is possible to skip forward in the list of documents for a given amount of documents.
* `$limit` − This limits the amount of documents to look at, by the given number starting from the current positions.
* `$unwind` − This is used to unwind document that are using arrays. When using an array, the data is kind of pre-joined and this operation will be undone with this to have individual documents again. Thus with this stage we will increase the amount of documents for the next stage.
```jshelllanguage
db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : "$likes"}}}])
```