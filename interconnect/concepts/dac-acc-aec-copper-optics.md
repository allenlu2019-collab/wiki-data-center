---
title: DAC, ACC, AEC — Copper Interconnects
created: 2026-07-26
updated: 2026-07-26
type: comparison
tags: [copper, interconnect, ethernet, distance, optics, comparison]
sources: [raw/articles/optics.md]
---

# DAC, ACC, AEC — Copper Interconnects and the Optics Boundary

Copper cabling remains the dominant interconnect for short-reach links (within-rack and adjacent-rack). As speeds increase, the gap between copper's reach limits and optical's cost floor narrows, creating a dynamic tradeoff at each Ethernet generation.

## The Copper Triad

| Type | Full name | Active? | Reach (400G) | Reach (800G) | Power | Cost | Best use |
|------|-----------|---------|-------------|-------------|-------|------|----------|
| **DAC** | Direct Attach Copper | Passive | 2-3m | 1-2m | 0W | $ | TOR-to-server, short spine-leaf |
| **ACC** | Active Copper Cable | Active retimer | 3-5m | 2-3m | ~1-2W | $$ | Medium rack runs, some inter-rack |
| **AEC** | Active Electrical Cable | Active equalization + retiming | 5-7m | 3-5m | ~3-5W | $$$ | Edge cases — bridge between copper and optics |

## The Physics Problem

Copper's limitation is the **skin effect** and **dielectric loss** at high frequencies:
- At 53 Gbps (PAM4 signaling, 26.5 GHz fundamental): skin depth in copper is ~0.4 μm
- Signal amplitude drops exponentially with cable length
- PAM4 makes it worse: 4-level signaling has 3x smaller eye opening than NRZ at same baud rate

Result: **Every Ethernet generation halves copper reach**. At 1.6T (200 Gbps per lane), even AEC struggles past 3m.

## When to Switch to Optics

| Speed | DAC max length | Recommend optics above |
|-------|---------------|----------------------|
| 25G | 7m | 5m+ (cost crossover near 7m) |
| 100G | 5m | 3m+ |
| 400G | 3m | 2m+ |
| 800G | 2m | 1.5m+ |
| 1.6T | ~1.5m | 1m+ |

For AI clusters with **pod-scale fabric** (tor-to-leaf = 3-5m cables), 400G+ clusters are increasingly all-optical within the pod. Copper is relegated to in-rack (TOR-to-server).

## Cable Comparison Table

| Property | DAC (passive) | ACC | AEC | SR4 optics | DR4 optics |
|----------|--------------|-----|-----|-----------|-----------|
| Max reach (400G) | 3m | 5m | 7m | 100m | 500m |
| Power (per end) | 0W | 1-2W | 2-5W | ~10W (total) | ~12W (total) |
| Diameter (400G) | ~8mm (thick, stiff) | ~6mm | ~5mm | ~3mm (flexible) | ~3mm |
| Bend radius | 40mm (poor) | 30mm | 25mm | 10mm (good) | 10mm |
| Weight | Heavy | Moderate | Moderate | Light | Light |
| Reliability | Excellent | Good | Good | Good | Good |
| Cost per cable (3m) | ~$50 | ~$100 | ~$150-200 | ~$300 | ~$500 |
| Failure mode | Physical damage | Retimer IC failure | Retimer IC failure | Laser failure | Laser failure |

## Practical Decision Framework

```
Server distance from TOR switch:
  < 2m  → DAC (cheapest, zero power, reliable)
  2-5m  → ACC or AEC (depends on speed and budget)
  > 5m  → Optics (SR up to 100m, DR up to 500m)
```

### AI Cluster Specifics
- In DGX H100 racks: GPU-to-TOR = ~1-2m → **DAC** works at 400G NDR IB
- Between adjacent racks (to leaf switch): 3-5m → **AEC** or **SR4 optics**
- Within pod (to spine): 10-50m → **SR4 optics**
- Inter-pod: 100-500m → **DR or FR optics**
- Multi-campus: 2-120km → **FR/LR or ZR optics**

### Co-Packaged Optics Impact
CPO replaces the pluggable optic module with on-package optical engines. This changes the calculus:
- No pluggable module thermal/power budget
- Optical directly from switch ASIC → break-out to fiber at rack faceplate
- Reduces power by 30-50% for optical links
- Makes 800G+ optical feasible for shorter reaches
- Broadcom, Cisco, Marvell all sampling CPO switches in 2025-2026

### Linear Signal Conditioning

Linear pluggable optics and analog active copper reduce or avoid full module-side DSP/retimer functions. They can lower power and latency when the host SerDes and channel are sufficiently controlled, but they tighten system-level linearity, noise, interoperability, and temperature margins. [[interconnect/entities/semtech-signal-integrity]] is one component-vendor example.

## Related Pages
- [[interconnect/entities/semtech-signal-integrity]] — linear optical PMDs and copper equalizers
- [[interconnect/concepts/ethernet-speeds-and-standards]] — speed standards with reach details
- [[interconnect/concepts/infiniband-vs-ethernet]] — fabric that uses these cables
- [[interconnect/concepts/nvlink-and-nvswitch]] — GPU interconnect (mostly PCB, not cables)
- [[passive/concepts/pcb-ccl-signal-integrity]] — laminate, copper roughness, and glass-weave limits inside board-level channels
- [[interconnect/concepts/ethernet-speeds-and-standards]] — optical reach standards
