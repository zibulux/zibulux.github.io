---
title: Radio Theories
date: 2025-09-29
categories: [4-Radio, 1Rad-Novice]
tags: [radio]
author: <author_zoog>
---

# Radio Theory 📻

## The Electromagnetic Spectrum 🌈

The **electromagnetic spectrum** is the complete range of electromagnetic radiation, organized by frequency or wavelength. The spectrum is divided into distinct bands, with different names for the electromagnetic waves within each band. From low to high frequency, these are:

- 📡 Radio waves
- 🍕 Microwaves
- 🔥 Infrared
- 👁️ Visible light
- ☀️ Ultraviolet
- 🦴 X-rays
- ☢️ Gamma rays

The electromagnetic waves in each of these bands have different characteristics, such as how they are produced, how they interact with matter, and their practical applications.

### Wavelength Examples 📏

- **Microwaves**: 1mm to 1m wavelength
- **Radio waves**: 1m to 100km wavelength

---

## Radio Frequency Bands 📶

We use three types of radio frequencies: **HF**, **VHF**, and **UHF**.

### HF (High Frequency) 🌍

- **Wavelength**: 100m to 10m
- **Frequency**: 3MHz to 30MHz
- **Use Case**: Suitable for long-distance BLOS (Beyond Line of Sight) communication
- **Propagation**: HF waves travel long distances by bouncing off the ionosphere, creating skip zones

```
[ Transmitter ] ⇧            ⇩       ⇧             ⇩
                ↗ IONOSPHERE ↘   ↗ IONOSPHERE ↘
                   ↘ Earth ↖        ↘ Earth ↖
              [ Distant Receiver ]
```

### VHF (Very High Frequency) 🏔️

- **Wavelength**: 10m to 1m
- **Frequency**: 30MHz to 300MHz
- **Use Case**: Suitable for medium-distance communication, works well in open areas
- **Propagation**: Line of Sight (LOS)

### UHF (Ultra High Frequency) 🏙️

- **Wavelength**: 1m to 30cm
- **Frequency**: 300MHz to 3000MHz
- **Use Case**: Suitable for short-distance communication in complex terrain or urban environments
- **Propagation**: Line of Sight (LOS) with excellent penetration capabilities

---

## Line of Sight Concepts 👁️

### LOS (Line of Sight)

Line of Sight means that both antennas need to see each other to transmit effectively (without considering the penetration factor). The limit of line-of-sight is the curvature of the Earth—known as the **radio horizon**.

### BLOS (Beyond Line of Sight) 🔮

Beyond Line of Sight means that both the receiver and transmitter do not need to see each other. This allows for greater distance and coverage.

**Note**: SATCOM is considered BLOS, but the dish and the satellite communicate with each other via LOS. 🛰️

---

## Wavelength and Frequency Principles 📊

- **Longer wavelengths** can travel farther distances. For example, ELF (Extremely Low Frequency) can travel across the entire planet. 🌍
- **Higher frequencies** can carry more energy and data; however, this limits propagation distance.

---

## Why Choose HF Over SATCOM? 🤔

We tend to use HF over SATCOM for BLOS communication because it is better not to rely on other infrastructure.

---

## Comparison of Radio Frequencies ⚖️

### HF Advantages ✅

- Independent operation without relying on external infrastructure
- Excellent for long-distance BLOS communication
- No dependency on satellites

### HF Disadvantages ❌

- Creates skip zones
- Low bandwidth
- Requires large antennas, which restricts mobility

### VHF Advantages ✅

- Short to medium range capability
- Excellent performance in open areas
- Best balance between HF and UHF characteristics

### VHF Disadvantages ❌

- Less effective in complex terrain
- Easier to detect

### UHF Advantages ✅

- Smaller wavelengths allow better penetration through obstacles
- Best performance in complex terrain or urban areas
- Requires small antennas, therefore easier to carry
- Allows greater bandwidth

### UHF Disadvantages ❌

- Short range
- More susceptible to atmospheric interference (e.g., less effective in rainfall)

---

*Note: This document covers fundamental radio theory concepts for understanding electromagnetic spectrum communication.*

## Modulation 🎛️

There are numerous modulation types, including FM, AM, QAM, P25, and many others. We principally use **FM** and **AM**.

### AM (Amplitude Modulation) 📊

Amplitude Modulation works by varying the **amplitude (voltage strength)** of the carrier wave.

**Characteristics**:
- Allows greater transmission distance
- More susceptible to noise and interference
- Can carry less data
- **Primary Use**: Aircraft communication ✈️

### FM (Frequency Modulation) 🎵

Frequency Modulation works by varying the **frequency** of the carrier wave.

**Characteristics**:
- Provides better voice quality
- More resistant to interference
- Signal drops rapidly once it begins to fade
- **Primary Use**: Common in VHF/UHF communications

---

## NVIS (Near Vertical Incidence Skywave) ⬆️

![NVIS Radiation Pattern](https://upload.wikimedia.org/wikipedia/commons/thumb/2/26/NVIS_Radiation_Pattern.svg/1024px-NVIS_Radiation_Pattern.svg.png)

NVIS is a technique that aims the signal almost straight up to the sky, causing it to bounce back down closer to the transmitter. This approach allows us to reduce the skip zone significantly.

**Advantages**:
- Reduces or eliminates skip zones
- Makes the transmitter harder to detect and locate
- Provides reliable regional coverage (typically 0-400 km radius)
- Effective for mountainous or difficult terrain where line-of-sight is blocked

**Typical Use**: Short to medium-range HF communications where BLOS coverage is needed without the large skip zones of traditional HF skywave propagation.

---

---