#cloud #distributed-systems #notes

Virtualization and emulation are both techniques used to abstract hardware resources, but they differ in *how* they achieve compatibility and performance.

---

## Virtualization

Virtualization creates **virtual instances of real hardware** by exposing a software layer (hypervisor) that allows multiple systems to share the same physical resources.

- Runs **guest systems close to native hardware**
- Requires the guest OS to be compatible with the underlying architecture
- Typically uses a hypervisor (e.g. KVM, Xen, VMware)
- High performance (near-native execution)

### Key idea
> “Same architecture, shared efficiently”

### Example
- Running multiple Linux VMs on a physical server via a hypervisor

---

## Emulation

Emulation simulates **a different hardware or system architecture entirely**, translating instructions from one system to another and, by doing so, **adding features**.

- Can run software designed for a *different CPU/architecture*
- Slower due to instruction translation
- More flexible than virtualization
- Can include additional features not present in original hardware

### Key idea
> “Different architecture, simulated behavior”

### Example
- Running a Game Boy emulator on a modern PC
- Running ARM software on x86 hardware via emulation

---

## Main difference (core intuition)

- Virtualization → shares real hardware efficiently  
- Emulation → reproduces hardware behavior in software

---

## In cloud computing context (OpenStack/Kubernetes)

- **Virtualization** is used to create VMs (OpenStack Nova, hypervisors)
- **Emulation** is rarely used for production workloads due to performance cost
- Cloud systems prioritize virtualization for scalability and efficiency

---

## Quick comparison
|Feature|Architecture|Performance|Flexibility|Use case|
|---|---|---|---|---|
|Virtualization|Same|High|Medium|Cloud, VMs|
|Emulation|Can differ|Lower|High|Compatibility, legacy systems|
