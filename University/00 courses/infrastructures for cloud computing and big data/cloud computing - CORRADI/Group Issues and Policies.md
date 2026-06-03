
The core of this module delves into the complexities of **group communication** and **multicast semantics**, particularly highlighting different semantics of multicast and ways to handle messages sent to multiple receivers, exploring synchronization and ordering of those, going into **reliable multicasting**. 


>[!note]
>Every concept expressed in this section is based on the assumption that messages can't be lost in the system, at most they're delayed (sent another time if lost)
>
>**Negative ACK** = Acks are usually used to signal to the sender that a message has been delivered correctly. Negative ACKs do the opposite of that, signaling when a message hasn't been delivered. Usefull for stable communications.
>
>**Holdback** =  holding the message that has been delivered without processing it, to make it so that other messages that should be ordered before that one can arrive.




## Event ordering
In distributed systems there are lots of different paradigms to describe ordering of events being transmitted to a group of entities (e.g. active copies).

These include:

| **Ordering Type**          | **Description**                                                                                                                            | Type                 | **System Cost** |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | -------------------- | --------------- |
| [[GIAP - No ordering]]     | Copies process messages freely; no synchronization required.                                                                               | No ordering          | Minimum Cost    |
| [[GIAP - FIFO]]            | Messages from the _same sender_ reach group members in the exact order they were sent.                                                     | **Partial** ordering | Low/Moderate    |
| [[GIAP - Causal Ordering]] | Messages in a cause-effect relationship (even if from _multiple different senders_) must be delivered in the correct sequence to everyone. | **Partial** ordering | Moderate        |
| [[GIAP - Atomic Ordering]] | Imposes a global order; all correct members receive messages in the exact same sequence.                                                   | **Total** ordering   | High/Expensive  |

## Message relationships Expressed Formally
Relationships (of ordering) between distributed events or messages are expressed formally and mathematically using Lamport's **Arrow relationship ($\rightarrow$)** (or **happened-before** relationships), which establishes a partial "happened-before" (cause-effect) sequence.

>[!example]
>$$a \rightarrow b$$


This means event *a* causally preceded or "**happened-before**" event *b*. This happens if:

1. *a* and *b* occur sequentially within the same process.
2. *a* is the sending of a message and *b* is the reception of that same message.
3. The relationship is transitive (if $a \rightarrow b$ and $b \rightarrow c$, then $a \rightarrow c$).    

To track this ordering without relying on physical time (not reliable across different machines), a monotonically increasing integer function called a **Logical Clock ($LC$)** is assigned to events.

**The Clock Condition:**

$$\text{If } a \rightarrow b, \text{ then } LC(a) < LC(b) \text{ [cite: 69, 70]}$$


>[!warning]
(One-Way Implication): The happened-before relationship is strictly **one-way and not bidirectional**.
>
>It is TRUE that if $a \rightarrow b$ then $LC(a) < LC(b)
>
>BUT
>
>It is **NOT necessarily TRUE** that if $LC(a) < LC(b)$, then $a \rightarrow b$.



Two events can be entirely concurrent and independent ($a \parallel b$), meaning neither has a cause-effect relationship on the other. Even though they are concurrent, the logical clock algorithm will still naturally assign one of them a smaller arbitrary timestamp number, meaning you cannot deduce causality in reverse simply by looking at Lamport timestamps. (To achieve a bidirectional relationship where you can deduce causality from timestamps, **Vector Clocks** must be used instead ).

## Lamport algorithms
leveraging the concepts of logical clocks and timestamping Lamport has developed some algorithms to ensure different types of message ordering. (I'm not sure these are lamport's)
- [[GIAP - causal ordering implementation]];
- [[GIAP - total ordering implementation]].
- [[GIAP - Vector clock ordering (total and bidirectional ordering implementation)]]

## Synchronization
Synchronization in cloud systems is usually made possible by the use of event ordering.
We're not so interested in physical clock synchronization as, even if it used in some cases (See [[UTC (universal coordinated time]]) or [[NTP (Network Time Protocol)]]), it requires a lot of expenses in terms of coordination, which is too expensive in most of the cases to be done with precision or, in general, to be adopted in a cloud system.

To implement synchronization, we still **leverage message sending** between processes and the **possibility to order them**.

When we speak of synchronization in this context think of a **shared resource** that needs to be accessed in a mutual exclusive way by more than one process of a group. 

The main objectives are:
- **Safety**: only one process at a time can have access to the resource;
- **Liveness**: every process that has done a request receives the access after a limited delay (**no starving**);
- **Fairness**: different requests must be managed by a fair policy



Different approaches include:
-  [[GIAP- Synchronization with coordiantor]];
- [[GIAP - Lamport synchronization]];
- [[GIAP - Ricart & Agrawala synchronization]];
- [[GIAP - Ring synchronization]].




## CATOCS (Casually and totally ordered communication support)

CATOCS indicates the capacity of implementing, in well spread **(distributed) operating systems**, event ordering and synchronization.
In each CATOCS implementation there are all the orderings implemented, in a **distributed way**, with no single point of failure.
All them assume that we have the capacity to send messages to all the group in an efficient way, using a multicast (in ISIS called **broadcast**).

An example of this is the [[GIAP - ISIS]] OS.

## Distributed snapshot protocols
Snapshot are used to restore to a previous state (snapshot) the whole system.
The state that is saved can be successively used to **replay the system from a previous point** and restart execution in a **safe situation**.

Creating a global snapshot of the system, including lots of different nodes and communicating entities, is **not that easy**, as if it is done in a simple naive way, can lead to **inconsistencies** of the final snapshot.

>[!example]
>A bank transfer.
>
>One bank account data is on serverA (100 €) and the other bank account data is on serverB (100 €).
>If  50€ needs to be transfered from serverA and serverB and a global snapshot is taken in a naive way, the snapshot could be taken of serverA with only 50€ left, and the snapshot of serverB could be taken with 100€, not considering that there are 50€ in transit. If we end up reverting to that snapshot for any reason, 50€ are lost from the system.

of course it is not possible to block the entire system for the whole duration of the snapshot, so there are different protocols to actually create snapshot while the system is operating without creating inconsistencies.

[[GIAP - distributed  snapshot protocol]]
