**BASE** is a design philosophy for distributed/cloud systems. It stands for:

- **B**asically **A**vailable: The system tries to remain available even when parts of it are failing;
- **S**oft state: The system doesn't have a durable memory;
- **E**ventually consistent: If no new updates occur, all replicas will eventually converge to the same value.

It emerged as an **alternative to strict ACID** guarantees in large-scale distributed systems where availability and scalability are often prioritized.