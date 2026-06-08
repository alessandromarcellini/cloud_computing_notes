This is the feature that changed kubernetes from being a technology to becoming an actual framework.
The operator pattern is a pattern that can be leveraged to create custom resources and associate custom controllers and reconciliation loop logic to them.

A **CRD** (Custom resource definition) can be stated as a yaml file, indicating the resource name, namespace, its metadata, the specs and so on.
When a **CRD is applied** inside the cluster the api server automatically **creates an endpoint** to handle it.
The **controller** that we associate with the resource **will use this endpoint** to keep track of the resources and their states.

>[!note]
>One main difference between standard controllers and these custom ones is that standards are spawned as threads of the controller manager (control plane).
>
>These, instead, are spawned inside pods (in workers), but still have their own custom controller manager.


## Related concepts
- [[Internal development platforms]]