When we talk about **load balancing** we're talking of the practice of  ==migrating== resources to different locations ==at runtime== if it is worth it for a performance increase.

>[!note]
>Load balancing means also that I/O needs to follow the instance in its migration, without losing incoming requests. This is not that easy, processes are **really bound** to the underlying system because of I/O.

## Possible problems to take into account
- Avoid trashing: continuously moving a process without having the time to make it execute;
- Avoid moving processes that are about to finish their execution;
- Avoid moving processes that are heavily using local stuff (data);
- Have a strategy to avoid losing requests and to make the clients know the new location of the process;
- ...

## Migration strategies
###### Data
To tackle the problem of data that is being used by the process locally in the early days people started to adopt only shared and distributed file systems, in order to not have data on the actual machine on which the process is running but have it accessible on the network


###### Messages migration
When we're migrating a process while running the process can still receive requests. It is our job to ensure that no message is lost and that their management is as efficient as possible.
These are some strategies:
 - **Message Redirection**: The request arrives to the old node on which was the process and the old node is keeping track of the new location. The old node simply ==forwards== the message to the new node; This can become a chain and isn't really efficient;
 - **Requalifying allocation**:  The old node is still keeping track of the new node on which the process was allocated and when a request comes it ==responds with the new location== of the process, without forwarding anything;
 - **Client recovery**: the old node doesn't respond at all and when the client senses that the process was moved it sends out a probe request ==flooding the network== to find the new location;

## Migration policies
We need to find a way to define policies to guide the migration.
In particular about the evaluation of the situation to decide **when** to migrate, **who** to migrate and **where** to migrate.

There are 3 ways to do it:
 - Static: having static policies means that everything is predefined.
	 - creating static tresholds that if overcome start the migration process.
	 - Deciding the process to move statically (e.g. the newest one)
	 - Deciding the location statically (e.g. the closest);
 - Semi-dynamic: same as static but using varianble tresholds
 - Dynamic: depending on the current state of the system decide dynamically how to operate.
>[!note]
>Dynamic approaches and are difficult to implement while respecting the minimal intrusion principle.
>One strategy to do so is to partition the system in regions and apply a dynamic approach on each region, to avoid having to control the whole system at once.

>[!note]
>Usually really simple policies are the best in order to provide reasonable migrations while keeping the minimal intrusion principle in act.
>
>More sophisticated policies do not make the system significantly better.
## Migration decision
Who's the one that is checking the system's performance and decides when to migrate stuff?
There are 2 possible approaches:
- **Centralized**: a unique entity monitors the system and decides when, who and where to migrate. ==Has more cost but is more precise==;
- **Decentralized**: distributed decision between nodes, usually organized in partitions to limit overhead. ==Less precise but more reliable==.
	- **Receiver initiative**: when a node is underloaded it proatively looks for load notifying the other nodes that they can migrate stuff to him;
	- **Sender initiative**: when a node is overloaded it looks for another node that can receive the load (suitable for ==low loaded systems==);
	- **Mixed implementation**.
