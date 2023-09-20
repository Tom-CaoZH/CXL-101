
CXL is a fairly new technology, and there aren't any commercial hardware options for CXL devices on the market yet. That's why it's crucial to have ways to emulate CXL devices for testing and development purposes.

There are primarily two methods for simulating CXL-based memory. The first involves using Qemu for emulation, and the second method uses NUMA node memory for emulation. Additionally, there are some lightweight methods available for emulating CXL-based memory, focusing mainly on performance considerations.

# Qemu Emulation

CXL devices can be emulated with the help of Qemu. As of 8/22/2023, the [mainline QEMU](https://www.qemu.org/docs/master/system/devices/cxl.html) have full support for creating CXL volatile memory devices and also non-volatile memory devices. Also, [Linux kernel](https://docs.kernel.org/driver-api/cxl/memory-devices.html) supports the CXL-related drivers. Thus, it's pratical to set a CXL-based Memory device in this way, you can find [here](https://memverge.com/cxl-qemuemulating-cxl-shared-memory-devices-in-qemu/)

# NUMA Memory Emulaton

In a NUMA setup, processors within a computer system share local memory and collaborate. Think of NUMA as a kind of microprocessor cluster contained in a single box. Microsoft's paper, which you can find [here](https://dl.acm.org/doi/pdf/10.1145/3575693.3578835), suggests a way to simulate cxl devices on 2-socket server systems based on two key characteristics of cxl-connected dram. The first characteristic is a latency of 150 ns, and the second is that no local CPU can directly access this CPU-less node. Essentially, they've established two virtual nodes, each aligned with a physical node. One has CPUs, while the other is without CPUs. They've designated the memory associated with the CPU-less node as cxl-memory. According to Pond, the latency introduced by this setup closely matches what cxl promises. You can find instructions on how to set up a Pond virtual machine with this design [here](https://github.com/vtess/Pond).


# Lightweight Emulation

There are also simpler methods available that can help your program perform almost as well as if it were running on CXL-based Memory. [CXLMemSim](https://github.com/SlugLab/CXLMemSim) is a quick and efficient CXL.mem simulator designed for performance analysis. It utilizes a performance model based on performance monitoring events, which are widely supported by common processors. Additionally, it can simulate complex system topologies.