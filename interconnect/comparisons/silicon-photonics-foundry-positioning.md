---
title: Silicon Photonics Foundry Positioning
created: 2026-08-16
updated: 2026-08-16
type: comparison
tags: [silicon-photonics, optics, foundry, pdk, advanced-packaging, 3dic, comparison, vendor]
sources: [https://pr.tsmc.com/english/news/3136, https://investor.tsmc.com/static/annualReports/2025/english/index.html, https://towersemi.com/2024/11/26/11262024/, https://ir.towersemi.com/news-releases/news-release-details/tower-semiconductor-signs-customer-contracts-13-billion-silicon/, https://ir.towersemi.com/news-releases/news-release-details/tower-semiconductor-meti-support-announces-strategic-capacity, https://gf.com/technologies/silicon-photonics/, https://gf.com/gf-press-release/globalfoundries-acquires-advanced-micro-foundry-accelerating-silicon-photonics-global-leadership-and-expanding-ai-infrastructure-portfolio/, https://investors.gf.com/news-releases/news-release-details/globalfoundries-signs-letter-intent-us-department-commerce-300]
confidence: medium
---

# Silicon Photonics Foundry Positioning

## Question

How do TSMC, Tower Semiconductor, GlobalFoundries (GF), and SMIC differ in silicon-photonics manufacturing capability, integration scope, market position, and opportunity?

This comparison began from multiple AI-generated analyses supplied by the user. Those analyses are treated as claim leads, not evidence. Statements below are limited to what could be supported by primary company materials as of 2026-08-16. Absence of public evidence is not proof that private development does not exist.

## Correct system decomposition

| Layer | Typical contents | Manufacturing issue |
|---|---|---|
| PIC | Silicon/SiN waveguides, couplers, filters, Mach–Zehnder or ring modulators, Ge photodetectors, heaters | Optical loss, phase control, lithographic variation, fiber coupling, laser integration |
| EIC | Driver, TIA, clocking, control, SerDes and sometimes DSP/switch logic | CMOS node, analog/RF bandwidth, power, signal integrity |
| Package | PIC–EIC attachment, laser/fiber attach, electrical escape, thermal path, test and repair strategy | Known-good die, alignment, hybrid bonding, yield, serviceability |
| Host system | Switch ASIC, accelerator, HBM and optical engines | CPO/NPO architecture, bandwidth density, power and system availability |

The detector is normally **germanium (Ge) integrated on silicon**, not a “SiGe detector” in the same sense as a SiGe BiCMOS transistor platform. Drivers and TIAs are normally EIC functions, although a monolithic electronic-photonic process can place some electronics on the PIC wafer.

## Verified positioning

| Company | Verified public platform | Best-supported positioning | Principal opportunity | Evidence boundary |
|---|---|---|---|---|
| TSMC | COUPE using SoIC-X to stack EIC and PIC, then co-package with HPC silicon | System-level heterogeneous integration combining photonics, leading logic, SoIC and CoWoS | CPO and optical I/O located close to AI accelerators and switch ASICs | Public material supports the architecture and roadmap direction, but not many precise customer, yield, wafer-capacity or bandwidth claims in the supplied AI text |
| Tower | High-volume 200 mm PH18 plus standard 300 mm SiPh offering; PH18DA and TPS45PH variants | Open specialty foundry serving pluggables, optical engines, OCS, sensing and later CPO | Scale existing merchant-foundry business while adding 300 mm capacity, integrated lasers and optical packaging | Tower supplies unusually concrete commercial and capacity disclosures, but customer-specific volumes still require named primary announcements |
| GlobalFoundries | 300 mm GF Fotonix plus acquired AMF 200 mm capability; SCALE CPO platform | Broad merchant SiPh platform spanning monolithic electronic-photonic integration, discrete/stacked EICs, packaging and multiple geographies | Pluggables, NPO/CPO, US-based supply and expanded Asian capacity | GF's “largest pure-play SiPh foundry by revenue” is GF's own post-acquisition characterization, not an independently audited market ranking |
| SMIC | No comparable public SiPh platform located in reviewed SMIC primary material | Public evidence is insufficient to rank SMIC alongside the three established platforms | Potential future China-local manufacturing role if SMIC publishes a qualified process, PDK, MPW or production program | Do not convert China's broader SiPh research/foundry activity into an SMIC capability claim without direct evidence |

## TSMC: integration-led strategy

TSMC's differentiator is not established superiority of every photonic device. COUPE stacks an electrical control die on a photonic die using SoIC-X, minimizing the EIC–PIC electrical path, and is intended to be co-packaged with HPC silicon. This makes the competitive unit a qualified **logic + HBM + photonic-engine package**, not the PIC wafer alone.

TSMC's published 2024 roadmap targeted small-form-factor pluggable qualification in 2025 and CoWoS CPO integration in 2026. The 2025 annual report continued to describe COUPE as under development for low-power, high-speed optical interconnect. Schedule language should therefore be tracked as qualification/development status, not silently converted into confirmed mass production.

**Supported moat:** leading logic, SoIC, CoWoS, customer co-design, and system-level yield learning.

**Open questions:** available PDK/customer access, photonic process details, laser strategy, fiber attach, test coverage, repair model, actual production qualification, and economics relative to external optical engines.

## Tower: open specialty foundry with disclosed demand

Tower describes PH18 as a mature, high-volume 200 mm SiPh platform and released a 300 mm SiPh process as a standard foundry offering in November 2024. Its portfolio includes modulators, photodetectors, passive devices, low-loss silicon-nitride options, and variants with heterogeneous III-V lasers. MPW access and a multi-fab specialty-foundry model make Tower structurally accessible to fabless PIC companies.

Tower disclosed in May 2026 that it had signed contracts representing US$1.3 billion of 2027 SiPh revenue, received US$290 million in capacity-reservation prepayments, and served more than 50 active SiPh customers. In July 2026 it announced additional Japanese 300 mm SiPh and advanced-packaging capacity, with the first expansion targeted for production readiness in Q4 2027. These are meaningful demand signals, but future revenue and readiness dates remain forward-looking.

**Supported moat:** mature open process, PDK/MPW access, device breadth, customer diversity, and committed capacity expansion.

**Open questions:** how its PIC/EIC/package integration yield and economics compare with TSMC COUPE and GF's evolving turnkey platforms at CPO scale.

## GlobalFoundries: platform breadth and acquisition-led scale

GF Fotonix combines photonic devices with RF-CMOS capability on 300 mm wafers and supports external advanced-node EIC integration through TSVs and 2.5D/3D packaging. GF publicly describes production-proven 100G/λ capability, demonstrated 200G/λ, and a path toward 400G/λ, with CWDM/DWDM options.

GF completed its acquisition of Singapore's Advanced Micro Foundry in November 2025. GF says the combination made it the largest pure-play SiPh foundry by revenue, adding AMF's established 200 mm process, customers, IP and capacity, with a possible transition toward 300 mm as demand grows. Treat the ranking as a vendor claim while treating the acquisition and manufacturing footprint as established facts.

In July 2026 GF signed a letter of intent—not a final award—with the US Department of Commerce for an expected US$300 million CHIPS R&D incentive. The stated scope includes photonic materials, wafer technology, advanced packaging and 3D hybrid bonding. GF's SCALE platform targets modular 400 Gb/s optical engines and claims a fivefold efficiency improvement over current implementations; those performance targets require workload and baseline details before independent comparison.

**Supported moat:** mature 300 mm SiPh, monolithic RF/electronic options, acquired AMF capacity, geographic diversity, and growing packaging/design support.

**Open questions:** AMF integration, 200-to-300 mm migration, customer portability between processes, and production qualification of SCALE/NPO/CPO offerings.

## SMIC: evidence-constrained assessment

The reviewed AI analyses alternately described SMIC as possessing SiPh capability or as only an indirect participant in China's SiPh ecosystem. A search of accessible SMIC primary material did not establish a named, customer-accessible SiPh process comparable with PH18, Fotonix or COUPE.

The defensible conclusion is therefore **not enough public evidence**, rather than a two-star technology score or a categorical claim of technical incapability. Evidence needed to upgrade the assessment includes an official process name, PDK or MPW access, supported device library, wafer size, qualification data, production customers, packaging flow, and manufacturing capacity.

Chinese research institutes and other foundries may have SiPh programs, but their capability must not be attributed to SMIC by geographic association. Export-control effects on specific Ge epitaxy, SOI, etch or lithography steps also require direct equipment/process evidence rather than inference.

## Claim audit of the supplied AI material

| Supplied claim | Assessment |
|---|---|
| “Tower = PIC; GF = PIC + some EIC; TSMC = PIC + EIC + packaging” | Useful shorthand but incomplete. Tower also has heterogeneous lasers, 300 mm SiPh and packaging expansion; GF supports monolithic electronics and advanced packaging; all architectures require external ecosystem participation. |
| TSMC COUPE uses SoIC-X for stacked EIC/PIC and can enter CoWoS | Supported by TSMC. |
| TSMC uses a specific 65 nm PIC node, has >99% 3D yield, 100 Tb/s I/O, named NVIDIA/Broadcom/AMD adoption, or the quoted monthly wafer ramps | Not established by the reviewed TSMC primary sources; omit pending direct evidence. |
| Tower PH18 is a mature high-volume open platform and Tower offers 300 mm SiPh | Supported by Tower. |
| Tower has US$1.3 billion of 2027 SiPh contracts and US$290 million prepayments | Supported by Tower's May 2026 investor release; still forward-looking commercially. |
| Tower targets more than five times Q4-2025 SiPh shipment capacity | Supported as a Tower target, not completed capacity. |
| GF acquired AMF and calls the result the largest pure-play SiPh foundry by revenue | Supported as acquisition fact plus GF's own ranking claim. |
| GF has a finalized US$300 million government award | Incorrectly stated: the July 2026 announcement is a letter of intent for an expected award. |
| GF Gen1/Gen2/Gen3 correspond to 100/200/400G per wavelength | Supported as production capability, demonstration, and roadmap respectively; these maturity levels are not equivalent. |
| SMIC is already an open commercial SiPh foundry comparable with Tower/GF | Not established from reviewed SMIC primary sources. |
| Broader Chinese SiPh platforms prove SMIC capability | Invalid attribution without an explicit SMIC relationship. |

## Strategic synthesis

- **Tower and GF compete most directly today** for open SiPh production, though their process and integration choices differ.
- **TSMC competes at a broader system boundary:** photonic-engine integration with leading logic, HBM and advanced packaging.
- **Packaging is becoming part of the foundry moat:** EIC–PIC bonding, laser and fiber attach, thermal design, known-good-die strategy, test, yield and repairability can dominate device-level advantages.
- **SMIC should remain “unverified/emerging” in this comparison** until a primary-source commercial platform is disclosed.
- Vendor roadmaps, contracts, capacity targets and efficiency numbers must remain labeled as company claims until realized or independently measured.

Related: [[interconnect/concepts/optical-interconnect-pic-eic-packaging-integration]], [[interconnect/concepts/silicon-photonics-pdk-and-foundry-ecosystem]], [[interconnect/comparisons/silicon-photonics-vs-cmos-pdk]], [[shared/concepts/3dic-bonding-and-tsvs]], and [[interconnect/concepts/ethernet-speeds-and-standards]].
