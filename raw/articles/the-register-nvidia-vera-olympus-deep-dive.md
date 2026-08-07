---
title: The Register - NVIDIA Vera CPU and Olympus Core Deep Dive
created: 2026-08-03
updated: 2026-08-03
type: summary
tags: [cpu, vendor, lpddr, bandwidth, cxl, nvlink, chiplet]
sources: [https://www.theregister.com/systems/2026/08/01/nvidias-vera-cpu-and-the-olympus-cores-that-power-it-deep-dive/5282056]
---

# NVIDIA Vera CPU and Olympus Core Deep Dive

## Source

- Tobias Mann, The Register, August 1, 2026
- [A deep dive into Nvidia's Vera CPU and the Olympus cores that power it](https://www.theregister.com/systems/2026/08/01/nvidias-vera-cpu-and-the-olympus-cores-that-power-it-deep-dive/5282056)
- The article primarily interprets NVIDIA's Vera architecture whitepaper and
  vendor-selected performance disclosures.

## Executive Summary

Vera is NVIDIA's first data-center CPU built around its own Armv9.2-compatible
Olympus core. Its architectural emphasis is not simply core count. NVIDIA is
combining a monolithic 88-core compute die with disaggregated memory and I/O
chiplets, high-capacity LPDDR5X, a wide coherent on-die fabric, and NVLink-C2C.

The intended roles are:

1. Host CPU and control plane for Vera Rubin GPU systems.
2. Standalone CPU platform for agent runtimes, orchestration, graph traversal,
   pointer-heavy programs, and generated Python execution.

This makes Vera relevant to both [[compute/concepts/gpu-architecture]] and
[[memory/concepts/lpddr-memory]], rather than only to conventional CPU
competition.

## Reported Vera Specifications

| Dimension | Single Vera CPU | Dual-socket Vera CPU Superchip |
| --- | ---: | ---: |
| CPU cores | 88 Olympus | 176 Olympus |
| Hardware threads | 176 | 352 |
| ISA | Armv9.2-compatible | Armv9.2-compatible |
| Maximum LPDDR5X capacity | 1.5 TB | 3 TB |
| Memory bandwidth | 1.2 TB/s | 2.4 TB/s aggregate |
| Memory modules | 8 SOCAMM2 | 16 SOCAMM2 |
| NVLink-C2C | 1.8 TB/s bidirectional interface | 1.8 TB/s CPU-to-CPU |
| PCIe/CXL | 96 PCIe lanes; PCIe 6.4 and CXL 3.1 | 176 lanes reported |
| Configurable TDP | 250-450 W | Not stated in the article |

The article reports a TSMC 3 nm monolithic compute die surrounded by eight
LPDDR5X-controller chiplets and separate I/O functions for PCIe/CXL and
NVLink-C2C. This differs from a CPU architecture that distributes cores across
multiple compute chiplets.

## Olympus Core

The article reports the following per-core organization:

- 10-wide decode and dispatch.
- Eight simple integer ALUs plus two complex integer units.
- Four branch units.
- Six 128-bit SVE2 vector/FP pipelines with FP8 support.
- Four load units and two store units.
- 64 KB L1 instruction cache and 96 KB L1 data cache.
- 2 MB private L2 per core.
- A 164 MB system-level cache distributed across the chip.

NVIDIA attributes higher utilization to three unusual mechanisms:

- A neural branch predictor that can explore two branches per cycle.
- Memory renaming intended to shorten store-to-load dependency chains.
- Value prediction that speculatively supplies repeated or stable values.

These mechanisms target pipeline bubbles in branch-heavy, pointer-heavy, and
runtime-oriented workloads. Their production benefit remains workload-dependent.

## Spatial Multithreading

NVIDIA calls Olympus' two-thread mode **spatial multithreading**. The article
describes it as closer to core bifurcation than conventional x86 SMT: software
can use one wider logical core or two narrower execution partitions that share
cache resources.

The proposed agent-runtime benefit is isolation between an agent's generated
code and the container/runtime services that schedule, network, and store its
work. This is a plausible architectural mapping, not yet an independently
validated performance result.

## Memory and Coherency

The second-generation scalable coherency fabric connects all 88 cores, cache
slices, memory controllers, and external interfaces. NVIDIA reports 3.4 TB/s of
bisection bandwidth. Each coherency switch node connects up to two cores and
also acts as a routing point.

The LPDDR5X subsystem is reported to provide:

- Up to 1.5 TB capacity per socket.
- 1.2 TB/s bandwidth per socket.
- Approximately 14 GB/s per core.
- 30-40 W under sustained load, compared by NVIDIA with 100-200 W for
  conventional server-memory configurations.

The power comparison is a vendor claim and depends strongly on channel count,
DIMM population, capacity, data rate, and workload. It should not be treated as
a universal LPDDR-versus-RDIMM ratio.

## I/O and System Role

Vera combines three I/O domains:

- PCIe 6.4 for NICs, storage, and accelerators.
- CXL 3.1 for coherent expansion and fabric-attached memory.
- NVLink-C2C for coherent CPU-CPU or CPU-GPU attachment.

This makes Vera a possible bridge between [[interconnect/concepts/nvlink-and-nvswitch]],
[[interconnect/concepts/ai-fabric-scaling-taxonomy]], and host memory capacity.
The article reports that NVIDIA's agentic reference design scales to 128 Vera
CPU Superchips, or 256 CPUs, in a liquid-cooled rack.

## Reported Performance and Caveats

NVIDIA estimates Vera at 1.8 times the per-core performance of AMD EPYC 9755
on selected agentic-AI workloads and up to 2.6 times on graph traversal.

The article explicitly cautions that:

- The comparison uses an older Turin-generation competitor.
- CPU microbenchmarks can be selected to favor architectural strengths.
- The production effect of monolithic-core latency depends on communication
  patterns and working-set placement.
- Independent benchmarks on shipping hardware are still required.

## Data-Center Interpretation

Vera's central system bet is that agentic infrastructure needs a CPU optimized
for irregular control flow, runtime isolation, large low-power memory capacity,
and coherent GPU attachment. That is distinct from optimizing only dense tensor
throughput.

The important research questions are therefore whole-system questions:

1. Does LPDDR5X reduce host-memory power at equal capacity and service level?
2. Does spatial multithreading improve agent sandbox determinism and density?
3. Can the monolithic fabric preserve useful latency at 88 active cores?
4. How much of Vera's advantage remains against contemporary AMD Venice and
   Intel Xeon platforms?
5. When does CXL-attached capacity complement or undermine the local LPDDR
   bandwidth advantage?

## Related Pages

- [[compute/entities/nvidia-vera-cpu]]
- [[compute/entities/nvidia-gpu-lineage]]
- [[memory/concepts/lpddr-memory]]
- [[interconnect/concepts/nvlink-and-nvswitch]]
- [[interconnect/concepts/ai-fabric-scaling-taxonomy]]
