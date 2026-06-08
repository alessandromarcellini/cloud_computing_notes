## Overview
Kubernetes is based on a mater-worker architecture, in this notes you will find the description of each component of the master:
![[Kubernetes - master architecture.png]]

## api-server
The api-server is the actual component that enables the control plane to **communicate** with the outside world (and vice-versa). 

It exposes some **HTTP Rest Endpoints** that enable the user or the components to interact with the cluster. Of course, to avoid problems, it checks both **permission** for the user to change the desired resource and also **checks the schema** of the passed resource.

As it is the communication service it's of course connected with all the other components of the main panel in order to coordinate them and implement the kubernetes's orchestration logic.

>[!info]
>Kubectl (the CLI) communicates with the apiserver under the hood.

## etcd
All elements (deployable and related) in the kubernetes clusters are expressed as **yaml files** called manifests.
These manifests state both metadata and specs of resources. When a manifest is submitted to the api-server also the "current state" informations are added to it and all of it is stored inside a database, called etcd (not as a plain yaml file).

So etcd is the component **responsible to keep state** inside the master, all state is concentrated here.
It's a **NoSql key-value** oriented database that prioritizes **partition tolerance** and **consistency** (Uses [[RAFT]] to do it) over high availability.

To avoid race conditions when a value is accessed etcd puts a **lock** on it to avoid other changes being made at the same time. Note: lock **on the key-value**, not on the entire database.

>[!note]
>The developers decided to concentrate all the state inside this one component in order to enable the other components to scale in an easier way. See [[The problem of state]]
## controller-manager
As we already said, orchestration in kubernetes is based on the simple principle of control loop ([[Kubernetes - Declaration under the hood]]). The entities that implement this mechanism to watch resources and adjust them if a change occurs are called **controllers**.

It's important to say that these controllers watch over the resources for which they implement the control loop logic and so are both:
- **Proactive**: when the **user** applies CUD operations to the resources, **changing the desired state**;
- **Reactive**: when they react to unexpected **changes** in the **current state** of the resources, such as **fault** cases.
To do so they, as we said, they need to have some kind of **watch mechanism** to keep track of the current and desired state of the resource, comparing them and, in case of inconsistencies, reconcile the resource. This mechanism is split into 2 parts:
- **LIST**: Regular HTTP request that asks the api server to list all the resources that the controller needs to keep track of and their resourceVersion (See [[Kubernetes - Declaration under the hood]] to know more). This is done to start up the controller logic and actually knowing the desired state and current state of each resources;
- **WATCH**: this is the second phase of the controller logic, where he opens up a **long-lived http stream** with the api-server where the api-server itself will pass messages indicating the changes in the resources (ADDED, DELETED, ...). This is made to avoid continuous polling that would be too heavy and create additional (most of the time unusefull) traffic.

Most of the time multiple controllers are deployed for the same resource and so conflicts on accessing the informations on etcd could be a problem. To avoid this etcd puts a lock on the key-value accessed until the operation is done.

Coming to the **controller manager**, this is a component of the control plane that acts as a wrapper of most of the default and basic controllers, giving them the right execution environment and features to run. At startup the controller manager actually spawns the different controllers as threads.

>[!note]
>Not all default controllers are deployed like this, some of them can be deployed as pods, but they still run inside their own manager inside that pod.

But kubernetes doesn't limit its features at this, as we said kubernetes nowadays is more of a framework than a plain technology. In kubernetes, infact, there's also the possibility of creating **custom resources and controllers** associated with them. This is called [[Kubernetes - Operator Pattern]].

## Cloud controller manager
The Cloud Controller Manager (CCM) is a Kubernetes control plane component that integrates the cluster with the underlying cloud provider. It runs cloud-specific controllers that manage resources such as load balancers, nodes, and network routes.

By separating cloud-provider logic from the core Kubernetes components, the CCM allows Kubernetes to remain cloud-agnostic while enabling seamless integration with platforms such as AWS, Azure, and Google Cloud. The Cloud Controller Manager continuously watches Kubernetes resources and reconciles them with the corresponding infrastructure resources in the cloud environment.
## Scheduler
The kube-scheduler is a Kubernetes control plane component **responsible for assigning newly created Pods** to **suitable worker nodes**. It continuously watches for Pods that do not yet have a node assignment and evaluates the available nodes **based on resource requirements, scheduling constraints, affinities, taints, tolerations, and other policies**.

To do so itc leverages a technique called **labelling**. Labelling a resource means assigning some metadata stating the type of the resource, or the group of it. Policies can be created for these labels, guiding the scheduler on how and where to schedule pods.

The scheduler selects the most appropriate node for each Pod and updates the Pod specification with the chosen placement. By making intelligent scheduling decisions, kube-scheduler helps ensure efficient resource utilization, workload distribution, and overall cluster stability.

>[!note]
>It is actually an operator, so it can be customized.
