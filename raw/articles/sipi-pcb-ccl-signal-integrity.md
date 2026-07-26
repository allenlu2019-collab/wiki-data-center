---
source_url: https://sipitogether.blog/signal-integrity/pcb-ccl-si/
title: "800G to 1.6T: How PCB CCL Materials Affect Signal Integrity and Market Direction"
author: SI/PI Frontier (柑仔店)
published: 2025-02-28
ingested: 2026-07-26
content_mode: summarized
sha256: fb95d4a8a5a2fa3771eb3b52461ee152041b36a93c1b44da6cda11e81967bfe7
---

# PCB CCL Materials and Signal Integrity

This source explains how copper-clad laminate (CCL) selection constrains high-speed PCB channel loss, routing reach, and the transition from 400G and 800G systems toward 1.6T switching and 224G PAM4.

## Channel-design context

The article places CCL selection inside the broader SerDes channel-design workflow:

- select the resin system, copper foil, and glass-fabric reinforcement;
- choose core and prepreg thicknesses;
- select the glass weave;
- set differential impedance through trace width and spacing; and
- determine the total PCB layer count.

## CCL composition

CCL is described as four interacting material systems:

- **Resin:** bonds the laminate and provides electrical insulation.
- **Filler:** tunes mechanical strength and coefficient of thermal expansion (CTE), while contributing strongly to dissipation factor (Df).
- **Copper foil:** forms the conductor; surface roughness raises high-frequency conductor loss.
- **Glass fabric:** provides mechanical reinforcement; lower-Dk/Df fabrics reduce dielectric loss.

## Loss-class framework

The source cautions that suppliers, cloud providers, and chip vendors use inconsistent loss-class names. It offers this approximate mapping:

| Class | Approximate Df | Representative application | Example reference class |
|---|---:|---|---|
| Low Loss (LL) | 0.010–0.007 | Up to 10G, below PCIe Gen3 | Panasonic M4 |
| Very Low Loss (VLL) | 0.007–0.005 | 10–25G, PCIe Gen4, 100G CAUI-4 | Panasonic M5 |
| Super Low Loss (SLL) | 0.005–0.003 | 32G NRZ, 56–112G PAM4, PCIe Gen5/6, 400G Ethernet | Panasonic M6 |
| Ultra Low Loss (ULL) | 0.003–0.002 | 112G PAM4 and 800G Ethernet | Panasonic M7 |
| Extreme Low Loss (ELL) | <0.002 | 224G PAM4 and 800G/1.6T Ethernet | Panasonic M8 |

Named examples include EMC, ITEQ, TUC, and Panasonic products. These ranges are presented as industry heuristics rather than universal standards.

## Copper-foil roughness

Beyond resin improvements, smoother copper is presented as a major lever for reducing channel loss. IPC standard terminology reaches VLP, while vendors use non-standard HVLP generations above it. The article warns that HVLP4/HVLP5 labels are not directly comparable across suppliers.

- **Ra** measures average surface-height variation and may hide extreme peaks and valleys.
- **Rz** emphasizes peak-to-valley height and is often more useful for modeling high-speed copper loss.
- The Huray roughness model is cited as a practical SI model using roughness-related parameters.
- Roughness penalties increase with frequency; the source highlights a widening HVLP3-versus-HVLP4 loss difference near 53 GHz.

## Glass-fabric generations

The source organizes reinforcement fabrics as E-glass, first-generation low-Dk, second-generation low-Dk, and quartz fabric.

- **E-glass:** higher Df; generally associated with PCIe Gen4 and earlier or other moderate-speed channels.
- **Low-Dk generation 1:** associated with the 400G era and about 0.7 dB/in at 13.28 GHz in the cited design context.
- **Low-Dk generation 2:** associated with 800G and 112G/224G PAM4; the source cites about 0.85 dB/in at 26.56 GHz and describes adoption in high-end data-center switch boards from 2024.
- **Quartz fabric:** projected to reduce loss another 10–15% relative to second-generation low-Dk fabric, with improved CTE stability, but constrained by supply, UL qualification, and cost/scale.

## 1.6T and 224G PAM4 implications

The article argues that second-generation low-Dk fabric can support some 1.6T optical-transceiver board designs but leaves little loss budget for longer copper links or DAC use at 224G PAM4. It presents quartz fabric as a potential enabler for broader 1.6T and future 3.2T designs, while forecasting that meaningful scale may not arrive until 2026 Q3–2027 Q2.

These roadmap dates, product examples, and performance figures are source claims and should be corroborated before use as procurement or design specifications.
