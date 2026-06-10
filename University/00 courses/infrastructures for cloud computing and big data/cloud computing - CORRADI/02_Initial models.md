## About Abstraction and Transparancy
Where complexity lays, abstraction is often needed.
Abstraction enables **to take away informations of the underlying complexity** in a simpler high level system (of course, at the cost of introducing some overhead) that usually acts as a black box.

When we say that abstraction gives a black box point of view to the upper layer we intend that the underlying complexity is transparent, not visible, not easily noticeable.
Transparency in systems:
- **Access** transparency: homogeneous access for both local and remote;
- **Performance** transparency: same performances on really different instances (e.g. local and remote);
- **Fault** transparency: if a fault occurs we don't notice it;
- ...

## Logical Architecture of a software system

![[Initial models - planes.png]]
Usually when we design a cloud software system we divide it into 3 planes:
- **User plane**: the one where the actual application runs and opeates;
- **Management plane**: the one that collects metrics from the application about its performances (this is ==monitoring==);
- **Control plane / Signaling plane**: the one that reacts to system changes (mostly based on the metrics gathered by the management plane). (This is ==observability==)

>[!question]
>What's the difference between monitoring and observability?
>
>Monitoring is the gathering of metrics, observability is analyzing those metrics in order to make changes in the system for better performances.

>[!note]
>Monitoring and observability shouldn't take much resources, we need to follow the **minimal intrusion principle**, the most resources should be allocated for the application's instances themself, not to monitor them.
>
>In modern applications usually monitoring and observability don't take up more than 5% of the resources


## Service Oriented Architecture (SOA)
Service Oriented Architecture is a model imposing some general principles and methodologies to design high-end and performative software.

It relies on these principles:
- Each service should be available anywhere;
- Each service should **have an interface** that it implements, in order to have consistency across instances (called providers);
- The service should be **composable and reusable** so that we can compose multiple services (e.g. creating a pipeline) to implement different functionalities;
- requester and service (provider) should be **loosely coupled**;
- Each service should be **stateless**, in order to enable easier replication and scaling. The state should be kept by the client;
- In the system there should be a Discovery service that keeps track of the interfaces of services and who implements what, in order to guide the client on reaching the service he wants (on the most convenient instance of it).

	![[Initial models - SOA.png]]
As shown in the picture each service provider should register its service on the discovery agent with its interface.
When a requestor comes wanting to use a service he looks it up via the discovery agent and then interacts with the actual service's instance.

>[!note]
>Legacy applications where developed in a **SILOS** approach, having big teams developing big monoliths, not easily scalable and reusable.
>Nowadays we use more of a SOA approach.

>[!note]
>SOA was first thought of around 90s and was one of the first concepts that was moving towards a similar to [[01 Microservices and containers]] approach. 
>Starting to move towards more fine-grained (as opposed to coarse-grained) systems, that can be reusable and composable.


## Active Objects vs. Passive Objects
The cloud is moving towards a more active-object approach, where resources are not only there to interact with but do stuff **proactively** and **reactively**.
More like **Actors** than normal objects.

## Components & Containers
Going from big coarse objects towards having **smaller composable and reusable** elements that are **standalone** (**self-contained**) (so they do not rely heavily on the presence of other components).
So, components, **encapsulate resources** that can be ==easily moved== from one environment to another, replicated and scaled.
Also, components, usually enable communication with other entities using their **IN port** and **OUT port**.

But **Components do not run alone**, they run inside a special environment called container (or middleware).
The container is like a manager / operating system for components handling their lifecycle, resources, communications and so on.

So:
- the **component** handles the **business logic**;
- the **container** is infrastructure for the component;

>[!note]
>SOA is basically an evolution of component thinking applied to networked services.

>[!note]
>middlewares are an implementation of this concept, containers.
