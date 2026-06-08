- **Deployment**: stateless applications, gestione di ReplicaSet e rolling updates
- **StatefulSet**: applicazioni con stato (database, cluster, ecc.)
- **DaemonSet**: un Pod per ogni nodo (logging, monitoring agent, ecc.)
- **Job**: esecuzione finita (batch job)
- **CronJob**: Job schedulati

Supporting resources (not workloads):  
- **ConfigMap** (configuration)  
- **Secret** (sensitive configuration)  
- **Service** (networking / load balancing)  
- **Ingress** (HTTP routing)
- **Nodeport** (basic traffic exposure)

## Workloads
 ## Deployment
A deployment is built upon the abstraction of Pod and Replicaset (a manifest that describes the number of replicas of an instance).
So the deployment is a group of pods of the same type and treated as replicas.

The deployment also adds features to the pods like:
 - rolling updates (updates of the service with zero downtime) (done by updating one pod at a time, queuing requests and redirecting them to the other pods until all pods are updated);
 - rollbacks.

 ## Stateful set
Stateful sets are an abstraction that is done to create a group of pods, with stable naming, that manage volumes (one per each pod).
This is built upon the **PersistentVolumeClaim** (PVC) resource, that defines a volume of X GB to be reserved for a pod. This can also be created using a volumeClaimTemplate.
After this the pod requests to use that volume, mounting it inside its filesystem. If the pod moves, the volume moves with it.

This is particularly useful for all those instances that need to have persistent storage or need to have persistent names across replicas (in normal deployments replicas change name each time they get recreated or moved, here each pod has a progressive number that sticks with it at each recreation, particularly useful for setups that use leader-follower architecture like in [[RAFT]])

>[!Types of volumes]
>Volumes can be of 2 types:
>- **Ephemeral**: get deleted when the pod dies;
>- **Persistent**: data is preserved across container restarts.

 ## Daemon Set
A DaemonSet ensures that **all (or some) Nodes run a copy of a Pod**. 
**As nodes are added to the cluster, Pods are added to them**.
As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.

Usecases include:
- Running a daemon;
- Running log collection;
- Running monitoring pod.

 ## Job
Jobs are a workload that is run once, without remaining active but after completion the pod gets to the "completed" state.
It can also be run and scheduled as ChronJob periodically (e.g. cleanup of a volume, ...)

---

## Supporting resources
 ## ConfigMap
To store non confidential configs for services.
  ## Secret
 To store confidential configs for services.
 ## Service
An abstraction layer built on top of a **group of Pods** (usually managed by a Deployment), exposing them with a **single, persistent name and IP address**. It abstracts away the ephemeral nature of Pods, enabling **kube-proxy** to automatically **load-balance** incoming traffic across all healthy instances.

>[!note]
>The pods that are under the service are selected by Kubernetes by matching them with labels. If the service states that it is for app=frontend then all pods that have app=frontend as a label will be exposed with the service.

>[!Difference between service name and Stateful set name]
>Service gives a persistent name to the entire group of pods, load balancing automatically between them.
>
>Stateful sets on the other hand give a persistent name to each pod of the set, even after recreating them.
 ## Ingress (legacy) / Gateway
Ingress exposes **HTTP and HTTPS** routes **from outside the cluster to services within the cluster, building on top of the service abstraction**. Traffic routing is controlled by rules defined on the Ingress resource. A single cluster Kubernets can host an arbitrary number of Ingress Implementations all configured trough the same interface. To enable custom behavior a custom Controller is associated with each implementation to enable configuration and manage lifecycle of the ingress

 ## Nodeport
 Similar to ingress as it works to expose traffic from outside the cluster to an instance but it does so working at a lower level. If Ingress relied on the abstraction of service and worked only for HTTP and HTTPS traffic, the **NodePort doesn't have any knowledge about routes or the type of traffic**, it only exposes a port to receive traffic and that's it.
 Most of the time Ingress is built on top of nodeport, adding to it the service for loadbalancing across instaces keeping track of ports and names.