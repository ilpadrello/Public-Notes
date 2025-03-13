---
title: "MONGO DB"
date: "2018-11-18"
---

Resource: [https://www.tutorialspoint.com/mongodb/index.htm](https://www.tutorialspoint.com/mongodb/index.htm)

# MONGO DB

MongoDB is a no SQL database, when you install it usually starts as a network service, so all the computer can access it... (but it is usually on localhost). It has is own javascript runtime shell that you can access using the command mongo! But before you have to run it using the mongod command! MongoDB is case sensitive, so pay attention! Do not use for DB and collections: $, /0, "":empty string , but you can create subcollection using the . ex: blog.posts Documents, Collections, Databases: Documents are like rows Collections are like table Databases are like... databases :) MDB use JSON like structure for its documents but it has more data type that you can put in:

```
{"x":null} //null

{"x": true} // Boolean

{"x": 3.14} //64-byte floating type

{"x": numberInt("3")} //Integer

{"x": numberLong("3")} //Long integer

{"x": "foobar"} //String

{"x": new Date()} //date

{"x": /foobar/i} //Regex or regular expression

{"x":["a","b"]} //Arrays

{"x":{"foo":"bar"}} //Embedd documents

{"x": ObjectId()} //Object Id

{"x": function (){ /* ...*/}} //Code

//Bynary data
```

MOST USED COMMANDS IN MONGO DB

```
use testdb // changing db

db.mycollection.find() //If there is no parameter returns all the collection

db.mycollection.update({"obj":"to find"},newobj) //This will update all the values in the db, attention to multiple result when passing the obj to find



//$set to just change some values of the obj:

db.test.update({name:"Simone"},{$set:{surname:"Panzerotto"}}) // This will change just the surname leaving the others field intacts

//Like this this will work for just one document, to make it work with all document we must add some parameters: 

db.test.update({name:"Simone"},{$set:{surname:"Panzerotto"}},false,true)




```

More on $set operator : [https://docs.mongodb.com/manual/reference/operator/update/set/](https://docs.mongodb.com/manual/reference/operator/update/set/)

## The find() Method

To query data from MongoDB collection, you need to use MongoDB's **find()**method.

### Syntax

The basic syntax of **find()** method is as follows −

```
>db.COLLECTION_NAME.find()


```

**find()** method will display all the documents in a non-structured way.

## The pretty() Method

To display the results in a formatted way, you can use **pretty()** method.

### Syntax

```
>db.mycol.find().pretty()


```

## Example

```
>db.mycol.find().pretty()

{

   "_id": ObjectId(7df78ad8902c),

   "title": "MongoDB Overview", 

   "description": "MongoDB is no sql database",

   "by": "tutorials point",

   "url": "http://www.tutorialspoint.com",

   "tags": ["mongodb", "database", "NoSQL"],

   "likes": "100"

}

>
```

Apart from find() method, there is **findOne()** method, that returns only one document.

## RDBMS Where Clause Equivalents in MongoDB

To query the document on the basis of some condition, you can use following operations.

| Operation | Syntax | Example | RDBMS Equivalent |
| --- | --- | --- | --- |
| Equality | {<key>:<value>} | db.mycol.find({"by":"tutorials point"}).pretty() | where by = 'tutorials point' |
| Less Than | {<key>:{$lt:<value>}} | db.mycol.find({"likes":{$lt:50}}).pretty() | where likes < 50 |
| Less Than Equals | {<key>:{$lte:<value>}} | db.mycol.find({"likes":{$lte:50}}).pretty() | where likes <= 50 |
| Greater Than | {<key>:{$gt:<value>}} | db.mycol.find({"likes":{$gt:50}}).pretty() | where likes > 50 |
| Greater Than Equals | {<key>:{$gte:<value>}} | db.mycol.find({"likes":{$gte:50}}).pretty() | where likes >= 50 |
| Not Equals | {<key>:{$ne:<value>}} | db.mycol.find({"likes":{$ne:50}}).pretty() | where likes != 50 |

## AND in MongoDB

### Syntax

In the **find()** method, if you pass multiple keys by separating them by ',' then MongoDB treats it as **AND** condition. Following is the basic syntax of **AND** −

```
>db.mycol.find(

   {

      $and: [

         {key1: value1}, {key2:value2}

      ]

   }

).pretty()


```

### Example

Following example will show all the tutorials written by 'tutorials point' and whose title is 'MongoDB Overview'.

```
>db.mycol.find({$and:[{"by":"tutorials point"},{"title": "MongoDB Overview"}]}).pretty() {

   "_id": ObjectId(7df78ad8902c),

   "title": "MongoDB Overview", 

   "description": "MongoDB is no sql database",

   "by": "tutorials point",

   "url": "http://www.tutorialspoint.com",

   "tags": ["mongodb", "database", "NoSQL"],

   "likes": "100"

}
```

For the above given example, equivalent where clause will be **' where by = 'tutorials point' AND title = 'MongoDB Overview' '**. You can pass any number of key, value pairs in find clause.

## OR in MongoDB

### Syntax

To query documents based on the OR condition, you need to use **$or** keyword. Following is the basic syntax of **OR** −

```
>db.mycol.find(

   {

      $or: [

         {key1: value1}, {key2:value2}

      ]

   }

).pretty()
```

### Example

Following example will show all the tutorials written by 'tutorials point' or whose title is 'MongoDB Overview'.

```
>db.mycol.find({$or:[{"by":"tutorials point"},{"title": "MongoDB Overview"}]}).pretty()

{

   "_id": ObjectId(7df78ad8902c),

   "title": "MongoDB Overview", 

   "description": "MongoDB is no sql database",

   "by": "tutorials point",

   "url": "http://www.tutorialspoint.com",

   "tags": ["mongodb", "database", "NoSQL"],

   "likes": "100"

}

>
```

## Using AND and OR Together

### Example

The following example will show the documents that have likes greater than 10 and whose title is either 'MongoDB Overview' or by is 'tutorials point'. Equivalent SQL where clause is **'where likes>10 AND (by = 'tutorials point' OR title = 'MongoDB Overview')'**

```
>db.mycol.find({"likes": {$gt:10}, $or: [{"by": "tutorials point"},

   {"title": "MongoDB Overview"}]}).pretty()

{

   "_id": ObjectId(7df78ad8902c),

   "title": "MongoDB Overview", 

   "description": "MongoDB is no sql database",

   "by": "tutorials point",

   "url": "http://www.tutorialspoint.com",

   "tags": ["mongodb", "database", "NoSQL"],

   "likes": "100"

}

>


```

In MongoDB, projection means selecting only the necessary data rather than selecting whole of the data of a document. If a document has 5 fields and you need to show only 3, then select only 3 fields from them.

## The find() Method

MongoDB's **find()** method, explained in [MongoDB Query Document](https://www.tutorialspoint.com/mongodb/mongodb_query_document.htm) accepts second optional parameter that is list of fields that you want to retrieve. In MongoDB, when you execute **find()** method, then it displays all fields of a document. To limit this, you need to set a list of fields with value 1 or 0. 1 is used to show the field while 0 is used to hide the fields.

### Syntax

The basic syntax of **find()** method with projection is as follows −

```
>db.COLLECTION_NAME.find({},{KEY:1})


```

### Example

Consider the collection mycol has the following data −

```
{ "_id" : ObjectId(5983548781331adf45ec5), "title":"MongoDB Overview"}

{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}

{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Tutorials Point Overview"}
```

Following example will display the title of the document while querying the document.

```
>db.mycol.find({},{"title":1,_id:0})

{"title":"MongoDB Overview"}

{"title":"NoSQL Overview"}

{"title":"Tutorials Point Overview"}

>
```

Please note **\_id** field is always displayed while executing **find()** method, if you don't want this field, then you need to set it as 0.
