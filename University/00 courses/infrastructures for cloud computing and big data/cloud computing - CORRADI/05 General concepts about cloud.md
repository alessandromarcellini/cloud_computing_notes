This is part of the slides was dedicated to middleware but I don't find them that much related, so I decided to extract this contents in a dedicated note.

## Classification of services:
- **Software as a Service**: The service provided is a a cloud software. The user doesn't control anything, not even the business logic;
- **Platform as a Service**: The provided service is a platform containing lots of tools and packages that support the development and runtime of the applications instantiated in them. ==Really lock-in==, it's not easy to switch from one to the other, each one has its own features and interfaces;
- **Infrastructure as a Service**: The provided service exposes a way of provisioning infrastructure (mostly virtualized) to the user, on which he will run its services;
- **Serverless computing**: its quite of a new paradigm that enables the user to deploy its services without having to manage them at all, everything is managed by the provider, the only concern of the user is to write the business logic. Worth noting is the fact that the user doesn't have any control over the lifetime and runtime of the application, the application could be zero-scaled in some cases making space for other workloads ([[06 Serverless]]).
- **Anything as a Service**: ;

Not quite a classification of services but worth mentioning are also:
- **utility computing**: those models of cloud computing (or computing in general) that are pay-per-use, so you don't pay a fixated fee but the fee changes based on how many resources you use for your computations;
- **grid computing**: Usually used in HPC contexts, its a computing paradigm that enables massively parallel computation leveraging the division of the whole infrastructure in grids of ==non-communicating resources== that are all dedicated to a task or user.
  This is quite the opposite of normal ==cloud==, in which ==multi-tenancy== is a standard.

## Classification of Cloud
- **public**: Public cloud is the most common type of cloud computing. It is a cloud infrastructure made available over the internet by providers and ==accessible globally to anyone== who wants to use it. Resources (such as computing power, storage, and applications) are shared among multiple customers (multi-tenant model). Users typically pay on a pay-as-you-go basis.
	- Examples include AWS, Microsoft Azure, and Google Cloud Platform.
- **private**: Private clouds are cloud environments ==dedicated exclusively to a single organization==. They can be physically located on-premises (within the company’s own data centers) or hosted by a third-party provider, but access is restricted to that organization only. Private clouds offer greater control, security, and customization, making them ==suitable for sensitive data or strict compliance requirements==.
- **community**: Community clouds are shared cloud infrastructures used by a ==specific group of organizations== that have common requirements, such as security policies, regulatory constraints, or mission objectives. The infrastructure is jointly managed by the participating organizations or a third-party provider. It allows ==cost sharing== and collaboration while maintaining a level of isolation from the general public cloud.
	- Examples include clouds ==shared by government agencies==, research institutions, or ==healthcare organizations==.

- **hybrid**: Hybrid cloud is a combination of two or more cloud environments (typically private and public clouds) that remain distinct but are connected through standardized technology. It allows ==data and applications to be shared between them==, enabling greater flexibility. Organizations typically use hybrid cloud to keep s==ensitive workloads in a private cloud while leveraging the scalability and cost efficiency of the public cloud== for less critical workloads or peak demand.
- **multi-cloud**: Multi-cloud refers to the use of multiple cloud services from different providers (e.g., AWS, Azure, Google Cloud) within the same organization. Unlike hybrid cloud, multi-cloud does ==not necessarily imply integration between environments==; instead, it focuses on distributing workloads across different providers. This approach helps avoid vendor lock-in, improves resilience, and allows organizations to choose the best services from each provider depending on performance, cost, or features.

## About QoS
- safety (in the sense of correctness): ;
- Efficiency; ;
- Scalability; ;
- Robustness: ;
- Security: ;
- ...

## Roles in cloud
- **customer**: the one who uses the service;
- **provider**: the one who provides the service;
- **auditor**: mostly a third-party person that can conduct ==assessments of cloud services==, checking their security, optimization and so on;
- **broker**: manages ==relations between different providers== that need to integrate their systems and also between provider and customer;
- **Carrier**: the one who ==provides connectivity== to enable communication between customer and provider.

## Datacenter organization
Cloud is typically organized in **different remote Data Centers** that can host the new stores.
They must be **organized carefully** to favor the local intraDC organization and the inter-DC infrastructure. 

Any family of data must be based on **replication widely localized**: several copies in different DCs and several ones in anyone of them.
Also, any DC must **optimize access to its copies** and should have some mechanisms to ease the access (key-values, DHT, local ring configuration, …).
Some policies for configuration must be decided and actuated (out of band, before data access) and also data operations must be monitored and controlled during execution (in band monitoring, dynamic reconfiguration, ….)

This is quite connected with [[DataStorage - Cassandra]].
