Of course machines inside datacenters need to communicate with each other. So how the network interconnection is organized is a really strong and important topic to address.

## Types of traffic
First of all to describe how the machines are interconnected we need to know which types of traffic can occur inside our architecture.
- **North-South**: from outside the datacenter (the internet) (north) to a particular server (or set of servers)(south). So, this type of traffic, goes from top to the bottom of the interconnection architecture;
- **East-West**: from a particular server to another particular server (in the same datacenter). This type of traffic goes across our interconnection architecture, from one server to another.

## 3 tiers (almost legacy)

![[Basics - DC 3 tiers.png]]
###### Core Layer
- High-speed backbone of the data center (**high bandwidth**).
- Moves traffic between different parts of the network.
- Designed for **maximum throughput and reliability**.
They're **connected with all the aggregation layer machines**.
###### Aggregation Layer
- **Connects multiple access switches together**.
- Applies network policies, routing, filtering, and load balancing.
- Acts as an intermediary between access and core.
Each aggregation machine is connected with a **particular set of switches**.
###### Access Layer
- **Connects directly to servers**.
- Servers plug into access switches.
- Handles device connectivity.
Each access machine is connected with a **particular set of servers**.
###### Cons
**East-west traffic is NOT particularly optimized** as, in the worst case scenario, we could have to **go up** in the infrastructure to the core layer in order to communicate with a different server.

Also, as said, the routing and the slowness of the communication really depends on where the 2 different servers are located inside the infrastrucure.
As an example two servers that are under the same switch (access layer) can communicate through it. But 2 servers really distant from each other in the architecture could have to go higher in the layers to communicate.

This creates **inconsistent latency**, making it not so predictable and optimizable.
## Spine - Leaf
![[Basics - DCs spine leaf.png]]
###### Spine
- it's the entry point of the Dcs (other than routers).
- Similar to access switches in the three-tier model.
- Every server connects to a leaf switch.
Every spine switch **is connected with all the leafs**. (more interconnections than 3 tiers)
###### Leaf
- Form the network backbone.
- Every leaf connects to every spine.
- Spine switches do not connect to other spine switches.
Every leaf switch is **connected with a set of servers**.
###### pros
**East-west traffic is optimized** and **consistent latency**

>[!note]
This is an implementation of a CLOS network, networks theorized by Charles Clos with the aim of being highly scalable without having to connect everything with everything.