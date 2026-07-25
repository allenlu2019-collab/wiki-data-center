---
title: Liquid Cooling Technologies
created: 2026-07-26
updated: 2026-07-26
type: concept
tags: [liquid, cooling, thermal, cdu, rack, gpu]
sources: [raw/articles/gpu-cooling.md]
---

# Liquid Cooling Technologies

Liquid cooling has transitioned from niche HPC to standard requirement in AI data centers. As rack density reaches 100-300 kW, air cooling is physically and economically infeasible.

## Cooling Density Tiers

| Tier | kW per rack | Cooling technology | GPU generation |
|------|-------------|-------------------|----------------|
| 1 | <15 kW | Air (CRAC/CRAH + contained aisle) | Pre-AI |
| 2 | 15-40 kW | Air + rear-door heat exchanger | A100 |
| 3 | 40-80 kW | Direct-to-chip liquid (cold plate) | H100 |
| 4 | 80-150 kW | Direct-to-chip + liquid-to-liquid | B200 |
| 5 | 150-300 kW | Direct-to-chip + immersion | Rubin-era |

## Direct-to-Chip (Cold Plate) Liquid Cooling

The dominant approach for current AI clusters (H100, B200 generation):

### How It Works
1. **Cold plate** — copper or aluminum plate with microchannel fins, mounted directly on GPU/CPU die
2. **Coolant** — deionized water + glycol (25-35% glycol) or dielectric fluid
3. **CDU (Coolant Distribution Unit)** — pumps coolant, rejects heat to facility water loop, regulates temperature
4. **Facility water loop** — cooling tower, chiller, or heat reuse to building HVAC

### Key Parameters
- **Supply temperature**: 25-45°C (depends on GPU TDP and CDU)
- **ΔT (temperature rise across cold plate)**: 8-15°C
- **Coolant flow rate**: ~2-4 L/min per GPU (H100), ~4-6 L/min (B200)
- **Pumping power**: ~3-5% of IT load
- **PUE impact**: Direct-to-chip reduces PUE from 1.3-1.4 (air) to 1.05-1.15

### Cold Plate Design Variants
| Type | Description | Thermal performance | Cost |
|------|-------------|-------------------|------|
| Standard copper | Milled copper block, straight microchannels | ~0.1°C·cm²/W | $ |
| Jet impingement | Coolant jets directly onto die surface | ~0.05°C·cm²/W | $$ |
| 3D vapor chamber | Wick structure + phase change inside cold plate | ~0.03°C·cm²/W | $$$ |
| Hybrid (cold plate + rear door) | Cold plate for high-power cpts, rear-door for rest | Balanced | $$ |

Microchannel lids move coolant distribution closer to the package and introduce additional sealing, pressure-drop, chemical-compatibility, and thermo-mechanical constraints. See [[thermal/concepts/microchannel-liquid-cooling]].

## Immersion Cooling

### Single-Phase Immersion
- **Dielectric fluid** (e.g., 3M Novec, Engineered Fluids) — non-conductive
- Servers fully submerged in fluid bath
- Fluid pumped through heat exchanger to facility loop
- **Pros**: Complete thermal coverage, no moving parts in fluid, silent
- **Cons**: Server service is messy (lift, drain, dry), fluid cost (~$50-100/gal), fluid aging
- **Typical**: 50-100 kW per tank, tanks side-by-side in rows

### Two-Phase Immersion
- Dielectric fluid boils at ~50-60°C
- Vapor rises, condenses on water-cooled condenser coils in the tank
- **Pros**: Very high heat transfer coefficient, passive circulation (no pumps in tank)
- **Cons**: Fluid loss (vapor escapes on open), higher fluid cost (Novec 7000), condensation management
- **Typical**: 80-150 kW per tank
- **Known issue**: 3M discontinued Novec production (2022), alternatives from Engineered Fluids and others

## Rear-Door Heat Exchangers

- Passive or active cooling coil mounted on server rack's rear door
- Captures exhaust air, reduces CRAH load
- **Typical capacity**: 10-35 kW per rack door (40-55°C water inlet)
- **Use case**: Retrofitting existing air-cooled DC for higher density (bridge to liquid)
- **Not sufficient** for 80+ kW racks alone

## Coolant Distribution Units (CDUs)

The CDU is the heart of any liquid-cooled system:

### Typical CDU Specs (NVIDIA / CoolIT / Boyd)
| Class | Capacity | Pumps | Racks supported | Footprint | 
|-------|----------|-------|----------------|-----------|
| Small | 50-100 kW | 1+1 redundant | 1-2 | 4U |
| Medium | 200-500 kW | 2+1 | 4-8 | 4U-8U |
| Large | 500-1000 kW | 3+1 | 10-20 | 10U-15U |
| Pod-scale | 1-5 MW | N+1 | 40-100 | Sub-floor / mezzanine |

### CDU Selection Criteria
- **ΔT range** — wider ΔT = less flow = smaller piping
- **Pump redundancy** — N+1 or 2N critical for availability
- **Leak detection** — humidity sensors, flow sensors, pressure drop monitoring
- **Coolant quality** — conductivity <0.5 μS/cm, pH 7-8.5, particle size <50 μm
- **Material compatibility** — avoid galvanic corrosion (Cu + brass + stainless = OK, Al + Cu = galvanic cell)

## Cooling in Superpod Architecture

A typical DGX SuperPOD (e.g., H100 cluster):

```
Rack (8× H100 DGX = ~80 kW) → CDU (per 4-8 racks) → Facility water loop
                                         ↓
                              Cooling tower or
                              Chiller plant (20-30 MW for 10k GPU cluster)
```

At superpod scale, the cooling architecture becomes a significant design driver:
- **Piping** — large diameter (6-8") supply and return running under the data center
- **Pump power** — 3-5 MW pump power for a 50 MW facility
- **Redundancy** — N+1 pumps, chillers, cooling towers
- **Water usage** — evaporative cooling requires ~5-10 gal/MWh. Regulations increasingly restrict water-based cooling.

## Heat Reuse

The 20-30 MW of waste heat from a large cluster can be repurposed:
- **District heating** — 50-70°C return water heats nearby buildings (Meta in Odense, Denmark)
- **Greenhouse / agriculture** — lower temp (30-40°C) for greenhouse heating
- **Absorption chillers** — waste heat drives cooling for other parts of the facility
- **Economics**: heat reuse only viable if DC is colocated with heat demand (district heating network within ~1 km)

## Related Pages
- [[thermal/concepts/microchannel-liquid-cooling]] — package-integrated channels, manifolds, and mechanical risks
- [[compute/concepts/gpu-architecture]] — heat source
- [[power/concepts/dc-power-distribution-architectures]] — power and cooling are coupled
- [[rack-pod/concepts/pod-and-superpod-architectures]] — cooling at pod scale
