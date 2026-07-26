---
title: Ethernet Speeds and Standards
created: 2026-07-26
updated: 2026-07-26
type: concept
tags: [ethernet, interconnect, optics, copper, distance, standard]
sources: []
---

# Ethernet Speeds and Standards

Ethernet is the universal data center networking fabric. Its evolution from 10G to 1.6T defines the backbone of modern AI clusters, cloud networks, and enterprise data centers.

## Speed Evolution Roadmap

| Standard | Speed | Lane config | Electrical | Optical | IEEE Spec | Year ratified |
|----------|-------|-------------|-----------|---------|-----------|---------------|
| 10GbE | 10 Gbps | 1 × 10G | Cat6a (100m) | SR (300m) | 802.3ae | 2002 |
| 25GbE | 25 Gbps | 1 × 25G | Cat8 (30m) | SR (100m) | 802.3by | 2016 |
| 50GbE | 50 Gbps | 2 × 25G | Cat8 (30m) | SR (100m) | 802.3cd | 2018 |
| 100GbE | 100 Gbps | 4 × 25G | Cat8 (30m) | SR4 (100m), DR (500m), FR (2km) | 802.3bg/bs | 2010/2017 |
| 200GbE | 200 Gbps | 4 × 50G PAM4 | ACC (3m) | SR4 (100m), DR (500m), FR (2km) | 802.3bs | 2017 |
| 400GbE | 400 Gbps | 8 × 50G PAM4 | ACC/AEC (3-5m) | SR8 (100m), DR4 (500m), FR4 (2km), LR8 (10km) | 802.3bs | 2017 |
| 800GbE | 800 Gbps | 8 × 100G PAM4 | AEC (up to 5m) | SR8 (100m), DR8 (500m), FR4 (2km), LR8 (10km) | 802.3df | 2024 |
| 1.6TbE | 1.6 Tbps | 8 × 200G PAM4 | AEC (up to 3m) | 2×FR4 (2km), 2×LR8 (10km) | 802.3dj | 2026 (expected) |

## Physical Layer Types

### Copper (within-rack, top-of-rack)
| Type | Max Reach | Cost | Power | Bend radius | Best for |
|------|-----------|------|-------|-------------|----------|
| **DAC** (Direct Attach Copper) | 2-3m (400G), 1-2m (800G) | $ | 0.1W | Poor (thick) | TOR-to-server, short leaf-spine |
| **ACC** (Active Copper Cable) | 3-5m | $$ | ~1W | Moderate | Longer rack runs, some inter-rack |
| **AEC** (Active Electrical Cable) | 5-7m (400G), 3-5m (800G) | $$$ | 2-5W | Better | Edge cases where optics are overkill |

See [[interconnect/concepts/dac-acc-aec-copper-optics]] for detailed comparison.

### Optical (inter-rack, pod, campus)
| Type | Reach | Fiber | Wavelength | Typical use |
|------|-------|-------|------------|-------------|
| **SR** (Short Reach) | 100m | MM (OM3/4/5) | 850nm VCSEL | Within row |
| **DR** (Data Rate) | 500m | SM (OS2) | 1310nm | Intra-pod |
| **FR** (Fiber Reach) | 2km | SM (OS2) | 1310nm | Inter-pod / building |
| **LR** (Long Reach) | 10km | SM (OS2) | 1310nm | Metro campus |
| **ZR** (Zettabit Reach) | 120km+ | SM (OS2) | C-band DWDM | Metro WAN |

### Co-Packaged Optics (CPO)
The next frontier: integrate optical engines directly into the switch ASIC package, eliminating the pluggable module. 800G CPO switches appearing in 2025-2026 from Broadcom, Cisco, Marvell. Saves 30-50% power vs. pluggable optics.

Repeatable CPO production depends on foundry-qualified photonic building blocks, compact models, design rules, and packaging-aware verification; see [[interconnect/concepts/silicon-photonics-pdk-and-foundry-ecosystem]].

## Ethernet in AI Clusters

Modern AI training clusters (10,000+ GPUs) require massive east-west bandwidth. Ethernet competes with InfiniBand for the AI fabric role:

### Strengths for AI
- **Open ecosystem** — multi-vendor interoperability (vs. InfiniBand's NVIDIA control)
- **RoCEv2** (RDMA over Converged Ethernet) — enables GPU Direct RDMA over Ethernet
- **Ultra Ethernet Consortium** — industry effort to optimize Ethernet for AI (2023+)
- **Lower cost per port** — 30-50% cheaper than InfiniBand at equivalent speeds

### Limitations
- **Higher tail latency** — fine-grained congestion control is harder than InfiniBand's token-based credit scheme
- **Larger switch radix** — fewer ports per chip vs InfiniBand switches at equivalent power
- **RoCEv2 complexity** — PFC (Priority Flow Control) tuning is notoriously difficult

## Related Pages
- [[interconnect/concepts/infiniband-vs-ethernet]] — head-to-head comparison for AI fabrics
- [[interconnect/concepts/dac-acc-aec-copper-optics]] — detailed copper vs. optical analysis
- [[interconnect/concepts/nvlink-and-nvswitch]] — GPU fabric complementing Ethernet
- [[rack-pod/concepts/pod-and-superpod-architectures]] — how Ethernet topologies scale
