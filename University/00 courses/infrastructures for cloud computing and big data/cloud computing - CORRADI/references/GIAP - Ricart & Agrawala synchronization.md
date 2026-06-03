The Ricart & Agrawala algorithm is an extension and optimization of the [[GIAP - Lamport synchronization]] algorithm, reducing its cost.

The key factor that changes is that if a process **receives a request** to access a resource but the process himself is **currently accessing** the requested resource, or it has **requested itself** to access the resource with a **lower timestamp**, than he just doesn't reply to the requester until he releases the resource (no message of release, just response to all the ones that have requested the resource in the meantime).

By doing this a **process will have access to the resource only when he receives all the responses** from the other processes, if the resource is already taken or it is claimed by someone with an higher priority he will not receive the response until the resource is free.

>[!important]
>This approach reduces the cost of the algorithm from $3 \times (N-1)$ to $2 \times (N-1)$

>[!note]
>This can cause problems because the delayed response can be confused with a fault or message lost.

