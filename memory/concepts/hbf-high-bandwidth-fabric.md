---
title: HBF — High Bandwidth Fabric (Disaggregated Memory)
created: 2026-07-26
updated: 2026-07-26
type: concept
tags: [hbf, memory, interconnect, memory-pool, fabric, cxl]
sources: []
---

# HBF — High Bandwidth Fabric

HBF (High Bandwidth Fabric) is the emerging paradigm of **disaggregated accelerator memory** — physically separating HBM-like memory from the GPU/accelerator and connecting it via a high-bandwidth fabric. This is distinct from HBM (stacked on the interposer) and CXL (CPU memory expansion).

## Motivation: The HBM Wall

HBM scaling faces fundamental physical limits:
- **Stack height**: 16-Hi (16 DRAM dies stacked) is current limit; 20-Hi is theoretical
- **Interposer size**: Silicon interposer reticle limit (~850 mm²) caps stacks at ~8 per GPU
- **Power**: 8 HBM3e stacks draw ~100W per GPU — already significant
- **Cost**: HBM3e costs ~$20-30/GB — 10x DDR5, and scales superlinearly with bandwidth

HBF proposes: put the memory somewhere else, connect via optical fiber or advanced electrical links, and pool it across many accelerators.

## Architecture Concepts

### Pooled Memory Topology

```
┌──────────┐    ┌──────────┐    ┌──────────┐
│  GPU 0   │    │  GPU 1   │    │  GPU 2   │
│  (1/4 BW) │    │  (1/4 BW) │    │  (1/4 BW) │
└────┬─────┘    └────┬─────┘    └────┬─────┘
     │              │              │
     └──────────────┼──────────────┘
                    │
           ┌────────┴────────┐
           │  HBF Fabric     │
           │  (Optical/NVLink)│
           └────────┬────────┘
                    │
           ┌────────┴────────┐
           │  Memory Pool    │
           │  8-32 TB HBM/HBF│
           └─────────────────┘
```

### Key Idea
- Each GPU keeps a small, fast "local" HBM stack (e.g., 1/4 of today's capacity — 20-48 GB)
- Bulk capacity sits in a shared memory pool, connected via fabric at moderate bandwidth
- Software-managed data migration: hot data in local HBM, cold/medium data in fabric-attached pool

## Technology Approaches

### NVIDIA Approach (Grace-Blackwell / Rubin)
- **NVLink domain expansion**: The NVLink fabric already provides memory coherence within an 8-GPU node. Future NVLink generations could extend this to larger groups.
- **NVSwitch inter-pod**: NVSwitch 4/5 already routes NVLink traffic; with longer reach optics, this becomes an HBF.
- **Grace CPU as memory server**: Grace's LPDDR5X (512 GB at 546 GB/s) acts as a staging buffer for GPU HBM — effectively tiered memory via NVLink-C2C.

### CXL-Based (Compute Express Link)
- **CXL Type 3** devices: memory expanders attach via CXL running over PCIe Gen 5/6
- **Bandwidth**: PCIe Gen 6 x16 = 128 GB/s — fast but 10-20x slower than HBM
- **Latency**: ~200-300 ns via CXL vs ~100 ns for local HBM
- **Capacity**: Up to 8 TB per CXL memory expander (using DDR5 in the expander) — much cheaper than HBM
- **Use case**: CPU memory expansion, not accelerator-grade bandwidth

### Optical Memory Fabric (Startups)
- **Eliyan**: NuLink — chiplet interconnect using standard silicon (not interposer) for HBM-like bandwidth
- **Celestial AI**: Photonic fabric for disaggregated memory — optical links directly to memory
- **Lightmatter**: Passage — photonic interconnects for chiplet-to-chiplet and chip-to-memory
- **Ayar Labs**: Optical I/O using silicon photonics — TeraPHY + SuperNova (DWDM optical engine)

### HBF: Bandwidth Targets vs Existing Technologies

| Technology | Bandwidth per endpoint | Reach | Latency | Capacity per endpoint | Maturity |
|-----------|----------------------|-------|---------|---------------------|----------|
| HBM3e (local) | 1.2 TB/s per stack | - | ~100 ns | 24-36 GB/stack | Mature |
| NVLink 5 (local) | 1.8 TB/s per GPU | ~1m (PCB) | ~500 ns | - | Shipping |
| CXL 3.0 | 128 GB/s (x16 Gen6) | ~1m (PCB) | ~200 ns | 8 TB | Shipping |
| Optical HBF (target) | 0.5-1 TB/s | 10-100m | ~500-1000 ns | 8-64 TB | Lab |
| EDRAM / HBM-on-fabric | 256-512 GB/s | Pool | ~300-500 ns | Depends | 2026-2028 |

### The HBM+HBF Hybrid

The likely long-term architecture:

```
┌────────────────────────────────────────┐
│  Accelerator Die                       │
│  ┌──────────────────────────────────┐  │
│  │ Compute (Tensor/CUDA Cores)      │  │
│  │                                  │  │
│  │ L1/L2 Cache (on-chip SRAM)       │  │
│  │ 20-80 MB                         │  │
│  ├──────────────────────────────────┤  │
│  │ Local HBM (4-8 stacks, 192 GB)   │  │
│  │ 8 TB/s total (HBM4)             │  │
│  └──────────────────────────────────┘  │
│                  │                     │
│         ┌────────┴────────┐            │
│         │ HBF Interface   │            │
│         │ 1-2 TB/s optical│            │
│         └────────┬────────┘            │
└──────────────────┼──────────────────────┘
                   │
          ┌────────┴────────┐
          │ Shared Memory   │
          │ Pool (HBM/DDR5) │
          │ 4-16 TB total   │
          └─────────────────┘
```

## Why This Matters

HBF could **break the HBM wall** by:
1. **Decoupling capacity from bandwidth** — buy the bandwidth you need locally, rent capacity from a pool
2. **Reducing GPU cost** — fewer HBM stacks per GPU (HBM is ~50% of GPU BOM)
3. **Enabling larger model inference** — a 1T+ parameter model fits in the pool, with active layers in local HBM
4. **Lowering per-GB cost** — HBF-connected memory can use cheaper DRAM (DDR5 or older HBM nodes)

## Open Questions
- Can optical interposers reach HBM-like energy efficiency (<5 pJ/bit)?
- Will software (CUDA/HIP) evolve to handle 2-tier memory efficiently?
- Is HBF a 2028 or 2032 technology?

## Related Pages
- [[memory/concepts/hbm-memory-architecture]] — the technology HBF aims to complement
- [[interconnect/concepts/nvlink-and-nvswitch]] — NVLink as a HBF foundation
- [[interconnect/concepts/dac-acc-aec-copper-optics]] — optical interconnect for HBF
- [[compute/concepts/gpu-architecture]] — GPU that needs HBF memory