Distributed transaction niddleware makes it easy to develop systems that need [[ACID]] behaviours in its computations.

As for the cloud usage of this, usually this is implemented in a more relaxed way, as full consistency is often not reachable while providing high availability and partition tolerance.
[[2 Phase commit]] is too expensive