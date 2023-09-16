*This introduction is a summary of the referrence paper and there also exists my understandings to CXL.*

# Background 

The Compute Express Link (CXL) is an open industry standard that defines a family of interconnect protocols between CPUs and devices. As a general device interconnect, CXL takes a broad definition of devices including GPUs, GP-GPUs, FPGAs, as well as a wide range of purpose-built accelerators and storage devices. Traditionally, these devices use the PCIe serial interface. CXL also targets memory which is traditionally connected to the CPU through the DDR parallel interface.

While PCIe and DDR have been great interfaces for a wide range of devices, they also come with some inherent limitations. These limitations lead to the following challenges that motivated the development and deployment of CXL.

## Challenge 1: coherent access to system and device memory. 

**Summary: PCIe attached devices(GPU, SmartNICs and etc) cann't directly access host memory. Also host cann't directly access the memory of PCIe attached devices. There exists cache coherence problem.**

System memory is conventionally attached via DDR and cacheable by the CPU cache hierarchy. In contrast, accesses from PCIe devices to system memory happen through
non-coherent reads/writes. A PCIe device cannot cache system memory to exploit temporal or spatial locality or to perform atomic sequences of operations. Similarly, memory attached to a PCIe device is accessed non-coherently from the host, with each access handled by the PCIe device. Non-coherent accesses work well for streaming I/O operations (such as network access or storage access). For accelerators(GPUs, SmartNICs and etc), entire data structures are moved from system memory to the accelerator for specific functions before being moved back to the main memory and software mechanisms are deployed to prevent simultaneous accesses between CPUs and accelerator(s). This challenge also arises in the evolving area of processing-in-memory (PIM), which seeks to move computation close to data. There is currently no standardized approach for PIM devices to coherently access data that may be present in the CPU cache hierarchy. 


## Challenge 2: memory scalability. 

**Summary: Due to the characteristic of DDR, the bandwidth of DDR attached memory is low and also DDR only support DRAM-based memory.**

Demand for memory capacity and bandwidth increases proportionate to the exponential growth of compute. Unfortunately, DDR memory has not been keeping up with this demand. This limits **memory bandwidth** per CPU. A key reason for this mismatch in scaling is the pin-inefficiency of the parallel DDR interface. In principle, PCIe pins would be a great alternative due to their superior memory bandwidth per pin. For example, a x16 Gen5 PCIe port at 32 GT/s offers 256 GB/s with 64 signal pins. DDR5-6400 offers 50 GB/s with ~200 signal-pins. Unfortunately, PCIe does not support coherency. Thus, PCIe has not been able to replace DDR.

Another scaling challenge is that DRAM memory cost per bit has recently stayed flat. While there are multiple promising media types including Managed DRAM, ReRam, 3DXP/Optane, the DDR standard relies on DRAM-specific commands for access and maintenance, which hinders adoption of new media types.


## Challenge 3: memory and compute inefficiency due to stranding. 

**Summary: memory stranding problem in data center which leads to low memory utilization.**


Today’s data centers are inefficient due to stranded resources. A resource, such as memory, is stranded when idle capacity remains while another resource, such as
compute, is fully used. The underlying cause is tight resources coupling where compute, memory, and I/O devices belong to only one server. As a result, each server needs to be overprovisioned with memory and accelerators to handle workloads with peak capacity demands. For example, a server that hosts an application that needs more memory (or accelerators) than available cannot borrow memory (or accelerators) from another underutilized server in the same rack and must suffer the performance consequences of page misses. On the other hand, servers where all cores are used by workloads often have memory remaining unused.


## Challenge 4: fine-grained data sharing in distributed systems. 

**Summary: we need high performance protocal which can handle fine-grained sychronization in distributed systems.**

Distributed systems frequently on fine-grained synchronization. The underlying updates are often small and latency sensitive. Like distributed databases rely on kB-scale pages and distributed consensus with even smaller updates. Sharing data at such fine granularity, means that the communication delay in typical datacenter networks dominates the wait time for updates and slows down these important use cases. For example, transmitting 4kB at 50GB/s takes under 2us, but communication delays exceed 10us on current networks. A coherent shared memory implementation can help cut down communication delays to sub microseconds.


# CXL Specification

CXL has been developed to address these four and other challenges. CXL has evolved through three generations. Each generation specifies the interconnect and multiple
protocols while remaining fully backward compatible. The CXL 1.0(1.1) specification adds coherency and memory semantics on top of PCIe. This addresses Challenges 1 (coherency) and Challenge 2 (memory scaling). CXL 2.0 additionally addresses Challenge 3 (resource stranding) by enabling resource pooling across multiple hosts. CXL 3.0 addresses Challenge 3 on a larger scale with multiple levels of CXL switching. Furthermore, CXL 3.0 addresses Challenge 4 (distributed data sharing) by enabling fine-grained memory sharing across host boundaries.

## CXL 1.1

There are three protocols and also three types devices.

### Protocols

CXL is implemented using three protocols, CXL.io, CXL.cache, and CXL.memory (aka. CXL.mem), which are dynamically multiplexed on PCIe PHY. Below is their functions.

1. CXL.io: device discovery, configuration, initialization, I/O virtualization, and DMA using non-coherent load-store semantics.(like PCIe)
2. CXL.mem: allow host access device memory.(HDM: Host-managed Device Memory)
   1. Memory expander: HDM-H(H: host-only coherence)
   2. Accelerator memory: HDM-D(D: Device-managed coherence)
3. CXL.cache: enable a device to use host memory.


### Devices

Type 1 devices are accelerators such as SmartNICs that use coherency semantics along with PCIe-style DMA transfers. Thus, they implement only the CXL.io and CXL.cache protocols. 

Type 2 devices are accelerators such as GP-GPUs and FPGAs with local memory that can be mapped in part to the cacheable system memory(cxl.mem: host access device memory). These devices also cache system memory for processing(cxl.cache: device access host memory). Thus, they implement CXL.io, CXL.cache and CXL.mem protocols. 

Type-3 devices are used for memory bandwidth and capacity expansion and can be used to connect to different memory types, including supporting multiple memory tiers attached to the device. Thus, Type-3 devices would implement only the CXL.io and CXL.mem protocols.

## CXL2.0 

CXL2.0 enables resource pooling which allows assigning the same resources to different hosts over time. The ability to reassign resources at run time solves resource stranding (Challenge 3) as it overcomes the tight coupling of resources to individual hosts. If one host runs a compute intensive workload and does not use the device
memory assigned from the pool, operators can reassign this device memory to another host, which might run a memory intensive workload. The same pooling construct is applicable to other resources like accelerators.

**Caution: for CXL2.0, a memory region can only assign to one host, it can not be shared among multiple hosts.**

CXL 2.0 adds **Hot-Plug**, **Single Level Switching**, Quality-of-Service (QoS) for Memory, **Memory Pooling**, **Device Pooling**, and Global Persistent Flush (GPF). 

Hot-Plug was not allowed in CXL 1.1 which precludes adding CXL resources after platform boot. CXL 2.0 enables standard PCIe hot-plug mechanisms enabling traditional physical hot-plug and dynamic resource pooling.

## CXL3.0 


# Referrence

[An Introduction to the Compute Express Link (CXL) Interconnect](https://arxiv.org/ftp/arxiv/papers/2306/2306.11227.pdf)