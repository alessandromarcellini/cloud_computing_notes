Not really strict consistency while updates are done but, ==eventually, when updates stop==, the system will ==converge to a consistent state==.

To implement this usually the update is broadcasted to each copy of the data without waiting for an ACK to get back.

Then, at some time in the future, a **dissemination protocol** (like gossip protocol) is used to spread the updated information among the other instances.