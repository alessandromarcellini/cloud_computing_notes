Lamport proposes a **decentralized** solution without single failure points or central entities. All processes have the same "priority" over the others and the same role in the system.

To do this Lamport thought of a system in which every process is **interconnected with every other** process, and messages need to be ordered in a **FIFO** way.
>[!info]
>In a system with N processes this means that there will be $N^2$ network connections or $\frac{N^2}{2}$ if they're bidirectional.

Also **total ordering** is ensured in the system, so every process sends a message attaching to it its LC and its Process id.

---
For a process to access the shared resource it needs to initiate a **global coordination protocol**.

Each process has an **ordered** (by Timestamp and $P_{id}$) message **queue** where it stores its own sent messages (with LC and $P_{id}$) and the ones that it receives;

The protocol consists of this:
- The process that wants to access the resource sends a **request to access** **to all** others;
- When an other process receives a request it **replies** with its own LC and $P_{id}$ (this message is the effect and the request is the cause);
- The process requesting to access the resource **gradually receives all the responses** (effects) of the other processes and stores them in its queue;
- When he **received all** the responses it **checks if its request is at the top** of its queue (keep in mind its an ordered queue), if so it can access the resource, if not, it needs to wait until its request is at the top.

>[!important]
>Keep in mind that while the requesting process is waiting for responses from the others, other requests can come to him and they get placed inside the queue. If these requests have a lower timestamp the process can't access the resource straight away, but needs to wait until they're removed from the queue (see next step of the protocol)
- When a process has actually accessed the resource and has finished using it, it 
	- **deletes its own request**;
	- **deletes all the related responses** from its queue;
	- it **sends a release message** to all others;
- When a **release message** is received by a process, it **deletes the request** of access of that process from its queue.


>[!info]
>Note that the cost of this is pretty high but in ensures that the access is fully distributed, having no single point of failure.
>
>The actual cost is: $3 \times (N - 1)$

>[!note]
>It is usually used for little groups like for **active copies** (See ...), where the processes are not more than 5 and have a **static** number.

>[!note]
>If multicast is available between the entities it reduces greatly the cost of communication for requesting and releasing the resource (responses are obviously still point to point).

---
## Example

![[GIAP - lamport synch protocol 1.png]]
Initial situation, every queue is empty, no process is accessing the shared resource.

---

![[GIAP - lamport synch protocol 2.png]]
Process1 wants to access the resource so it sends the request to all the others and also stores it in its queue.
Process2 receives the request and stores it in its queue.
ProcessN hasn’t still received the request from Process1.

---

![[GIAP - lamport synch protocol 3.png]]
Process2 responds to process1 with its timestamp and process id.  
ProcessN, that still hasn’t received the request from process1, sends a request itself to access the resource.

---

![[GIAP - lamport synch protocol 4.png]]
Process2 response arrives to Process1.
ProcessN receives the request of process1, puts it in its queue and responds to it.

>[!note]
>Here a reordering of the queue of processN is performed. (Queues are ordered by T and $P_{id}$) 

---

![[GIAP - lamport synch protocol 5.png]]
Process1 receives both the request of processN and the response of it containing its timestamp.    
Now process1 has all the responses from the other entities of the group so it checks its queue.  
Process1 proceeds to access the resource since its request is at the top of the queue

---

![[GIAP - lamport synch protocol 6.png]]
Process 1 finishes using the resource, clears its queue from its own request and the related responses and finally sends a **release message** and its response to ProcessN

---

![[GIAP - lamport synch protocol 7.png]]
Request of process1 gets deleted from all the queues of each process.  
Now Process N has received all responses, checks its queue, its request is at the top so it can finally access the resource.

>[!note]
in some cases Process1 could respond to processN even while accessing the resource. Still Process N wouldn’t access the resource as its request is not at the top of the queue.

