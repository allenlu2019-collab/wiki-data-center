---
title: Samsung Z-NAND Low-Latency Flash
created: 2026-08-05
updated: 2026-08-05
type: concept
tags: [z-nand, bandwidth, capacity, latency, vendor]
sources: [raw/papers/samsung-z-nand-ssd-technology-brief.md, raw/articles/technews-samsung-zhbm-znand-o-v10-bv-nand.md]
---

# Samsung Z-NAND Low-Latency Flash

Z-NAND is Samsung's low-latency NAND technology for storage tiers between DRAM
and conventional NAND SSDs. Samsung's 2017 SZ985 Z-SSD provides the first
concrete baseline in the source material reviewed here.

Samsung says Z-NAND retains the fundamental 3D-flash structure of V-NAND while
using a specialized circuit design and controller to reduce latency and improve
performance. The reviewed brief does not disclose enough media-level detail to
attribute the improvement to a particular cell mode or array organization.

## Reference Product

| Metric | SZ985 value |
| --- | ---: |
| Capacity | 800 GB |
| Interface | PCIe Gen3 x4 |
| Sequential read/write | 3.2/3.2 GB/s at 128 KB |
| Sustained random read/write | 750K/170K IOPS at 4 KB |
| Sustained random-read latency | 12-20 microseconds |
| Typical random-write latency | 16 microseconds |
| Endurance | 30 DWPD |

This combination is significant because it couples low read latency with much
higher write endurance than ordinary capacity-oriented NAND SSDs. It remains a
storage device with PCIe and SSD-controller semantics, not byte-addressable
DRAM.

## Architectural Role

Z-NAND is most credible when capacity is more important than DRAM latency but
ordinary SSD latency is too high:

- Database and key-value-store working sets.
- Large flash-backed caches.
- Storage acceleration for data analytics.
- Read-intensive indexes and metadata.
- Emergency or deliberate paging tiers.

Z-NAND does not remove the storage/memory distinction. Software still pays host
I/O, controller, protocol, queueing, and block-management overhead.

## Z-NAND Versus HBF

[[memory/concepts/hbf-high-bandwidth-flash]] moves NAND closer to processors
through stacked packaging and UCIe. Z-NAND instead demonstrates an optimized
NAND medium and SSD implementation behind PCIe.

The two approaches are potentially complementary:

```text
Z-NAND: improve NAND media, circuitry, controller, latency, and endurance
HBF:    improve package-level parallelism and processor attachment bandwidth
```

A future HBF implementation could theoretically use low-latency NAND
techniques, but no reviewed source establishes that relationship. HBF's 3 TB/s
headline bandwidth also cannot predict random access latency; SZ985's
12-20-microsecond figure shows why latency must be specified independently.

## Vendor Benchmark Evidence

Samsung reports, relative to the PM1725a:

- 2x RocksDB throughput and one-half RocksDB average latency.
- 1.6x Fatcache throughput.
- 3x Memcached paging throughput and one-third paging read latency.

These results show possible application-level benefit, but the brief lacks the
full test configuration and uncertainty needed for reproducibility. They should
be treated as historical vendor evidence rather than current cross-vendor
benchmarks.

## zNAND-O Edge-AI Concept

At FMS 2026, Samsung presented zNAND-O as a developing 4-layer or 8-layer
memory concept optimized for edge AI. The reported attributes are high spatial
efficiency, low latency, and strong I/O performance. No numerical capacity,
bandwidth, latency, endurance, or energy values were disclosed.

| Documented by the article | Not yet established |
| --- | --- |
| 4-layer and 8-layer concepts | UCIe, TSV, or micro-bump interface |
| Edge-AI positioning | Direct AP/NPU cache access |
| Density, latency, and I/O emphasis | LPDDR or system-DRAM bypass |
| Under development | Near-memory compute capability |

zNAND-O must not be treated as Samsung's implementation of
[[memory/concepts/hbf-high-bandwidth-flash]]. HBF has a disclosed UCIe-based
proposal and quantitative bandwidth and capacity grades; the reviewed zNAND-O
report does not disclose its attachment or data path. Direct LLM-weight
streaming is a useful hypothesis to test, not a verified zNAND-O capability.

## Open Questions

- Which media and controller mechanisms distinguish Z-NAND from V-NAND?
- What were the actual power and energy-per-I/O values?
- How did latency change with queue depth, write pressure, and drive fill?
- How does Z-NAND compare with contemporary low-latency TLC, SCM, and CXL
  memory tiers?
- Could Z-NAND-like media meet HBF package power and thermal requirements?
- Is Z-NAND still an active Samsung product roadmap or primarily a historical
  technology lineage?
- What physical interface, protocol, access granularity, and buffering model
  will zNAND-O use?
- Can zNAND-O bypass LPDDR in a real edge system, and under what coherence and
  DMA constraints?

## Related Pages

- [[raw/papers/samsung-z-nand-ssd-technology-brief]]
- [[raw/articles/technews-samsung-zhbm-znand-o-v10-bv-nand]]
- [[memory/concepts/hbf-high-bandwidth-flash]]
- [[memory/concepts/hbm-memory-architecture]]
- [[memory/concepts/lpddr-memory]]
