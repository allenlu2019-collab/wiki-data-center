---
title: PCB CCL Materials and Signal Integrity
created: 2026-07-26
updated: 2026-07-26
type: concept
tags: [pcb, ccl, copper, ethernet, pcie]
sources: [raw/articles/sipi-pcb-ccl-signal-integrity.md]
confidence: medium
---

# PCB CCL Materials and Signal Integrity

Copper-clad laminate (CCL) is the material platform from which rigid printed circuit boards are fabricated. At high SerDes rates, the laminate is part of the electrical channel: its dielectric loss, copper-foil roughness, glass weave, and stack-up determine insertion loss, routing reach, impedance control, and ultimately the equalization burden.

## Material stack

| Constituent | Primary role | Signal-integrity impact |
|---|---|---|
| Resin | Binds and insulates the laminate | Resin chemistry and fillers set much of the dielectric constant (Dk) and dissipation factor (Df) |
| Filler | Tunes strength, CTE, and process behavior | Changes Df and can affect dimensional and thermal stability |
| Copper foil | Conductive path | Roughness increases effective current path and conductor loss as skin depth shrinks |
| Glass fabric | Mechanical reinforcement | Weave geometry and glass composition affect Dk/Df, skew, and local impedance |

CCL selection therefore cannot be separated from core/prepreg thickness, differential-pair geometry, via design, layer count, connector loss, and package escape routing.

## Loss classes

Supplier labels are not universal, but the source provides a useful directional ladder:

| Informal class | Approximate Df | Typical high-speed context |
|---|---:|---|
| LL | 0.010–0.007 | Up to 10G and pre-PCIe Gen3 |
| VLL | 0.007–0.005 | PCIe Gen4 and 25G-class signaling |
| SLL | 0.005–0.003 | PCIe Gen5/6, 56–112G PAM4, 400G Ethernet |
| ULL | 0.003–0.002 | 112G PAM4 and 800G Ethernet |
| ELL | <0.002 | 224G PAM4 and 800G/1.6T Ethernet |

These values are screening heuristics, not a substitute for frequency-specific laminate data, copper-profile parameters, stack-up modeling, and coupon measurement. Vendor class names such as M4–M8 or HVLP generations should not be assumed equivalent across suppliers.

## Copper roughness

At high frequency, skin effect confines current near the copper surface. Surface texture lengthens and perturbs the current path, adding loss beyond smooth-conductor models.

- Ra is an average-height metric and may underrepresent extreme surface features.
- Rz captures peak-to-valley behavior and is often more relevant to roughness modeling.
- Physics-based models such as Huray should be parameterized using supplier or measured foil data.
- The value of smoother foil rises as Nyquist frequency and routed distance increase.

Low-profile copper must still meet adhesion and fabrication requirements, so material choice is a system tradeoff rather than a single-metric optimization.

## Glass fabric evolution

Traditional E-glass provides economical reinforcement but carries higher dielectric loss. Low-Dk glass generations reduce Df for 400G and 800G switch boards. Quartz fabric offers a further path toward lower loss and more stable CTE for 224G PAM4, but supply capacity, qualification, manufacturability, and cost remain gating factors.

Glass weave also creates local dielectric variation. Routing geometry and weave choice must therefore address both average insertion loss and intra-pair skew.

## System implications

As links move from 112G to 224G PAM4, PCB loss consumes a larger share of the channel budget. Better CCL can preserve routing reach, reduce equalizer or retimer requirements, and delay conversion from board-level copper to optics. It cannot eliminate losses from packages, vias, connectors, cables, and discontinuities.

For AI switches and accelerator systems, this creates direct coupling between board materials and the architectural boundary described in [[interconnect/concepts/dac-acc-aec-copper-optics]]. Shorter PCB paths and higher-grade laminates can keep copper viable locally; longer reaches increasingly require active copper, retimers, or optics. The relevant interface generations are summarized in [[interconnect/concepts/ethernet-speeds-and-standards]] and [[interconnect/concepts/nvlink-and-nvswitch]].

## Open questions

- How quickly can quartz fabric achieve qualified volume and acceptable cost?
- Which supplier loss classes remain comparable after accounting for test method and frequency?
- At 224G PAM4, what combinations of CCL, copper profile, vias, and equalization preserve useful passive-copper reach?
- How should board-level CCL choices be co-optimized with package substrates and chiplet escape routing?

## Related concepts

- [[interconnect/concepts/dac-acc-aec-copper-optics]] — copper-reach and optics boundary
- [[interconnect/concepts/ethernet-speeds-and-standards]] — Ethernet lane-rate roadmap
- [[interconnect/concepts/nvlink-and-nvswitch]] — accelerator fabrics using short board-level channels
