#cloud #distributed-systems #lecture

# suggested topics before going into this lecture:
- [[Infrastructure as a Service (IaaS)]]
- [[Middleware - Message oriented Middleware (MOM)]]
- [[01 Microservices and containers]]

# Quick history
Serverless computing is a really modern cloud computing paradigm that emerged in the last few years. It started gaining traction around 2015 with AWS Lambda and got really widespread in the years after that.

It was first adopted by AWS because they had lots of inactive or under-loaded servers during each day and before these they were selling that computational power using a bidding system.

After the adoption of Serverless and in particular FaaS they were able to "sell" that computational power for much more, due to the fact that people were paying for the whole management of the application, even its lifecycle, so those instances could be spread across the under-loaded nodes as they are ephemeral.

---

# Definition and usages
![[cloud_models_differences.png]]
Serverless computing is a family of cloud computing models that share the same concept:
everything except the business logic of the application is managed by the cloud provider in their own way, the user just uploads the code and everything works.
Of course this is both a relief and a burden as the developer doesn't have to manage the underlying infrastructure, scaling, the runtime environment and not even the lifecycle of the application, but at the same time he has no control over those things. Because of this it stands perfectly between [[Platform as a Service (PaaS)]] and SaaS.
![[FaaS_between_PaaS_and_SaaS.png]]
One thing to keep in mind is that these cloud model is a strictly pay-per-use model, as the user doesn't have any control over the lifecycle of the application, the scaling and the resource management of it. This is the great advantage of this cloud model and what people are actually paying for.
# Models classified as Serverless
As we said serverless is not a model itself but more of a family of cloud computing models.
Some of them that are classified as serverless are:
- Backend as a Service;
- Function as a Service.

# Backend as a Service (BaaS)
BaaS is a cloud paradigm that includes a group of services in which the provider gives you a very vertical and specialized service, outsourcing a really specific need of developers, that you can use and integrate in your application, as per usual with serverless models not controlling anything other than the application level.
It's usually plug and play so it's really easy to use leveraging SDKs or APIs that are well documented and integrated with the system. Of course a part of authorization is required so they usually give you a token or key of some kind that you need to use in all your api or SDK calls.
Another quite interesting thing of BaaS services is that they're usually well integrated with each other and they can come with many features.

A great example of all of this is Database as a Service (e.g. mongoDB, amazon kinesis, ...).
In database as a service you don't control anything other than the business logic. You can't control how it works underneath nor the lifecycle of it, if it's down you can't do anything about it, it's the provider that manages all that. The only thing you can do is controlling the business logic of it by creating tables/schemas, triggers, views, defining how to manage sessions, and so on.
They are most of the time well integrated with Auth as a Service (e.g. if you use amazon kinesis you're forced to use their auth service to use their db service).


Other BaaS examples could be:
- DNS as a Service;
- CDN as a Service;
- Authentication as a service;
- ...
# Function as a Service (FaaS)
%%In aws terminology this is called lambda%%
![[towards_FaaS.png]]


event centric model so there are functions that are triggered by events arriving from outside. The functions and the mapping of them to the events is the user defined business logic.

Function as a Service is a serverless paradigm that is **event centric** as it enables the user to create its service or app through the definition of functions that will be activated when certain events arrive.
As per usual, therefore, the user doesn't have any control over the deployment (in most of the cases ==not even the communication details like protocols==) but can define its business logic. The business logic is expressed in terms of **user defined functions** bound to **events** using what are called **workflows**.

This has several **advantages** for both the developers using this paradigm for their applications and the provider itself.
This cloud model, infact, enables even ==low experienced developers== to create their own app without having to worry about anything other than the business logic (that is expressed in functions, so usually, really simple) and having ==fine-grained autoscaling== (this is what is really valuable, one day you can have 100 requests per day and the next one have 1M requests per day without having to change anything).
The provider, on the other hand, has ==complete control over the deployment==, enabling elastic compute resources management since the **execution environment of the functions is ephemeral** and **time-sharing** is possible, scaling the applications based on the current needs in terms of traffic. Function instances can be eventually scaled to zero (See [[zero-scaling and the cold start problem]]) . This means that **if an application scales down another one can scale up taking its computational resources**, as a provider I can free space from a customer and sell that space to another one (==Functions are executed in nodes or DCs in which the load is not as high as expected, "filling the gaps" of other customers, this optimizes the cost)==.

Note: the model itself is usually **stateless** so either the data comes from the function invocation itself (the event) or a third party Database as a Service is needed (this is why FaaS providers usually are also DbaaS providers).
![[FaaS_finer_decomposition.png]]
## FaaS architecture and components
![[FaaS_components.png]]

The architecture that enables FaaS to work is based on 3 different basic components that are:
- [[Serverless - FaaS trigger]]  
- [[Serverless - FaaS controller]]
- [[Serverless - FaaS invoker]]

- [[Serverless - FaaS workflows]]

## FaaS Different Function composition patterns
**Their all based on choreography**.
These patterns enable the developer to create any kind of logic he wants leveraging FaaS, possibly overcoming the provider's workflow's setup limitations.
(e.g. the provider doesn't directly support [[MapReduce]]? We can Implement it ourselves using these patterns)

[[Serverless - FaaS function merging]]
[[Serverless - FaaS Reflective invocation]]
[[Serverless - FaaS Continuous passing]]


## Opensource platforms
Even though the FaaS solutions provided by cloud providers are the most advanced ones as they play with their entire ecosystem and they have lots of interest in optimizing everything the most they can some opensource platforms FaaS are still present and can be used in private clusters, for experimentations or to study some open models (of course providers don't share every information about the setup they're running).

But why would someone still want to host FaaS platforms inside their own clusters?
Function as a Service, infact, is additional effort and duty as if I have the constraint to develop stuff on a cluster of my own, it's easier to host my own kubernetes than to host my own FaaS.

But this can still be be useful in some cases as we can hire some people only to bring up that additional infrastructure and then hire some less expensive and less experienced developers (compared to the one that would develop micro-services or monolithic applications) that can develop faster and for less money all the services that we need.

these are some of the details of three of the most used opensource platforms for FaaS.
[[Serverless - OpenFaaS]]
[[Serverless - OpenWhisk]]
[[Serverless - Knative]]


Knative is function as a service platforms on top of k8s and is used in google functions.

OpenWhisk: is hosted by salesforce


---


# Some Key Considerations
Of course this is wonderful and great but we need to keep in mind that everything comes with a cost. Serverless computing helps tremendously in using the whole infrastructure in its fullness by allocating resources in a new really dynamic way, but it also means having no more control on the application life cycle, with probable zero scaling at some point in time (optimizing cold starts are one of the most difficult challenges of nowadays). Like always, leaving the control to someone else is both a relief (because you don't have to manage anything you have delegated) and a burden (because you can't manage anything you have delegated).