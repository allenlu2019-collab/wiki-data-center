---
source_url: https://www.simpletechtrend.com/post/silicon-photonics-pdk-explained-foundry-moat
title: "Why Is Silicon Photonics So Difficult to Design? The Answer Is in the PDK"
original_title: "為什麼矽光子這麼難設計？答案藏在一個叫 PDK 的盒子裡"
author: Simple Tech Trend
published: 2026-07-22
ingested: 2026-07-26
content_mode: summarized
sha256: 5170ee8252d24cc68f65a5152359db74067dc1fac2b54cba25f200c50f762d03
---

# Silicon Photonics PDKs and the Foundry Moat

The article argues that silicon-photonics commercialization is constrained not only by lasers, modulators, fiber alignment, or packaging, but by the quality of the process design kit (PDK) that translates a foundry's manufacturing capability into a predictable design system.

## Why the PDK matters

A photonic tape-out can require months and million-scale mask expenditure. A mistake in waveguide bend radius, modulator frequency response, or coupler loss may force another fabrication cycle. A mature PDK reduces this risk by supplying foundry-validated building blocks, manufacturing rules, and models that let designers predict behavior before tape-out.

The article frames the PDK as a contract between the foundry and designer: it specifies what can be manufactured, how well it is expected to perform, and which geometry or process boundaries cannot be crossed. This infrastructure enables a fabless silicon-photonics business model.

## Three required layers

1. **Validated component library:** waveguides, multimode-interference couplers, grating couplers, splitters/combiners, modulators, photodiodes, tapers, and fiber couplers that the foundry has fabricated and characterized.
2. **Design rules and DRC:** layer definitions, stack information, line widths, spacing, minimum bend radius, and forbidden combinations. Automated checking catches manufacturing and optical-layout violations before tape-out.
3. **Compact models:** behavioral models covering amplitude and phase/S-parameters plus wavelength, temperature, voltage, RF, and thermal dependencies. Accurate compact models make circuit-level simulation trustworthy enough for manufacturing decisions.

## Why photonic PDKs are harder than CMOS PDKs

- Photonic components behave as continuous analog functions of wavelength, voltage, temperature, and process state rather than simple digital switches.
- Nanometre-scale waveguide variation can accumulate phase error and substantially shift interferometric-device behavior, so statistical process variation must be modeled.
- Electro-optic devices require self-consistent optical, high-frequency electrical, and thermal models.
- Optical carriers near 193 THz and electrical signals in the tens of GHz span roughly four orders of magnitude, requiring envelope or equivalent-baseband techniques for practical co-simulation.
- Component naming, model formats, and tool interfaces remain less standardized than mature CMOS ecosystems, increasing portability costs.

## Foundry and EDA ecosystem

The article presents PDK quality and tool integration as a foundry moat. After a design team builds libraries, flows, expertise, and proven silicon around one PDK, moving foundries can require requalification of components, models, layouts, and engineering practice. PDK integration with tools such as Synopsys OptoCompiler, Luceda IPKISS, Keysight ADS, and Cadence creates a second layer of ecosystem attachment.

Reported platform positions include:

- **GlobalFoundries Fotonix:** described as one of the more complete and mature PDK ecosystems. The article also cites criticism of older modeling limits around 26.5 Gbaud and missing noise modeling, and points to GF/Cadence photonic-native compact-model work.
- **Tower Semiconductor/OpenLight:** an integrated fabless-to-foundry flow in which OpenLight's PDK connects Luceda IPKISS design to Tower manufacturing.
- **TSMC COUPE:** positioned as a silicon-photonics platform with a circuit-designer-oriented PDK and packaging integration.
- **Samsung Foundry:** a 300 mm silicon-photonics platform emphasizing a path from photonic integrated circuits to co-packaged optics.

These competitive assessments and platform details are secondary-source claims and should be checked against current foundry documentation.

## Openness and portability

The source points to open, verifiable, cross-foundry compact models as a possible counterweight to closed PDK lock-in. It cites an open Verilog-A photonic model library demonstrated at up to 64 Gbps as evidence that some circuit-level modeling can be portable and inspectable.

The strategic conclusion is that mass production depends on the ability to simulate accurately before committing to fabrication. Component performance matters, but reusable, measured, variation-aware models and interoperable design flows determine how broadly silicon photonics can scale beyond vertically integrated specialists.
