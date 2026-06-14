Middlewares are infrastructural components that stand between the communication of different entities, making it, in some way, better.
>[!note]
>when we talk about entities we usually assume they're remote services.
>This is not always the case, middlewares stand between some kind of communication, it could also be, as an example, the one between a process and the Operating System.

MIddlewares are one of the most important topics of the course when it comes to cloud, they implement lots of the concepts that we already saw, enabling:
- **loose coupling** of communicating services:
- Better QoS and possibility of defining **QoS tiers** for users;
- Have **embedded some services** that enable for better and easier communication and development:
	- e.g. internal DNS to look for the receiver;
	- Dashboard to control the current state of the system;
	- ...


Most of all they:
- **Hide distribution**: it is not clear if the communication is being made with something local or remote.
- **Hide heterogenity**:
	- Including making communication possible between services that run in different OSs, on different hardware, written in different programming languages, using different communication protocols;
- **Make communication in different protocols be possible**.

>[!note]
>Middlewares can manage both interactions between "service<->service" and interactions between "user<->service".

>[!note]
>In the early days when we talked about middleware we were intending something that manages communication between the application and the underlying OS.
>e.g. a JVM even if it is not really a middleware since it is tied to java. Real middleware should be independent from the single programming language.
>
>A better example is .NET

## Layers of a Middleware
We'll list every layer from the highest to the lowest level.
- Specific domain services;
- Common service;
- Distribution;
- Host infrastructure;
###### Specific services
Provides **domain specific services** useful for the development of the applications.
>[!example]
>Fintech, banks...
###### Common services
Provides **reusable services** that can be used by applications without having to reinvent the wheel each time.
>[!example]
>- logging;
>- -security;
>- -monitoring;
>- - ...

###### Distribution
Makes the **communication** between **remote** entities possible making the interaction easy and **hiding** the fact that the entity may not be local.

>[!example]
>RPCs or CORBA.
###### Host Infrastructure
The closest to the OS level, enables to **abstract from the underlying infrastructure** making stuff that is built on top of it **portable**.
>[!example]
>JVM or .NET



## Types of Middleware
MIddlewares are classified as:
- **Adaptive**: non ho capito;
- **Reflective**: non ho capito.

Different Types of middleware include:
- [[MIddlewares - Remote Procedure Call (RPC)]];
- [[Middleware - Message oriented Middleware (MOM)]]
- [[Middleware - Message oriented Middleware (MOM)]];
- [[MIddlewares - Distributed Object Computing (DOC)]];
- [[Middleware - Distributed Transaction processes (DTP)]];
- [[MIddleware - DataBase Middleware (DBM)]].
- [[Middleware - Self-*]]


## Middleware design
Middlewares need to be:
- Always available, if they fail all the communicating components that where relying on it can't talk anymore;
- Scalable, in order to provide its service in the best way possibile and adapting to different scenarios during runtime;
- Possibly extensible, in order to create added functionalities in it (such as Interceptors, used for security).

## Middleware usage scenarios
- **EntryPoint Middleware**: ==static== configuration middleware aiming for simplicity, efficiency and usability. Usually called ==disappearing middleware== as it is not really perceived by the users;
	- Really used in banks setups to be able to talk with each other for transfers without knowing the other's location or implementation.
	- E.g. MOMs.
- **Personal Middleware**: more ==dynamic==, the middleware is able to ==sense and manage the lifecycle of the applications==; applications can appear, disappear or change during runtime. The middleware needs to ==adapt accordingly==;
	- E.g. Microsoft middleware. I run word, I then run excel, the middleware senses that excel is running and exposes its functionalities to word;
	- E.g. more programming oriented: COM or .NET.
- **Organization Middleware**: It's the most evolved one as it is used by the whole organization, not only by single applications and its lifetime should be, ideally, infinite.
  Here its not the middleware that add to the applications, but ==the applications themselves enrich the middleware with new functionalities==.
	- E.g. [[Middleware - Corba]]

