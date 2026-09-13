---
title: Silicon Photonics PDK and Foundry Ecosystem
created: 2026-07-26
updated: 2026-08-16
type: concept
tags: [silicon-photonics, optics, pdk, foundry, eda, advanced-packaging]
sources: [raw/articles/simple-tech-trend-silicon-photonics-pdk.md]
confidence: medium
---

# Silicon Photonics PDK and Foundry Ecosystem

A silicon-photonics process design kit (PDK) converts a foundry's physical process into reusable design abstractions. It is the interface between photonic-circuit intent and manufacturable silicon: verified devices, geometric rules, statistical limits, and compact models that can be consumed by photonic and electronic design tools.

The PDK is therefore production infrastructure, not merely documentation. Without predictive models and validated components, every photonic tape-out becomes an expensive experiment. With a mature PDK, fabless teams can compose larger optical systems and verify them before committing to masks and months of fabrication.

## Three PDK layers

| Layer | Typical contents | Purpose |
|---|---|---|
| Component library | Waveguides, MMIs, splitters, couplers, modulators, photodiodes, tapers | Reuse foundry-characterized devices with known process compatibility |
| Design rules | Layer map, widths, spacing, bend radius, stack and forbidden geometries | Keep layouts inside manufacturing and optical-interaction limits; enable DRC |
| Compact models | S-parameters, wavelength response, voltage/temperature dependence, RF and thermal interfaces | Predict circuit and link behavior in system-level simulation |

All three are required. A layout library without accurate behavioral models may be manufacturable but not predictable; models without design rules may describe devices that cannot be fabricated reliably.

## Why photonic models are difficult

### Continuous and wavelength-dependent behavior

Modulators, filters, interferometers, and couplers respond continuously to wavelength, voltage, temperature, geometry, and loss. Circuit behavior is analog and phase-sensitive, so small local errors can accumulate into a large system error.

### Process variation

Nanometre-scale changes in waveguide width or thickness alter effective index and phase. A production PDK needs nominal models plus variation-aware corners or statistical distributions tied to measured process data.

### Optical, electrical, and thermal coupling

High-speed modulators and photodetectors cross three simulation domains:

- optical amplitude and phase;
- RF impedance, electrodes, skin effect, and electrical bandwidth; and
- self-heating and temperature-driven wavelength drift.

The optical carrier near 193 THz is also far above the electrical modulation rate. Compact models use envelope or equivalent-baseband methods so circuit simulators can model modulation without sampling every optical cycle.

## PDK as a foundry moat

A proven PDK creates switching costs through accumulated layouts, simulation flows, characterized silicon, designer expertise, and qualification data. Moving a design to another foundry may require replacing devices, revalidating compact models, changing DRC, and repeating packaging and test qualification.

The moat is reinforced by EDA integration. Foundry PDKs are bound into flows such as Synopsys OptoCompiler, Luceda IPKISS, Keysight ADS, and Cadence environments. Ecosystem quality therefore depends on both the manufacturing process and how smoothly its libraries, models, verification, and packaging data work across tools.

## Leading foundry landscape

“Leading” depends on the required service model. An open merchant foundry that accepts external designs is different from a vertically integrated manufacturer with large internal volume, while an R&D platform may offer easier MPW access but not comparable production capacity.

| Organization and platform | Service model | Current position and strength |
|---|---|---|
| GlobalFoundries — GF Fotonix | Open commercial foundry | One of the strongest established choices for external high-volume production. Its 300 mm platform combines photonics and RF CMOS, supports pluggables through CPO, and has production-proven 100G/λ capability, demonstrated 200G/λ, and a 400G/λ roadmap. |
| Tower Semiconductor — PH18 | Open commercial foundry | Mature 220 nm SOI platform with low-loss waveguides, modulators, Ge photodetectors, PDK/MPW access, and heterogeneous InP or GaAs quantum-dot laser options. Particularly accessible for fabless photonic products. |
| TSMC — COUPE | Commercial foundry and advanced-packaging integration | Strategically important for AI and HPC CPO. COUPE uses SoIC-X to stack an electrical die over a photonic die and can be integrated into CoWoS. Its main differentiation is co-optimization with advanced logic and packaging; its open SiPh ecosystem is less mature than GF or Tower's established offerings. |
| Intel Silicon Photonics | Primarily vertically integrated manufacturer | Demonstrates the largest disclosed internal production scale among this group: Intel reports more than eight million PICs and 32 million integrated lasers shipped. It is a manufacturing and optical-I/O leader, but historically has not offered the same broad merchant-foundry access as GF or Tower. |
| imec — iSiPP200/iSiPP300 | R&D, prototyping, process transfer, and low volume | Leading development platform with a mature 200 mm silicon-validated PDK and a 300 mm path. Strong for device research, custom process modules, MPWs, and transferring technology toward commercial foundries. |
| AIM Photonics | Public-private R&D and MPW ecosystem | Provides 300 mm SiPh prototyping through Albany NanoTech plus packaging, assembly, and testing in Rochester. Strong US access for startups, universities, and government programs rather than being a direct peer to a high-volume merchant foundry. |
| UMC — licensed iSiPP300 | Emerging commercial entrant | Licensed imec's 300 mm, CPO-compatible iSiPP300 technology in December 2025. It is important to watch, but should not yet be treated as equally production-established in SiPh as GF or Tower. |

### Practical selection

- **Established open production:** GlobalFoundries and Tower Semiconductor.
- **Tight AI/CPO integration with leading logic and packaging:** TSMC.
- **Proven vertically integrated SiPh volume:** Intel.
- **Research, MPW, and process development:** imec and AIM Photonics.
- **Emerging 300 mm merchant capacity:** UMC.

For AI interconnects, the most consequential comparison is often **GF Fotonix versus TSMC COUPE**. GF offers a mature open photonics manufacturing platform and PDK ecosystem. TSMC's differentiator is placing the photonic engine inside its SoIC/CoWoS heterogeneous-integration stack alongside advanced compute dies and HBM.

Primary platform references: [GlobalFoundries Fotonix](https://gf.com/technologies/silicon-photonics/), [Tower PH18](https://towersemi.com/2023/03/02/03022023/), [TSMC COUPE](https://pr.tsmc.com/english/news/3136), [Intel Silicon Photonics](https://www.intel.com/content/www/us/en/products/details/network-io/silicon-photonics.html), [imec photonics services](https://www.imec-int.com/en/what-we-offer/development/photonics), [AIM Photonics MPW](https://www.aimphotonics.com/mpw), and [imec–UMC iSiPP300 transfer](https://www.imec-int.com/en/press/umc-licenses-imecs-isipp300-technology-extend-silicon-photonics-capabilities-next-generation).

This landscape changes quickly. PDK availability, customer eligibility, MPW schedules, device options, model bandwidth, laser integration, packaging design kits, and volume qualification must be verified for each program rather than inferred from similar platform language.

## Open models and portability

Open compact-model libraries, including Verilog-A approaches, can improve inspectability, reproducibility, and cross-foundry portability. They do not replace a foundry PDK's confidential process data or manufacturing guarantees, but they can standardize behavioral interfaces and reduce dependence on one proprietary toolchain.

The long-term ecosystem question is where abstraction stabilizes:

- foundry-specific device geometry and process corners;
- portable compact-model interfaces;
- reusable optical/electrical circuit IP; and
- package, fiber-attach, thermal, and test abstractions shared across platforms.

## Data-center implications

Silicon-photonics PDK maturity affects whether optical engines can move from custom demonstrations into repeatable 800G, 1.6T, and co-packaged-optics production. Better models reduce link-margin uncertainty, improve yield learning, and let designers co-optimize the photonic IC, driver/TIA electronics, fiber coupling, and package before tape-out.

This connects directly to the CPO roadmap in [[interconnect/concepts/ethernet-speeds-and-standards]] and the copper-to-optics boundary in [[interconnect/concepts/dac-acc-aec-copper-optics]]. Photonic fabrics may also extend accelerator and memory reach as described in [[memory/concepts/hbf-high-bandwidth-fabric]]. Physical integration remains an advanced-packaging problem related to [[shared/concepts/3dic-bonding-and-tsvs]].

## Open questions

- Which compact-model formats will become portable across EDA tools and foundries?
- How should PDKs expose statistical process variation without revealing proprietary process details?
- When will electrical, optical, thermal, packaging, and test models form one qualified design flow?
- Can open models reduce ecosystem lock-in while retaining foundry-backed manufacturability guarantees?

## Related concepts

- [[interconnect/concepts/optical-interconnect-pic-eic-packaging-integration]] — PIC/EIC processes, partition options, foundry mappings, and COUPE physical integration
- [[interconnect/comparisons/silicon-photonics-foundry-positioning]] — verified comparison of TSMC, Tower, GF, and the evidence boundary for SMIC
- [[interconnect/comparisons/silicon-photonics-vs-cmos-pdk]] — differences in physics, models, verification, and portability
- [[interconnect/concepts/ethernet-speeds-and-standards]] — CPO and Ethernet lane-rate roadmap
- [[interconnect/concepts/dac-acc-aec-copper-optics]] — reach, power, and packaging tradeoffs
- [[memory/concepts/hbf-high-bandwidth-fabric]] — photonic accelerator and memory fabrics
- [[shared/concepts/3dic-bonding-and-tsvs]] — heterogeneous integration and packaging interfaces
