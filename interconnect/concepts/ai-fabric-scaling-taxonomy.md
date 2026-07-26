---
title: AI Fabric Scaling Taxonomy
created: 2026-07-26
updated: 2026-07-26
type: concept
tags: [fabric, topology, scale, gpu, tpu, pcie, cxl, nvlink]
sources: [raw/articles/ai-data-center-interconnect.md, raw/articles/baidu-china-ai-superpod-waic-2026.md]
---

# AI Fabric Scaling Taxonomy

AI systems use several interconnect domains with different semantics. “Scale-up,” “scale-out,” and “scale-across” describe accelerator-to-accelerator communication domains; host links such as PCIe and CXL serve CPU–accelerator attachment and should not be conflated with them.

## Architectural View

| Domain | Scope | Typical communication | Examples | Primary design goal |
|---|---|---|---|---|
| Host attachment | CPU ↔ accelerator/device | Control, DMA, memory mapping | PCIe, CXL | Compatibility and coherent attachment |
| Scale-up | Accelerators within a tightly coupled system | Load/store-like peer access, collectives | NVLink/NVSwitch, TPU ICI | Very high bandwidth and low latency |
| Scale-out | Systems or racks across a cluster | RDMA and message passing | InfiniBand, RoCE/Ethernet | Large fabrics and efficient collectives |
| Scale-across | Pods/sites or composable optical domains | Long-reach fabric extension | Optical circuit switching, photonic fabrics | Reach, reconfiguration, and power efficiency |

The labels are architectural rather than purely physical. A path that detours through the host CPU is host-mediated communication, even if it ultimately moves data between accelerators.

## Superpod Boundary

The term **superpod** (including Chinese **超节点**) does not by itself identify one fabric domain. A tightly coupled superpod may extend the scale-up domain across multiple servers or racks through proprietary electrical or optical links, unified memory addressing, and global scheduling. A larger deployment may instead combine several scale-up islands over a scale-out fabric. Architecture should therefore be classified by communication semantics and fault boundaries, not by the vendor's card-count label.

Chinese vendor presentations at WAIC 2026 illustrate this distinction: Huawei's Lingqu and Moore Threads' MTLink are positioned as scale-up mechanisms, while thousand-card and larger clusters still require system-level topology, reliability, cooling, and orchestration beyond the link protocol. ^[raw/articles/baidu-china-ai-superpod-waic-2026.md]

## Coherency Semantics

GPU peer fabrics do not necessarily provide CPU-style, system-wide cache coherence. AI workloads are commonly coordinated through explicit kernels, collectives, barriers, DMA, and software-managed ownership. Coherence becomes valuable for tightly shared data structures or unified programming models, but it adds directory state, invalidation traffic, latency, and implementation complexity.

## Three Scale-up Approaches

| Approach | Switching medium | Strength | Constraint |
|---|---|---|---|
| NVSwitch | Electrical packet/crossbar switching | Mature GPU memory semantics and dense all-to-all connectivity | Proprietary ecosystem and electrical reach |
| Google TPU optical circuit switching | Reconfigurable optical paths | Topology can be reshaped around workload placement | Circuit setup and orchestration complexity |
| Photonic fabric | Optical switching close to compute | Potentially high bandwidth density and lower energy over distance | Packaging, yield, control-plane, and ecosystem maturity |

## Design Questions

- Does the workload require direct peer memory access or only message passing?
- What fraction of communication stays within a server, rack, pod, or site?
- Is topology static, packet-switched, or optically reconfigurable?
- Which layer owns congestion control, collectives, reliability, and isolation?
- Does memory capacity scale with the fabric, or only compute participation?

## Related Pages

- [[interconnect/concepts/nvlink-and-nvswitch]] — NVIDIA scale-up fabric
- [[interconnect/concepts/infiniband-vs-ethernet]] — scale-out alternatives
- [[interconnect/concepts/ethernet-speeds-and-standards]] — Ethernet physical and protocol roadmap
- [[memory/concepts/hbm-memory-architecture]] — local accelerator memory feeding the fabric
- [[rack-pod/concepts/pod-and-superpod-architectures]] — deployment hierarchy

