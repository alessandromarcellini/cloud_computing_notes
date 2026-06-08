
#cloud 
#distributed-systems
#lecture

# suggested topics before going into this lecture:
- [[Virtualization]]
- [[Infrastructure as a Service (IaaS)]]
- [[Virtualization vs Emulation]]
- [[Pub-Sub]]


# what it is and usages
Openstack is often referred to as the =="Cloud Operating System"== as it manages, abstracts, schedules and so on the underlying physical infrastrucure, following the **cloud paradigm** of **Infrastructure as a Service** ([[Infrastructure as a Service (IaaS)]]).
Unlike [[02 Kubernetes]] openstack abstracts, controls and automates what's **outside of the execution environment** (networks, virtual servers, routers, vm configurations, cloud init, ...), leveraging virtualization and emulation of resources.
%% Key difference:
virtualization creates virtual resources as is while emulation creates virtual resources possibly adding some features on top of them. %%


---

# Quick history
Nowadays resource optimization is so important that 
First developed by NASA, now opensource and actively maintained and developed by lots of companies.
Usally realeases a new update of the system (or its internal components) each 6 months, following the linux kernel updates. 

---

# Openstack organization overview
precursor to microservices since it is built by lots of different software components (services), highly independent from each other, scalable, interconnected, own their data (using a db optimised for the task, ranging from mysql to mongo).
Each service is independent and owns its data so openstack "stack" can be easily tailored to user needs. 
![[openstack_services.png]]

## interfaces
Compute API
Image API
User Dashboard
Customer Portal
Admin API
![[openstack_layers_&_interfaces.png]]

## Services
Openstack is built in a =="microservice-like"== approach.
#### Services design principles
[[OpenStack-Services design principles]]

#### Core services
![[openstack_basic_services_with_real_names.png]]

- [[OpenStack-Queue]]
- [[OpenStack-User Dashboard]]
- [[OpenStack-Auth (Keystone)]]
- [[OpenStack-Compute (Nova)]]
- [[OpenStack-Storage (swift)]]
- [[OpenStack-block storage (cinder)]]
- [[OpenStack-Images (glance)]]
- [[OpenStack-Networking (Neutron)]]

Note: also orchestration (service name: **heat**) and metrics can be added on.
> [!warning] Exam reminder  
> Remember service names and responsibilities.
## Request flow for provisioning instances


![[openstack_workflow.png]]Note: auth is done using tokens after the first interaction (stateless, async, fast).

## Service quick mapping
| Service           | Responsibility |
| ----------------- | -------------- |
| Glance            | Images         |
| Quantum (Neutron) | Network        |
| Cinder            | Block storage  |
| RabbitMq          | Queue          |
| Nova              | Compute        |


# Some Key Considerations
Of course this is wonderful and great but we need to keep in mind that everything comes with a cost, every abstraction that we add on top of the other makes for more overhead of the whole system. Also learning to use Openstack in its fullness requires lots of time.

Idea: using Kubernetes on top of Openstack. Great but introduces more overhead and requires someone that knows how to use both of the technologies and coordinate them.