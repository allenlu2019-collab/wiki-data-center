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
