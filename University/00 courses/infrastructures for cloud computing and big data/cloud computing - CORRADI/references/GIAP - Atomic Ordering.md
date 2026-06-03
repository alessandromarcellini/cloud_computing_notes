In atomic ordering all receivers of the group will receive **all the messages** sent by the senders **in the same exact order**.



>[!note]
>usually the order of the messages is **not deterministic**, but it is ensure that it will be the same for all the group entities. 

>[!info]
>A possible implementation of this could be to have an hub that receives all messages and sends them to the receivers in the same order.
>It is feasible, simple, but introduces a single point of failure.

