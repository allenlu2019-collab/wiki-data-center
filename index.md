# Wiki Index — Data Center Infrastructure

> Content catalog. Every wiki page listed under its type with a one-line summary.
> Read this first to find relevant pages for any query.
> Last updated: 2026-08-05 | Total pages: 28

## Compute — Concepts
- [[compute/concepts/gpu-architecture]] — GPU accelerator design, memory subsystem, scaling architecture
- [[compute/concepts/tpu-and-npu-architectures]] — Google TPU, AWS Trainium, Cerebras, Groq LPU

## Compute — Entities
- [[compute/entities/nvidia-gpu-lineage]] — Full NVIDIA data center GPU product history
- [[compute/entities/nvidia-vera-cpu]] — Vera CPU, Olympus cores, LPDDR5X memory, coherent fabric, and agentic-system role

## Compute — Comparisons

## Memory — Concepts
- [[memory/concepts/hbm-memory-architecture]] — HBM generations, bandwidth, Ops:BW gap, the "HBM wall"
- [[memory/concepts/hbf-high-bandwidth-flash]] — OCP-standard stacked NAND tier using UCIe, up to 512 GB and 0.4-3.0 TB/s reported
- [[memory/concepts/hbf-high-bandwidth-fabric]] — Legacy local label for disaggregated memory via NVLink, CXL, or optical fabric; no longer abbreviated HBF
- [[memory/concepts/z-nand-low-latency-flash]] — Samsung low-latency NAND, SZ985 specifications, application evidence, and relationship to HBF
- [[memory/concepts/lpddr-memory]] — LPDDR in data center servers, Grace Hopper, edge inference

## Memory — Entities
- [[memory/entities/rambus-hbm-interface-ip]] — Rambus controller/PHY IP, validation role, and architectural boundaries

## Memory — Comparisons

## Interconnect — Concepts
- [[interconnect/concepts/ai-fabric-scaling-taxonomy]] — Scale-up, scale-out, scale-across, and host-attachment domains
- [[interconnect/concepts/ethernet-speeds-and-standards]] — Ethernet roadmap 10G→1.6T, physical layer types, CPO
- [[interconnect/concepts/infiniband-vs-ethernet]] — Head-to-head for AI fabrics, UEC, cost/performance/topology
- [[interconnect/concepts/nvlink-and-nvswitch]] — GPU-to-GPU fabric, NVSwitch crossbar, NVLink-C2C
- [[interconnect/concepts/dac-acc-aec-copper-optics]] — Copper cable types, optics boundary, AI cluster cabling
- [[interconnect/concepts/silicon-photonics-pdk-and-foundry-ecosystem]] — Photonic PDK layers, multiphysics models, EDA integration, and foundry platform moats

## Interconnect — Entities
- [[interconnect/entities/semtech-signal-integrity]] — Linear optical PMDs and active-copper signal conditioning

## Interconnect — Comparisons
- [[interconnect/comparisons/silicon-photonics-vs-cmos-pdk]] — Side-by-side comparison of devices, physics, compact models, verification, portability, and foundry lock-in

## Packaging — Concepts

## Packaging — Entities

## Packaging — Comparisons

## Passive — Concepts
- [[passive/concepts/pcb-ccl-signal-integrity]] — CCL resin, copper roughness, glass weave, and loss classes for 112G/224G PAM4 boards

## Passive — Entities

## Passive — Comparisons

## Power — Concepts
- [[power/concepts/dc-power-distribution-architectures]] — 48V/400V/800V architectures, PSU, UPS, density trends

## Power — Entities

## Power — Comparisons

## Thermal — Concepts
- [[thermal/concepts/microchannel-liquid-cooling]] — Package-integrated channels, manifolds, TIM, and mechanical risks
- [[thermal/concepts/liquid-cooling-technologies]] — Direct-to-chip, immersion, CDUs, pod-scale cooling, heat reuse

## Thermal — Entities

## Thermal — Comparisons

## Rack/Pod — Concepts
- [[rack-pod/concepts/pod-and-superpod-architectures]] — Rack/pod/superpod hierarchy, scale-up superpods, DGX SuperPOD, and facility constraints

## Rack/Pod — Entities

## Rack/Pod — Comparisons

## Shared — Entities

## Shared — Concepts
- [[shared/concepts/3dic-bonding-and-tsvs]] — Micro-bumps, hybrid bonding, face orientation, and TSV requirements

## Queries
