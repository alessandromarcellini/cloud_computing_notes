In FIFO ordering the order of messages is preserved for messages sent by the same sender.
![[GIAP - FIFO.png]]
In the example above is shown how the messages of each sender are received in the exact same order in which they've been emitted by the sender itself. Still order with messages of other senders can be different.
Receiver 1 receives Sender A's messages first, whereas Receiver N receives Sender B's messages first. However, in both cases, the messages from each sender are still delivered in order.