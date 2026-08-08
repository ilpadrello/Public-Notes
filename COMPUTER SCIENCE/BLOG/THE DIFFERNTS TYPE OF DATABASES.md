This is just a recap of the differents kind of database that you can find, of course event if they are of the same category, each database has its differences but the core paradigm stay the same.

## RELATIONAL DATABASES (SQL)

Ex :
- Mysql
- MariaDB
- Postgres

The base paradigm is that they uses tables, and tables can have relationships with each others.
This is the most used paradigm, and some people think that is the only database that you'll ever need.

## WIDE COLUMN DATABASE

Ex : 
- Cassandara
- ScillaDB
- Hbase
- Bigquery
- Redshift

Wide column databases are a type of NoSQL database that store data in table, but there table are very flexible: 
- Each row doesn't have to have the same columns.
- Columns are grouped into column families for performance and scalabilty
- It's optimized for huge volumes of sparce data, often used in big data applications.

This kind of database is often used for logs, IOT, OLAP or massive-scale reads/writes.

### Column Family
Column families are an important part of these kind of databases, here's an example : 

```
Row Key: user123
  Column Family: personal_info
    - name: "Alice"
    - age: 30

  Column Family: preferences
    - theme: "dark"
    - language: "en"

  Column Family: activity_log
    - 2024-01-01: "Login"
    - 2024-01-02: "Click"

```

Column families are a like mini-table inside a table, groups of columns. 
OK but why ? In a relationship database, you have more than one table, sometime for the same "entity", like the user can have multiple email or phone number, so you create a table that represent the user and all the data that the user have that we know the user have in a specific quantity. For example the birthday, the first-name and last-name.
We know that there is only one of those, for all the data of the user where we do not know the quantity we create another table for example, and use the relationship one-to-many to have all the info.

If you see the example, you do not need to create another table for this kind of database, you can create just a column family (email, phone number, photos etc...) and all the data is already connected with the base row.

Reading performance are faster since you need just to say what column family you want to read and access only those data.

Also writing is faster.

Horizontal distribution is also easier since you can separate column families in different shard.

### No joins.
But that comes with a price, most of those database don't have any "JOIN" mechanism.
That is why, this kind of database are not very popular for BUNINESS LOGIC, but are very good for stuff like statistics, logs, IOT etc...

## DOCUMENT ORIENTED DATABASES*

Ex: 
- MongoDB
- CouchBase
- CouchDB
- DocumentDB (amazon)

Those are NoSQL databases, there is no schema so you could theoretically put everything as you want, even if a "schema" for each document is always better.

These databases have something good of the WIDE COLUMN DATABASES, since they are schema-less, and you can do joins with them.

Documents in this kind of database are often represented as BSON (Binary JSON), that are very similar to JSONs.

The other advantage of those database is that this kind of database are easy to scale horizontally.

Queries are not as fast as CASSANDRA type of database unfortunately

CONS: 
Since the data are supposed not to have relations, you can easily have some normalization problems, like having two users with the same order id.
No relation is enforced so, that could happen.

## KEY VALUE DATABASES

Ex: 
- Redis
- Valkey
- Memcached

Key values databases, have the simplest paradigm, you add a key and a value (usually text)
Those database are usually in memory database, and the data live in the RAM, and that make the database super fast in reading and writing.

Those kind of database are usually used for Caching databases, so that you can access data way faster, but not only, they are also used for real time messaging (queuing) and session managing


## TIME SERIES DATABASE

Ex: 
- Prometheus
- Grafite
- Influxdb

Those databases are used mostly for time mapped data, and to make graphs
Like when you want to monitor you CPU and other stuff, you will save all the data in an time series manner.

When you want to collect the data, maybe you want an average of the day, since you are using this kind database, the reading performance are optimum.

Also since this kind of database have a schema, they usually stock all the data (CPU temp etc), the database do not need to store the whole value but just the difference with the previous value

| Time            | sensor_id | temperature | what is actually stored |
| --------------- | --------- | ----------- | ----------------------- |
| 2024-02-02 8:00 | sensor_1  | 10          | 10                      |
| 2024-02-02 8:15 | sensor_1  | 11          | 1                       |
| 2024-02-02 8:30 | sensor_1  | 13          | 2                       |
| 2024-02-02 8:45 | sensor_1  | 10          | -3                      |
This makes the data way faster to read and write, and the data will take way less space in the memory.
With this kind of logs the volume is very important.

Also, sharding is very easily done in this kind of database.

## GRAPH BDD

Ex:
- Neo4j
- Neptune
- Arangodb

Graph databases are for data relations on steroids !
Graph databases stocks nodes and relationships between nodes.
They are popular for social networks, fraud detections, recommendation engines.

Relations between data can be of the type you generate, (friendship, family, follows, member_of), and the database is structured to easily find, for example all the friend of a friend of a friend...
Otherwise said, navigate in nodes...

This is a nightmare to do in a relationship database.

## VECTORIAL DATABASES
Ex: 
- Pinecone
- Weaviate
- Qdrant
- Postgres(yes it could do the job)
Vectoral are very special tool, useful for search engine and AI.
Vectors are just a series of numbers 

| id        | vector           | name    |
| --------- | ---------------- | ------- |
| product_1 | [0.12,0.95,0.56] | laptop  |
| product_2 | [0.24,0.56,0.45] | mouse   |
| product_3 | [0.65,1.25,0.85] | monitor |

 