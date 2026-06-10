Cassandra is a wide-Column oriented storage database. originally designed at Facebook open-sourced later, today an Apache project

A distributed key-value store intended to run in a datacenter (and also across DCs)


Cassandra maps several Data centers to be organized in a global Cloud, interconnected by high bandwidth and high Quality communications among each others


Configuration Cassandra API Tools R/W operation support in memory caching Management Replication Membership Consistency level


CASSANDRA DC MODEL

Any family of data must be based on replication widely localized: several copies in different DCs and several ones in anyone of them

Any DC must optimize access to its copies and should have some mechanisms to ease the access (key-values, DHT, local ring configuration, …)

Some policies for configuration must be decided and actuated (out of band, before data access) and also data operations must be monitored and controlled during execution (in band monitoring, dynamic reconfiguration, ….)

Cluster is the set of all possible servers in all data centers
DataCenter is the set of all servers in one DC, organized as a ring and the base for the replication
Rack is the set of local servers in all data centers, where at least one rack must be present as the configuration unit
Server is the instance present on one server, and it can contain several virtual entities
Virtual server is the VNODE normally controlled automatically with a Cassandra load of C > 1,2 the goal is 256 on one serve

The configuration is automatic and dynamic

VNODES VIRTUAL DISPOSITION
Cassandra maps virtual nodes in a ring on Nodes (servers)

Partition basic data unit replicated on Vnodes Controlled by a Partitioner


How do you decide which server(s) a key-value resides on?
The main point is to map efficiently and in a very suitable way for the current configuration based on the different data centers and on the placement of replicas there
So that it can change and adapt fast to needs and variable requirements and configurations

KEYSPACES AND REPLICATION
KeySpaces (KS) are namespace container that defines the data replication on nodes and how they contain tables, in number of replicas and their replica placement strategy KS has a Replication Factor (RF) and replica placement strategy: • Data replication is defined per datacenter • max (RF) = max (number of nodes) in only one data center

DATA PLACEMENT STRATEGY
Two different Replication Strategies based on partition policies:


1. SimpleStrategy: in one Data Center with two strategies of Partitioning:
	1. a. RandomPartitioner: Chord-like hash partitioning
	2. b. ByteOrderedPartitioner: Assigns ranges of keys to servers • Easier for range queries (e.g., Get me all twitter users starting with [a-b]);

2. SimpleStrategy: NetworkTopologyStrategy: for multi-DC deployments.
	1. a. Two replicas per DC
	2. b. Three replicas per DC
	3. c. Per Data Center • First replica placed according to above Partitioner • Then go clockwise around ring until you hit a different rack

SNITCHES
Uno **snitch** dice a Cassandra:

> “Quali nodi sono in quale datacenter e in quale rack?”


Snitches must map IPs to racks and DCs they are configured in cassandra.yaml config file Several options: • SimpleSnitch: Unaware of Topology (Rack-unaware) • RackInferring: Assumes topology of network by octet of server IP address • 101.201.202.203 = x... • PropertyFileSnitch: uses a configuration file • EC2Snitch: uses EC2 • EC2 Region = DC • Availability zone = rack



OPERATIONS
WRITE
must be lock-free and fast (no reads or disk seeks) Client sends write to one coordinator node in the Cassandra cluster: • Coordinator may be per-key, or per-client, or per-query • Per-key Coordinator ensures that writes for the key are serialized Coordinator uses Partitioner to send query to all replica nodes responsible for key When at least X replicas respond, coordinator returns an acknowledgement to the client X is the majority