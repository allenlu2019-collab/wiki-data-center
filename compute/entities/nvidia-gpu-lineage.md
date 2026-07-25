---
title: NVIDIA GPU Lineage
created: 2026-07-26
updated: 2026-07-26
type: entity
tags: [gpu, compute, vendor, nvidia, accelerator]
sources: []
---

# NVIDIA GPU Lineage (Data Center)

## Desktop / Workstation → Data Center Origins

| Year | Architecture | Key DC GPU | Memory | Interconnect | TDP | Significance |
|------|-------------|-----------|--------|-------------|-----|-------------|
| 2012 | Kepler (GK110) | K80 (dual GK210) | 2×12 GB GDDR5 | PCIe Gen3 | 300W | First NVIDIA "compute accelerator" |
| 2014 | Maxwell (GM200) | M40 | 12 GB GDDR5 | PCIe Gen3 | 250W | Early Deep Learning adoption |
| 2016 | Pascal (GP100) | P100 | 16 GB HBM2 | NVLink 1 (160 GB/s) | 300W | First with HBM, first with NVLink |

## Data Center-Dedicated Architectures

| Year | Architecture | Flagship | TFLOPS FP16 | Memory | Interconnect | TDP | Notes |
|------|-------------|----------|------------|--------|-------------|-----|-------|
| 2017 | Volta (GV100) | V100 SXM2 | 125 | 32 GB HBM2 | NVLink 2 (300 GB/s), PCIe Gen3 | 300W | **Tensor Cores introduced** — transformed AI training |
| 2020 | Ampere (GA100) | A100 SXM | 312 (bf16) | 80 GB HBM2e | NVLink 3 (600 GB/s), PCIe Gen4 | 400W | MIG (Multi-Instance GPU), bfloat16, sparse training |
| 2022 | Hopper (GH100) | H100 SXM | 989 (bf16) | 80 GB HBM3 | NVLink 4 (900 GB/s), PCIe Gen5 | 700W | Transformer Engine (FP8), DPX instructions |
| 2024 | Blackwell (GB100) | B200 | 4,500 (bf16) | 192 GB HBM3e | NVLink 5 (1.8 TB/s), PCIe Gen6 | 1000W | FP4 support, 2-die MCM, liquid cooling standard |
| 2026 | Rubin (VR100?) | R100 | ~10,000 (bf16) | 288 GB HBM4 | NVLink 6 (~2.5 TB/s) | ~1200W | Expected — 4-die MCM, CPO interconnect |

## Architecture Deep Features

### Tensor Core Evolution
| Architecture | Tensor Core Gen | Math formats | Compute per SM | Notes |
|-------------|----------------|-------------|---------------|-------|
| Volta | V1 | FP16 | 8 TFLOPS | No bf16, no sparsity |
| Turing | V2 | FP16, INT8, INT4 | 16 TFLOPS | DC limited (RTX-centric) |
| Ampere | V3 | FP16, bf16, TF32, INT8, INT4 | 32 TFLOPS | Sparsity (2x math on sparse inputs) |
| Hopper | V4 | + FP8 (E4M3, E5M2) | 64 TFLOPS | Transformer Engine, FP8 training |
| Blackwell | V5 | + FP4, FP6 | 128 TFLOPS | FP4 inference, 2x die via MCM |
| Rubin | V6 | + FP2?, 2:4 sparse | ~256 TFLOPS | Unknown — likely 4-die MCM |

### Memory Bandwidth Evolution
| GPU | HBM gen | Stacks | Total BW | BW per TFLOP (bf16) |
|-----|---------|--------|----------|-------------------|
| V100 | HBM2 | 4 | 900 GB/s | 7.2 GB/s/TF |
| A100 | HBM2e | 5 | 2.0 TB/s | 6.4 GB/s/TF |
| H100 | HBM3 | 6 | 3.35 TB/s | 3.4 GB/s/TF |
| B200 | HBM3e | 8 | 8.0 TB/s | 1.8 GB/s/TF |
| R100 | HBM4 | 8? | ~16 TB/s | ~1.6 GB/s/TF |

### NVLink Evolution
| Gen | Per-link speed | Links per GPU | Total BW | Switch |
|-----|---------------|---------------|----------|--------|
| NVLink 1 | 20 GB/s | 4-8 | 160 GB/s | None (P2P mesh) |
| NVLink 2 | 50 GB/s | 6 | 300 GB/s | None (hybrid cube mesh) |
| NVLink 3 | 50 GB/s | 12 | 600 GB/s | NVSwitch 1 |
| NVLink 4 | 50 GB/s | 18 | 900 GB/s | NVSwitch 3 |
| NVLink 5 | 100 GB/s | 18 | 1,800 GB/s | NVSwitch 4 |
| NVLink 6 | 125 GB/s? | 18? | ~2,250 GB/s | NVSwitch 5 |

## Data Center GPU Naming Convention (Current)

```
Product: [Arch Letter][Generation number][Variant]
Example: H100 → Hopper architecture, 1st gen, flagship
         B200 → Blackwell, 2nd gen, flagship

Variant suffixes:
  No suffix = SXM mezzanine (standard)
  "NVL" = NVLink-connected multi-GPU board
  "PCIe" = PCIe add-in card version
  "HGX" = Baseboard hosting 4 or 8 SXM modules
```

## Key Milestones
- **2017: Tensor Cores** — Volta shifted ML from ASIC/FPGA era to GPU compute
- **2020: MIG** — Ampere enabled GPU partitioning for multi-tenant; critical for cloud GPU-as-a-service
- **2022: FP8 + Transformer Engine** — Hopper matched FP8 at training-grade fidelity; reduced memory by 2x
- **2024: MCM Design** — Blackwell uses 2 reticle-limited dies (104B transistors total) connected via 10 TB/s die-to-die link
- **2026: Rubin** — Expected to use 4 dies, co-packaged optics, HBM4

## Related Pages
- [[compute/concepts/gpu-architecture]] — GPU architecture design deep-dive
- [[memory/concepts/hbm-memory-architecture]] — memory evolution
- [[interconnect/concepts/nvlink-and-nvswitch]] — interconnect evolution
- [[compute/concepts/tpu-and-npu-architectures]] — competitive landscape