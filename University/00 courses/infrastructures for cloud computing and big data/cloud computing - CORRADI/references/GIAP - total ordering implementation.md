
![[GIAP - total ordering implementation.png]]
This approach builds upon the [[GIAP - causal ordering implementation]], **extending it**.

The extension consists on **appending** not only the **LC** to the message but also the **Process ID**.

By including the Process ID, we can establish a **deterministic priority rule**: if two messages share identical LC values, **tie-breaking** is resolved in favor of the process with the **higher priority ID**.