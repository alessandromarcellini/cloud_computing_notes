![[LS ring.png]]

A ring can be used to distribute load across nodes without having a single point of failure and still having easy management when an entity joins the group.

Entities are disposed as a ring where everyone know its neighbors and can communicate with them.
A token (special message) is circulating in the ring.
At each request of resource allocation, the entity that has the token decides to which entity to dispatch the resource, then passes the token to the next entity.

>[!note]
>This introduces the problem of regenerating the token if the machine that is currently holding it fails.
>This is not so easy as multiple machines could be sensing the token is lost at the same time and try to regenerate a token. If this happens the system will have 2 tokens at the same time in it.
>

>[!important]
>This is a pessimistic proactive strategy.
>
>Not really used nowadays

## Pros
- No single point of failure;
- Easy for machines to enter or leave the ring while still keeping the logic untouched.
## Cons
- Added complexity;
- Added traffic.
