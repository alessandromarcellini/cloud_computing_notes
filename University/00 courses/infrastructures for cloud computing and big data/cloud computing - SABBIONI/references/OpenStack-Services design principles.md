The openstack services are designed with open standards and **versatility in mind**, quite similar to (and one can say a precursor of) **[[microservices]]**.
E.g. ==Multiple Hypervisors== are supported (the compute is just an "adapter" for the hypervisor that manages the infrastructure)
Easily tailored to user needs.

---

All OpenStack services share the same internal architecture organization that follow a few clear design and implementation guidelines: 
- **Scalability and elasticity**: gained mainly through horizontal scalability; 
- **Reliability**: minimal dependencies between different services and replication of core components;
- Shared nothing between different services: each service stores all needed information internally (**owns its data**);
- **Loosely coupled asynchronous interactions**: internally, completely decoupled ==pub/sub== communications between core components/services are preferred, even to realize highlevel synch RPC-like operations.

Deriving from the guidelines, every service consists of the following core components:
- **pub/sub** messaging service: Advanced Message Queuing Protocol (AMQP) standard and RabbitMQ default implementation;
- one/more **internal core components**: realizing the service application logic
-  an **API** component: acting as a service front-end to export service functionalities via interoperable ==RESTful APIs==;
- a **local database** component: storing internal service state adopting existing solutions, and making ==different technological choices depending on service requirements== (ranging from MySQL to highly scalable MongoDB, SQLAlchemy, and HBase).
