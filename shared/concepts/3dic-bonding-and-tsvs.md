---
title: 3DIC Bonding, Stack Orientation, and TSVs
created: 2026-07-26
updated: 2026-07-26
type: concept
tags: [hbm, gpu, accelerator, standard]
sources: [raw/articles/3dic-bonding.md]
---

# 3DIC Bonding, Stack Orientation, and TSVs

3D integration joins dies or wafers vertically to shorten interconnects and raise bandwidth density. The two principal connection approaches are micro-bumps and hybrid bonding. They solve the same vertical-connectivity problem at different pitches and manufacturing complexity.

## Micro-bumps vs Hybrid Bonding

| Dimension | Micro-bumps | Hybrid bonding |
|---|---|---|
| Electrical joint | Copper pillar plus solder | Direct copper-to-copper pads |
| Mechanical joint | Bumps plus underfill | Bonded dielectric surfaces |
| Surface | Discrete raised joints | Planarized, nearly gapless interface |
| Typical pitch regime | Roughly 10–50 μm | Sub-10 μm, with a path below 1 μm |
| Main strengths | Mature process, lower cost, established supply chain | Much higher interconnect density, lower parasitics, thinner stack |
| Main risks | Solder bridging, underfill stress, pitch scaling limit | Particle sensitivity, alignment, surface planarity, yield |
| Representative uses | HBM stacks, many chiplet packages | AMD 3D V-Cache, CMOS image sensors, emerging 3D logic/memory |

Hybrid bonding is “hybrid” because two bonds form on the same plane: dielectric-to-dielectric bonding supplies mechanical integrity, while copper-to-copper diffusion supplies electrical continuity.

## Face Orientation

- **Face-to-face (F2F):** two active surfaces meet. One die is flipped relative to the other; a two-die stack can avoid a TSV in the upper die when no connection is needed on its backside.
- **Face-to-back (F2B):** one active surface connects to a thinned and metallized backside. This is the repeatable orientation used for taller stacks.
- “Face-down” describes placement, not the bonding technology: both micro-bump and hybrid-bonded assemblies can use flipped dies.

## When TSVs Are Required

For a simple two-die F2F stack, direct bonding can connect the two active faces without traversing the upper die. In a stack of three or more dies, each intermediate die must relay signals between interfaces on opposite sides; TSVs or an equivalent vertical conductor are therefore required through those intermediate layers. The top die normally needs no pass-through TSV unless the package architecture requires a backside connection.

HBM uses TSVs through DRAM dies and micro-bumps between layers. Hybrid bonding can shrink the pad interface, but it does not remove the need for vertical routing through intermediate silicon.

## System Implications

- Finer pitch raises die-to-die bandwidth density and reduces energy per bit.
- Removing solder and underfill reduces interface height and electrical parasitics.
- Yield becomes more sensitive to particles, wafer bow, planarity, and alignment.
- Thermal behavior improves at the bond interface, but taller stacks still create vertical heat-removal and thermo-mechanical challenges.

## Related Pages

- [[memory/concepts/hbm-memory-architecture]] — TSV-based stacked memory
- [[thermal/concepts/microchannel-liquid-cooling]] — package-level heat extraction
- [[compute/concepts/gpu-architecture]] — accelerators using 2.5D/3D packaging

