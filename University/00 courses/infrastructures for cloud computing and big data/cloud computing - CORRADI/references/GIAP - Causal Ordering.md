Causal ordering is an **Extension of FIFO** ([[GIAP - FIFO]]) introducing the **possibility** to order **messages sent from different event source**.
It is particularly designed for scenarios where multiple senders communicate with the same receiver group and their messages are causally related, meaning that some messages (called **effect**) may be generated as a consequence of others (called **cause**).
The delivery order, therefore, must be preserved also among different senders in order to preserve the cause-and-effect relationships of the messages.

![[GIAP - Causal ordering.png]]

>[!example]
>in the example above a2 (sent by senderA) is said to be the cause of message b1 (sent by senderB).
>Because of this relationship between the messages, if causal ordering is implemented, all receivers of the group should receive the messages in the order:
> `a1,a2,b1,b2`

>[!note]
>If no message is related to another then causal ordering becomes a normal FIFO.
