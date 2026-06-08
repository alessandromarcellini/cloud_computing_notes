## Basic introduction
Kubernetes (Litterally navigator in Greek) is an orchestration system developed by Google and now opensource.
Actually, Kubernetes is more of a **framework** than a system itself as it enables extendibility and customizability by design of the system itself and this is one of the main reasons it has become so widespread.

The importance of Kubernetes is made by its purpose, orchestrate complex systems, automating everything that's possible in deployment, scaling and management of the instances inside a cluster, hiding the complexity behind it.

>[!note]
>This is one of the most important things, during the lectures it is said lots of the time that cloud exists because it is convenient. Kubernetes enables it to be as convenient as possible (if used right of couse) by saving resources and allocating them only if really needed.

Some of the features that Kubernetes enables are:
- **Service continuity**: rolling updates, updating the application without any downtime while it is live; 
- **Automated rollbacks**: revert to a previous application version if an update causes issues;  
- **Automatic scaling**: scale the number of instances based on metrics;  
- **Self-healing**: automatically restart, replace, or reschedule failed containers and Pods;  
- **Load balancing**: distribute incoming traffic across multiple application instances;  
- **Service discovery**: enable applications to find and communicate with each other through stable endpoints;  
- **Resource management**: allocate CPU and memory resources efficiently across workloads;  
- **High availability**: maintain application availability despite node or container failures;  

>[!note]
>One other really important feature is Workload Portability as Kubernetes can run on lots of different providers, that probably implement stuff differently under the hood, but still expose the same api to interact with it. This enables Kubernetes to be really portable and not dependent on the underlying infrastructure.

>[!note]
>Kubernetes (at default) uses containers under the hood to deploy the instances.

## The declarative model
As discussed, Kubernetes allows users to manage applications and cluster resources without manually handling every operational detail, such as instance creation, updates, or recovery from failures.

To do so users communicate with the Kubernetes API (same old [[Rest APIs]]) (usually through some kind of CLI like kubectl), submitting yaml files that act as **manifests** to describe **what** they want in their cluster (desired state).

In this declarative approach, users specify **what** they want the cluster to look like, rather than **how** to achieve it.

Kubernetes will then under the hood get the cluster to the state that the user specified, leveraging its own mechanisms.

>[!question]
>How does declaration work under the hood?
>
>[[Kubernetes - Declaration under the hood]]

## Underlying Architecture
The Kubernetes architecture is based on the master-worker model where respectively:
- The **master** is  the control plane, responsible for the orchestration, scheduling and so on;
- The **workers** are the actual nodes in which the instances of the different services live and are handled.
![[Kubernetes - architecture.png]]
[[Kubernetes - Master architecture]]
[[Kubernentes - workers architecture]]



## Different types of deployable workloads
[[Kuberenetes - Types of workloads]]


## Kubernetes - Network
Each pod gets its own ip with which it can talk to every other pod (barring network segmentation) in the pod network (network inside the cluster). Note that this ips are not fixated in any way, if a pod gets restarted it's ip can change, there fore the necessity of the Service abstraction.


The element in the cluster that actually implements the network logic is the CNI (Container Network Interface). It's the component that is responsible for:
- IP assignment;
- implementing the actual network;
- implementing network policies (can be specified with manifests).

>[!note]
>There are more implementations of CNIs out there, each with its own features and characteristics, choosing one or the other can create really different results.

>[!example]
>Flannel is one of the most popular ones and it uses **linux bridges** and **VXLAN tunnels** to implement the actual pod network.
>It basically creates a linux bridge for communication inter-node (inside the node) and attaches it to an eth internface of the node through the use of a daemon called flannelid, connecting to the VXLAN tunnel.
>
>So if two pods of the network need to communicate they just use the bridge if they're on the same node, if they're across different nodes the packet needs to traverse the overlay network of the VXLAN tunnel (each packet gets wrapped inside a VXLAN packet).

## Kubernetes - Autoscaling
A big part of the kubernetes automation is of course scaling the services based on the current load and the current number of replicas.

The scaling can be both:
- **Horizontal** (most versatile and used): adding or deleting replicas;
- **Vertical**: adding or removing resources to existing replicas (e.g. adding 2 GB more of RAM or 1 more CPU)

We're going to **focus on the Horizontal** approach as the Vertical one is rarely used.

The scaling needs to be done of course based on metrics that can be:
- Pod **resource** metrics (% used CPU, ...), not really effective in most of the cases;
- Pod **custom** metrics;
- **external** metrics.

Based on the metric that we want to track, the **formula** to get the appropriate number of replicas that need to be instantiated is this:
$$
\text{replicas} =
\left\lceil
\text{currentReplicas}
\times
\frac{\text{currentMetric}}{\text{desiredMetric}}
\right\rceil
$$


To configure autoscaling there's a resource called **HorizontalPodAutoscaler** where you can specify the metrics that need to be tracked and configure the settings of the autoscaler. The controller associated with it will then proceed to implement the logic, scaling up and down the number of replicas based on the settings specified.

