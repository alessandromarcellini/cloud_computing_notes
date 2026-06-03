ISIS is a **unix-based Operating system** that has the protocols and **mechanisms** to order messages and synchronize access to resources **embedded** in it.
In particular it supports different types of broadcast (**BCast**):
 - FBCast (FIFO BCast);
 - CBCast (Causal BCast);
 - ABCast (Atomic BCast);
 - GBCast (Group BCast).


There's no central entity in the group but for any operation there can be an entity called **manager** that manages the communication for that request. The manager is usually chosen dynamically.

## ABCast (Atomic Broadcast)
![[GIAP - ISIS ABCAST 1.png]]
In an ABCast request ISIS chooses how to **order the request** with the other request that are being processed or are arriving using the same old method of Timestamping.

In this case the ABCast request arrrives to one of the entities of the group (the manager, dynamically chosen). The server sends $(N-1)$ messages to all the other components of the group to get their timestamp.
All other entities respond with their timestamp.

The final timestamp of the operation that will decide the ordering of it will be the max of all Timestamps that have been shared between the entities in this communication (sent from the manager to all other entities, the final N-1 messages).

>[!important]
>I'm not that sure I got this right, please check these informations.


>[!note]
>still cost is $3 \times (N-1)$ (without considering possible broadcast)



>[!important]
>It is not really important to choose the max T, this is just the first implementation of ISIS, we can decide whichever policy to order the messages, as long as all the copies order (and therefore execute) their message in the same way.

## CBCast (Causal Broadcast)
Causal, so only cause and effects are ordered


Non l'ho seguito, mi è sembrato uguale  a quello di prima.


>[!important]
>If the effect reaches first the group, it gets labeled with a T, it gets processed like a normal message.
>When the cause reaches the group the group needs to **detect** that something went wrong and either **revert** to original state or send an error.

>[!note]
>Same protocol and cost for Atomic broadcast, still $(N-1$)

## GBCast (Group Broadcast)

As we said, all these types of communications work great when the group is static; but in real world replicas go down from time to time, faults occur, or the number of replicas can scale.

The GBCast solves this problem by providing every server with a table that represents the current state of the group (each row represents one of the other servers). The table can be altered to add or subtract servers, keeping track of the system, checkpoint and adjusting accordingly to its evolution. This solves lots of problems, for example, when a copy goes down during one of the 3 phases of coordination.

