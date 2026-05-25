![[openwhisk.png]]


Openwhisk has been designed by Apache to serve **high throughput systems**. thousands of invocations of the same function at the same time.

In order to do this they first decided to **decouple trigger and controller** as opposed to [[Serverless - OpenFaaS]].

elements:
- trigger: **nginx**, a simple http static server highly scalable and really efficient;
- controller;
- **[[Kafka]]** as a [[Middleware - Message oriented Middleware (MOM)]] to enable many events to be streamed at the same time to all invokers;
- Invokers leveraging docker **containers**;
- Cache: **couchDB**. They decided to add a cache level to the infrastructure as lots of functions could be stateless and highly requested so it's not needed to execute these functions if the result is already being stored inside a cache. This speeds up thing quite a bit and reliefs the infrastructure from a bit of load.  

>[!note]
>CouchDB is a ==document based== database, so the responses of the functions need to all be **json files**.

>[!note]
>As for [[Serverless - OpenFaaS]] the trigger is only one and can ==only serve http requests==
