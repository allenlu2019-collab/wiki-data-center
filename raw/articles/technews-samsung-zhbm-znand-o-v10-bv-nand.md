---
title: TechNews - Samsung zHBM, zNAND-O, and V10 BV-NAND
created: 2026-08-05
updated: 2026-08-05
type: summary
tags: [hbm, z-nand, lpddr, advanced-packaging, vendor]
sources: [https://technews.tw/2026/08/05/samsung-showcases-next-generation-memory-technology/]
---

# TechNews - Samsung zHBM, zNAND-O, and V10 BV-NAND

## Source

- Article: [Samsung showcases next-generation memory technology](https://technews.tw/2026/08/05/samsung-showcases-next-generation-memory-technology/)
- Publisher: TechNews
- Publication date: 2026-08-05
- Event covered: FMS 2026
- Evidence type: secondary reporting of Samsung announcements and projections

## Reported Technologies

### zHBM

Samsung's zHBM concept places stacked HBM vertically above an AI accelerator,
rather than beside it on an interposer. The article reports Samsung estimates
of up to 8x HBM5 performance, more than 10x HBM5 memory density, 3x energy
efficiency, and less than half the thermal resistance. It also describes
customizable interlayer IP.

These are vendor projections for a concept, not independently reproduced
measurements or a shipping-product specification. The article does not disclose
the workloads, metric definitions, package limits, or comparison configuration.

### V10 BV-NAND

The reported V10 BV-NAND uses more than 400 layers and wafer bonding. Samsung
claims 58% higher density than V9 and improvements in read, write, I/O, and
energy efficiency, but the article provides no numerical performance values.

### zNAND-O

Samsung describes zNAND-O as a developing 4-layer or 8-layer concept optimized
for edge AI. The reported benefits are high spatial efficiency, low latency,
and strong I/O capability for real-time, data-intensive AI workloads.

The article does not specify zNAND-O's capacity, bandwidth, latency, endurance,
power, controller, protocol, package interface, write behavior, or data path.

## Interpretation Audit

| Claim | Assessment | Reason |
| --- | --- | --- |
| Traditional Samsung Z-NAND SSDs use PCIe/NVMe | Supported by the historical SZ985 brief, not established by this article | SZ985 is a concrete PCIe SSD product. |
| Conventional storage I/O normally stages data in system DRAM | Generally correct default, but not mandatory | Peer-to-peer DMA and direct-storage paths can avoid host-DRAM staging on supported platforms. |
| Direct Storage or GPU peer-to-peer DMA is a Z-NAND feature | Incorrect framing | It is a system and platform capability, not an intrinsic property of Z-NAND media. |
| zNAND-O has 4-layer and 8-layer variants for edge AI | Supported | This is explicitly reported. |
| zNAND-O uses micro-bumps, TSVs, UCIe, or another die-to-die interface | Not established | None of these interface details appears in the article. |
| zNAND-O directly feeds an AP cache or SRAM while bypassing LPDDR | Not established | The data path and buffering architecture are not disclosed. |
| zNAND-O performs near-memory computation | Unsupported and potentially misleading | The article describes memory, not compute within or beside the flash array. |
| zNAND-O can stream LLM weights directly into an NPU cache | Plausible research hypothesis only | It requires verified bandwidth, latency, access granularity, buffering, and write-path details. |
| Samsung zNAND-O supports a feature named GIDS | Not established | The article does not use or define this term. |

## Product and Concept Boundaries

| Technology | Status in reviewed sources | Attachment | Quantitative evidence |
| --- | --- | --- | --- |
| SZ985 Z-NAND | Historical shipping SSD | PCIe Gen3 x4 | Capacity, bandwidth, IOPS, latency, endurance |
| zNAND-O | Samsung edge-AI concept under development | Not disclosed | No capacity, bandwidth, or latency values |
| HBF | Separate SK hynix/Sandisk OCP proposal | UCIe | Up to 512 GB and approximately 0.4-3.0 TB/s by grade |

The zNAND-O concept should not inherit HBF's UCIe interface or numeric
specifications without a Samsung primary source confirming them.

## Roadmap Context

The article also reports Samsung roadmap activity around HBM4E, HBM5,
LPDDR5X-PIM, and enterprise SSDs. These establish a broad memory portfolio but
do not resolve the open zNAND-O architecture questions.

## Related Pages

- [[memory/concepts/z-nand-low-latency-flash]]
- [[memory/concepts/hbm-memory-architecture]]
- [[memory/concepts/hbf-high-bandwidth-flash]]
- [[shared/concepts/3dic-bonding-and-tsvs]]
- [[raw/papers/samsung-z-nand-ssd-technology-brief]]
