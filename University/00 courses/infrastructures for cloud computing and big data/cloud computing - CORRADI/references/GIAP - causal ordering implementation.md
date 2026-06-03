
![[GIAP - lamport causal ordering.png]]


Any process $P_i$ has a logical clock $LC_i$ (timestamp)
- Everytime a process **sends** a message **it increases its logical clock by one** and with the message it **sends also its clock**;
- Everytime a process **receives** a message it checks its own LC, the one of the sender, takes the **maximum** number between the two and **increases** it by one. This will be its new logical clock.

In mathematical formulas:
$$LC_{receiver} = max(LC_{sender}, LC_{receiver}) + 1$$

This ensures that each message is tagged with a logical clock and following the tags you can reconstruct the order of those messages.

>[!note]
>This works, as shown in the above picture,  with **any starting LC** for each process.

>[!note]
>The more a process receives messages the more it increments its logical clock

>[!note]
>Events b1 and c1 should be concurrent but the system creates an order for them too as LC(b1)=27 and LC(c1) = 9. In reality they're not related, Lamport's Clocks function is **not bidirectional**.

>[!note]
>**if LC(a) = LC(b) they're concurrent**


