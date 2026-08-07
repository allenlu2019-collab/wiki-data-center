---
title: TechNews - SK hynix and Sandisk Publish First HBF Standard
created: 2026-08-04
updated: 2026-08-04
type: summary
tags: [hbf, bandwidth, capacity, chiplet, standard, vendor]
sources: [https://finance.technews.tw/2026/08/04/sk-hynix-announced-a-partnership-with-sandisk-to-release-the-first-standard-specification-for-hbf/]
---

# SK hynix and Sandisk Publish First HBF Standard

## Source

- Atkinson, TechNews Finance, August 4, 2026
- [SK hynix and Sandisk publish the first HBF standard specification](https://finance.technews.tw/2026/08/04/sk-hynix-announced-a-partnership-with-sandisk-to-release-the-first-standard-specification-for-hbf/)

## Executive Summary

SK hynix and Sandisk announced the first open specification for **High
Bandwidth Flash (HBF)**. The specification positions stacked NAND flash between
HBM and SSD storage: much greater capacity than HBM, but with bandwidth intended
to be far higher than conventional SSDs.

The most important architectural clarification is that this HBF proposal uses
**UCIe** between HBF and processors. HBF is therefore a package-level
heterogeneous-memory component rather than merely an SSD behind PCIe.

This is distinct from the older local-wiki use of “HBF” for a generic
high-bandwidth memory fabric. The standardized acronym now refers to
[[memory/concepts/hbf-high-bandwidth-flash]].

## Reported Specification

| Dimension | Reported value |
| --- | --- |
| Storage medium | Stacked NAND flash |
| Stack options | 8 NAND dies or 16 NAND dies |
| Maximum capacity | 512 GB |
| Bandwidth grades | Grade 1, Grade 2, Grade 3 |
| Bandwidth range | Approximately 0.4-3.0 TB/s |
| Processor interface | Industry-standard UCIe |
| Standard body | Open Compute Project (OCP) |
| Intended attachment | CPU, GPU, and other heterogeneous processors |

The specification reportedly covers:

- Interconnect interface and electrical characteristics.
- HBF die-stacking reliability requirements.
- Packaging guidance.
- Software guidance for data reads and writes.

The article does not provide the exact capacity associated with each stack
option, the precise thresholds for each bandwidth grade, latency, endurance,
random-access behavior, write bandwidth, or energy per transferred bit.

## Standardization Timeline

- August 2025: SK hynix and Sandisk began HBF standardization cooperation.
- February 2026: the companies formed an HBF alliance.
- August 2026: the first specification was published through OCP after roughly
  six months of alliance work.

The article reports that Google and Tenstorrent have joined the alliance.

## Architectural Meaning

HBF is intended to create a new memory/storage tier:

```text
On-chip SRAM/cache
        |
Local HBM: lowest external-memory latency and highest sustained bandwidth
        |
HBF: much larger NAND capacity with package-level UCIe bandwidth
        |
NVMe SSD / storage network: largest and slowest conventional storage tier
```

The 0.4-3.0 TB/s range overlaps aggregate HBM bandwidth from earlier
generations, but bandwidth alone does not make NAND equivalent to DRAM. Flash
retains materially different read latency, write latency, endurance, erase
granularity, and access behavior. HBF should therefore be modeled as a distinct
tier, not as inexpensive drop-in HBM.

## Read and Write Interpretation

The article says the standard includes software guidance for both reads and
writes. That establishes some write path, but it does not show that HBF supports
frequent fine-grained runtime writes at HBM-like latency or energy.

Until the primary OCP specification is reviewed, system studies should keep
these policies separate:

1. Hardware-supported writes for loading, maintenance, and offline updates.
2. Runtime read-mostly use for model weights or large immutable datasets.
3. Fine-grained runtime writes, which remain unproven and may be unattractive
   because of NAND program/erase cost and endurance.

## Ecosystem Context

SK hynix also discussed its tenth-generation V10 4D NAND with 375 layers and a
reported 2.5-times improvement in performance per watt over the prior
generation. Enterprise SSD production based on this NAND is planned for early
2027. This is adjacent technology context, not proof that the first HBF products
will use V10 NAND or achieve the same efficiency ratio.

## Questions Requiring Primary-Specification Verification

- Capacity and bandwidth assigned to each HBF grade.
- Read versus write bandwidth and latency.
- Random versus sequential access behavior.
- UCIe lane count, generation, PHY reach, and protocol stack.
- Cache coherence, memory semantics, and addressability.
- Endurance and error-correction requirements.
- Package power, thermal density, and energy per bit.
- Whether HBF is directly load/store addressable or managed through block,
  object, or software-defined memory semantics.

## Related Pages

- [[memory/concepts/hbf-high-bandwidth-flash]]
- [[memory/concepts/hbm-memory-architecture]]
- [[memory/concepts/hbf-high-bandwidth-fabric]]
- UCIe chiplet interconnect (concept page not yet created)
