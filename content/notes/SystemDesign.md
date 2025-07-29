---
title: System Design
date: 2025-07-20
lastmod: 2025-07-20
tags:
- software
- learning
enableToc: true
---
> This is a space for me to jot down notes as I prepare for system desgin interview rounds. This will also (hopefully) help keep me accountable in actually taking the necessary steps to effectively become proficient in system design.

# The Plan

This [video](https://www.youtube.com/watch?v=Ru54dxzCyD0) has a solid 4 step plan to get prepared for system design interviews:

1. Understand what is expected 
2. Refresh/learn the fundamentals
3. Learn the basic building blocks used in common system design interview problems
4. Practice with commonly asked problems and extract patterns from each problem

I plan on using this 4 step process, but I think switching 1 and 2 would be best for me because I think understanding the fundamentals first will help me understand more clearly what is expected. So my process will be:

1. Refresh/learn the fundamentals
2. Understand what is expected 
3. Learn the basic building blocks used in common system design interview problems
4. Practice with commonly asked problems and extract patterns from each problem

I will hopefully be working through these steps in the coming months. I am writing this as of July 20th, 2025. I am hoping to be proficient within the next 2-3 months.

# The Fundamentals
## Storage
How data is persisted in a system, whether it be structured or unstructured data is one of the most essential parts of a system. It is important to know that different databases and storage systems have different advantages that are useful for different data types and data access patterns.

### Databases
Data can be stored in many formats

#### Records/Tables
The most popular data format is storing data as rows/records in a table.
- This is useful when the structure and the fields of data are well defined an not likely to change
- It is fast to perform "joins" on tables such that you can make queries across multiple tables for data that may be related.
- Data correctness can be maintained because of the rigid nature and forced types

#### Documents
Document DBs store data in a JSON like nested format
- It does not have a defined schema for the data, so it is flexible with data with different fields and structures
- Good for when your data model can be changing more frequently.
- Good for data locality, as data for a record can be embedded within a record, unlike tables where you may need to perform some complex join.
- Good for horizontal scaling because data is easily partitionable

#### Graphs
Sometimes it is better to model data as a network of relationships
- Data is stored as nodes with metadata and edges that connect the nodes together, also with metadata.
- Good for when the relationship between objects is important (e.g. Social networks).

#### Scaling Databases
There are two ways to scale your database. First is vertical scaling. This is quite simple, you just add more disk space to your database node.

Horizontal scaling is a little more complex. It can happen in two ways:
Replication and Sharding. These two methods can be used together as well.

**Replication**
- Replication is when you distribute the same data across multiple database nodes. This is useful not when space is your bottleneck, but throughput is your bottleneck. Your system needs to be able to handle more requests per second.
- By distributing the data across multiple nodes, you can relieve the stress on one node by giving multiple nodes to perform reads or writes on.
- Single Leader Replication
    - This is when you only use a single node for writes for a DB, but you replicate the writes from DB to all other nodes. 
    - This is good for when your read throughput is much higher than your write throughput.
    - It will not help your write throughput, as you are still bottlenecked by one node
    - Less likely to have conflicts in writes. Can probably still have ACID guarantees because of only 1 writing node.
    - Good scaling option when you don't make a lot of writes, but you make a large amount of reads.
- Multileader/Leaderless Replication
    - This allows for multiple nodes to accept write requests to the DB, and data has to be replicated from all DB nodes to each other.
    - ACID guarantees here is only possible with a huge decrease in performance, which is not worth. You are more likely to have write conflicts.
    - You can make eventually consistent guarantees, which basically means the data between all databases will eventually converge to the same state.
    - Good to use when you need to handle a large amount of reads and writes, and it is not super important for the data to be accurate and consisten right away.

**Partitioning**
- Good mainly for when data storage is your bottleneck (though it also does help with throughput bottleneck)
- You have to split your data in to multiple shards because your original node simply did not have enough space.
- It is common to assign records to partitions based on a specific attribute/key of the data (e.g. all IDs 0-1MM go to shard 0, 1MM-2MM go to shard 1, etc.)
- You want to make sure that one DB node does not become a hotspot because of natural data distrbution. It is good to use some hashing function to make the distribution of the keys more random/uniform and use consistent hashing to have better distribution of data.

**Combining both**
You can combine partitioning and replication to scale your database in all directions. Say you have nodes: $n_1, n_2, n_3$, and data $X$. $X$ can be separated into $x_1, x_2, x_3$. Then each of the shards can be replicated three time such that:
<p align="center">

$$ n_1: x_{11}, x_{21}, x_{31} \\ n_1: x_{12}, x_{22}, x_{32} \\ n_1: x_{13}, x_{23}, x_{33} $$

</p>

This way you can use the advantages of replication for scaling with the advantages of sharding for scaling.

#### (Basically) All the Databases
**SQL**
- Relational database storing normalized data. Data is stored as rows of a table.
- Has ACID guarantees because of transactions and 2 phase commits, and Write Ahead Logs
    - (A)tomicity: Transactions are all or nothing. If there is an error in the middle of a transaction, the whole operation is rolled back
    - (C)onsistency: Data is valid before and after. There is no data corruption or partial failures.
    - (I)solation: No race conditions occur when transactions are concurrent with each other.
    - (D)urability: Data is not lost even when the system crashes
- Having ACID guarantees makes SQL good for when data correctness is needed, as there is very little chance of data being corrupted, and if a transaction is failed, it will let you know, and it can be retried.
- Having these guarantees makes SQL hard to scale horizontally with multiple nodes though because it is hard to make these guarantees in a distributed system. **SQL is hard to scale horizontally**
- Best thing you can probably do is single-leader replication.
- It is the default option (Postgres) to go to because it is so popular, tooling is good, and it is the industry standard for DBs

**MongoDB (Document)**
- Document data model
    - As explained above, data is stored in hierarchical manner similar to JSON
- Can scale better horizontally, but does not offer the same ACID guarantees as SQL (guarantees BASE)
- Not really preferred to SQL because it has similar performance but not as standard practice.

**Cassandra (Wide Column Store)**
- Known as a column-family store. This means the data is stored as a table similar to SQL, but the data is grouped by columns instead of rows.
- Makes it easy to make queries for full columns instead of full rows
- Still considered NoSQL because it can have flexible data model
- Supports Multileader and leaderless replication strategies
    - This is good for when you need higher write throughput because multiple nodes of the DB can accept writes.
    - Uses last write wins strategy to handle merge conflicts.
    - Good for when availability is very important, but because of LWW strategy, not the best for consistency
**Riak**
- Basically the same as Cassandra, but it uses Conflict-Free Replicated Distributed Tokens (CRDTs) to resolve merge conflicts, which is more robust

**HBase**
- Another wide column data store
- Underlying infrastructure is Hadoop Distributed File System (HDFS)
- Uses Single Leader Replication for replication strategies.

**Neo4J**
- Graph database. When you have many to many relationships in your data and SQL becomes not optimal
- Pretty niche, but good for when your data can be naturally represented as a graph (e.g. social networks, knowledge graphs, etc.)
- Cypher query language

**Redis**
- Not technically a database, but it is an in-memory key value store.
- Essentially, it is a hashmap you can make exact match queries for for data that either is frequently accessed, or is esssential to be retrieved fast

### Blob Storage
- BLOB (Binary Large Objects) storage is used for when you need to store data that is not exactly structured for a database (e.g. pictures, videos, audio, logs)
- It would not make sense to cram them into databases because it is note structured data with defined fields etc.
- Sort of like a file system to dump large files into.
- Also good for archives, as BLOB storage is cheap for large amounts of data. 
- The reason you would pick a DB over BLOB storage is because of queryability.
- Typically, BLOB storage is used in the following manner:
    - There is metadata of the file/object that needs to be stored (e.g. the date created, length, and key that will be used to look up the object in the BLOB store). This data can be stored in DB because it is structured, and it is needed so that we can find the location of the objects that we store.
    - Actual object is stored in data bucket (e.g. S3)
    - Presigned URLs
        - Usually, if the user wants to download or upload the file to/from their client machine, it would not be a good idea to do it straight through the server
        - This is because the whole object would need to be put into the request body, which is bad, and it can cause unnecessary load on the server
        - To circumvent this, we use presigned urls, where a request is made to the server for an upload/download, and instead of performing the upload/download, the backend returns a presigned URL, that will give the user temporary permissions to upload/download to/from the BLOB storage directly

