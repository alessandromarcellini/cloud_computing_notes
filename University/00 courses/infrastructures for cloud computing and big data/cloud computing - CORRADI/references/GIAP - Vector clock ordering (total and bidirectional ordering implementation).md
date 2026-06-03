
Vector clocking was invented to overcome the fact that the other 2 approaches ([[GIAP - causal ordering implementation]] and [[GIAP - total ordering implementation]]) give an artificial ordering to events that are not related with each other. This is because the function used is not bidirectional and so if $LC(a) < LC(b)$ doesn't necessarily mean that $a \rightarrow b$
 
![[GIAP - vector clocking implementation.png]]

Each process knows how many sender processes are there and keeps a **vector of length equal to the number of total senders** of **LCs**.
When sending a message the same protocol as total ordering is applied with a difference;

The **sending is extended**, the process increments by one its LC and then it **sends the whole vector of LCs** that he's keeping.

The **receiving part is extended**:
- The **receiver increments** its own LC by one unit (every time);
- The **receiver updates** the other LCs of the vector with $max(LC_{sender}[counter], LC_{receiver}[counter])$ Where counter goes from 1 to N (number of total processes).

A vector clock carries the **causal history** of a process.

It does not only record the logical time of the process itself (e.g., _"P1 is at logical time 3"_), but also the most recent logical times of all other processes known to it.

Therefore, when a message is exchanged, information about the causal past of multiple processes is propagated throughout the system. This allows processes to reconstruct cause-and-effect relationships between events and to distinguish them from concurrent events.


>[!note]
> This approach is purely academic, it is not really used in real situations as it is too expensive

>[!note]
>At the start of the interactions each process knows only its LC, so the other entries in the vector are set to 0.

