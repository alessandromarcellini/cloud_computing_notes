## Why NoSQL
In modern times the way we handle data storage in really distributed and high volume (**Big Data**) systems has really been revolutionized, going from SQL to NoSQL (Not Only SQL) data systems.
This is due to the fact that the requirements of today's workloads are really different from the past, in particular, most of the times, we have data that is **extremely large**, really **unstructured and heterogeneous**, that needs to be **optimized** for a particular set of **operations** (mostly writes or range reads) **or type** of data (geo-spatial, weather, ...), that needs to be **consistent but not so tightly**, (eventual consistency) needs to be easily **scalable** (out, not up) and **partitionable**.

**Modern global systems require reconsidering big data repositories, both in strategies and mechanisms**

Most of the proposals for these new types of DataBases have arised around **2010**.
## Common Types
There are lots of types of databases, each taylored for a specific use, type of stored data and operations.
These include:
- **Key-Value Stores** ([[DataStorage - Key-value]]): data are managed as (key, value) pairs stored in efficient and scalable ways (typically as in DHT) Redis, Oracle NoSQL, **Cassandra**;
- **Document Stores**: extended key-value stores with value as a document encoded in standard formats (XML, JSON, or BSON) **MongoDB**, CouchDB, CosmoDB, Firebase, …;
- **Wide-Column Stores** ([[DataStorage - wide-column]]): data are represented in a tabular format of rows and column families stored per column-family dynamically and flexibly **Cassandra**, BigTable, HyperTable, Hbase, …;
- **Graph Stores**: graphs for storing data efficiently and providing more effective operations Neo4j, Giraph, Virtuoso, ArangoDb, Titan, AllegroGraph, …

>[!note]
>As we said in microservices each service should own its data.
>Because of this we can have different databases for different services, each optimized for not only the type of data that needs to be stored but also the operations that we're expecting.
>
>The service should do more range operations? We'll use a wide column storage system
>The service should do more single element writes? We'll use a key-value or document storage system.
>The service has really low traffic, the data is really structured, we care deeply about consistency and most of the operations will be reads? We'll use a standard SQL database.




## Studied Dbs
During the lecture we've seen how [[DataStorage - Cassandra]] and [[DataStorage - MongoDB]] work.