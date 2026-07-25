---
title: LPDDR Memory in Data Center
created: 2026-07-26
updated: 2026-07-26
type: concept
tags: [lpddr, memory, server, bandwidth, capacity]
sources: []
---

# LPDDR Memory in Data Center

LPDDR (Low-Power Double Data Rate) has traditionally been the memory of choice for mobile devices, but is increasingly adopted in data centers for its exceptional power efficiency, compact packaging, and competitive bandwidth.

## Generations

| Generation | Year | Peak data rate | BW per 64-bit channel | Voltage | Density per package |
|-----------|------|---------------|----------------------|---------|-------------------|
| LPDDR5 | 2019 | 6.4 Gbps | 51.2 GB/s | 1.05V | 64 Gb |
| LPDDR5X | 2021 | 8.533 Gbps | 68.3 GB/s | 1.05V | 128 Gb |
| LPDDR5T | 2023 | 9.6 Gbps | 76.8 GB/s | 1.05V | 128 Gb |
| LPDDR6 | 2025 | 10.667 Gbps | 170.7 GB/s (2-ch per package) | 1.05V | 192 Gb |

## Data Center Adoption

### Why LPDDR in DC?
- **Power efficiency**: LPDDR5X uses ~0.37 pJ/bit vs. DDR5 at ~1.1 pJ/bit — 3x more efficient
- **Compact form factor**: soldered-on-package vs. DIMM connectors saves physical space
- **No DIMM slots** = fewer mechanical failures, better airflow, higher density
- **Integrated voltage regulators** — reduces motherboard complexity
- **Lower thermals** — can be more easily cooled in dense configurations

### Key Use Cases

#### Grace Hopper Superchip (NVIDIA)
- NVIDIA's Grace CPU uses **512-bit LPDDR5X** (8 channels × 64-bit)
- 512 GB total capacity, 546 GB/s bandwidth
- Connected to H100 via NVLink-C2C (900 GB/s coherent interconnect)
- Demonstrates the CPU-adjacent memory role: LPDDR for CPU, HBM for GPU

#### Cloud Server Platforms
- **Ampere Altra / AmpereOne**: ARM servers using LPDDR4X/LPDDR5 for efficiency
- **Apple M-series in DC**: M1/M2 Ultra Mac minis used as cloud nodes, LPDDR unified memory
- **Samsung's LPCAMM**: LPDDR-based replaceable memory module form factor
- **Project Tangle** (Microsoft): ARM-based servers with LPDDR for HCI workloads

#### Edge / Inference
- LPDDR's low power makes it ideal for inference at the edge
- 128 GB LPDDR5X can run 70B-parameter LLMs at Q4 quantization
- Combined with NPU / mobile SoCs for local inference

## Comparison: LPDDR vs HBM vs DDR5

| Metric | LPDDR5X | HBM3 | DDR5 |
|--------|---------|------|------|
| Peak BW (per chip/stack) | 68 GB/s (64-bit) | 819 GB/s (1024-bit stack) | 51 GB/s (64-bit channel) |
| BW density | ~1.7 GB/s/mm² | ~3.5 GB/s/mm² | ~0.5 GB/s/mm² |
| Power efficiency | 0.37 pJ/bit | 3 pJ/bit | 1.1 pJ/bit |
| Capacity per package | 128 Gb (16 GB) | 192 Gb (24 GB) | 256 Gb (32 GB) |
| Latency | ~80 ns | ~100 ns | ~70 ns |
| Form factor | Soldered BGA | Interposer / 2.5D | DIMM socket |
| Cost per GB | ~$2-3 | ~$20-30 | ~$3-4 |

### Division of Labor in a DC Node
- **HBM**: GPU accelerator primary memory — high bandwidth for compute
- **LPDDR**: CPU system memory — capacity and efficiency for host workloads
- **DDR5**: Traditional servers — still dominant for general-purpose compute nodes

## Related Pages
- [[memory/concepts/hbm-memory-architecture]] — HBM for accelerator memory
- [[compute/concepts/gpu-architecture]] — how LPDDR serves CPU in hybrid nodes
- [[power/concepts/dc-power-distribution-architectures]] — power savings from LPDDR at rack scale
- [[compute/concepts/tpu-and-npu-architectures]] — NPU memory subsystems