In the course we particularly focus our attention on understanding the underlying infrastructure that supports the operation of really big and widespread cloud systems and big data platforms. In this section we'll see a bit of the basics of where cloud systems are hosted, how these physical systems are organized and how they operate.

## Data centers
A datacenter is a facility that houses computer systems and related equipment used to store, process, and distribute data and applications. We focus on datacenters that are particularly built for cloud systems but multiple types of datacenters are present across the globe, even specialized for different tasks (e.g. HPC).

![[Datacenter.png]]
DCs are usually organized in parallel distanced rows composed of racks where the actual machines are placed (servers, switches and so on). The more the distance between the rows the less the entire system needs to be cooled down.

### DCs internal organization
These facilities need to be organized in a logical way to increase the performances as much as possible and to limit the resource usage.
We've seen both:
- [[Basics - DC interconnection]]: 2 methods on how the machines are interconnected with each other;
- [[Basics - Compute architecture]]: how the machines are organized to distribute compute load.
### Modern DCs (hyperscalars and cloud continuum)
Nowadays there are so many workloads that need to be run and so complex that the scale of these facilities has grown a lot, going from small DCs to hyperscalar ones.
But size isn't the only thing that has been revolutionized during the last years. Also the way datacenters are designed and organized has changed deeply.
Nowadays big datacenters need to optimize every single aspect, from cooling, to energy consumption, to machine's usage, in order to sustainable, to save up money and be still competitive on the market.

Because of this different strategies arised, not only on the single datacenter but also in the organization of them.

We went from a monolithic datacenter structure that had lots of resources and all the traffic was routed there, to a more distributed approach (just like in software architecture, see [[01 Microservices and containers]]), where there are still really big datacenters handling lots of requests and services, but these communicated with lots of other "local" datacenters distributed around the globe, that can serve part of the traffic for a certain region, with less latency and with costs optimized for the region they're located in (e.g. enery cost is less in a region than in the one where the hyperscaler is located at).

This created what's called as **cloud continuum**: the capacity of **distributing workload locally** if it is better or **going global for optimization**.

**Cloud continuum** is the seamless integration of computing resources across environments—from ==devices at the edge==, to ==on-premises infrastructure==, to ==public and private clouds==—allowing applications and data to run where it makes the most sense based on performance, cost, security, or other requirements.
## A bit about Big Data and their handling
Nowadays we talk about big data. This data is big in all its characteristics:
- Volume;
- Variety;
- Value;
- Velocity;
- ...
We need to have systems and infrastructures that are able to handle all this incoming data, changing lots of paradighms from the normal "in the small" handling.

The most important thing in this data handling and the biggest revolution of cloud in general is that **consistency (or safety in the sense of correctness) is not guaranteed**. This systems in order to not lose the market need to be always highly available, have very little latency and so on, this means that the [[ACID]] approach can't be followed every time.

>[!note]
>The one of the first companis that decided to take this path was ebay.


## A bit about middlewares
Middlewares are one of the most common infrastructural elements that really enable better and more controlled communication.
They can bufferize requests, save them for recovery, rate limit them and so on.

They're also really used by cloud providers in order to grant different QoS tiers to different users.
## Cloud providers
They have DCs and manage them. It's really important to leverage every single resource in the datacenter (e.g. 130% load on each cpu is the goal) in order to have the most cost-revenue possible.
This means that they need to handle things elastically, if an application scales down the other one can take up its space and use it to scale up.