What we want for a cloud service is for it to **work forever and normally**.
This of course is only ideal, not possible, but we can adopt some **strategies** to ensure that faults, problems and conditions changes do not affect our system as much.
We're speaking of the **necessity of controlling resources**, by **monitoring**, **observing** them for their effectiveness, deciding their behavior and changing them.
**While the application works**, we need to look at the execution and decide whether is the case of changing something.

>[!note]
>This has a cost, it should be as little as possible.



## Quality of Service
Any situation of a relationship can be **qualified by the intended quality**, that needs to be carefully **defined before the communication** actually start and has to be provided by providers to requestors. This is usually defined in the [[Service Level Agreement (SLA)]]

## Resource Management
Systems are very differentiated in requirements and there is **no magic recipe** for all cases.
There are **many implementation models** and many different ways of operating and serving results.

the design of the interaction is split into 2 phases:
- **static**: these are static rules planned and defined ==before the system's execution==, setting up everything to work as expected (e.g. scheduling policies). Done **out of band**, if we need to tweak something we need to shut down everything and restart;
- **dynamic**: these are rules that are generated and enforced ==at runtime==, enabling ==elasticity== of the system, **reacting** to the actual load.

>[!note]
>Usually the 2 approaches are used together.

>[!note]
>**Static stuff does NOT generate lots of overhead** opposed to the dynamic. Setting up static rules before deploying is really useful.
>[!note]
>We do all of this to handle concurrency.
>Concurrency can generate overhead but, if handled well, can also balance out the system. (e.g. load balancing ensuring that all instances have around the same resource usage...)

###### Resource management models
- **Preventive**: does stuff in advance in order to avoid problems during runtime. It's a **Pessimistic** approach because we do everything tailored for the worst-case scenario (e.g. in streaming platforms where the cost of dynamic scalability is too high);
- **Reactive**: react to the system's changes at runtime.


## Partitioning the application
>[!important]
>Asked a lot


partition the application into constituent components that are going to be deployed across different machines.
Rely on a support for remote references and to load balance across instances.
**The application is divided into resources that represent partition (P1-P9) to be mapped on the specific deployment resources**.

Based on this we need to create a system that is tailored for the application, **deciding how many resources** are going to be destined for each partition (**the least amount**, overloading the system is key for efficient resource consumption).

Also we need to think of how to physically distribute the various partitions across the machines following the **principle of locality**. It makes sense that if 2 partitions need to communicate a lot at runtime we should place them as close as possible, ideally on the same machine.

>[!note]
>Nowadays placing highly communicative partitions on the same machine is not that advantage because modern connections (especially the ones in datacenters) are really fast and using **network accelerators** the communication is close to being equal to communication in the same machine.
>
>This enables to still adopt the locality principle but placing the partitions on 2 really close different machines, in order to avoid concurrency over CPU and memory.

## Process allocation
After partitioning the application we need to allocate the different partitions to our resources.
To do so we usually reason on cost, performances.

This is not as easy as we expect, there are lots of really complex algorithms that tackle this problem.

As always we can work both:
- **Statically**, static considerations before deploying the system;
	- Pros: no additional overhead at runtime;
	- Cons: inflexible, static.
- **Dynamically**, reducing overhead dynamically through the system tailoring to the current load and still not introducing too much overhead for these computations themselves. Dynamic reasoning needs to be done with very simple policies.
	- Pros: flexibility of the system;
	- Cons: additional runtime overhead.
>[!note]
>As always modern systems usually use **both** approaches together, creating a nice **initial setup** at the start and adjusting it dynamically during execution to avoid congestion.


In real world situations these is done in 3 main ways:
- **Manual setup**: the user needs to setup everything directly by himself using commands;
- **Script setup**: the user creates a script in some scripting language and by running it the system configures as intented:
- **Model driven setup**: Setting up a system using a declarative way of proceeding, by the use of some kind of tool like Ansible or Chef.


But most of the time we don't know how many and what processes need to be allocated.
Therefore the practices of [[Resource Management - Load Sharing]] and [[Resource Management - Load Balancing]] arised.


## Speedup
When parallelizing load we aim to speedup the processing of the load itself.
Ideally we would want a situation in which the more processors we allocate to the load the more speedup we have.

This is not the case most of the times, not all loads can be parallelized, and not all loads can be infinitely parallelized. This results in a plateau.

###### Ahdmal's Law
![[Ahdmal law.png]]
>[!note]
>Most of the time the real speedup gets to the plateau but then slowly descends back to zero adding more processors.
>This is because adding unuseful processors to the tasks generates additional overhead due to coordination and synchronization.

Ahmdal's law states that the **speedup in parallelization is bounded with the degree of parallelization of the task**.
If a task is 25% sequantial the maximum speedup we can have is a 4x.

>[!Grosh's Law]
>We also have Grosh's law stating that if we want something really efficient we need to execute it the most local we can.
>
>**Ideally everything on one processor**.

## Data flow architectures

Data flow architectures are a way of interconnecting different processors to process the incoming load of data, distributing the task to different processors that do not run the whole task, but execute only a part of it, passing the result to the next processor of the architecture.

Some example on how to distribute processors creating a data flow architecture are:
 - Pipelines;
 - Trees;
 - Graphs.
>[!Important]
>In these architectures is really important to keep every processor the most full it can, avoiding idle times in order to keep the structure running.


