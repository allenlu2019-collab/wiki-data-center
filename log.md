# Wiki Log — Data Center Infrastructure

> Chronological record of all wiki actions. Append-only.
> Format: `## [YYYY-MM-DD] action | subject`
> Actions: ingest, update, query, lint, create, archive, delete
> When this file exceeds 500 entries, rotate: rename to log-YYYY.md, start fresh.

## [2026-07-26] create | Wiki initialized
- Domain: Data Center Infrastructure (compute, memory, interconnect, power, thermal, rack/pod/superpod)
- Structure: multi-topic layout with compute/, memory/, interconnect/, power/, thermal/, rack-pod/, shared/ subdirectories
- Core files: SCHEMA.md, index.md, log.md

## [2026-07-26] seed | Initial seed pages (10 concept/entity/comparison pages)
- Created compute/concepts/gpu-architecture.md
- Created compute/concepts/tpu-and-npu-architectures.md
- Created compute/entities/nvidia-gpu-lineage.md
- Created memory/concepts/hbm-memory-architecture.md
- Created memory/concepts/hbf-high-bandwidth-fabric.md
- Created memory/concepts/lpddr-memory.md
- Created interconnect/concepts/ethernet-speeds-and-standards.md
- Created interconnect/concepts/infiniband-vs-ethernet.md
- Created interconnect/concepts/nvlink-and-nvswitch.md
- Created interconnect/concepts/dac-acc-aec-copper-optics.md
- Created power/concepts/dc-power-distribution-architectures.md
- Created thermal/concepts/liquid-cooling-technologies.md
- Created rack-pod/concepts/pod-and-superpod-architectures.md
- Updated index.md with all 13 pages
- All pages cross-linked with [[wikilinks]]

## [2026-07-26] lint | 0 real issues found
- Ran full lint (orphans, broken links, index completeness, frontmatter, tags, page size)
- **Fixed:** 7 unknown tags (compute, interconnect, memory, memory-pool, nvidia, power, thermal) added to SCHEMA.md taxonomy
- **Layer-2 clean:** All 18 wiki pages have valid frontmatter, all wikilinks resolve, all pages indexed, all 45 tags in taxonomy
- **False positives (no action):** SCHEMA.md `[[wikilinks]]`/`[[compute/gpu-architecture]]` are backticked convention examples; raw/ `![[Pasted image]]` are Obsidian image embeds; `[[2024-1231-Perplexity]]` is an external note link
- **Expected (Layer 1):** raw/articles/* are immutable source material — not indexed, not orphan-checked, no wiki frontmatter required

## [2026-07-26] ingest | Data Center consulting notes
- Ingested raw/articles/3dic-bonding.md
- Ingested raw/articles/ai-data-center-interconnect.md
- Ingested raw/articles/gpu-cooling.md
- Ingested raw/articles/hbm-for-data-center.md
- Ingested raw/articles/optics.md
- Created shared/concepts/3dic-bonding-and-tsvs.md
- Created interconnect/concepts/ai-fabric-scaling-taxonomy.md
- Created thermal/concepts/microchannel-liquid-cooling.md
- Created interconnect/entities/semtech-signal-integrity.md
- Created memory/entities/rambus-hbm-interface-ip.md
- Updated HBM, NVLink/NVSwitch, copper/optics, and liquid-cooling concept pages
- Updated index.md and cross-links

## [2026-07-26] update | Add packaging and passive domains
- Added packaging as a peer domain for chip-level advanced packaging, including CoWoS, 2.5D/3D IC, chiplets, interposers, hybrid bonding, and TSVs
- Added passive as a peer domain for MLCCs, PCBs, ABF, substrates, capacitors, inductors, and resistors
- Added controlled packaging and passive tags to SCHEMA.md
- Added Packaging and Passive sections to index.md
- Created packaging/ and passive/ domain directories

## [2026-07-26] ingest | PCB CCL materials and signal integrity
- Ingested raw/articles/sipi-pcb-ccl-signal-integrity.md from SI/PI Frontier
- Added the ccl tag to SCHEMA.md
- Created passive/concepts/pcb-ccl-signal-integrity.md
- Updated interconnect/concepts/dac-acc-aec-copper-optics.md with a backlink
- Updated index.md page count and Passive concepts section

## [2026-07-26] ingest | China AI superpods at WAIC 2026
- Ingested raw/articles/baidu-china-ai-superpod-waic-2026.md from 报告派/Baidu
- Normalized the source term 超节点 as superpod per wiki-owner terminology
- Updated rack-pod/concepts/pod-and-superpod-architectures.md with Chinese scale-up superpod architectures, facility constraints, reliability, and TCO considerations
- Updated interconnect/concepts/ai-fabric-scaling-taxonomy.md to distinguish superpod product labels from scale-up and scale-out communication semantics
- Updated the rack/pod index summary

## [2026-07-26] ingest | Silicon photonics PDK and foundry ecosystem
- Ingested raw/articles/simple-tech-trend-silicon-photonics-pdk.md from Simple Tech Trend
- Added silicon-photonics, pdk, foundry, and eda tags to SCHEMA.md
- Created interconnect/concepts/silicon-photonics-pdk-and-foundry-ecosystem.md
- Updated interconnect/concepts/ethernet-speeds-and-standards.md with a PDK/CPO backlink
- Updated index.md page count and Interconnect concepts section

## [2026-07-26] create | Silicon photonics PDK vs CMOS PDK comparison
- Created interconnect/comparisons/silicon-photonics-vs-cmos-pdk.md
- Compared device libraries, physical domains, compact models, process variation, verification, portability, packaging interaction, and foundry lock-in
- Updated interconnect/concepts/silicon-photonics-pdk-and-foundry-ecosystem.md with a backlink
- Updated index.md page count and Interconnect comparisons section
## [2026-08-03] ingest | NVIDIA Vera CPU and Olympus core deep dive
- Ingested `raw/articles/the-register-nvidia-vera-olympus-deep-dive.md`
- Created `compute/entities/nvidia-vera-cpu.md`
- Recorded Vera's monolithic 88-core compute die, Olympus pipeline, spatial multithreading, LPDDR5X subsystem, coherent fabric, PCIe/CXL, and NVLink-C2C architecture
- Separated NVIDIA performance and memory-power claims from independently established facts
- Updated the compute entity index and page count

## [2026-08-03] update | Vera CPU coherence boundaries
- Added Q&A covering single-socket coherence, dual-socket coherent NUMA over NVLink-C2C, and the boundary beyond one Vera CPU Superchip
- Distinguished networked rack-scale nodes from a hardware cache-coherent domain
- Clarified that CXL 3.1 memory coherence and pooling do not automatically create rack-wide CPU cache coherence
- Compared the hierarchy with AMD xGMI/Infinity Fabric and Intel UPI systems

## [2026-08-03] update | Vera roles, x86 comparison, and CPU-only server
- Added Q&A on Vera's GPU-host and agent-runtime roles
- Clarified that x86 can perform both roles and that Vera's differentiation is integration and specialization rather than exclusive capability
- Added the potential advantages, target workloads, and limitations of a Vera-only CPU server
- Distinguished memory-intensive CPU suitability from GPU-class dense tensor throughput

## [2026-08-04] ingest | First OCP High Bandwidth Flash specification
- Ingested `raw/articles/technews-sk-hynix-sandisk-hbf-standard.md`
- Created `memory/concepts/hbf-high-bandwidth-flash.md`
- Recorded 8/16-die stacks, up to 512 GB, three bandwidth grades spanning approximately 0.4-3.0 TB/s, and UCIe processor attachment
- Distinguished existence of a write path from HBM-like runtime write behavior
- Corrected the repository taxonomy so unqualified HBF means High Bandwidth Flash
- Retained the older disaggregated-memory-fabric page under an explicit legacy-label warning
- Updated the memory index and page count

## [2026-08-05] ingest | Samsung Z-NAND SSD technology brief
- Ingested `raw/papers/samsung-z-nand-ssd-technology-brief.md` from Samsung's 2017 primary-source brochure
- Created `memory/concepts/z-nand-low-latency-flash.md`
- Recorded SZ985 capacity, PCIe interface, sequential bandwidth, random IOPS, latency, and 30 DWPD endurance
- Preserved Samsung's RocksDB, Fatcache, and Memcached benchmark ratios as vendor claims with reproducibility caveats
- Compared Z-NAND's PCIe SSD implementation with HBF's proposed UCIe package-level tier without asserting that HBF uses Z-NAND
- Added the controlled `z-nand` tag and updated the memory index

## [2026-08-05] ingest | Samsung zHBM, zNAND-O, and V10 BV-NAND
- Ingested `raw/articles/technews-samsung-zhbm-znand-o-v10-bv-nand.md`
- Recorded Samsung's zHBM, V10 BV-NAND, and zNAND-O announcements with vendor-claim caveats
- Added zNAND-O's documented 4-layer and 8-layer edge-AI positioning to the Z-NAND concept page
- Distinguished zNAND-O from the separately specified UCIe-based HBF proposal
- Audited the supplied interpretation and marked UCIe, TSV, DRAM bypass, near-memory compute, and direct-cache access as unverified for zNAND-O

## [2026-08-16] update | Leading silicon-photonics foundries
- Expanded the SiPh PDK ecosystem page with GlobalFoundries, Tower, TSMC, Intel, imec, AIM Photonics, and UMC
- Distinguished open merchant foundries, vertically integrated manufacturing, R&D/MPW platforms, and emerging commercial capacity
- Identified GF and Tower as established open-production choices and TSMC as strategically differentiated by SoIC/CoWoS integration
- Added primary platform references and cautions about PDK access, qualification, and production maturity

## [2026-08-16] verify | AI-generated SiPh foundry comparison
- Audited user-supplied AI analyses covering TSMC, Tower, GlobalFoundries, and SMIC against primary company material
- Added a dedicated comparison page separating PIC, EIC, package, and host-system integration boundaries
- Verified Tower's 200/300 mm platforms, 2027 contracts, prepayments, customer count, and announced capacity expansion as company disclosures
- Verified GF's AMF acquisition and SCALE/CHIPS roadmap while correcting the US$300 million item from “award” to letter of intent
- Retained TSMC's COUPE/SoIC/CoWoS architecture but withheld unsupported customer, yield, bandwidth, node, and wafer-capacity claims
- Classified SMIC as evidence-constrained rather than assigning an unsupported capability score

## [2026-08-16] synthesize | PIC, EIC, packaging, and COUPE integration Q&A
- Consolidated the optical-interconnect architecture discussion into a dedicated concept page
- Documented representative PIC and EIC fabrication flows and corrected Ge photodetector terminology
- Compared integrated versus split DSP, SerDes, driver, TIA, and control-die configurations across pluggable, LPO, NPO, CPO, and optical I/O
- Mapped Tower, GF Fotonix, and TSMC COUPE to their respective PIC/EIC integration philosophies
- Explained the probable GPU-to-COUPE electrical path through CoWoS, PIC TDVs, SoIC-X bonds, EIC, and the PIC modulator/detector
- Compared EIC-on-PIC with PIC-on-EIC while labeling product-specific routing as inference where public detail is incomplete
