---
title: Pod and Superpod Architectures
created: 2026-07-26
updated: 2026-07-26
type: concept
tags: [rack, pod, superpod, topology, scale, fabric]
sources: []
---

# Pod and Superpod Architectures

Modern AI clusters scale by grouping compute into hierarchical units: the rack, the pod, and the superpod. Each level introduces bandwidth oversubscription, cooling distribution, and power delivery boundaries.

## The Hierarchy

```
┌─────────────────────────────────────────────────────┐
│                    Superpod                          │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ │
│  │   Pod 0  │ │   Pod 1  │ │   Pod 2  │ │   Pod 3  │ │
│  │ ┌──┐┌──┐ │ ┌──┐┌──┐ │ ┌──┐┌──┐ │ ┌──┐┌──┐ │ │
│  │ │R0││R1│ │ │R0││R1│ │ │R0││R1│ │ │R0││R1│ │ │
│  │ └──┘└──┘ │ └──┘└──┘ │ └──┘└──┘ │ └──┘└──┘ │ │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘ │
└─────────────────────────────────────────────────────┘
```

## Level 1: The Rack

### Typical AI Rack Configuration

| Component | H100 DGX rack | B200 NVL72 rack |
|-----------|--------------|-----------------|
| Compute nodes | 4× DGX H100 | 1× NVL72 (72 GPUs) |
| GPUs per rack | 32 (4×8) | 72 |
| Power | 40-50 kW | 100-150 kW |
| Cooling | Direct-to-chip (cold plate) | Direct-to-chip (cold plate) |
| Interconnect | 16× InfiniBand NDR | 18× InfiniBand XDR |
| Weight | ~1,500-2,000 lbs | ~3,000 lbs |
| Form factor | 48U or 52U rack | Nvidia-proprietary rack |

### Rack Components
- **Power shelf** — converts facility power (400V AC or 800V DC) to rack-level 48V DC
- **CDU** (coolant distribution unit) — small CDU per rack or shared across 2-4 racks
- **Top-of-Rack (TOR) switch** — leaf switch connecting compute nodes to fabric
- **PDU / bus bar** — power distribution within the rack
- **Cable management** — optical/copper cable routing

## Level 2: The Pod

A pod is the smallest complete AI deployment unit — typically 64-256 GPUs with non-blocking or 1:1 oversubscribed intra-pod fabric.

### DGX SuperPOD Architecture (H100)

NVIDIA's standard pod definition:

| Component | Count | Notes |
|-----------|-------|-------|
| DGX H100 nodes | 64 | 8 GPUs each |
| Total GPUs | 512 | 64 × 8 |
| Pod power | ~800 kW | 64 × 12.5 kW per node |
| InfiniBand leaf switches | 32 × QM9700 | 64-port NDR each |
| Network oversubscription | 1:1 (non-blocking) | Full bisection bandwidth |
| NVLink domain per node | 900 GB/s | Within each 8-GPU node |
| Fabric topology | Fat-tree (Dragonfly+) | 3-tier: Leaf → Spine → Dragonfly |
| Cooling | CDU per ~8 racks | 4-6 racks per CDU |
| Physical footprint | ~4-6 rows of racks | + networking + CDU |

### B200 NVL72 Pod (NVIDIA Reference)

| Component | Count | Notes |
|-----------|-------|-------|
| NVL72 racks | 8 | 72 GPUs per rack |
| Total GPUs | 576 | 8 × 72 |
| Pod power | ~1-1.2 MW | 8 × 120-150 kW |
| Interconnect | NVLink 5 (in-rack) + XDR IB (between racks) | In-rack all-to-all via NVSwitch |
| Oversubscription | Non-blocking within rack; 1:1 or 2:1 between racks | |
| Cooling | CDU per rack or 1:2 | Direct-to-chip |

### Non-NVIDIA Pods (OCP / Open Architecture)

- **AMD MI300X pods**: 8-16 racks with ROCm + InfiniBand
- **Google TPU pods**: 4096 TPU v4 chips via OCS optical circuit switching
- **AWS Trainium UltraCluster**: Up to 100,000 Trainium2 chips via EFA fabric
- **Meta Grand Teton**: OCP-based GPU compute with 400/800GbE fabric

These typically follow a **leaf-spine CLOS** topology with 32-128 leaf switches and 16-64 spine switches per pod.

## Level 3: The Superpod

Multiple pods connected via a higher-tier network to form a superpod — 1,000+ GPUs, up to 100,000+.

### NVIDIA DGX SuperPOD (Large Scale)

| Component | Single Superpod | Dual Superpod |
|-----------|----------------|---------------|
| GPUs | 4,096 (8 pods × 512) | 8,192 (16 pods) |
| Power | ~6.5 MW | ~13 MW |
| Spine switches | 16-32 | 32-64 |
| Fabric | Dragonfly+ (3-tier) | Dragonfly+ (4-tier) |
| Cooling | Chiller plant + CDUs | Chiller plant + CDUs |
| Inter-superpod | Optical / DWDM | 800G ZR for 120 km reach |
| Physical footprint | ~20,000 sq ft | ~40,000 sq ft |

### Exascale / Frontier-Scale Topology

The fastest supercomputers combine 10,000+ GPUs/accelerators:

**Frontier (HPE Cray EX, Oak Ridge):**
- 37,632 AMD MI250X GPUs (not NVIDIA)
- 74,624 AMD EPYC CPUs
- 2.2 MW per row, 60 rows
- 8.2 MW cooling
- Total: ~30 MW HPL, ~40 MW power
- Fabric: HPE Slingshot (Ethernet derivative) — Dragonfly topology
- 90-mile (150 km) fiber optic cabling

### Key Scaling Challenges

| Challenge | Rack | Pod | Superpod |
|-----------|------|-----|----------|
| Power distribution | 48V bus bar | 400V AC / 800V DC breakers | Medium-voltage utility feed |
| Cooling | Rack-level CDU | Pod-level CDU + chiller | Chiller plant, cooling towers |
| Network | TOR switch (1U) | Leaf-Spine (32-64 switches) | Multi-tier fabric, optical interconnects |
| Cabling | <10m DAC/AOC | <100m SR/DR optics | <2km FR optics or ZR for multi-campus |
| Management | BMC/IPMI per node | Cluster manager (Slurm/Kubernetes) | Facility-wide orchestration |

## Related Pages
- [[compute/concepts/gpu-architecture]] — compute building block
- [[interconnect/concepts/nvlink-and-nvswitch]] — intra-node fabric
- [[interconnect/concepts/infiniband-vs-ethernet]] — inter-node fabric choice
- [[power/concepts/dc-power-distribution-architectures]] — power at each level
- [[thermal/concepts/liquid-cooling-technologies]] — cooling at pod/superpod scale
- [[interconnect/concepts/ethernet-speeds-and-standards]] — fabric speed standards