---
title: GPU Architecture for Data Centers
created: 2026-07-26
updated: 2026-07-26
type: concept
tags: [gpu, compute, accelerator, server]
sources: []
---

# GPU Architecture for Data Centers

Modern data center GPUs are purpose-built accelerators optimized for parallel computation — primarily AI training/inference and HPC workloads. Unlike consumer GPUs, they prioritize memory capacity, bandwidth, interconnect scalability, and reliability over rasterization performance.

## Key Design Dimensions

### Compute Throughput
- **Tensor Cores / Matrix units** — specialized hardware for matrix multiply-accumulate (the dominant AI operation). Each generation increases TFLOPS by 2-3x.
- **FP8 / FP4 support** — reduced-precision formats that double throughput per generation, enabling efficient inference and training.
- **CUDA cores / Stream processors** — general-purpose SIMT units for non-matrix workloads (data processing, physics simulations).

### Memory Subsystem
- **HBM (High Bandwidth Memory)** — stacked DRAM directly on the interposer, delivering 2-4 TB/s bandwidth
- **Capacity** — 80 GB (H100) → 192 GB (B200) → 288 GB (future). The ratio of BW:FLOPS (the "Ops:BW gap") is critical — modern GPUs need ~2 GB/s bandwidth per TFLOP to avoid starving compute.
- See [[memory/concepts/hbm-memory-architecture]] for detailed HBM evolution.

### Interconnect
- **NVLink / NVSwitch** — NVIDIA's GPU-to-GPU fabric, providing 900 GB/s (H100) to 1.8 TB/s (B200+) per GPU, enabling memory coherence across pods
- **PCIe Gen5 / Gen6** — host interface for CPU communication, DMA transfers
- **InfiniBand NDR / XDR** — inter-node fabric for multi-pod scaling
- See [[interconnect/concepts/nvlink-and-nvswitch]] and [[interconnect/concepts/infiniband-vs-ethernet]].

### Form Factor and Power
- **SXM / HGX / OAM** — mezzanine form factors designed for dense clusters (vs. PCIe add-in cards)
- **TDP** — 350W (A100) → 700W (H100) → 1000W+ (B200, future). Power density is the defining constraint for next-gen clusters.
- See [[power/concepts/dc-power-distribution-architectures]] for how the 1000W+ GPU impacts facility power design.

## Major GPU Lines

| Vendor | Architecture | Key Accelerator | Memory | Interconnect | TDP |
|--------|-------------|----------------|--------|-------------|-----|
| NVIDIA | Hopper | H100 SXM | 80 GB HBM3 | NVLink4 (900 GB/s), PCIe Gen5 | 700W |
| NVIDIA | Blackwell | B200 | 192 GB HBM3e | NVLink5 (1.8 TB/s), PCIe Gen6 | 1000W |
| NVIDIA | Rubin (2026) | R100 | 288 GB HBM4 | NVLink6, CX9 InfiniBand | ~1200W |
| AMD | CDNA 3 | MI300X | 192 GB HBM3 | Infinity Fabric, PCIe Gen5 | 750W |
| AMD | CDNA 4 | MI400 | 288 GB HBM3e | PCIe Gen6, Infinity Fabric | ~900W |
| Intel | Xe HPC | Max 1550 | 128 GB HBM2e | Xe Link, PCIe Gen5 | 600W |

## Scaling Architecture

GPUs in a data center are deployed in hierarchical groups:

- **Single node**: 8 GPUs via NVSwitch (full crossbar, any-to-any)
- **Pod**: 64-256 GPUs via InfiniBand leaf-spine
- **Superpod**: 1000s+ GPUs via multi-stage fabric
- See [[rack-pod/concepts/pod-and-superpod-architectures]].

## Related Pages
- [[memory/concepts/hbm-memory-architecture]] — HBM generations and bandwidth evolution
- [[interconnect/concepts/nvlink-and-nvswitch]] — GPU-to-GPU fabric
- [[compute/entities/nvidia-gpu-lineage]] — detailed NVIDIA product history
- [[power/concepts/dc-power-distribution-architectures]] — power for 1000W+ accelerators
- [[compute/concepts/tpu-and-npu-architectures]] — alternative accelerator designs