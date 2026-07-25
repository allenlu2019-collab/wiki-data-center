---
title: InfiniBand vs Ethernet for AI Fabrics
created: 2026-07-26
updated: 2026-07-26
type: comparison
tags: [infiniband, ethernet, interconnect, fabric, comparison, topology]
sources: []
---

# InfiniBand vs Ethernet for AI Fabrics

InfiniBand and Ethernet compete as the primary inter-node fabric for AI training clusters. The choice defines interconnect cost, performance, scalability, and vendor lock-in.

## Generations Comparison

| Generation | Speed per lane | Year | Typical 1-port speed | Switch radix | Topology support |
|-----------|---------------|------|---------------------|-------------|-----------------|
| **InfiniBand HDR** | 50 Gbps | 2020 | 200 Gbps (4x) | 40 ports (QSFP) | Fat-tree, Dragonfly+ |
| **InfiniBand NDR** | 100 Gbps | 2022 | 400 Gbps (4x) | 64 ports (QSFP) | Fat-tree, Dragonfly+ |
| **InfiniBand XDR** | 200 Gbps | 2025 | 800 Gbps (4x) | 128 ports? | Fat-tree, Dragonfly+ |
| **Eth 400GbE** | 100 Gbps PAM4 | 2022 | 400 Gbps | 64-128 ports | Leaf-spine, CLOS |
| **Eth 800GbE** | 200 Gbps PAM4 | 2024 | 800 Gbps | 64 ports | Leaf-spine, CLOS |
| **Eth 1.6TbE** | 200 Gbps × 8 | 2026 | 1.6 Tbps | 64-128 ports | CLOS, Dragonfly |

## Key Comparison Dimensions

### Performance

| Metric | InfiniBand NDR | 400GbE | Winner |
|--------|---------------|--------|--------|
| Raw bandwidth | 400 Gbps | 400 Gbps | Tie |
| Effective throughput (RDMA) | >98% line rate | 92-97% (RoCEv2) | IB |
| P99 tail latency | ~1-2 μs | ~3-8 μs | IB |
| Congestion control | Token-based, hardware | PFC + DCQCN (software heavy) | IB |
| GPU Direct support | Native, mature | Native but complex tuning | IB |
| Multi-pod scale | Dragonfly+ for 64K nodes | CLOS for 32K+ nodes | IB (edge) |
| Jitter (collective ops) | Very low | Moderate-High (PFC storms) | IB |

### Ecosystem

| Dimension | InfiniBand | Ethernet |
|-----------|------------|----------|
| **Vendors** | NVIDIA (Mellanox) only | Broadcom, Marvell, Cisco, Arista, Juniper, Intel, AMD/Pensando |
| **Switch ASICs** | NVIDIA Quantum | Tomahawk (Broadcom), Jericho (Marvell), Silicon One (Cisco) |
| **NIC vendors** | NVIDIA ConnectX | Broadcom, Intel, NVIDIA, AMD |
| **CPU support** | Best on x86, growing ARM | Universal |
| **Software stack** | OFED (proprietary drivers) | OS-native + rdma-core |
| **Lock-in risk** | High (single vendor) | Low (multi-vendor) |

### Cost

| Item | InfiniBand NDR (400G) | 400GbE |
|------|----------------------|--------|
| NIC per port | ~$4,000-6,000 | ~$1,500-2,500 |
| Switch per 400G port | ~$2,500-4,000 | ~$1,200-2,000 |
| Cable (optic, 3m AOC) | ~$400-600 | ~$300-500 |
| **Total per 400G link** | ~$7,000-10,600 | ~$3,000-5,000 |

Ethernet is typically **40-60% cheaper** per connected GPU for an equivalent-speed fabric. At 100,000 GPU scale, this difference is tens of millions of dollars.

### Topology and Scale

#### InfiniBand: Dragonfly+
- NVIDIA's preferred topology for H100/B200 training clusters
- Each group of switches forms a "dragonfly" — low diameter, high path diversity
- Supports up to ~64,000 endpoints (NDR) without oversubscription
- Adaptive routing automatically distributes traffic across paths
- See [[rack-pod/concepts/pod-and-superpod-architectures]]

#### Ethernet: CLOS / Leaf-Spine
- Traditional two-tier (leaf-spine) or three-tier (spine-super-spine) CLOS
- Each switch connects to every other at the next tier
- Easier to design and troubleshoot
- Higher diameter than Dragonfly — 3-5 hops vs 2-3 hops
- Ultra Ethernet Consortium defining new topologies optimized for AI

## The Ultra Ethernet Consortium (UEC)

Founded 2023 by AMD, Arista, Broadcom, Cisco, Intel, Meta, Microsoft. Goal: make Ethernet competitive with InfiniBand for HPC/AI.

### Key UEC features (in progress)
- **Packet spraying** — distribute packet-level (not flow-level) traffic across paths
- **Congestion control** — end-to-end credit-based (like IB), not PFC
- **App-aware load balancing** — collective operation-aware routing
- **Improved RDMA** — lower latency, better collective offload

UEC 1.0 specification expected ~2026. Early silicon from Broadcom (Tomahawk 6/7) and Marvell.

## Verdict

| Scenario | Recommended Fabric | Rationale |
|----------|-------------------|-----------|
| Greenfield 10K+ GPU cluster | InfiniBand NDR/XDR | Proven at scale, lowest tail latency, Dragonfly+ topo |
| Multi-tenant cloud (AI + general) | 400/800GbE | Shared infrastructure, multi-vendor, lower cost |
| Cost-sensitive training | 400/800GbE RoCE | UEC improvements expected to close gap |
| NVIDIA-exclusive stack | InfiniBand | Best NVIDIA integration, NVLink + IB combo |
| OCP-style disaggregated | Ethernet | Open networking, whitebox switches, SONiC |

## Related Pages
- [[interconnect/concepts/ethernet-speeds-and-standards]] — Ethernet technical details
- [[interconnect/concepts/nvlink-and-nvswitch]] — GPU fabric complementing IB/Ethernet
- [[interconnect/concepts/dac-acc-aec-copper-optics]] — physical cabling options
- [[rack-pod/concepts/pod-and-superpod-architectures]] — how fabrics scale to pod level
- [[compute/concepts/gpu-architecture]] — GPU interconnect requirements