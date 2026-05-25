
In order for functions to execute we need to have **3 main elements** inside the node hosting the function:
-  The function itself, to run and manage the incoming event;
-  An execution environment for the function;
-  An invoker: an entity that activates the function.
## Function
This is the component that actually runs and "responds" to the incoming event.

## Execution Environment
Functions need to be executed inside an environment that provides them with all the needed features and, especially, features that enable the provider to move the function across nodes or entire DCs, manage its lifecycle, and so on. This means that the Execution environment needs to have all the **4 main characteristics of Containers** (TODO ADD LINK).
Each provider creates and manages their own execution environments, specialized for the technology in which the functions are written, in order to **optimize** their startup, execution and overall resource usage.

>[!example]
> a nodejs function once would take 3 seconds to startup, on aws the startup of it is subsecond

>[!note]
>This is why running the same function on 2 different provider has different performances.

## Invoker
Functions and execution environments are ephemeral, continously created and destroyed.

A third component, called invoker, is instanciated on the node to solve this problem and manage the function instances.
It basically is the relative to the Kubernetes kubelet (TODO ADD LINK), a long living proxy that can receive events, forwarded to him by the controller, and create locally (on the node it runs on) functions inside execution environment (or send events to the existing instances).

>[!note]
>The invoker stays active on the node for the whole life of the node itself