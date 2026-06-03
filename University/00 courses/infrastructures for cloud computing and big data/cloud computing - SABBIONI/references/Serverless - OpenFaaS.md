OpenFaaS is not hosted by anyone but it's the easier one to setup and do some experiments on.

In order to implement this platform, the creators of OpenFaaS, dind't create everything from scratch. Infact they used lots of other opensource existing projects to recreate the 3 essential FaaS elements (trigger, controller and invoker). 

![[openfaas_layers.png]]

In this picture we see all the main technologies adopted to create OpenFaaS.
**Only the Controller is a custom element**, all the others are all opensource projects widely used by the cloud native community.
- [[Kubernetes]] as the resource manager;
- containerd (docker) as the execution environment;
- prometheus to collect and expose metrics of the system: this is particularly useful to scale the system (and eventually zero scale functions) based on the number of events that are currently streamed;
- a [[Middleware - Message oriented Middleware (MOM)]] to decouple in space and time events and the relative function execution;


## Architecture

![[openfaas_architecture.png]]
In the above picture we can see OpenFaaS's architecture split into the 3 basic components of FaaS.

The trigger is implemented by the **gateway**, receiving requests from the outside (http), creating events and emitting them inside the infrastructure.

The **gateway** itself is also part of the controller as it knows which function needs to be activated according to an event. The rest of the controller is made by the **faas-provider** who spawns the execution environments for functions.

Executors are simple **pods** in which the function is instantiated and ran.

As per normal docker containers the images of the different functions are kept inside a **registry**.

>[!note]
>functions are not created at each event arrivale since creating a function here means creating an entire container (really expensive).
>In the infrastructure we usually see reusual of the same container (function) instance to serve multiple requests.
>This enables OpenFaaS to save up on resources sacrificing a bit of that fine-grained scalability.

## Event connector pattern
![[openfaas_event_connector.png]]
The infrastructure can handle only http ([[Rest APIs]]) events.

There's no need to create lots of different triggers to receive different types of requests. Instead of creating different types of gateways they decided to create a series of connectors that **adapt the external events to events that can be processed by the infrastructure**.

This, of course, is a tradeoff as you sacrifice some performance (with one additional request for non-http events) for ==ease of use and development==.

#### Gateway
![[openfaas_gateway.png]]
#### Prometheus
Opesource project and one of the most used tools used in the entire world for observability of the cluster. It is only a collector of metrics from the cluster, more precisely, a **metrics aggregator**.

It's way of functioning is really naive as it just goes to each ==metric defined endpoint== of the cluster ==asking== for metrics to retrieve. This is easily scalable and usable inside any cluster.

It is used to collect metrics from the cluster and use them to scale instances of different services, enabling scalability based on more metrics than just CPU usage (using only CPU usage as a metric for scalability is pretty bad).
#### NATS
NATS is a very high performance and lightweight [[Middleware - Message oriented Middleware (MOM)]] ([[Pub-Sub]]).

It is paired with the controller (as done in many FaaS systems) in order to **decouple in space and time** events with function execution.

This can be particularly useful for **asynchronous calls** (e.g. the creation of a pdf).

So, in this case, the trigger doesn't directly activate the function but it "posts" the event into the NATS queue and the event will be handled at the right time.

![[Pasted image 20260521181456.png]]

#### Providers
OpenFaaS is really nice also because it has adapters that enable it to spawn and control resources to execute function on multiple "supports" like:
- [[Kubernetes]];
- AWS;
- containerd (to run it directly inside your own machine as single node FaaS, see the [[#faasd]] section).

This is useful as we can host this on an [[edge computing]] server, enabling it to run much more entities and serve more requests than a micro-services approach, because you can delay the serving of requests if needed and also fill the small resource gaps that are left by other processes.
#### Invoker (watchdog)
(**Probably not that accurate**)

![[openfaas_watchdog.png]]

**This is where the real tradeoff lies**.

The creators of openfaas saw that most of the time it wasn't optimal to create a container for each request arriving; In particular, most of the time, resources where used more to create the actual container than to execute the function.

In order to fix this behaviour they decided to move the fine-grained scalability from the execution environment (the container) to the actual process level.
Infact, the actual infrastructure is really like **any other [[University/00 courses/infrastructures for cloud computing and big data/cloud computing - SABBIONI/microservices]] infrastructure**, the containers are not being created really dynamically at each event arriving, but they are being reused for multiple events, without being destroyed (basically like a service serving events).

In order to still offer that fine-grained scalability the infrastructure **doesn't spawn dynamically containers, but processes or threads inside them** (of course, containers can still be scaled by Kubernetes but they do it in a slower way than actually using them the way the should be intended in FaaS).
To do this each container in it has a function and a **proxy** called watchdog that receives events and spawns processes to serve them inside the long-living container.
In particular there are 2 different kinds of watchdog:
- **forked watchdog** => at every http request the proxy does a ==linux fork==, creating a ==heavy process==, executing the function in it and then destroying the process as it served its purpose;
- **http based watchdog** => The fork was still too slow for some applications, so they decided to make the possibility to ==wrap== it with an actual ==http server== that receives http requests and spawns ==threads== (startup is faster than processes).

%%
Comparison of RAM occupation:
- forked: environment, watchdog and the process;
- thread based watchdog: environment and watchdog;
- AWS lambda: no occupation of memory for the environment as it is dynamically generated
%%


#### faasd
Instead of having an entire k8s cluster we spawn single containers with functions in it leveraging containerd in order to have a single node FaaS infrastructure.

![[openfaas_faasd.png]]

>[!note]
>the API doesn't change, so it's really good for experiments.