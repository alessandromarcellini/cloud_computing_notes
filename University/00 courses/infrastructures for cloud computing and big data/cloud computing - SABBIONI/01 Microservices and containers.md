In this lecture we will discuss the microservices approach to building an application, why this paradigm arised, what are its advantages and when we should design an application in a microservices-oriented approach.

Finally we will discuss what containers are and how they solve lots of the problems that a microservices approach introduces. 

## Monolithic approach
When we say an application is structured as a monolith we intend that that application is self contained in a unique environment, everything is in one place, there's no real separation between roles in the application as per the codebase.

#### Pros
Even if this approach is the first that arised for the development of complex systems it is actually not to be considerable outdated.
Infact, building a monolithic application has lots of pros:
 - It's easy to structure (easier than using other paradighms at least);
 - It's easier and faster to develop;
 - Can provide really good performances due to having everything in the same place, without having to introduce the overhead of communication (via the network) of components, the cost of coordination and so on...;
 - It has lower operational costs; for exaple: It's simply scalable, you only have one thing to scale and that's it, you don't need to take in account lots of other stuff.

So if you have to develop the thing fast, with low operational costs and you already have a small team starting with a monolith can suit your case. 

#### Cons
- No fine-grained scalability: when you scale the application you're scaling the entire application, the whole monolith.
- It takes more time to build (in microservices you can build single microservice and deploy them concurrently, see [[DevOps]])
- slow deployments (everything redeployed together);
- large codebases becoming hard to understand;
- difficult to adopt different technologies in the same project.

>[!example]
>Take for instance an application that has an admin part and another part that is available to all users.
>The application has millions of users that are interacting with it everyday but only one admin.
>When we scale the application we're forced to scale it as a whole, occupying resources also for the admin part, even if the admin part could be managed by one replica. 


## Microservices Approach
The microservices approach started to gain traction as the cloud started to become widespread (early to mid 2010s) because it addresses lots of the limitations that we underlined in the monolithic architecture, particularly around scalability, independent deployment, and team autonomy, but does so at the cost of introducing additional complexity in system design, operations, and distributed communication.
It is, therefore, often better suited to **large, evolving systems** with multiple teams rather than small or simple applications.


> [!quote]  
>In short, the microservice architectural style is an approach to developing a single application as a **suite of small services**, each running in its own process and **communicating** with **lightweight mechanisms**, often an HTTP resource API. These services are built around business capabilities and **independently deployable** by **fully automated deployment machinery**. There is a bare minimum of centralized management of these services, which may be written in **different programming languages** and use **different data storage** technologies.
>  
> — [James Lewis and Martin Fowler (2014)](https://martinfowler.com/articles/microservices.html)


#### Microservices characteristics
Each microservice:
- Is **characterized by a contract**:  The contract needs to be versionable in order to enable retro-compatibility and update the features during the application's life cycle;
- **Owns its data**: it does not share data with other microservices, it has its own stuff in its own database (or partition of database or, more commonly, in its own tables). If another microservice is in need of that data it can request it to the microservice owning it itself;
- **Is "independent" from the others**: The "owns its data" goes along with the fact that each microservice is loosely coupled with the others, and can be replaced with another component or redeployed in another version without them knowing it, still having the application function properly;
- ...;

>[!important]
>Each microservice owns its data to limit conflicts on said data (only the service itself accesses it), stay independently scalable even data-storage-wise, security to regulate access to that data.
#### Pros
Most of the pros of microservices are a direct effect of their characteristics and, mostly, the exact opposite of the cons of monoliths.

Pros are:
- **Architectural**;
- **Technical**;
- **Organizational**.


Said pros are:
- **Indipendent**: Each microservice is independently ==developable== (even technology-wise, as programming languages, data storage systems and so on...), ==deployable==, ==scalable==;
- **Faster builds**: They enable for the system to be built faster, with building only the microservices that actually changed and using rolling updates to update the system causing no downtime whatsoever;
- **Partitioned complexity**: Each microservice can be developed and managed by a ==small team== of engineers that have the ==whole codebase== (of that service) ==in their heads== and know everything about it like the back of their hands;
- ...;

#### Cons
- **Management cost**: running these systems is not as easy as deploying a monolith, here microservices need to communicate, be coordinated, be independently scalable, automatically managed to save on resources and yet provide the best user experience possible, ...
  The [[DevOps]] figure started to arise because of this;
- **Cost of automation**: this goes along with the management cost. As you have an architecture this complex you need to automate everything you can in order to test, scale and deploy in the best way possible and with limited cost. This introduces a lot of complexity and also the need of [[DevOps]] members that can manage this complex system.
- **Overhead**: this architectures add a lot of overhead in terms of coordination and also communication. The application doesn't run as a whole in the same place but components componing the logic can run in different continents and need to communicate with each other to provide with the final result;
- **Observability**: added to the cost of management there's the one of monitoring each service in order to manage it and keep it reliable. 



## Comparison between Monoliths and Microservices

| Aspect | Monolithic Architecture | Microservices Architecture |  
|------|--------------------------|----------------------------|  
| **Structure** | Single self-contained application where all components live in one codebase and runtime environment | Collection of small, independent services communicating via lightweight protocols (e.g., HTTP APIs) |  
| **Deployment** | Entire application is deployed as a single unit | Each service is independently deployable |  
| **Scalability** | Coarse-grained: the whole application must be scaled together | Fine-grained: individual services can be scaled independently |  
| **Performance** | High performance due to in-process communication and no network overhead | Potential overhead due to network communication between services |  
| **Development speed (early stage)** | Faster and simpler to develop initially | Slower initial setup due to infrastructure and service boundaries |  
| **Codebase complexity** | Can become large and hard to manage over time | Each service has a smaller, more manageable codebase |  
| **Team structure** | Often requires coordination within a single shared codebase | Enables small, autonomous teams per service (aligned with DevOps practices) |  
| **Technology stack** | Typically constrained to a single or limited stack | Each service can use different languages, frameworks, or databases |  
| **Data management** | Shared database across components | Each service owns its own data and storage |  
| **Fault isolation** | Failure in one part can impact the whole system | Failures are more isolated to individual services |  
| **Operational complexity** | Lower operational overhead; simpler deployment and monitoring | Higher operational complexity due to distributed system management |  
| **Maintenance & updates** | Slower updates since the whole system is redeployed | Faster updates via independent deployments and rolling updates |  
| **Communication** | Internal function calls within the same process | Network-based communication between services |  
| **Best suited for** | Small to medium applications, fast prototyping, low operational budgets | Large, complex, long-term systems with multiple teams |  
| **Main disadvantage** | Becomes hard to scale and maintain as it grows | Introduces distributed systems complexity and higher operational cost |




## Hosting Microservices on Bare Metal
Hosting microservices directly on top of the hardware and operating system of the host machine can be a pain in the ass, the system admin is given an environment that he needs to configure and manage, mostly running into these problems:
 - bare metal can't be shared;
 - There's no management of the services;
 - Security hazards;
 - Dependency management and conflicts resolution;
 - Possible conflicts among services;
 - Unpredictable resources allocation and deadlocks.

In general there are **4 hosting problems** that need to be addressed in order to deploy effectively an application.
These are:
 - **Process Isolation**: conflicts between processes (e.g. ports, resources, ...), security and so on.
   Unix systems isolate by default memory space and privilages;
 - **Process management**: handling the lifecycle of processes in a reliable way: starting, stopping, restarting, monitoring health, and recovering from failures. This also includes ensuring processes keep running as expected and can be supervised or automatically restarted when something goes wrong;
 - **Process packing**: moving software across machines, even with specs, in a seamless way;
 - **Reproducible environment**: having the same environment for that process to run in all the possible machines (e.g. version of programming language, dependencies, packages and so on...).


>[!Important]
>Know the 4 hosting problems like the back of your hand for the exam and also how containers address them.

## Containers
Most of the time when we talk about containers we talk about them as a lightweight virtualization method. But this isn't actually the case if we want to be precise.

**Containers are just like any other process but with a very restricted view of the system**.

There's no virtualization involved in containers, **it's a user-space construct**, the kernel of the operating system is shared between the container and the host machine. Also no emulation can be done using normal containers unlike VMs.
>[!example]
>If we want the container to use a GPU but the GPU is not available in the host machine there's nothing we can do (with only that container).
>
>Using VMs there would be no problem to do so, the VM would emulate, as part of itself, the behaviour of a GPU, producing an emulated machine in which processes actually see a GPU available and that can be used.

>[!note]
>How can we run containers in windows if the kernel is shared and in the containers we run linux? We simply leverage the WSL (Docker Desktop is based on WSL).
>
>Before WSL windows used DDLs to do so.

In the following sections we will se how containers work in more detail.


### Docker | Resource Isolation
As we said, containers don't emulate anything in the system, they're like normal processes.
So to actually provide the user with resource isolation and a vision of a completely normal machine, that has nothing to do with the one hosting it, they actually leverage normal OS stuff; **Cgroups and Namespaces**.

**Namespaces** are a UNIX construct that enables to **isolate** parts of the host machine to the process. Containers when created, at default, also create a new set of each type of namespace for the container's process. This enables the container to see the outside world in a really limited way.
Different types of namespaces are:
- MNT: filesystem;
- PID: process information;
- NET: network;
- IPC: interprocess communication (shared memory and so on);
- UTS: naming (?);
- USR: isolate user and privilege identity, UID/GID isolation and mapping between inside the container and outside of it, on the host;

Also, in containers, there's the possibility not only to limit the view of the system but also its usage, **defining resource quotas** through **Cgroups**.
For example one can enforce that a certain container can use up to 10% of the CPU of the host.
#### Sharing namespaces
As we said, having a container with its own namespaces is just the default option. We can have containers that share the same namespace or share namespaces with the system.
>[!example]
> A monitoring application needs to know the pID of the processes so PID namespace is shared.

>[!question]
>Why having every container share all the namespaces with the host can still make sense?
>
>Because we won't have resource isolation, but we still have management, reproducibility and packing over the containers.

### Container Environments
This is the **low-level engine** that actually sets up the environment, enforces container characteristics using different approaches (e.g. actually not all of them use namespaces) and actually **run containers**.

Some examples are:
- Containerd: the one behind docker;
- Firecracker: developed by amazon, containers are actually mini VMs that have performances comparable with normal containers (this is the one used by AWS of course);

>[!note]
>**Decoupling between "container implementation" and everything else:**
>
>There's no difference in how containers can be created, run and be managed when using these different technologies. There's no real lock-in, the contract to interact with the actual container is the same for all technologies. 
### Docker | Container management
We've seen how containers are implementable, now we'll see how containers can be managed.
There's a standard contract (it should be a HTTP Rest API contract) (See [[Rest APIs]]) that is followed by all implementations in order to interact with the actual container.

In docker for example there's a CLI that communicates with the Rest Api exposed by the docker engine and that under the hood actually communicates with the Rest Api of Containerd. (As they're RestApis the format of communication is JSON).

### Docker | Container Specifications and Images
The way we can specify the container's configuration and features is, as always, through a contract. This contract is called **Dockerfile** and enables the user to specify how the container basic environment should be built and what it should have in it. This enables for the **environment to be reproducible**.

From these Dockerfiles we can create **images**, a **snapshot** of the **execution environment** of the container. In the docker CLI we can do it using the command `docker create`.

>[!note]
>Images should be minimal. Better to have more than one container than having a single container with lots of stuff in it.
>
>This enables to have a more lightweight image, easily manageable, reinforce security and so on...

The images can be stored on a **registry** in order for different entities to retrieve them and use them. These registries usually store both the Dockerfile (in order to see how the image that we want to use is actually made, what features does it come with and what can we do with it) and the image itself.

#### Multi-layer configuration
Multi-layer configuration is a technique in which container **images are built as a stack of layers**, where each layer adds or modifies content on top of a base image, allowing reuse of existing image components and incremental composition of new images.

Usually you don't create your own image from scratch, you create you own image **on top of a pre-existing image** (like the one of an operating system or the one of a specific programming language).

So the usual workflow is using an image as a base layer on which we'll create our own **layers** to create our own image.

>[!note]
>Every operation specified in a Dockerfile creates a new layer of the final image.

>[!note]
>Layers can be **cached** so that when we change a Dockerfile and build it again to create the new image only the layers that have been modified will be recomputed, the unmodified ones will be taken directly from the cache.


#### Layered On Write
We said that multiple images can have the same base image. So how can we handle writes on that image?
We leverage what is called Layerd on Write: 
	a mechanism where the base image layers are **immutable**, and any modification performed by a running container is written to a **new thin writable layer on top of the image**, without altering the underlying shared layers.
So, in practice, each container creates a partition for the file and links it, using pointers, to the original file (the base image).

>[!note]
The Image is actually a pointer to a metadata file that has pointers to the layers.

#### Multi-stage build
Multi-stage build is a technique that enables the user to compile an image, and then taking stuff from that image and putting it inside another image as a layer.

This enables to **create lighter and more secure** containers.

>[!note]
>Everything we put inside a container can become a vulnerability, keeping only the really needed stuff inside the container is advised.

>[!example]
>Say we want to run a golang program inside a container.
>
>As we know golang is a compiled language so we would need to create a container in which we copy all the code, install the compiler, the go environment and so on. Then compile the code itself and finally run the executable. But in reality, during execution we only need the executable, not the code, the compiler and all the other garbage that we installed inside the container.
>
>To avoid this we can create a first stage of the build where we copy the code, install the compiler and compile the executable. After this we'll create a second stage that will actually be the final image that the container will run, taking the executable from the first stage and transporting in a lighter and minimal image.

### Docker |  plugins
TODO








>[!Question]
>The professor often asks how containers handle:
> - Isolation: using cgroups and namespaces;
> - reproducible environment: using Dockerfiles as contracts;
> - Packing: image building and registries;
> - Management: through the CLI.



>[!note]
>Its important to not confuse containers and docker.
>
>Containers are a software and infrastructural **concept**.
>Docker is a **tool/platform** that creates and runs containers.
>
>E.g. the image layer approach, the namespaces, cgroups and so on are docker's not principles of containers.

