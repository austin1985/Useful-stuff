# MongoDB useful commands

## Install (Ubuntu 18.04)
**Open a command line and add these commands**

`sudo apt update`

`sudo apt install -y mongodb`

## Checking the Service and Database

**First, check the service's status**

`sudo systemctl status mongodb`

**Try connecting to the database server and execute a diagnostic command**

`mongo --eval 'db.runCommand({ connectionStatus: 1 })'`

## Managing the MongoDB Service

**Verify the status of the service**

`sudo systemctl status mongodb`

**You can stop the server anytime by typing**

`sudo systemctl stop mongodb`

**Start the server when it is stopped**

`sudo systemctl start mongodb`

**Restart the server**

`sudo systemctl restart mongodb`

**By default, MongoDB is configured to start automatically with the server. If you wish to disable the automatic startup**

`sudo systemctl disable mongodb`

**Enable MongoDb on startup again**

`sudo systemctl enable mongodb`

## Basic CRUD operations

**Selecting a dababase**

`use <database_name>`

**Selecting all documents in a collection**

`db.<collection_name>.find()`

**Selecting specific documents in a collection**

`db.<collection_name>.find({<fieldname>:<value>})`

`db.<collection_name>.find({<fieldname>:{$gt:<number>}})`

Operators: https://docs.mongodb.com/manual/reference/operator/query/


**Inserting one new document in the collection**

`db.<collection_name>.insertOne({<field1>:<value1>,...})`

**Inserting multiple new documents in the collection**

`db.<collection_name>.insertMany([{<field1>:<value1>,...},...])`

**Deleting one specific document from a collection**

`db.<collection_name>.deleteOne({<fieldname>: <value>})`

**Deleting multiple document matching the filter from a collection**

`db.<collection_name>.deleteMany({<fieldname>: <value>})`

**Delete all documents from a collection**

`db.<collection_name>.deleteMany({})`

**Update one specific document**

First argument tells which document to alter, second argument tells what needs to update 

`db.<collection_name>.updateOne({<filterField>:<filterValue>},{$set: {<field>:<value>}})`

**Update multiple specific document**

First argument tells which documents to alter, second argument tells what needs to update  

`db.<collection_name>.updateMany({<filterField>:<filterValue>},{$set: {<field>:<value>}})`

**Update all documents in a collection**

`db.<collection_name>.updateMany({},{$set: {<field>:<value>}})`


**Projection - filtering out fieds from result documents**

Every field that is assigned 1 will be shown. Default is always 0.

`db.<collection_name>.find({},{<field>: 1})`


**Embedded document limit is 100 levels and the total size of one document is limited to 16mb**

Display embedded document from a document

`db.<collection_name>.findOne({<field>:<value>}).<embedded_document_name>`

**Search in embedded documents**
Search in embedded array value

`db.<collection_name>.find({<array_field>:<value>})`

Search in embedded object field

`db.<collection_name>.find({<object>.<field>:<value>})`
`db.<collection_name>.find({<object1>.<object2>...<objectx><field>:<value>})`

# Data types

Text:

`"test text"`

Boolean:
`[true,false]`

Number:

- `integer(int32)`
- `NumberLong(int64)`
- `NumberDecimal(float)`

ObjectId

`ObjectId("idstring")`

ISODate - can be created with "new Date()"

`ISODate("2018-09-09")`

Timestamp - can be created with "new Timestamp()"

`Timestamp(11421532)`

Embedded Document

`{ "a" :{...}}`

Array

`{ "b" :{...}}`


