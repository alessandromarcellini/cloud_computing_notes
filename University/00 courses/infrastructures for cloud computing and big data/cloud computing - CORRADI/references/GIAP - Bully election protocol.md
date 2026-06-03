In this election protocol the nodes are distributed in an **hirarchy**, dictated by their ids.
The higher the id the higher the priority of the node.

When a process detects that the current leader has failed it initiates a bully election.
The bully election consists of these phases:
-  The initiator sends an election message to all nodes with an higher id;
-  If an higher id node responds to this message it means that that node is going to take care of the election from now on;
-  The higher node that just responded does the same with its higher nodes;
-  The protocol goes on like this until some node doesn't receive any response from higher nodes, he will be the new leader;
- The new leader notifies all the other nodes that the election has concluded and that he's the new leader.

>[!important]
>The **cost** of this is **variable**. This can generate a lot of overhead if the one that detects that the leader has failed is a very low priority node.

>[!note]
>usually higher nodes have more capabilities, usually in terms of hardware.
>
