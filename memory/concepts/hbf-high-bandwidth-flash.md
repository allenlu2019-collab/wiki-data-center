---
title: HBF - High Bandwidth Flash
created: 2026-08-04
updated: 2026-08-04
type: concept
tags: [hbf, bandwidth, capacity, chiplet, standard]
sources: [raw/articles/technews-sk-hynix-sandisk-hbf-standard.md]
---

# HBF - High Bandwidth Flash

High Bandwidth Flash (HBF) is a proposed package-level memory/storage tier that
stacks NAND flash and connects it to CPUs, GPUs, or other processors through
UCIe. SK hynix and Sandisk published the first open HBF specification through
the Open Compute Project in August 2026.

HBF targets a gap between [[memory/concepts/hbm-memory-architecture]] and NVMe
SSDs: substantially more capacity and lower cost than HBM, with much higher
bandwidth than conventional flash storage.

## First Standard Baseline

| Property | First reported specification |
| --- | --- |
| Medium | NAND flash |
| Stack height | 8 or 16 NAND dies |
| Maximum capacity | 512 GB |
| Bandwidth | Approximately 0.4-3.0 TB/s |
| Grades | Grade 1 through Grade 3 |
| Interface | UCIe |
| Standardization | Open Compute Project |

The exact mapping among grade, bandwidth, stack height, and capacity has not yet
been established in the reviewed secondary source.

## Position in the Hierarchy

| Technology | Main strength | Main weakness | Likely HBF relationship |
| --- | --- | --- | --- |
| SRAM/cache | Lowest latency | Very low capacity and high area cost | Hot working set above HBF |
| HBM | High bandwidth and DRAM semantics | Capacity and cost | Active state and hottest weights above HBF |
| HBF | NAND capacity with package-level bandwidth | Flash latency, writes, and endurance | Large read-mostly model/data tier |
| NVMe SSD | Capacity and mature storage ecosystem | Lower bandwidth and I/O latency | Backing storage below HBF |

HBF is not simply “slower HBM.” NAND and DRAM differ in latency, write
granularity, endurance, refresh behavior, and software semantics. Any analytical
model must preserve those distinctions instead of comparing only TB/s and GB.

## Candidate AI Uses

- Read-mostly LLM weights, especially inactive MoE experts.
- Retrieval corpora and embedding indexes.
- Checkpoints and model variants close to accelerators.
- Large immutable tables or datasets.
- Capacity tier below HBM in disaggregated inference systems.

Dynamic KV cache, activations, optimizer state, and other frequently written
objects remain questionable fits until write behavior and endurance are known.

## UCIe Significance

UCIe gives HBF a standardized die-to-die attachment path and allows packaging
with heterogeneous processors. It does not, by itself, establish:

- HBM-compatible commands or timing.
- CPU cache coherence.
- Byte-addressable load/store semantics.
- HBM-like random-read latency.
- Efficient fine-grained runtime writes.

Those properties depend on the HBF protocol, controller, package, and software
stack layered over UCIe. A dedicated UCIe concept page has not yet been created.

## Runtime Write Policy

The first reported specification includes read/write software guidance, so HBF
is not physically read-only. However, NAND program and erase operations remain
fundamentally less attractive than reads. Early systems may expose writes for
model loading, offline updates, garbage collection, and maintenance while using
HBF as a read-mostly tier during inference.

The wiki should not equate “write path exists” with “HBM-like writable memory.”

## Acronym Disambiguation

This repository previously used **HBF** to mean “High Bandwidth Fabric,” a
generic disaggregated-memory concept. Following the OCP-backed 2026
specification, unqualified **HBF now means High Bandwidth Flash**.

The older fabric concept remains documented at
[[memory/concepts/hbf-high-bandwidth-fabric]], but it should be called
**disaggregated high-bandwidth memory fabric**, not HBF.

## Open Questions

- What are the normative Grade 1-3 bandwidth and capacity points?
- What protocol and memory semantics run over UCIe?
- What are sustained random-read and write performance?
- What endurance and write-amplification limits apply?
- Can HBF directly serve GPU page faults or must software stage into HBM?
- What are package power, energy per bit, and cooling requirements?
- How should HBF capacity be shared across accelerators?

## Low-Latency NAND Precedent

Samsung's [[memory/concepts/z-nand-low-latency-flash]] is a useful historical
comparison. The 2017 SZ985 reported 12-20 microsecond sustained random-read
latency, 16 microsecond typical random-write latency, and 30 DWPD behind PCIe
Gen3 x4. It demonstrates that NAND can be optimized for latency and endurance,
but it does not prove that HBF uses Z-NAND or will inherit those characteristics.

The comparison also reinforces that HBF's bandwidth grades are insufficient for
performance modeling without random-access latency, write behavior, endurance,
and software semantics.

## Related Pages

- [[raw/articles/technews-sk-hynix-sandisk-hbf-standard]]
- [[memory/concepts/z-nand-low-latency-flash]]
- [[memory/concepts/hbm-memory-architecture]]
- [[memory/concepts/hbf-high-bandwidth-fabric]]
- UCIe chiplet interconnect (concept page not yet created)
