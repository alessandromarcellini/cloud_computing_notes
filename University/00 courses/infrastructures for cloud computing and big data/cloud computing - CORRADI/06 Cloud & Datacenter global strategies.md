When we talk about cloud we talk about complex systems that serve millions of users at the same time, in a seamless way.
To do so, nowadays, different strategies are adopted that make it seem as if everything is safe and correct during the communication process, but this is not always the case and inconsistencies are often hidden to the final user and resolved after a while.

## CAP Theorem
[[CAP theorem]]

---
## Cloud 2 Layers structure
![[Cloud Globality - 2 layers.png]]
In order to provide users with fast answers while still keeping some consistency a 2 layer of the cloud approach emerged.

The 2 layers are:
- **External (Edge)**: a layer usually closer to the user, that is responsible for ==responding fast==, ==not safely==. Because of this responses could be incorrect or not aligned with the system's real state.
  Strategies could include:
	- Guessing the answer;
	- Use cached data;
	- Use data that we don't know is consistent with the other copies;
	- ...
- **Internal**: the internal layer is the one that actually ==keeps the state== and tries to ==keep it consistent==, communicating with the external layers. ACID is implemented at this level, not on the edge. If inconsistencies occur they're resolved asynchronously during runtime (**Reconciliation**).

###### Replication of Edge Layer
Edge layer is strongly replicated as it is the one in charge to communicate with the users. Sometimes even 1 copy per client or more.

###### Replication of Internal Layer
Even the internal layer is replicated, mostly partitioning the data in order to minimize conflicts of the same shard. So the data is divided into shards, partitions of the original data.
The size of the shard is usually dynamic and shrinks or grows depending on the traffic.
The more clients are working with a shard, the more it usually shrinks and divides itself into more shards, in order to avoid conflicts.

---
## Several types of consistencies
In cloud systems different types of consistencies are available and suitable for different systems:
- **Eventual** consistency ([[Cloud Globality - Eventual Consistency]]): not really strict consistency while updates are done but, ==eventually, when updates stop==, the system will converge to a consistent state;
- **Strict consistency**: ACID, all ==writes and reads are blocked while== a write is processing. Everything is consistent at all times.
- **Linearizable** consistency: writes are put in an ==ordered queue== and ==only writes are locked== while another write is processing. Reads can go on.
- **Sequential** consistency: same as ==linearizable but only writes coming from the same entity== are ==put in an order==. 

## Ebay's 5 commandments
1. Partition Everything: divide the system in more easy to handle shards;
2. Use asynchrony Everywhere;
3. Automate Everything: no man should be in the loop;
4. Remember: Everything Fails;
5. Embrace Inconsistency: strict consistency can't be granted.


