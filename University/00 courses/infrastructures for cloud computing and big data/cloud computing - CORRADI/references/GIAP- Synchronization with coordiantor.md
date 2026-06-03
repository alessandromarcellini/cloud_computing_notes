![[GIAP - Synch with coordinator.png]]
This is the simplest approach.
The access of a shared resource is regulated by an "external" **process that acts as a coordiantor**.

So each process when it wants to access a resource needs to:
- contact the coordinator to **ask** for the resource;
- the coordinator, when it's the right time for the requesting process to access the resource, responds with the **permission** to access the resource;
- After the process has used the resource it sends a message to the coordinator indicating it has **released** the resource.


>[!note]
>This is a really simple approach and its **cost is really limited** as it requires just 3 messages to go through the network for a process to acquire the resource.
>Still this is not used lots of the time because of the **single point of failure** that it introduces in the system.
