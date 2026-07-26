---
title: Silicon Photonics PDK vs CMOS PDK
created: 2026-07-26
updated: 2026-07-26
type: comparison
tags: [comparison, silicon-photonics, pdk, foundry, eda, asic]
sources: [raw/articles/simple-tech-trend-silicon-photonics-pdk.md]
confidence: medium
---

# Silicon Photonics PDK vs CMOS PDK

Silicon-photonics (SiPh) and CMOS process design kits serve the same foundational purpose: they translate a foundry process into manufacturable design rules, reusable devices, predictive models, and tool integrations. The difference is the physical behavior each kit must capture and the maturity of its design ecosystem.

## Comparison

| Dimension | CMOS PDK | Silicon-photonics PDK |
|---|---|---|
| Primary objects | Transistors, resistors, capacitors, diodes, and metal interconnect | Waveguides, modulators, photodiodes, couplers, splitters, filters, and optical transitions |
| Main signals | Voltage and current | Optical amplitude and phase plus electrical drive and receive signals |
| Core physical domains | Primarily electrical, with parasitic and thermal effects | Optical, RF electrical, and thermal behavior coupled together |
| Key independent variables | Voltage, current, frequency, temperature, and process corner | Wavelength, phase, polarization, voltage, temperature, geometry, and process variation |
| Geometric sensitivity | Nanometre variation matters, but mature statistical corner models exist | Nanometre-scale waveguide variation can materially shift effective index, phase, resonance, and coupling |
| Interconnect model | Extracted resistance, capacitance, inductance, coupling, and transmission-line behavior | Complex optical S-parameters describing amplitude and phase versus wavelength, plus electrical parasitics |
| Verification | DRC, LVS, extraction, timing, power, noise, and signal integrity | DRC, optical connectivity, mode/polarization behavior, wavelength-domain analysis, and electro-optic co-simulation |
| Compact-model maturity | Refined through decades of production and extensive standardization | Still evolving, with substantial foundry-specific model formats and bandwidth limitations |
| Foundry portability | Process-specific, but overall workflows and abstractions are broadly similar | Moving foundries may require device replacement, layout redesign, model changes, and packaging requalification |
| Packaging interaction | Packaging is usually a downstream co-design step | Lasers, fiber attach, optical I/O, thermal tuning, and alignment are tightly coupled to photonic-circuit behavior |

## Compact-model difference

A CMOS transistor compact model predicts electrical behavior such as current versus terminal voltage, capacitance, leakage, noise, temperature response, and process corners. The abstraction is mature enough that digital designers can usually treat transistors as dependable primitives beneath standard-cell and synthesis flows.

A SiPh compact model must additionally predict:

- optical amplitude and phase;
- wavelength and polarization dependence;
- propagation, coupling, and insertion loss;
- high-frequency electrode and impedance behavior;
- voltage-driven modulation and photodetection;
- temperature-driven wavelength drift;
- nanometre-scale manufacturing variation; and
- interfaces between optical, electrical, and thermal simulators.

The optical carrier near 1550 nm operates around 193 THz, while electrical modulation is typically in the tens or hundreds of GHz. Directly simulating every optical cycle is impractical, so SiPh tools rely on envelope or equivalent-baseband representations to connect optical behavior to circuit-level time scales.

## Process variation

Both technologies require foundry-measured models and statistical corners, but the consequences differ. CMOS variation changes transistor speed, leakage, threshold voltage, and interconnect parasitics. SiPh variation can also accumulate optical phase error and shift resonance or coupling conditions, causing a circuit-level functional change even when the layout remains manufacturable.

This makes variation-aware photonic compact models especially important for interferometers, resonators, wavelength filters, and dense wavelength-division multiplexing.

## Verification and abstraction maturity

CMOS verification benefits from established, relatively portable abstractions:

- device models and process corners;
- standard cells and memory compilers;
- DRC/LVS and parasitic extraction;
- static timing, power, IR-drop, and electromigration analysis; and
- standardized flows between foundries and EDA vendors.

SiPh verification is less uniform. A complete flow must connect optical layout and connectivity to wavelength-dependent circuit simulation, RF drivers and TIAs, thermal drift and tuning, fiber/package coupling, test structures, and statistical yield. Tool and model interfaces remain more specific to each foundry/EDA combination.

## Foundry moat

Both PDK types create switching costs, but SiPh lock-in can be stronger because the design may depend on foundry-specific optical devices, compact models, fiber interfaces, laser integration, packaging, and test methodology. Re-targeting a CMOS design is expensive; re-targeting a SiPh design can also change its optical architecture and physical coupling strategy.

This is why SiPh PDK quality is a strategic foundry asset. The platform must offer not only manufacturable devices, but models accurate enough for designers to commit to costly photonic tape-outs with confidence. The ecosystem dynamics are covered in [[interconnect/concepts/silicon-photonics-pdk-and-foundry-ecosystem]].

## Synthesis

A CMOS PDK guarantees that electronic devices can be manufactured and electrically predicted. A SiPh PDK must additionally predict how light propagates, interferes, couples, modulates, heats, and responds to minute manufacturing variation.

SiPh PDKs are therefore not fundamentally different in purpose; they are broader in physical scope, more tightly coupled to packaging, less standardized, and less mature. Their progress directly affects whether co-packaged optics can move from custom demonstrations to repeatable production at the scales discussed in [[interconnect/concepts/ethernet-speeds-and-standards]].

## Related concepts

- [[interconnect/concepts/silicon-photonics-pdk-and-foundry-ecosystem]] — SiPh PDK structure and foundry platforms
- [[interconnect/concepts/ethernet-speeds-and-standards]] — CPO and high-speed Ethernet roadmap
- [[interconnect/concepts/dac-acc-aec-copper-optics]] — copper-to-optics boundary
- [[shared/concepts/3dic-bonding-and-tsvs]] — heterogeneous integration and packaging
