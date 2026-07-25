---
title: NVLink and NVSwitch
created: 2026-07-26
updated: 2026-07-26
type: concept
tags: [nvlink, interconnect, fabric, gpu, pcie, topology]
sources: []
---

# NVLink and NVSwitch

NVLink is NVIDIA's high-bandwidth, low-latency GPU-to-GPU interconnect, forming the backbone of multi-GPU nodes. NVSwitch is a crossbar switch ASIC that enables full any-to-any connectivity within a node. Together they define **NVIDIA's intra-node fabric** — distinct from InfiniBand (inter-node).

## Generations

| Generation | GPU | Per-GPU NVLink BW | NVSwitch? | Node topology | Peak node bisection BW |
|-----------|-----|-------------------|-----------|--------------|----------------------|
| NVLink 1 | P100 | 160 GB/s (4 links × 20 GB/s) | No | Hybrid Cube Mesh | 160 GB/s per GPU |
| NVLink 2 | V100 | 300 GB/s (6 links × 50 GB/s) | No | Hybrid Cube Mesh | 300 GB/s per GPU |
| NVLink 3 | A100 | 600 GB/s (12 links × 50 GB/s) | Yes (NVSwitch 1) | Full crossbar (NVSwitch) | 600 GB/s per GPU |
| NVLink 4 | H100 | 900 GB/s (18 links × 50 GB/s) | Yes (NVSwitch 3) | Full crossbar (NVSwitch) | 900 GB/s per GPU |
| NVLink 5 | B200 | 1,800 GB/s (18 links × 100 GB/s) | Yes (NVSwitch 4) | Full crossbar (NVSwitch) | 1,800 GB/s per GPU |

## Architecture

### Physical Layer
- **NVLink is a serial differential pair** — similar electrical signaling to PCIe but with NVIDIA's proprietary protocol
- NVLink 4: 50 Gbps PAM-4 per differential pair (same as PCIe Gen 5 electrical)
- NVLink 5: 100 Gbps PAM-4 per differential pair (PCIe Gen 6 class)
- Flexible lane configuration: a "link" = 4 differential pairs (x4)

### NVSwitch
- Dedicated switch ASIC connecting all GPUs in a node (DGX/HGX baseboard)
- NVSwitch 3 (H100): 64 ports × 50 GB/s = 3.2 TB/s aggregate switching capacity
- NVSwitch 4 (B200): 128 ports × 100 GB/s = 12.8 TB/s aggregate
- Implements **all-to-all non-blocking** connectivity — any GPU can communicate at full NVLink bandwidth with any other GPU
- NVSwitch is protocol-aware — understands GPU memory semantics for direct load/store

### Topology

**Without NVSwitch (V100 era):**
```
GPU0 ── GPU1
  │  X   │
GPU2 ── GPU3
```
Hybrid Cube Mesh: each GPU connects to 2-4 others. Traffic between non-connected GPUs must hop through intermediates.

**With NVSwitch (A100+ era):**
```
        NVSwitch
      ╱   │   ╲
    GPU0─GPU1─GPU2
      ╲   │   ╱
        NVSwitch
```
All-to-all: GPU0 ↔ GPU2 at full NVLink bandwidth, no intermediate hops. Two NVSwitches provide redundancy.

### DGX/HGX Configurations

| Node | GPUs | NVLinks per GPU | NVSwitches | Per-GPU BW | Total cross-sectional BW |
|------|------|----------------|------------|-----------|------------------------|
| DGX A100 | 8 × A100 | 12 (600 GB/s) | 6 × NVSwitch 1 | 600 GB/s | 4.8 TB/s |
| DGX H100 | 8 × H100 | 18 (900 GB/s) | 4 × NVSwitch 3 | 900 GB/s | 7.2 TB/s |
| HGX B200 | 8 × B200 | 18 (1.8 TB/s) | 4 × NVSwitch 4 | 1.8 TB/s | 14.4 TB/s |

## NVLink-C2C (Chip-to-Chip)

A variant of NVLink for connecting CPU↔GPU or GPU↔GPU on the same package/interposer:
- Used in **Grace Hopper Superchip**: Grace CPU + H100 GPU connected via 900 GB/s NVLink-C2C
- Coherent memory: CPU and GPU share a unified address space (UVA)
- Enables the **Grace Hopper memory hierarchy**: Grace's 512 GB LPDDR5X + H100's 80 GB HBM3
- Power: ~3 pJ/bit, 5x more efficient than PCIe Gen 5

## NVLink vs InfiniBand vs PCIe

| Metric | NVLink 5 | InfiniBand NDR 400G | PCIe Gen 6 x16 |
|--------|---------|--------------------|----------------|
| Bandwidth per GPU | 1.8 TB/s | 50 GB/s (unidirectional) | 128 GB/s (bidirectional) |
| Latency | ~0.5 μs | ~1-2 μs | ~1 μs |
| Reach | Within node | Inter-node (up to 100m+) | Within board (~12") |
| Protocol | Load/store (coherent) | Message passing | Load/store (MMIO) |
| Topology | All-to-all (NVSwitch) | Fat-tree / Dragonfly | Root complex & bridge |
| Cost | Internal (PCB/backplane) | NIC + cables + switches | On-board |

## Impact on AI Training

NVLink's high bandwidth significantly impacts training efficiency:
- **Model parallelism**: TeraFLOP-scale models split across GPUs communicate via NVLink (not network)
- **Pipeline parallelism**: 1.8 TB/s reduces bubble overhead vs 400 GbE
- **AllReduce**: Gradient synchronization across 8 GPUs in <100 μs (vs ~10 μs on InfiniBand)
- **Memory pooling**: GPU0 can read GPU1's HBM3 as if local — enables memory expansion at bandwidth

For a B200 DGX pod, NVLink handles intra-node communication at 1.8 TB/s, while InfiniBand XDR handles inter-node at 800 Gb/s (100 GB/s). ~18x more bandwidth inside the node than between nodes.

## Related Pages
- [[compute/concepts/gpu-architecture]] — GPU that NVLink connects
- [[interconnect/concepts/infiniband-vs-ethernet]] — inter-node fabric complementing NVLink
- [[interconnect/concepts/ethernet-speeds-and-standards]] — inter-node alternative
- [[rack-pod/concepts/pod-and-superpod-architectures]] — how NVLink+NVSwitch scales to pods
- [[memory/concepts/hbm-memory-architecture]] — memory accessed over NVLink