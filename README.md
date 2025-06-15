# 🔧 Sovol SV08 Toolhead Repair & Reverse Engineering

![Toolhead Repaired](images/replaced_mcu.jpg)

This repository documents my journey in repairing the Sovol SV08 toolhead after the main microcontroller (MCU) was damaged. From identifying components and tracing the circuit to replacing chips and reflashing firmware—everything is detailed here for anyone facing a similar situation.

---

## 📋 Table of Contents

- [Incident Summary](#incident-summary)
- [Reverse Engineering](#reverse-engineering)
- [Component Identification](#component-identification)
- [Firmware Flashing](#firmware-flashing)
- [Schematics & Diagrams](#schematics--diagrams)
- [Parts List](#parts-list)
- [Troubleshooting Log](#troubleshooting-log)
- [Tools Used](#tools-used)
- [License](#license)

---

## 🧨 Incident Summary

During a routine print, the toolhead stopped responding. Upon inspection:

- The MCU showed visible burn marks.
- No response over UART or USB.
- Toolhead fan was stuck in an "on" state.

Initial suspicion: overvoltage or a failed driver caused the MCU to short.

---

## 🧠 Reverse Engineering

I started by tracing the PCB to understand its layout.

![Reverse Engineering Diagram](images/pcb_reverse_engineering_diagram.png)

- Main MCU: **STM32F103C8T6**
- EEPROM: **AT24C32** (likely storing PID and calibration data)
- MOSFETs: **AOD4184** (heaters, fans)
- Temp sensor: **NTC thermistor**

See [`schematics/`](schematics/) for full layout in PDF and KiCad format.

---

## 🔍 Component Identification

| Component         | Part Number       | Notes                          |
|------------------|-------------------|--------------------------------|
| MCU              | STM32F103C8T6     | Burned, replaced               |
| EEPROM           | AT24C32           | Survived, retained calibration |
| Motor Driver IC  | Unknown (TBD)     | Reverse-engineering in progress |
| Heater MOSFET    | AOD4184           | Checked & still functional     |

See [`parts_list.md`](parts_list.md) for sourcing links.

---

## 🧪 Firmware Flashing

Steps I followed:

```bash
# Connect ST-Link to SWD pins
openocd -f interface/stlink.cfg -f target/stm32f1x.cfg -c init -c "program sv08_firmware.bin verify reset exit"
