# Wiki Schema — Data Center Infrastructure

## Domain
Modern data center infrastructure — compute hardware (CPU, GPU, TPU, NPU), memory architecture (HBM, HBF, LPDDR), interconnect (copper/optical networking at all speeds/distances), chip-level advanced packaging (CoWoS, 2.5D/3D IC, chiplets, interposers, bonding), passive components and substrates (MLCCs, PCBs, ABF, inductors, resistors), power delivery systems (48V, 400V, 800V), thermal & cooling, and rack/pod/superpod system architectures.

## Conventions
- File names: lowercase, hyphens, no spaces (e.g., `hbm-memory-architecture.md`)
- Every wiki page starts with YAML frontmatter (see below)
- Use `[[wikilinks]]` to link between pages (minimum 2 outbound links per page)
- Cross-topic links use the topic prefix: `[[compute/gpu-architecture]]` from a `rack-pod/` page
- When updating a page, always bump the `updated` date
- Every new page must be added to `index.md` under the correct section
- Every action must be appended to `log.md`
- Raw PDFs are NOT stored in git — use arXiv URLs as canonical sources in frontmatter; store markdown summaries in `raw/papers/`

## Frontmatter
```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy below]
sources: [raw/articles/source-name.md]
---
```

## Tag Taxonomy
All tags must be from this taxonomy. Add new tags here BEFORE using them.

### Architecture
- `rack` — rack-level systems, form factors, cabling
- `pod` — pod-level aggregation, groups of racks
- `superpod` — superpod-scale, Exascale deployments
- `topology` — network topology (fat-tree, dragonfly, torus)
- `scale` — large-scale deployment patterns

### Compute
- `cpu` — CPUs: x86, ARM, RISC-V, AMD/Intel/Ampere
- `gpu` — GPUs: NVIDIA, AMD, Intel, AI training/inference
- `tpu` — TPU: Google Tensor Processing Units
- `npu` — NPU: neural processing units, AI ASICs
- `accelerator` — general accelerator architecture
- `server` — server platforms, baseboards, BMC
- `asic` — custom ASIC design

### Memory
- `hbm` — HBM (High Bandwidth Memory): HBM2e, HBM3, HBM4
- `hbf` — HBF (High Bandwidth Flash): stacked NAND connected through UCIe
- `z-nand` — Samsung low-latency NAND and Z-SSD products
- `lpddr` — LPDDR: LPDDR5, LPDDR5X, LPDDR6
- `bandwidth` — memory bandwidth metrics, bw per TFLOP ratios
- `capacity` — memory capacity per node, per rack
- `latency` — memory latency, NUMA effects
- `memory-pool` — disaggregated / pooled memory

### Interconnect
- `copper` — copper interconnects: DAC, ACC, AEC
- `optics` — optical interconnects: SR, DR, FR, LR, ZR
- `silicon-photonics` — silicon-photonic devices, optical engines, and integration platforms
- `pdk` — process design kits, verified component libraries, design rules, and compact models
- `ethernet` — Ethernet: 25/50/100/200/400/800/1.6T
- `infiniband` — InfiniBand: HDR, NDR, XDR
- `nvlink` — NVIDIA NVLink, NVSwitch
- `pcie` — PCIe: Gen4, Gen5, Gen6, CXL
- `cxl` — Compute Express Link
- `fabric` — network fabric technologies
- `distance` — reach categories (SR 100m, DR 500m, FR 2km, ZR 120km)

### Packaging
- `advanced-packaging` — chip-level advanced packaging and heterogeneous integration
- `cowos` — CoWoS and related 2.5D integration platforms
- `3dic` — 3D IC stacking and vertical integration
- `chiplet` — chiplet architectures and die-to-die integration
- `interposer` — silicon, organic, and glass interposers
- `hybrid-bonding` — direct copper and dielectric hybrid bonding
- `tsv` — through-silicon vias and vertical interconnects

### Passive
- `mlcc` — multilayer ceramic capacitors
- `pcb` — printed circuit boards and high-speed board design
- `ccl` — copper-clad laminates, resin systems, copper foils, and reinforcement fabrics
- `abf` — Ajinomoto build-up film and package substrates
- `substrate` — organic, ceramic, and glass package substrates
- `capacitor` — capacitors and decoupling networks
- `inductor` — inductors, chokes, and magnetic passive components
- `resistor` — resistors and passive termination networks

### Power
- `48v` — 48V rack-level distribution
- `400v` — 400V / 415V facility distribution
- `800v` — 800V architecture, medium-voltage DC
- `efficiency` — efficiency metrics: PUE, TUE, 80 PLUS
- `psu` — power supply units, rectifiers
- `ups` — uninterruptible power supply, battery backup
- `distribution` — power distribution: busbars, PDUs, busways

### Thermal
- `cooling` — general cooling strategies
- `liquid` — liquid cooling: direct-to-chip, cold plate
- `immersion` — immersion cooling: single-phase, two-phase
- `air` — air cooling: CRAC, CRAH, containment
- `cdu` — coolant distribution unit
- `heat-reuse` — waste heat recovery, district heating

### Meta
- `comparison` — side-by-side vendor/product analysis
- `vendor` — vendor-specific content (NVIDIA, AMD, Intel, Google, AWS)
- `foundry` — semiconductor and photonics manufacturing platforms
- `eda` — electronic and photonic design-automation tools and workflows
- `standard` — industry standards (OCP, IEEE, JEDEC)
- `benchmark` — performance benchmarks
- `timeline` — technology evolution over time

## Page Thresholds
- **Create a page** when an entity/concept appears in 2+ sources OR is central to one source
- **Add to existing page** when a source mentions something already covered
- **DON'T create a page** for passing mentions, minor details, or things outside the domain
- **Split a page** when it exceeds ~200 lines — break into sub-topics with cross-links
- **Archive a page** when its content is fully superseded — move to `_archive/`, remove from index

## Entity Pages
One page per notable entity (vendor, product, standard). Include:
- Overview / what it is
- Key facts and dates
- Specifications in tables
- Relationships to other entities ([[wikilinks]])
- Source references

## Concept Pages
One page per concept or topic. Include:
- Definition / explanation
- Current state of knowledge (as of last update)
- Key metrics and dimensions
- Open questions or debates
- Related concepts ([[wikilinks]])

## Comparison Pages
Side-by-side analyses. Include:
- What is being compared and why
- Dimensions of comparison (table format preferred)
- Verdict or synthesis
- Sources

## Update Policy
When new information conflicts with existing content:
1. Check the dates — newer sources generally supersede older ones
2. If genuinely contradictory, note both positions with dates and sources
3. Mark the contradiction in frontmatter: `contradictions: [page-name]`
4. Flag for user review in the lint report
