# E-Paper Display System — Doha Access-Control Kiosks

E-paper signage integrated with access-control kiosks, engineered for Doha's climate — including outdoor sites exceeding 50°C ambient.

## Repository Contents

| File | Description |
| --- | --- |
| [`E-Paper_Comprehensive_Report_v1.0.pdf`](E-Paper_Comprehensive_Report_v1.0.pdf) | **Version 1.0** — 33-page sourcing & integration report: technology comparison (Spectra 6 · Kaleido 3 · ChLCD · Marquee · Carta), indoor/outdoor tracks, procurement tiers, wiring & integration, BOM pricing, power-outage behavior, NFC e-paper, procurement roadmap, and Qatar-specific engineering considerations |

## Overview

The system combines an offline-first architecture with bistable e-paper technology to deliver reliable, power-efficient signage alongside secure door access control.

- **Offline-first architecture** — Raspberry Pi 5 with Emteria Android 15; no cloud dependency
- **Bistable e-paper** — zero standby power; the panel keeps its image at 0 W
- **Environmental adaptability** — panel tracks for indoor and extreme outdoor (50°C+, IP65) sites
- **Structured procurement** — specification → prototyping → supplier evaluation before any production order

## System Architecture

| Component | Responsibility |
| --- | --- |
| Raspberry Pi 5 (Android 15) | HMI (Jetpack Compose), face recognition, content rendering |
| Local MQTT broker | Internal messaging; enables fully offline operation |
| ESP32 TCON driver | Receives images, validates CRC-32, drives panel waveforms, publishes heartbeats |
| E-paper panel | Bistable display; powered only during refresh |
| ESP32-S3 relay controller | Door-lock actuation over USB serial |
| UPS | Critical-path power backup (Pi + relay controller) |

**Content push:** Android renders a PNG sized to the panel, pushes it with a CRC-32 header over WiFi/USB; the TCON validates, refreshes, and reports completion over MQTT.
**Health monitoring:** TCON heartbeat every 30 s; staleness alarm after 3 missed heartbeats (90 s).

## Panel Selection Criteria

| Site condition | Color required | Recommended panel |
| --- | --- | --- |
| Indoor (<50°C) | Yes | Good Display 31.5" Spectra 6 |
| Indoor (<50°C) | No | GDEP312TT3 31.2" B&W |
| Outdoor / hot site (>50°C) | Yes | Kaleido 3 Outdoor 25.3" + IP65 |
| Outdoor / hot site (>50°C) | No | GDEP312TT3 / DMPH312EC1 IP65 B&W |

All selections are prototyped on a Waveshare 13.3" development kit before a production order is placed.

## Procurement Discipline

1. **Before ordering production panels** — write the content specification, build the content pipeline on the dev kit, measure refresh timing, and practice FPC/ESD handling.
2. **RFQ process** — send an identical-spec RFQ package to at least 3 suppliers; build a comparison matrix (price, lead time, MOQ, warranty, DOA policy, Incoterms).
3. **Decision gate** — production order only when spec is complete, quotes are in hand, and the prototype is validated.
4. **Acceptance testing** — every delivered panel passes a delivery acceptance test (visual, power-on, CRC round-trip, temperature sensor) before deployment.

## Author

**Eng. Abdulaziz Nasser AL-Haddad** — Industrial Project Engineer, Doha, Qatar

© 2026 Abdulaziz Nasser AL-Haddad. All rights reserved. The PDF report is provided for reference; verify prices, package contents, and specifications with suppliers before purchase.
