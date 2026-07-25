---
title: DC Power Distribution Architectures
created: 2026-07-26
updated: 2026-07-26
type: concept
tags: [power, 48v, 400v, 800v, distribution, efficiency, psu, ups, standard]
sources: []
---

# Data Center Power Distribution Architectures

Modern data centers distribute power at multiple voltage levels — from utility feed to individual silicon. The choice of distribution voltage (48V, 400V, or 800V) defines efficiency, density, and total cost of ownership.

## Voltage Levels and Their Roles

| Voltage | Domain | Typical use | Key advantage |
|---------|--------|-------------|---------------|
| **800V / ±400V** | Facility DC distribution | Medium-voltage DC (MVDC) from utility → building | Highest efficiency over long distances, eliminates multiple transformer stages |
| **400V / 415V** | Facility AC distribution | Traditional UPS output, PDU input | Industry standard, well-known gear, AC breakers |
| **277V / 480V** | Facility AC distribution | US data center PDU input | 480V-to-277V reduces step-down stages |
| **48V** | Rack-level DC distribution | Server/GPU backplane, OCP standard | Safe (SELV), OCP standard, hot-swap friendly |
| **12V** | Board-level | Legacy server VR, HDD/SSD | Legacy standard, being displaced by 48V |
| **0.7V - 1.8V** | Silicon-level | CPU/GPU core voltage (VR to die) | Final step-down |

## Architecture Comparison

### 400V AC (Traditional)

```
Utility (13.8 kV) → Transformer → UPS (480V AC) → PDU (400/277V AC) → PSU (48V/12V DC) → VR → Silicon
```

**Efficiency:** ~85-90% total path efficiency
**Pros:** Mature, well-understood, standard equipment
**Cons:** Multiple conversion stages, large transformers, high I²R loss at low voltage

### 48V DC (OCP / Open Rack)

```
Utility (13.8 kV) → Transformer → Rectifier → 48V Bus → Server Bus Bar → 48V-to-PoL VR → Silicon
```

**Pioneered by:** Facebook/Meta Open Compute Project (OCP), Google
**Efficiency:** ~90-93% total path efficiency
**Pros:**
- Eliminates redundant PSU stages
- Battery backup at 48V (simpler than UPS)
- OCP Open Rack V3 standard — hot-swappable 48V bus bars
- Safer for maintenance (SELV, no arc flash hazard)
- Higher copper utilization (I²R is 8x lower than 12V for same power)
**Cons:**
- Higher current than 400V — requires larger copper bus bars
- 48V DC breakers less common than AC
- Rectifier efficiency at scale

### 800V DC (Next Generation)

```
Utility (13.8 kV) → Transformer → 800V DC Bus → 800V-to-48V Converter → 48V Bus → VR → Silicon
```

**Pioneered by:** Nvidia GB200 NVL72 (the "power shelf" concept)
**Efficiency:** ~93-96% total path efficiency
**Pros:**
- Minimal I²R loss — 4x less than 400V for same power
- Enables 1000W+ GPUs with manageable bus bar sizes
- 800V→48V conversion is more efficient than multi-stage 400V→12V→48V
- Aligns with EV industry ecosystem (800V SiC/GaN components)
- Battery backup at 800V (direct EV battery packs)
**Cons:**
- Very few operating installations
- Arc flash at 800V is deadly — requires specialized maintenance
- 800V-rated components are more expensive today
- Standards still evolving (OCP considering 800V)

### The Nvidia GB200 NVL72 Power Architecture

Nvidia's largest system drives 1000W+ per GPU plus Grace CPUs and NVSwitch:

```
Utility → Transformer → 480V AC → GB200 Power Shelf → 800V DC Bus → 
  → Power Tray (800V-to-48V) → GPU Tray → 48V-to-PoL → Silicon (0.7V)
```

- Each Power Shelf: 132 kW output at 800V DC
- Rack-level: 72 GPUs × 1000W + switches + CPUs ≈ ~120 kW per rack
- This is **2-3x the power density** of previous generation racks
- 800V DC is essential for keeping copper bus bars physically manageable at 120 kW per rack

## Power Density Trends

| Era | Typical rack power | Voltage | GPUs per rack | Cooling required |
|-----|-------------------|---------|---------------|-----------------|
| Pre-AI | 5-10 kW | 120/208V AC | 0 | Air |
| Early AI (V100) | 15-25 kW | 208/400V AC | 8-16 | Air |
| AI Training (A100) | 30-40 kW | 400V AC | 16-32 | Air + liquid assist |
| AI Training (H100) | 40-80 kW | 400V AC / 48V DC | 16-64 | Direct-to-chip liquid |
| Next-gen (B200) | 100-150 kW | 800V DC | 32-72 | Direct-to-chip liquid |
| Future (Rubin) | 150-300 kW | 800V DC | 64-128 | Liquid + immersion |

## Key Components

### Power Supply Units (PSU)
- **Titanium-rated** — >96% efficiency at 50% load (80 PLUS Titanium)
- **Form factors**: OCP 48V, CRPS (Common Redundant PSU)
- **Cooling**: Most rated for 40-55°C intake (liquid-cooled racks push hotter)
- **12V vs 48V**: 48V PSUs are becoming standard in AI DC

### UPS / Battery Backup
- **Traditional**: VRLA battery banks at 400V AC (large footprint, 5-10 year life)
- **Lithium-ion**: LiFePO4 at 48V or 400V (smaller footprint, 15-20 year life, higher cost)
- **Rack-level**: Small UPS or battery backup units per rack for graceful shutdown
- **800V**: EV-derived battery containers directly at 800V DC bus

### Power Distribution Units (PDU)
- **Traditional**: 400V AC input → 30x C13/C19 outlets at 208V
- **OCP**: 48V DC bus bar → server trays via finger-safe connectors
- **800V**: Bus bar → power shelf → tray-level distribution

## Related Pages
- [[thermal/concepts/liquid-cooling-technologies]] — power and cooling are coupled
- [[compute/concepts/gpu-architecture]] — power-hungry accelerators driving voltage evolution
- [[rack-pod/concepts/pod-and-superpod-architectures]] — how power distributes through pods
- [[memory/concepts/hbm-memory-architecture]] — HBM power contribution