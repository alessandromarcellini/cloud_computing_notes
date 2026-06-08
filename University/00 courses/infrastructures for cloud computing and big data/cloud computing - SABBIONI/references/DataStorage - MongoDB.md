MongoDB is a **Document oriented database**, particularly optimized for single write and read operations. Still if we need to do range operations we can create custom indexes that will speedup quite a bit the range read but slowing down writes.

As most NoSQL database it follows the **in-memory first** principle, both for computation and storage, speeding up operations but sacrificing a bit of consistency.
In fact. MongoDB is really oriented towards the **AP** of the [[CAP theorem]], prioritizing availability and partition tolerance (called **sharding**).

Basic concepts include:
- **Collections**: group of document sharing the same basic index;
- **Shard**: partition of a collection;
- **Consistency**: the actual **data** is **eventually consistent**, but the **metadata** (such as indexes and partition hashes) are **strictly consistent**.

## Logic Architecture

![[MongoDB - logical architecture.png]]

The basic architectural elements of mongoDB are 3 and they're:
- **Router**: it is the component that interacts with the clients, accepting and handling requests from outside and translating them in actual db operations. The router is called **mongos**;
- **Config Server**: Its the component responsible of **storing the metadata**, keeping track of the shards, what's in them and their state (remember, metadata is consistent);
- **Shard**: Actual element storing the data. it is a partition of a collection and exposes it to the router (remember, data is eventually consistent). The shard is called **mongod**.
## Request's Workflow
![[MongoDB - architecture deployed.png]]


As we can see in the picture when a request arrives the workflow is as follows:
- The router intercepts the request and **checks** (if needed) on the **configserver** the location of the shard it needs to contact (this is the consistent part);
- After retrieving the metadata of the shard it actually **contacts the specific mongod instance** to do the operation (==Write operations are mostly done asynchronously==);
- The shard responds to the router and the router forwards the computed result to the client.

## Balancing Shards
If a shard gets too big we need to handle the **resharding** (or shuffling), moving data across the network and doing really expensive operations.

There are 2 possible approaches to this:
- **Splitting**: splitting the shard into 2 or more and allocating them onto other disks;
- **Balancing**: migrating chunks of data onto other existing shards.

>[!note]
>This also means that the configserver metadata needs to be updated (consistently)
## Replication
Replication in MongoDB follows a **leader-follower** approach **for shards** (not entire db, one leader per shard).

To implement this it uses the [[RAFT]] algorithms, propagating logs (kept in memory first, using **journald**).


>[!question]
>Difference between replication of etcd and mongoDB?
>
>They both use leader-follower and RAFT to keep everything consistent and to handle leader election but the key difference is that etcd defines a leader for the entire db, mongoDB has a leader for each shard.

>[!note]
>Having a leader per each shard is really expensive. In recent updates there's the possibility of having one leader for multiple shards.

## Operation rounting
**Write** operations need to pass through the **leader first**, that logs the operation and propagates it to the follower, most of the time doing the operation asynchronously.

For reads multiple different approaches can be executed:
- **Primary first**: This is slower as the primary shard acts as a bottleneck but it also ensures consistency of data;
- **Secondary**: This is faster as requests can be load balanced but results may be inconsistent;
- **Nearest**: distributing replicas of mongod near the pods that need to access them is a really nice way of doing things following the **principle of data locality**. MongoDB has the possibility of distributing requests onto the nearest replica of the shard.