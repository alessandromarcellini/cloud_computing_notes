Atomic
Consistent
Isolated
Durable

When an operation is ACID it means that it is perfectly replicated to each copy in the same and consistent way.

This is not so scalable and manageable for really large systems, usually consistency is sacrificed to provide high availability and partition tollerance.