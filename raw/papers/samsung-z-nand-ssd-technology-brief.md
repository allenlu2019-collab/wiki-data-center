---
title: Samsung Z-NAND SSD Technology Brief
created: 2026-08-05
updated: 2026-08-05
type: summary
tags: [z-nand, bandwidth, capacity, latency, vendor]
sources: [https://download.semiconductor.samsung.com/resources/brochure/Ultra-Low%20Latency%20with%20Samsung%20Z-NAND%20SSD.pdf]
---

# Samsung Z-NAND SSD Technology Brief

## Source

- Samsung Memory Solutions Lab, 2017
- [Ultra-Low Latency with Samsung Z-NAND SSD](https://download.semiconductor.samsung.com/resources/brochure/Ultra-Low%20Latency%20with%20Samsung%20Z-NAND%20SSD.pdf)
- Five-page Samsung vendor technology brief

## Executive Summary

Samsung positioned Z-NAND as a low-latency NAND tier for data-center workloads
that need more capacity than DRAM but substantially lower latency than
conventional NVMe flash. The brief's concrete product is the 800 GB SZ985
Z-SSD, a PCIe Gen3 x4 add-in card.

Unlike the 2026 [[memory/concepts/hbf-high-bandwidth-flash]] proposal, this
document reports a conventional host-attached SSD interface and gives explicit
random latency, IOPS, endurance, and application-level benchmark claims.

## Reported SZ985 Specifications

| Property | Samsung SZ985 |
| --- | ---: |
| Form factor | HHHL add-in card |
| Host interface | PCIe Gen3 x4 |
| Ports | One |
| Capacity | 800 GB |
| Sequential read/write | 3.2/3.2 GB/s at 128 KB |
| Sustained random read/write | 750K/170K IOPS at 4 KB |
| Sustained random-read latency | 12-20 microseconds |
| Typical random-write latency | 16 microseconds |
| Endurance | 30 drive writes per day (DWPD) |

Samsung states that Z-NAND shares the fundamental 3D-flash structure of its
V-NAND but uses a distinct circuit design and controller. The brochure does not
disclose cell level, layer count, die organization, channel count, queue depth,
power, energy per bit, or the exact mechanisms responsible for the latency.

## Vendor Comparison

Samsung claims that the SZ985 provides 5.5 times lower latency than the leading
NVMe SSDs available when the brochure was produced. The document's application
charts compare the SZ985 against Samsung's PM1725a NVMe SSD.

| Workload | Reported result versus PM1725a |
| --- | --- |
| RocksDB | Approximately 2x throughput |
| RocksDB | Approximately one-half average latency |
| Fatcache | Approximately 1.6x throughput |
| Memcached with OS paging | Approximately 3x throughput |
| Memcached with OS paging | Approximately one-third read latency |

The brochure does not provide enough experimental detail to reproduce these
results. Missing information includes processor and memory configuration,
software versions, dataset size, request distribution, queue depth, cache
state, run duration, repetitions, and error bars. Treat the ratios as vendor
claims tied to this comparison, not universal Z-NAND speedups.

## Intended Workloads

Samsung identifies these uses:

- High-capacity caching.
- NoSQL and key-value databases.
- Data stores and analytics.
- Latency-sensitive, I/O-intensive applications.
- Operating-system paging when DRAM capacity is exceeded.

The application results support the general point that lower device latency can
survive the software stack and improve user-level throughput. They do not imply
that SSD paging is equivalent to DRAM or that Z-NAND should replace memory for
all workloads.

## Historical and Evidence Caveats

- This is a 2017 vendor brochure, not a current product specification.
- The comparison target is a contemporary PM1725a, not a modern PCIe Gen5 or
  Gen6 SSD.
- Peak and sustained metrics use different request sizes.
- The brochure does not disclose pricing or power.
- Independent benchmark evidence is not included.
- Product specifications may have changed after publication.

## Relation to HBF

Z-NAND and HBF both try to narrow the gap between DRAM and ordinary NAND, but
they describe different system levels:

| Dimension | SZ985 Z-NAND SSD | HBF Grade 3 proposal |
| --- | --- | --- |
| Document date | 2017 | 2026 |
| Attachment | PCIe Gen3 x4 SSD | UCIe package-level component |
| Capacity | 800 GB | Up to 512 GB |
| Peak reported bandwidth | 3.2 GB/s | Up to 3,000 GB/s |
| Random-read latency | 12-20 microseconds reported | Not disclosed |
| Write behavior | 3.2 GB/s sequential; 170K random-write IOPS; 30 DWPD | Write guidance exists; performance/endurance undisclosed |
| Software model | Block-storage SSD | Memory/storage semantics not yet established in reviewed source |

Z-NAND demonstrates that NAND can be engineered for lower latency and high
endurance. It is a possible technology precedent for HBF, but this brochure does
not establish that standardized HBF uses Z-NAND or inherits SZ985 behavior.

## Related Pages

- [[memory/concepts/z-nand-low-latency-flash]]
- [[memory/concepts/hbf-high-bandwidth-flash]]
- [[memory/concepts/hbm-memory-architecture]]
- [[memory/concepts/lpddr-memory]]
