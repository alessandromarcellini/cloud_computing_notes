
Declaration enables the user to only state what he wants, not how he wants it done.

This is based on the fact that the user can specify a **desired state** for the resource he wants and KIubernetes will exploit its automation mechanisms in order to get that resource to have that desired state (if it's possible).

This done by **controllers**, actual entities responsible for the management and the reconciliation of the specific resource (or set of resources) that they're responsible for.
Usually standard controllers are threads that are spawned by the controller manager, but they can also live in pods (most custom ones do it).

The controllers communicate with the api server (by polling at startup, then with a long lived http watch request), asking if at least one of the resources that they control has been either added, deleted or changed. This is called **control loop**.
In case one of the previously mentioned events occurred the controller runs a reconciliation function to get the resource from the current state to the desired one.


>[!note on the communication style]
>As mentioned the controllers start operating with a normal http request to the api-server called **LIST**.
>This is done in order to get all the resources (that the controller needs to control) already instantiated and their resourceVerion (each time a resource is changed the resourceVersion increments by one unit).
>
>After this the controller opens up a l**ong-lived http watch** request where the api-server will stream the changes of that set of resources, avoiding regular polling. 

>[!example]
> Controller to manage pods instances:
> 1. The controller does a LIST request, this will returns all pods and the resourceVersion (let's say 1000):
>    `GET /api/v1/pods`
> 2. WATCH request to get notified on the updates of the resources, starting from resourceVersion=1000:
>    `GET /api/v1/pods?watch=true&resourceVersion=1000`
> 3. The api-server streams this event:
>    `MODIFIED pod-a`
> 4. The controllers receives it and runs the reconciliation function.


