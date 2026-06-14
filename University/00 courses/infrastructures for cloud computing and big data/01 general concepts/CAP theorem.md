The CAP theorem is one of the most important theorems when it comes to cloud computing.

It states that a cloud system can only grand 2 of the 3 CAP properties:
- **C**onsistency: having consistent copies and therefore always responding correctly and safely;
- **A**vailability: being always available and with low latency;
- **P**artition tollerance: being able to operate normally even if partitioned away from the rest of the system.

Nowadays most systems sacrifice Consistency creating **AP systems**, in order to stay in the market.
These systems follow the [[BASE]] design philosophy

>[!note]
>Of course inconsistencies are limited to edge cases and are often resolved asynchronously.

