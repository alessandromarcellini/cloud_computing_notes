RAFT is an algorithm to keep state consistent across multiple stateful instances of the same resource.

## Protocol
It uses a **leader-follower** approach, sacrificing quite a bit of availability **prioritizing consistency** of the data.
The protocol is quite easy:
 - A leader is elected between the instances;
 - All **write operations** pass through the **leader** first and then get propagated to the follower replicas;
 - Reads can pass through leader or replicas depending on the implementation (passing through leader means loading the bottleneck with more requests but having consistent answers);
 - First thing done by the leader when receiving a write is **logging it** in an append-only log that gets **propagated to the replicas**;
 - only after **reaching the quorum** ($50\% + 1$ followers responded to the leader saying they've logged the change on their system too) the **commit** is done and the **response** saying everything went well is sent;
 >[!important]
 > Reaching the quorum is the most important thing, as if the leader fails during the propagation we can know which is the latest updated log between the ones on the other nodes, as it is the one shared by $50\%+1$ instances.
 > 
 > Restoring the current consistent change is a matter of following and applying those logs on the follower instances.
 
## Leader Election
Having a leader-follower system also means having to undertake the possibility that the leader fails, leaving the system with no leader to process incoming requests.
If this occurs it the **followers's responsibility to notice the failure** (through missing heartbeats) and start executing the leader election protocol.

The protocol consists of this:
- When a follower that the leader may be down it **starts a random timer** in itself;
- When that **timer runs out** he **sends the other instances its request** to become a leader;
- If the receiving instance has its **timer still running** at request reception it needs to stop it and **vote for the requester** to become the leader;
- If the receiving instance's **timer has run out** and they've sent the request to become a leader they **don't vote for the requester** to become a leader;
- Finally, when everyone voted, the **follower with the most votes becomes the new leader**.

>[!important]
>To make this algorithm effective its important to have an **odd number of instances**, to prevent ties.

>[!note]
>The **random timer** is, in fact, random but needs to be **much greater than the latency between instances**, to assure that normal communication delays are not mistaken for a failed leader, and to reduce the chance of multiple followers starting an election at the same time.

