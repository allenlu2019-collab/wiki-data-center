---
title: Optical Interconnect PIC, EIC, and Packaging Integration
created: 2026-08-16
updated: 2026-08-16
type: concept
tags: [silicon-photonics, optics, pdk, advanced-packaging, 3dic, hybrid-bonding, foundry]
sources: [https://research.tsmc.com/page/interconnect/30.html, https://pr.tsmc.com/english/news/3136, https://gf.com/technologies/silicon-photonics/, https://towersemi.com/2024/11/26/11262024/, https://towersemi.com/2023/03/02/03022023/]
confidence: medium
---

# Optical Interconnect PIC, EIC, and Packaging Integration

## Architecture taxonomy

PIC, EIC, and packaging are useful categories because they correspond to different physics, processes, and suppliers. They must still be designed as one link:

```text
Host ASIC/GPU
  ↕ electrical data
EIC: SerDes/DSP, driver, TIA, clock and control
  ↕ short high-speed analog interface
PIC: modulator, detector, waveguide and wavelength optics
  ↕ optical interface
Package: die bonding, laser/fiber attach, power, cooling and test
  ↕
Fiber
```

Two additional views are needed for a complete taxonomy:

- **System/form factor:** pluggable, LPO, NPO, CPO, or optical-I/O chiplet.
- **Manufacturing lifecycle:** wafer test, known-good die, assembly yield, qualification, repairability, and cost.

## PIC focus and process

The photonic integrated circuit converts electrical modulation into light, routes and filters the light, and converts received light into current.

| PIC element | Design focus |
|---|---|
| Silicon/SiN waveguides | Propagation loss, bends, crossings, polarization and process variation |
| MZM or ring modulator | Bandwidth, drive voltage, extinction ratio, insertion loss and thermal sensitivity |
| Ge photodetector | Responsivity, bandwidth, dark current, capacitance and saturation |
| CWDM/DWDM optics | Filter loss, crosstalk, wavelength stability and tuning power |
| Fiber coupler | Edge/grating coupling loss, alignment tolerance and packaging method |
| Laser interface | External laser, attached III-V die, heterogeneous integration and reliability |

A representative SOI PIC flow is:

1. Pattern full and partial silicon etches for strip/rib waveguides, couplers, filters and modulators.
2. Implant P/N regions around modulator waveguides to form carrier-depletion junctions.
3. Selectively grow and contact germanium for photodetectors.
4. Add optional SiN, heater, temperature-sensor, isolation and laser-integration modules.
5. Form RF electrodes, contacts and metal interconnect.
6. Form grating or edge couplers and prepare optical attach regions.
7. Add bumps, copper pads, TSV/TDV or hybrid-bond interfaces as required.
8. Perform optical/electrical wafer test before assembly.

The detector should normally be called a **Ge-on-Si photodetector**, not a “SiGe detector” in the SiGe-BiCMOS sense. PIC node labels are also not directly comparable with CMOS logic nodes: dimensional control, optical loss and device models matter more than transistor density.

## EIC focus and process

The electronic integrated circuit translates between the host's digital electrical domain and the PIC's high-speed analog domain.

| EIC function | Primary concern |
|---|---|
| DSP/FEC | Dense logic, coding, equalization and power |
| SerDes/clocking | Data rate, jitter, clock recovery and die-to-die protocol |
| Modulator driver | Voltage swing, current, bandwidth, linearity and impedance |
| TIA | Input noise, detector capacitance, gain, bandwidth and dynamic range |
| Photonic control | Ring/filter tuning, heaters, laser control, monitoring and calibration |

The EIC follows a conventional CMOS, RF-CMOS, or SiGe-BiCMOS flow: transistor formation, contacts, multilayer metal, thick top metal, bonding pads/bumps and wafer test. Dense DSP favors advanced CMOS, while drivers may need higher voltage and TIAs need low-noise RF devices. One process is not automatically optimal for all functions.

## Where DSP, driver and TIA can reside

The term “EIC” is ambiguous: it may mean a complete DSP/SerDes/analog die or only the analog companion to the PIC.

| Configuration | Partition | Typical use/trade-off |
|---|---|---|
| One electronic die | DSP + SerDes + driver + TIA + control on one EIC; separate PIC | Compact DSP-based pluggable, but digital heat/noise and analog voltage requirements share one process |
| Split digital and analog | Advanced-node DSP/SerDes EIC plus RF-optimized driver/TIA EIC plus PIC | Each die uses a suitable process; more dies and interfaces |
| LPO | Host switch SerDes/equalization plus linear driver/TIA in module plus PIC | Removes module DSP, not driver/TIA; lower power with tighter channel margin |
| 2.5D/3D optical engine | Small driver/TIA/control EIC beside or on PIC; DSP/SerDes elsewhere | Minimizes sensitive analog connections and suits NPO/CPO |
| Monolithic EPIC | Some driver/TIA/control transistors and photonic devices share one wafer | Very low parasitics; process compromise makes a large modern DSP less attractive on the same die |

The robust placement rule is: put the **driver close to the modulator** and the **TIA close to the photodetector**. The DSP location is more flexible because a digital die-to-die link tolerates more distance than the detector-current or modulator-drive nodes.

## Mapping to Tower, GF, and TSMC COUPE

| Platform | PIC/EIC structure | Likely electronics partition | Main differentiation |
|---|---|---|---|
| Tower PH18/TPS45PH | Strong open PIC process; separate customer-selected EICs | DSP and analog EIC may be combined or separate; driver/TIA normally external to PIC | Flexible merchant PIC, MPW/PDK access, 200/300 mm capability and heterogeneous laser options |
| GF Fotonix | Monolithic photonics plus RF/CMOS capability, with external EIC integration also supported | Driver/TIA/control may share the photonic wafer; large advanced-node DSP commonly remains external | Low analog parasitics plus flexible 2.5D/3D and turnkey integration |
| TSMC COUPE | Separate EIC stacked face-to-face on PIC using SoIC-X | Stacked EIC can contain driver/TIA/control and possibly SerDes/DSP; exact partition is product-specific | Separately optimized dies, dense vertical interface and integration with CoWoS/HPC silicon |

Laser integration is a separate dimension: Tower's heterogeneous III-V laser options, for example, do not imply that the driver/TIA is also fabricated on the PIC.

## COUPE host-to-optics path

TSMC publicly describes COUPE as an EIC stacked on a PIC using SoIC-X and subsequently integrated into a substrate or CoWoS package. Published COUPE diagrams include through-die-via structures in the PIC. A plausible COUPE-on-interposer transmit path is:

```text
GPU/XPU I/O PHY
  → CoWoS interposer metal
  → COUPE package connection
  → PIC metal and through-die via (TDV)
  → SoIC-X copper bond
  → EIC SerDes/driver/control
  → nearby SoIC-X bond
  → PIC modulator
  → optical fiber
```

The receive path reverses the domains:

```text
Fiber → PIC Ge detector → SoIC-X → EIC TIA/receiver
      → PIC electrical feedthrough/TDV → interposer → GPU/XPU
```

The PIC acts both as a functional optical die and as an electrical feedthrough to the EIC above it. The apparent “up to the EIC and back down to the modulator” path remains short because the SoIC-X connections are dense vertical copper bonds. TSMC reports reduced EIC–PIC parasitics versus conventional micro-bump integration.

COUPE does not mandate one GPU protocol. The host-to-EIC interface might be proprietary short-reach I/O, UCIe-derived streaming, or another die-to-die SerDes. Likewise, public descriptions of an “electrical control die” do not prove that every implementation contains a full optical DSP. Exact routing, protocol and EIC contents remain product-specific.

## Why EIC-on-PIC instead of PIC-on-EIC?

Reversing the stack is technically possible, but it usually moves rather than eliminates the vertical-interconnect requirement:

```text
EIC-on-PIC:  EIC ⇅ SoIC ⇅ PIC/TDV ⇅ interposer
PIC-on-EIC:  PIC ⇅ SoIC ⇅ EIC/TSV or backside RDL ⇅ interposer
```

Reasons a PIC can be an effective base die include:

- Passive waveguides, filters and couplers often make the PIC larger than the active EIC tile.
- Small EICs can be placed only above modulator/detector arrays while leaving fiber-coupling regions exposed.
- The top EIC can have a more direct path to a heat spreader; placing a temperature-sensitive PIC above it could put optics in the EIC heat path.
- A tested small EIC die can be bonded onto a larger PIC wafer in a chip-on-wafer flow.
- PIC TDVs provide a regular bottom interface for host signals, power, control and test.

PIC-on-EIC may be attractive when the EIC/host die is larger, the PIC is a small top-facing optical tile, backside EIC power/I/O already exists, or fan-out routing provides efficient escape. Neither orientation is universally superior.

The specific TSMC floorplan, power network and product routing rules are not fully public. The explanation above distinguishes published COUPE elements—EIC-on-PIC, SoIC-X, TDV structures and CoWoS integration—from the inferred system-level signal path.

## Integration interfaces that dominate results

| Interface | Critical issues |
|---|---|
| Host ↔ EIC | Protocol, SerDes energy, interposer loss, clocking and bandwidth density |
| EIC ↔ PIC | Bond pitch, capacitance, impedance, power delivery and thermal coupling |
| PIC ↔ laser | Coupling loss, wavelength stability, lifetime and replaceability |
| PIC ↔ fiber | Alignment, connector pitch, assembly time and field reliability |
| Package ↔ cooling | EIC heat removal without disturbing rings, filters or laser wavelength |
| Test ↔ assembly | Wafer test, known-good die, optical test coverage, yield and repair strategy |

Related: [[interconnect/concepts/silicon-photonics-pdk-and-foundry-ecosystem]], [[interconnect/comparisons/silicon-photonics-foundry-positioning]], [[interconnect/comparisons/silicon-photonics-vs-cmos-pdk]], and [[shared/concepts/3dic-bonding-and-tsvs]].
