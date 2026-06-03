## Consistent cut

![[GIAP - snapshot consistent cut.png]]
## Inconsistent cut

![[GIAP - snapshot inconsistent cut.png]]
## Protocol
Assumption:
- each node can reach all other nodes (No partition);
- each node on every communication port with the others (both in and out) has a queue of messages.
- the messages that say that a snapshot needs to be done are called **markers**;


the idea to keep everything consistent is to **save all the messages in transit before doing the snapshot**.

As soon as a node starts doing a snapshot it puts in each "out queue" a marker to propagate the snapshot.
The marker will be processed (creating the actual snapshot) after all the other messages that have been sent prior to it have been processed themselves, keeping everything consistent.
An other similar approach (the one professor talks in his lectures) can be to **save all the messages in the queue prior to the marker in the snapshot itself**, in order to replay event the communication.


>[!note]
Corradi didn't speak of this during the lecture but I assume that if in 2 of the "in" queues of a node a marker is depositeted the node will execute 2 snapshots, keeping the last one as the consistent one.



