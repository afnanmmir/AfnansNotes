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


## Networking
The OSI Model is the 7 layer abstraction that describes how data is generally moved between machines over the network.

The layers can be seen below:

![OSI Model](/notes/images/OSI_Model.png)

Each layer builds on the layer below it. For example, the Transport layer depends on the Network layer to be able to perform the routing of the data, and the Application layer depends on the Transport layer to have established a connection to send data over.

The main three that are important for system design are Network, Transport, and Application. Example of flow of communication can be seen below:

![OSI Flow](/notes/images/http_request.png)

In the above image, you can see a three way handshake is first established to establish a connection. Then the request is sent and response is returned. Then the connection is terminated.

Each step has its own bit of latency.

### Network Layer
This is where the **Internet Protocol (IP)** lies.

- IP provides machines with addresses that allows packets of information to be routed to each other. Like addresses of homes, and USPS needs them to be able to route packages to their destination.

#### Public Address vs Private Address
- Public IP Addresses are addresses for machines that are known by all other machines in the world. This is needed for outward facing services/systems that need to accept requests/data from the public. These are usually IPv4 addresses because IPv4 was the standard initially and is supported almost universally

- Private IP Addresses are addresses you assign to a machine to communicate between machines in your subnetwork. Use case might be nodes talking to each other as different microservices, or two servers needing to communicate to process a request. _Used for internal communication_.

Network layer is what is needed to be able to route packets of data from one node to another.


### Transport Layer

Transport layer is needed to give context for the packets being sent node to node. It communicates the order of the packets as well as the source node of the packets to the destination node to be able to order the packets on the other side and be sure where it is coming from.

There are two main protocols to know in this layer: TCP and UDP

#### TCP
- TCP is the standard default protocol to use for establishing connection and ensuring data delivery in network communication.
- It _guarantees delivery of packets as well as the order in which the packets are delievered_.
    - Ordering is done by giving each packet of data a sequence number so it can be ordered on the other side
    - Delivery is guaranteed with many handshakes
- All the guarantees lead to an increase in latency and decrease in potential throughput though. It is not good to use this when _latency and throughput is your main optimizing goal_

#### UDP
- UDP is used for when you need to _minimize latency and maximize throughput_.
    - Sample use cases of this include video conferencing, streaming, etc.
- Use when you do not care about the occasional packet loss because receving all packets of data is not vital.
- Does not offer the same guarantees as TCP, so it can have less latency.

### Application Layer
This is where the Hyper-Text Transfer Protocal (HTTP) lives, a text formatted request and response model

Request:
```
GET /posts/1 HTTP/1.1
Host: host.com
Accept: application/json
User-Agent: Mozilla/5.0
```

The first line has the HTTP "method" (GET), the resource you are trying to access (/posts/1) and the http version (HTTP/1.1)

The remaining lines are headers, which are key value pairs that you can add to the request to give additional meta data to the destination node when sending the request over.

Response:
```
HTTP/1.1 200 OK
Date: Sun, 03 Aug 2025 12:34:56 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 292
{
    "userId": 1,
    "id": 1,
    "body": "Stuff"
}
```
The response first line contains the HTTP version again + the response code (200 OK) signaling the outcome of the request being processed.

The remaining lines are again response headers, and after the headers, is the body of the response, which is the data the client requested.

The `content-type` header is a special header that allows for "Content Negotiation"
- The client tells the server what type of data it can consume/accept, and the server uses this data to determine what it can send to the client, and whether it can return the content the client can consume.
- This ensures backwards and forward compatibility for HTTP versions.

There are many models within HTTP

#### REST
- **Re**presentation **S**tate **T**ransfer
- A way that we leverage HTTP protocol to make APIs
- We use HTTP methods and resources and map them to programming functions/methods to decide what action needs to be taken.
- Common HTTP Methods
    - GET: Usually used for retrieving data
    - POST: Usually used for inserting data
    - PUT: Usually used for updating data
    - DELETE: Usually used for deleting data
- These methods paired with a resource can define exactly what you want an API to do.
- Example:
    ```
    GET /user/{id} --> USER
    ```
    - In this example we use a `GET` HTTP method matched with the `/user/{id}` resource, which defines an API that will return the user with the id `{id}`.

    - We can have PUT, and POST, and DELETE methods associated with this resource to update an existing user, add a new user, or delete an existing user respectively.
- REST is considered the default/standard use case for API building, so unless there is some niche case, REST should be your choice of API modeling.

#### GraphQL
- GraphQL addresses some limitations of REST
    - If you need a bunch of information retrieved to load a page of your app, most likely you will need to make a lot of REST API calls to get all the data you need (e.g. profile pic, profile information, etc.)
    - Each one takes time and resources, leading to increase in overhead.
- Instead of making multiple requests to APIs to get all the information, GraphQL allows client to define exactly what data it needs from the server, and the server can go fetch exactly what the client needs all in the same request from whatever data source it needs to access.

- It is often used for when the requirements of your system are changing, so you don't need to constantly change multiple APIs.

#### gRPC
- **G**oogle **R**emote **P**rocedure **C**alls
- It is seen as protobuf + services
    - Protobuf: A way to define structures for data to be able to serialize the data into byte representation and eventually deserialize it back into original form
    - Allows for efficient serialization into compact format
- It is good to use if you want to communicate internally from one micro service to another.
- It is not so good for publicly facing API endpoints because it is not natively supported in most browsers.

#### Server Side Events
- Everything before has been request + response pattern, where client sends request, server sends back response.
- SSE used for when server needs to push data to users as it happens (e.g. notifications, messages, etc.)
- SSE is a _unidirectional_ connection from server --> client
- It is used to be able to stream content from server to client.
- It is good for short lived running events (e.g. Chat apps for AI Chatbots)
- It doesn't scale too well with large numbers of clients.

#### Websockets
- It is also a way for servers to be able to push data to clients, but it is _bidirectional_ meaning clients can send events to server as well.
- This makes it good for any messages/chat apps
- It is more resource intensive than SSE, needing a lot of infra set up
- Web sockets are also _stateful_ because they are connections that must remain open until user is no longer active, as the server needs to know the metadata of the connection(s) to clients it has established at the minimum.


### Scaling in Networking
- How can we get our system to be able to handle traffic at a global scale

#### Vertical Scaling
- A less technically complex but less practical way to handle scaling is vertical scaling. This just means beefing up the server you already have with increase in memory, CPU, disk, etc. to be able to handle more and more requests.

#### Horizontal Scaling
- A more practical but also more technically complex way is to use horizontal scaling, which means getting more copies of servers and stuff to diffuse the traffic to multiple machines.

- There are many things needed to perform horizontal scaling

##### Load Balancing
Placing a middle man between the client and the sever that takes the request from the client and routes the request appropriately to one of the servers based on current server traffic patterns (which server is the least congested with requests), server availability statuses (which servers are not crashed), etc.

![Load Balancer](/notes/images/load_balancer.png)

There are multiple ways the load balancer in the middle can be implemented: Client side Load Balancer, and Dedicated Load Balancer

Client Side:
- The routing is done locally from the client.
- The client must be aware of all clients, and performs the routing of the request.
- Because there isn't any real logic in the routing like there is for dedicated load balancers, this minimizes the latency increase of the load balancer
- Not good for when you have a lot of clients, or if you need real time updates on server statuses.

Dedicated LB:
- An actual load balancer receives the request from the client and performs the routing.
- LB constantly sending health checks to servers to make sure they are healthy and can receive traffic, and performs a routing algorithm to route requests to servers:
    - Round Robin
    - Random
    - Least Connections
-  There are also different levels the LB can live on: the network layer LB, and the application layer LB

- Network Layer Load Balancer
    - This sits in the network transport layer (TCP/UDP), and the routing is done based on IP addresses and ports. 
    - It is very low level, but very lightweight because it does not have to do deep dive into the contents of packets.
    - Cannot make decisions based on the content of the HTTP requests
    - Is compatible with any of the API models in the application layer since it is abstracted
- Application Layer Load Balancer
    - Sits at application layer.
    - Allows load balancer to make routing decisions based on body of request
    - More complex routing logic, so slower.
    - Because it has content available to it, it can be used for throttling and rate limiting because you have access to auth tokens
### Deep Dives
#### Regionalization
- How do we deal with communication when traffic is coming from all different parts of the world? How do ensure optimal experience for users no matter where in the world they are located.

- As discussed before with data, this can be done with partitioning of data and replication of data across multiple nodes across the world to ensure data is close enough to most of its users.

- It is good to try to find natural partitions in data to spread data across. For example, with Uber, a user in Seattle will not need any data regarding drivers in London or Austrailia.

- Colocation of Data:
    - You want to keep your data and your server close together geographically to ensure you aren't unecessarily adding latency where you don't need to.
    - Also can use a CDN.
        - Cloud Distrbution Network, is like a globally distributed cache, where data can live for some time if it was recently accessed
        - When a user wants to access the data again, it will make a request to the CDN as if it was the server first, and if the CDN does have it, it will return the data, but if it doesn't it will redirect the request back to the original server
#### Failure/Fault Handling
- A key to remember in all System Design interviews is that unexpected faults and failures are going to happen, so you need to be able to handle them.

- How can we handle network requests that fail at network level? This is different from getting a server or client error code from the request. This is when the packets are lost in transit, the server never receives the request etc.

- Timeouts

    - We have to introduce timeouts to the client, so that the request the client makes eventually times out if no response is given within a certain period of time.

    - Needs to be long enough to give the request the chance to be processed, but also should be short enough so we aren't unnecessarily waiting for something that won't happen.
- Retries
    - With timeouts, we want to give the request multiple efforts to succeed in case we failed due to a transient error.
    - We allow client to retry some amount of time before actually erroring out.
    - We use exponential backoff as a retry strategy, which means the interval between each retry increases after each retry.
        - This is to not overload the server with retry requests, and because if the 2nd or 3rd retry won't work, it is likely that the error is not transient, so no point in retrying so fast.
    - We also introduce a random +/- to the retry interval (Jitter). This ensures if requests are clustered together, their retry attemps won't also be clusterd together. More evenly distributed traffic pattern.
#### Cascading Failures + Circuit Breakers
Given the following design component:
![circuit breaker](/notes/images/circuit_breaker.png)

Let's say the DB is at 50% caopacity because of the snapshot it is producing. This means half the requests server B makes to DB will fail, and because of this, server B will make retries of these requests to the DB.

The retries also fail, which means server A is receiving failure responses from server B for its requests, so server A will also make retries.

With server B handling its own retries as well as server A's retries, it will become overloaded and may crash.

An engineer may look at this and think server B is the problem, and try to restart it, but won't find the issue because it is a _cascading failure_, originating from a different component.

We can introduce a **Circuit Breaker** to pause the sending of requests from server A to server B until the root cause is found.

The circuit breaker will automatically pause the requests sent from A to B if the failure rate of requests breaches a certain threshold, and waits a certain period of time. Then, it checks again to see the status of server B, and once it is healthy again, it will resume requests.

This will prevent us from having cascading failures, as we won't pull down the whole system, makes sure we aren't unnecessarily using up resources, and allows for failing system to recover.

