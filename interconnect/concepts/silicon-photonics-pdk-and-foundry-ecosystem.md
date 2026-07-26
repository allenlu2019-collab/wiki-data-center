---
title: Silicon Photonics PDK and Foundry Ecosystem
created: 2026-07-26
updated: 2026-07-26
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

## Platform landscape

| Platform | Reported positioning | Strategic emphasis |
|---|---|---|
| GlobalFoundries Fotonix | Mature silicon-photonics PDK ecosystem | Expanded high-speed and photonic-native compact modeling with EDA partners |
| Tower + OpenLight | OpenLight PDK/IP connected to Tower production | Fabless photonic design and heterogeneous laser integration workflow |
| TSMC COUPE | Silicon photonics integrated with advanced packaging | Circuit-designer flow and co-packaged optical integration |
| Samsung 300 mm SiPh | Large-wafer manufacturing platform | End-to-end path from photonic ICs toward CPO |

This landscape changes quickly and should be verified against current primary foundry documentation. PDK access, device options, model bandwidth, laser integration, packaging design kits, and volume-qualification status can differ substantially even when vendors use similar platform language.

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

- [[interconnect/comparisons/silicon-photonics-vs-cmos-pdk]] — differences in physics, models, verification, and portability
- [[interconnect/concepts/ethernet-speeds-and-standards]] — CPO and Ethernet lane-rate roadmap
- [[interconnect/concepts/dac-acc-aec-copper-optics]] — reach, power, and packaging tradeoffs
- [[memory/concepts/hbf-high-bandwidth-fabric]] — photonic accelerator and memory fabrics
- [[shared/concepts/3dic-bonding-and-tsvs]] — heterogeneous integration and packaging interfaces
