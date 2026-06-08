Kubernetes is based on a mater-worker architecture, in this notes you will find the description of each component of the workers:

![[Kubernetes - worker architecture.png]]


## Pods
Pods are the smaller unit of workload that can be deployed. They are environments to run a container (or multiple container, for most cases not recommended).

>[!question]
>Tell me an example in which deploying a pod with more than one container is useful:
>it makes sense when the containers are **tightly coupled and need to share the same lifecycle, network namespace, and storage**.
>
>The classic pattern is the **sidecar pattern**.
>
>If you deployed the helper as a separate Pod It could be scheduled on a different node, It wouldn't automatically share local files, Communication would require networking instead of localhost, Lifecycle coordination would be harder.


## Kubelet
The kubelet is a **local controller** that communicates with the apiserver receiving the workloads to execute on the worker nodes. This specifications are mainly **PodSpecs**, manifests that describe the workload to run as a pod.
So, after receiving what to run, the kubelet acts to schedule that workload on the worker node, monitoring it and ensuring **it is running as expected and healty** (same old control loop).

>[!note]
>Even though we said that the kubelet works with PodSpecs the pod isn't the only workload that can be run, see [[Kuberenetes - Types of workloads]] for more informations.

>[!note]
>The kubelet enables for observability of the pods that run on the node and also a bit on the node itself. Better observability of the node itself is usually implemented deploying Deamonsets.
## Kube-proxy
The Kubernetes network proxy runs on each node and **it reflects services as defined** in the Kubernetes API on each node and do simple TCP, UDP, and SCTP stream forwarding or round robin TCP, UDP, and SCTP forwarding across a set of backends.

>[!note]
>The kube-proxy is optional, if no service is exposed this isn't needed.

>[!note]
>There is an optional addon that provides cluster DNS for these cluster IPs.

>[!important]
>The user must create a service with the apiserver API to configure the proxy.