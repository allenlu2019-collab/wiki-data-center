---
title: TPU and NPU Architectures
created: 2026-07-26
updated: 2026-07-26
type: concept
tags: [tpu, npu, compute, accelerator, asic]
sources: []
---

# TPU and NPU Architectures

Google's TPU (Tensor Processing Unit) and a growing ecosystem of NPUs (Neural Processing Units) represent specialized ASIC approaches to AI acceleration — trading general-purpose flexibility for superior efficiency on specific workloads.

## Google TPU Generations

| Generation | Year | Key Feature | Memory | Interconnect | TDP |
|-----------|------|-------------|--------|-------------|-----|
| TPU v1 | 2016 | 8-bit MAC systolic array for inference | 8 MB on-chip | No (PCIe card) | ~75W |
| TPU v2 | 2017 | Training support, bfloat16 | 16 GB HBM per chip | TPUv2 mesh | ~200W |
| TPU v3 | 2018 | 2x clock, liquid cooling | 32 GB HBM per chip | TPUv3 mesh | ~450W |
| TPU v4 | 2021 | Sparse core, OCS optical switching | 64 GB HBM2e per chip | OCS optical circuit switches | ~500W |
| TPU v5p | 2024 | 2x TPUv4 performance | 95 GB HBM2e per chip | OCS, CIEmesh | ~600W |
| TPU v6 (Trillium) | 2025 | 4.7x TPUv4 performance | HBM3e | 2x inter-TPU bandwidth | ~700W |

### TPU Architecture Highlights

- **Systolic array** — dense grid of multiply-accumulate units optimized for matrix operations
- **Sparse Core (v4+)** — dedicated hardware for sparse matrix operations, 2x throughput on sparse models
- **bfloat16 native** — TPUs were the first accelerator to implement bfloat16 in hardware
- **Optical Circuit Switching (v4+)** — enables flexible pod topologies with 10x lower latency and power than electrical switches
- **Pod-scale** — TPU v4 pods support 4096 chips connected via a 3D torus + OCS, delivering ~1.1 exaFLOPS

## NPU Landscape

### AWS Trainium / Inferentia
- **Trainium 1** (2021): 128 GB HBM2e, 2.3 PetaFLOPS FP16/bfloat16
- **Trainium 2** (2024, Trn2): 192 GB HBM3, 20.8 PetaFLOPS bf16, 6.4 TB/s EFA interconnect
- **Inferentia 2** (2022): NeuronCores v2, 188 GB/s memory bandwidth, 4x faster than Inf1
- UltraCluster: up to 100,000 Trainium2 chips via EFA fabric

### Meta MTIA (Meta Training and Inference Accelerator)
- Custom RISC-V based ASIC for AI inference (first generation)
- v2 (2024): focused on ranking/recommendation workloads
- Tightly coupled with Meta's software stack

### Microsoft Maia 100
- First custom AI ASIC from Microsoft (2023)
- 105 billion transistors, TSMC 5nm
- 200 GB/s interconnect per chip
- Targeted at Microsoft's internal AI workloads

### Cerebras Wafer-Scale Engine
- **WSE-3** (2024): 4 trillion transistors, 900,000 AI cores on a single wafer (no reticle limit!)
- 44 GB on-chip SRAM distributed across cores (no HBM required)
- 125 PetaFLOPS bf16
- CS-3 system: 45U, 15 kW TDP (air cooled)
- Exascale systems via cluster of CS-3 units

### Groq LPU (Language Processing Unit)
- Tensor streaming architecture — deterministic execution, no scheduling overhead
- Single core achieves ~1 PetaFLOP bf16 (TSMC 5nm)
- 14 x 16 LPU crossbar fabric
- No HBM — uses SRAM (230 MB on-chip), eliminates memory bandwidth bottlenecks

## TPU/NPU vs GPU: Key Differences

| Dimension | GPU (NVIDIA) | TPU (Google) | NPU (AWS/Trainium) |
|-----------|-------------|-------------|-------------------|
| Flexibility | General compute + AI | AI-first (matmul) | AI-first (matmul) |
| Memory | HBM (high capacity) | HBM (moderate) | HBM (high capacity) |
| Interconnect | NVLink + InfiniBand | OCS + proprietary mesh | EFA (AWS) |
| SW Stack | CUDA (mature) | XLA/JAX (proprietary) | Neuron SDK |
| Strength | Training + inference | Training at scale | Specific cloud workloads |

## Related Pages
- [[compute/concepts/gpu-architecture]] — GPU accelerator design for comparison
- [[interconnect/concepts/nvlink-and-nvswitch]] — GPU fabric vs. OCS and EFA
- [[rack-pod/concepts/pod-and-superpod-architectures]] — how NPUs scale to pod level
- [[interconnect/concepts/ethernet-speeds-and-standards]] — EFA as an Ethernet derivative