---
title: HBM Memory Architecture
created: 2026-07-26
updated: 2026-08-05
type: concept
tags: [hbm, memory, bandwidth, capacity, gpu, accelerator]
sources: [raw/articles/hbm-for-data-center.md, raw/articles/3dic-bonding.md, raw/articles/technews-samsung-zhbm-znand-o-v10-bv-nand.md]
---

# HBM (High Bandwidth Memory) Architecture

HBM is a 3D-stacked DRAM technology that revolutionized accelerator memory by placing DRAM dies vertically, connected through silicon vias (TSVs) to a logic die on a silicon interposer alongside the processor. This delivers 10-20x the bandwidth of DDR at dramatically lower power per bit.

## Generations

| Generation | Year | Per-stack capacity | Stack height | Per-pin data rate | Aggregate BW per stack | JEDEC standard |
|-----------|------|-------------------|-------------|-------------------|----------------------|----------------|
| HBM1 | 2013 | 1 GB | 4-Hi | 1 Gbps | 128 GB/s | JESD235 |
| HBM2 | 2016 | 8 GB | 4-8 Hi | 2 Gbps | 256 GB/s | JESD235A |
| HBM2e | 2020 | 16 GB | 8-Hi | 3.6 Gbps | 460 GB/s | JESD235B |
| HBM3 | 2022 | 24 GB | 8-12 Hi | 6.4 Gbps | 819 GB/s | JESD238 |
| HBM3e | 2024 | 36 GB | 12-Hi | 9.6 Gbps | 1.2 TB/s | JESD238A |
| HBM4 | 2026 | 64 GB | 16-Hi | 12-16 Gbps | ~2 TB/s | JESD239 (draft) |

## How HBM Works

### Stacking
- **TSVs (Through-Silicon Vias)** — vertical connections through each DRAM die
- **Microbumps** — 40-50 μm pitch connections between dies in the stack
- **Logic die** — base layer containing refresh, timing, and training circuitry
- **Interposer** — silicon substrate with metal layers routing HBM stacks to the GPU/accelerator
- Intermediate dies require TSVs to relay signals between their front and back interfaces; the top die does not normally need a pass-through connection.
- Hybrid bonding is a future path to finer pitch and lower interface parasitics, but it does not eliminate vertical routing through intermediate silicon. See [[shared/concepts/3dic-bonding-and-tsvs]].

### Interface
- **1024-bit wide per stack** — 16 channels × 32 bits (HBM2e) or 8 channels × 64 bits (HBM3)
- This massive width is what delivers the bandwidth: 1024 bits × 6.4 Gbps = 819 GB/s
- DDR5 by comparison: 64-bit (per channel) × 6.4 Gbps = 51 GB/s
- **HBM is 16x wider, not 16x faster**

### Power Efficiency
- HBM3: ~3 pJ/bit (compared to DDR5 at ~6-8 pJ/bit)
- Lower power per bit despite higher aggregate bandwidth
- But total memory power at scale is significant: 8 stacks × 12W = ~96W per GPU

## Bandwidth Trends and the Ops:BW Gap

The critical metric in accelerator design is **BW:FLOPS ratio** — how much memory bandwidth is available per TFLOPS of compute.

| GPU | Compute (TFLOPS bf16) | Memory BW (TB/s) | BW per TFLOPS | Status |
|-----|----------------------|------------------|---------------|--------|
| A100 | 312 | 2.0 | 6.4 GB/s/TF | Healthy |
| H100 | 989 | 3.35 | 3.4 GB/s/TF | Tight |
| B200 | 4500 | 8.0 | 1.8 GB/s/TF | Constrained |
| Future | 10000+ | 16 (HBM4) | 1.6 GB/s/TF | Very constrained |

This is the **"HBM wall"** — compute scales with Moore's Law / process shrinks, but memory BW scales with interconnect density and stack height. The gap widens every generation.

### Mitigation Strategies
- **Sparse computation** — skip zero activations, effectively 2x math per memory access
- **Quantization** — FP8, FP4, INT8 reduce bits per parameter
- **On-chip SRAM** — H100 has 50 MB L2, B200 has 80 MB; large SRAM reduces HBM traffic
- **Memory pooling** — CXL-attached memory for capacity, HBM for bandwidth
- **HBF (High Bandwidth Fabric)** — disaggregated HBM via optical/CXL, discussed in [[memory/concepts/hbf-high-bandwidth-fabric]]

## Current Accelerator Memory Configurations

| Accelerator | HBM Gen | Stacks | Total Capacity | Total BW |
|-------------|---------|--------|---------------|----------|
| A100 80GB | HBM2e | 5 | 80 GB | 2.0 TB/s |
| H100 SXM | HBM3 | 6 | 80 GB | 3.35 TB/s |
| B200 | HBM3e | 8 | 192 GB | 8.0 TB/s |
| MI300X | HBM3 | 8 | 192 GB | 5.3 TB/s |
| TPU v5p | HBM2e | 8? | 95 GB | 4.8 TB/s |
| Trainium 2 | HBM3 | 8? | 192 GB | 6.4 TB/s |

## Samsung zHBM Concept

Samsung's FMS 2026 zHBM concept places stacked HBM vertically above an AI
accelerator rather than beside it. Samsung estimates up to 8x HBM5 performance,
more than 10x density, 3x energy efficiency, and less than half the thermal
resistance, with customizable interlayer IP.

These figures are vendor projections rather than a shipping specification or
independent benchmark. The comparison basis, workloads, package dimensions,
power envelope, cooling assumptions, and manufacturing constraints were not
disclosed in the reviewed article. zHBM should therefore be tracked as an
advanced-packaging direction, not entered into system models as measured HBM
performance.

## Related Pages
- [[raw/articles/technews-samsung-zhbm-znand-o-v10-bv-nand]]
- [[memory/entities/rambus-hbm-interface-ip]] — merchant HBM controller/PHY IP and enablement
- [[memory/concepts/lpddr-memory]] — LPDDR role in DC (boot, metadata, CPU memory)
- [[compute/concepts/gpu-architecture]] — how HBM integrates with GPU compute
- [[memory/concepts/hbf-high-bandwidth-fabric]] — HBF: the next paradigm beyond HBM
- [[interconnect/concepts/nvlink-and-nvswitch]] — GPU interconnect that complements HBM
- [[interconnect/concepts/dac-acc-aec-copper-optics]] — memory fabric interconnects
