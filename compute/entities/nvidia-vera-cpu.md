---
title: NVIDIA Vera CPU
created: 2026-08-03
updated: 2026-08-03
type: entity
tags: [cpu, vendor, lpddr, bandwidth, cxl, nvlink, chiplet]
sources: [raw/articles/the-register-nvidia-vera-olympus-deep-dive.md]
---

# NVIDIA Vera CPU

## Overview

Vera is NVIDIA's Armv9.2-compatible data-center CPU built around 88 custom
Olympus cores. It succeeds Grace and is designed both as the host CPU for Vera
Rubin GPU systems and as a standalone platform for agentic-AI runtimes.

Unlike CPUs that distribute cores across multiple compute chiplets, Vera keeps
all 88 cores on one monolithic compute die and moves memory-controller and I/O
functions onto surrounding chiplets. The architecture prioritizes coherent
bandwidth, irregular control-flow performance, large memory capacity, and tight
CPU-GPU coupling.

## Key Specifications

| Feature | Reported value |
| --- | --- |
| CPU architecture | Armv9.2-compatible Olympus |
| Cores / threads | 88 / 176 |
| Compute die | Monolithic, reported TSMC 3 nm |
| Maximum memory | 1.5 TB LPDDR5X per socket |
| Memory bandwidth | 1.2 TB/s per socket |
| On-chip coherent fabric | 3.4 TB/s bisection bandwidth |
| System-level cache | 164 MB distributed SLC |
| PCIe/CXL | 96 lanes, PCIe 6.4 and CXL 3.1 |
| NVLink-C2C | 1.8 TB/s bidirectional |
| Configurable TDP | 250-450 W |

The Vera CPU Superchip combines two sockets for 176 cores, 352 threads, 3 TB
LPDDR5X, and 2.4 TB/s aggregate memory bandwidth.

## Olympus Differentiators

- Wide 10-way decode/dispatch and a broad integer/vector back end.
- Neural branch prediction with two-path exploration.
- Memory renaming and value prediction to reduce dependency stalls.
- Spatial multithreading that partitions execution resources into two narrower
  threads instead of relying only on conventional shared-resource SMT.
- FP8-capable SVE2 vector pipelines for CPU-side low-precision processing.

## Vera Roles and Comparison With x86

### What are Vera's primary roles?

Vera has two principal roles in NVIDIA's data-center architecture.

The first is the **host and control CPU for Vera Rubin systems**. It performs
work that does not map naturally onto GPUs, including orchestration, scheduling,
data preparation, storage and network I/O, memory management, operating-system
services, and CPU-side inference work. NVLink-C2C is intended to reduce the
communication penalty between these CPU responsibilities and Rubin GPU compute.

The second is **standalone execution of agentic workloads**. An AI agent runs
more than an LLM: generated code, tool clients, retrieval, databases, graph
operations, control flow, and isolated runtime environments are commonly
CPU-oriented. Olympus targets such irregular execution through branch and value
prediction, large caches, memory bandwidth, and spatial multithreading.

### Can an x86 CPU perform both roles?

Yes. AMD EPYC and Intel Xeon already host GPUs and run agent software. Vera does
not introduce a function that x86 is fundamentally unable to perform. Its claim
is tighter specialization and integration.

| Dimension | Vera | Typical x86 server CPU |
| --- | --- | --- |
| GPU attachment | Coherent NVLink-C2C for NVIDIA systems | Commonly PCIe; CXL and vendor-specific coherent options vary |
| GPU-platform integration | Co-designed for Rubin | Vendor-neutral accelerator hosting |
| Local memory | High-capacity LPDDR5X | DDR5 or MRDIMM, usually more configurable |
| Memory objective | Bandwidth and lower subsystem power | Capacity, expandability, ecosystem flexibility |
| CPU ISA | Armv9.2-compatible | x86-64 |
| Agent emphasis | Olympus mechanisms target irregular runtime work | Mature general-purpose cores and conventional SMT |
| Software ecosystem | Arm software must be qualified | Broad and mature x86 compatibility |
| Platform flexibility | Strongest inside NVIDIA's stack | Supports accelerators from many vendors |

The most consequential distinction is CPU-GPU coupling. A conventional x86
host often exchanges data with GPU HBM over PCIe. Vera Rubin uses coherent
NVLink-C2C, potentially reducing copies, synchronization overhead, and bandwidth
bottlenecks when workloads frequently cross the CPU-GPU boundary.

That advantage is workload-dependent. A model that remains almost entirely in
GPU HBM may receive little benefit, while an x86 platform may win on software
compatibility, DIMM flexibility, core count, acquisition cost, or freedom from
vendor lock-in.

## Vera-Only CPU Servers

### Is there an advantage without Rubin GPUs?

Potentially. A Vera-only server is best understood as a high-capacity,
high-memory-bandwidth Arm server for irregular and memory-intensive workloads,
not as a replacement for GPU tensor throughput.

Potentially suitable workloads include:

- Agent orchestration and sandbox execution.
- Graph databases and graph traversal.
- Vector databases and retrieval services.
- Large in-memory databases and analytics.
- Storage, networking, and data-preparation services.
- CPU inference for smaller or strongly quantized models.
- CXL memory-control and fabric services.

| Vera-only characteristic | Potential system benefit |
| --- | --- |
| Up to 1.5 TB LPDDR5X per socket | Large resident databases, indexes, and agent state |
| 1.2 TB/s memory bandwidth per socket | Higher throughput for bandwidth-sensitive CPU work |
| Reported 30-40 W memory subsystem | Potentially lower memory power than heavily populated DIMMs |
| 88 Olympus cores | Concurrent services and agent sandboxes |
| Spatial multithreading | Possible isolation between generated work and runtime services |
| PCIe 6.4 and CXL 3.1 | High-speed I/O and memory expansion/pooling |
| Dual-socket NVLink-C2C | Lower inter-socket bottleneck than conventional links may provide |

A dual-socket Vera CPU Superchip is reported to provide 176 cores, 352 threads,
3 TB of LPDDR5X, and 2.4 TB/s aggregate memory bandwidth.

### What are the limitations?

- It does not provide GPU-class dense tensor throughput or HBM bandwidth.
- Arm application and operational compatibility must be validated.
- LPDDR/SOCAMM2 capacity may be less field-configurable than standard DIMMs.
- Platform pricing and availability determine whether power savings improve TCO.
- NVIDIA platform integration can increase vendor dependency.
- Vendor performance and power claims still require independent measurement.

The Vera-only proposition is therefore strongest when memory bandwidth,
capacity, control-flow performance, and power matter more than dense tensor
compute or maximum software portability.

## System-Level Significance

Vera connects three normally separate domains:

1. High-capacity, comparatively low-power [[memory/concepts/lpddr-memory]].
2. Coherent CPU-GPU attachment through
   [[interconnect/concepts/nvlink-and-nvswitch]].
3. PCIe/CXL attachment to storage, NICs, accelerators, and pooled memory.

This makes Vera relevant to agent orchestration and data movement as well as to
traditional CPU execution. Its success should be evaluated at system level,
including GPU utilization, host-memory power, agent density, and communication
overhead.

## Coherence Beyond Two CPUs

### Are two Vera CPUs in one coherent address space?

Yes. A dual-socket Vera CPU Superchip connects its two CPUs through
NVLink-C2C. The connection supports hardware-coherent access, allowing the
operating system and applications to use the pair as one coherent NUMA system.

Each CPU still has physically local LPDDR5X. Coherence therefore does not imply
uniform access latency: local memory should remain faster and less expensive to
access than memory attached to the other CPU.

```text
One coherent NUMA node

Vera CPU 0                   Vera CPU 1
1.5 TB local LPDDR5X         1.5 TB local LPDDR5X
      \                       /
       coherent NVLink-C2C
```

### What happens beyond the two-CPU boundary?

The public material reviewed here does not establish a hardware-coherent CPU
domain larger than one two-socket Vera CPU Superchip. Rack configurations with
many Superchips should therefore be modeled as multiple coherent nodes joined
by a scale-out network, not as one rack-wide cache-coherent machine.

```text
Coherent node A                         Coherent node B
Vera 0 <-> Vera 1                       Vera 2 <-> Vera 3
        \                                 /
         Ethernet or InfiniBand/RDMA
             not CPU-cache coherent
```

Candidate inter-node interfaces include Spectrum-X Ethernet, InfiniBand, and
ConnectX-class NICs attached through PCIe. Software communicates through MPI,
RDMA, RPC, distributed storage, or other distributed-runtime mechanisms.

### Does CXL 3.1 make the rack coherent?

No, not automatically. CXL 3.1 can provide coherent attachment to memory
expansion devices and fabric-attached memory pools. That can let multiple hosts
access shared or partitioned capacity under an explicit fabric and software
policy, but it does not by itself make hundreds of CPU caches behave like one
conventional SMP system.

CXL memory access also remains a different performance tier from local
LPDDR5X. A system model must distinguish:

- Local socket memory.
- Remote memory in the paired Vera socket.
- CXL-attached or pooled memory.
- Memory owned by another networked node.

### How does this compare with x86?

The hierarchy is conceptually similar to modern x86 infrastructure:

| Boundary | Vera | AMD/Intel x86 |
| --- | --- | --- |
| Within socket | Scalable Coherency Fabric | On-die or package-coherent fabric |
| Two sockets | Coherent NVLink-C2C | xGMI/Infinity Fabric or UPI |
| Beyond supported sockets | Networked distributed nodes | Networked distributed nodes |
| Memory expansion/pooling | CXL 3.1 | CXL where platform support exists |

Vera's potential advantage is the bandwidth and NVIDIA integration inside the
node, not a unique rack-wide coherence model.

### Why not extend hardware coherence across the whole rack?

Large coherence domains introduce growing costs for cache-line ownership,
invalidation traffic, latency, fabric bandwidth, failure isolation, and
predictable performance. The practical architecture is consequently
hierarchical:

1. Tight coherence within one Vera CPU.
2. Coherent NUMA across the two CPUs in a Superchip.
3. Distributed communication between Superchips.
4. CXL expansion or pooling for selected memory tiers.

This boundary must remain explicit in rack-level performance and memory models;
the existence of 128 Superchips in a reference rack does not imply 256-way CPU
cache coherence.

## Evidence Status

The architectural specifications and NVIDIA performance estimates are public,
but production hardware still needs independent benchmarking. Vendor-reported
claims such as 1.8 times per-core agentic performance and large memory-power
savings should remain provisional until compared at equal workload, capacity,
software stack, and service level.

## Related Pages

- [[compute/entities/nvidia-gpu-lineage]]
- [[compute/concepts/gpu-architecture]]
- [[memory/concepts/lpddr-memory]]
- [[interconnect/concepts/nvlink-and-nvswitch]]
- [[raw/articles/the-register-nvidia-vera-olympus-deep-dive]]
