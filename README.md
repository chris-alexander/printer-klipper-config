# printer-klipper-config

This repo contains the Klipper configuration for my custom 3D printer.

It started life as a budget DIY bedslinger and has been upgraded extensively over the years (frame, motion system, electronics, firmware, and toolhead). At this point it’s effectively a custom machine, and this repo is the “source of truth” for its configuration.

---

## Current Hardware (Snapshot)

### Electronics
- Mainboard: **BTT SKR 1.4 Turbo** (MCU: **LPC1769**)
- Stepper drivers: **TMC2209** (on-board / soldered, UART)
- Firmware: **Klipper**
- UI: **Mainsail** + **Moonraker**
- Display: TFT35 in 12864 emulation mode (legacy), plus KlipperScreen (separate device)

### Motion / Frame
- Kinematics: Cartesian bedslinger
- Frame: AM8-style frame (heavily modified)
- X axis: Bear-style X axis
- Z axis: Dual Z leadscrews, independently driven (**Z tilt compensation**)

### Toolhead
- Extruder: Printed **Bondtech BMG-style**
  - gear ratio: `50:17`
  - rotation_distance tuned in config
- Hotend: **E3D Revo**
- Typical nozzle: `0.4mm`

### Bed
- Heated bed: Prusa-style 220×220 (Fysetc)
- Surface: magnetic flex plates (multiple plates/surfaces)

### Probing / Endstops
- Probe: Trianglelab PINDA
- Endstops: Sensorless homing (X/Y), probe used as Z endstop

---

## Stepper Motors
- X motor: **LDO-42STH48-1684MAC** (0.9°)
- Y motor: **LDO-42STH48-1684MAC** (0.9°)
- Extruder motor: **LDO-42STH25-1404MAC** (0.9°)
- Z motors: stock (model unknown)

---

## Repo Layout
Main config: `printer.cfg`

Additional config:
- `sensorless.cfg` – sensorless homing macros
- `display.cfg` – TFT config (12864 emulation)
- `km_settings.cfg` – configuration for `jschuh/klipper-macros`
- `moonraker.conf` – Moonraker configuration and Update Manager entries
- `variables.cfg` – tracked calibration state (see below)

---

## Calibration / State Tracking

### Why `variables.cfg` is tracked
`variables.cfg` is tracked because it stores **bed surface offsets** (per flex plate / surface), which are time-consuming to retune.

It is updated by Klipper macros via `[save_variables]`.

---

## Notes / Design Choices

### `klipper-macros/` is not tracked
The folder `klipper-macros/` is intentionally not committed in this repo, because it is managed by Moonraker Update Manager.

If you need to record the exact version in use:
```bash
cd ~/printer_data/config/klipper-macros
git rev-parse --short HEAD
