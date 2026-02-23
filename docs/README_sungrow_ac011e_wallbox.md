# Sungrow AC011E Wallbox Template

## 📋 Overview

The **Sungrow AC011E Wallbox Template** provides integration for Sungrow EV wallboxes. It is based on the [Sungrow-Wallbox-Modbus-HomeAssistant](https://github.com/Louisbertelsmann/Sungrow-Wallbox-Modbus-HomeAssistant) project by Louis Bertelsmann.

## 🏭 Supported Models

- **AC007-00** – Device type code 0x20ED
- **AC011E-01** – Device type code 0x20DA (primary model)
- **AC22E-01** – Device type code 0x3F80

## 🔌 Connection

The wallbox is connected to the inverter via **RS485**. Modbus Manager connects to the inverter (Modbus TCP via WiNet-S or gateway). To address the wallbox on the inverter’s RS485 bus, configure the wallbox **slave ID** (default **3** – verify in the iSolarCloud app).

## ⚙️ Configuration

### Required Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| **Model** | Select | AC011E-01 | Wallbox model (Output Current limits from datasheet) |
| **Prefix** | String | WB | Unique prefix for all entities |
| **Slave ID** | Integer | 3 | Modbus slave address (verify in iSolarCloud) |

### Model Selection (config flow / options flow)

During setup you select your wallbox model. Min/max values for **Output Current** are taken from the datasheet:

| Model | Min | Max | Phases | Power |
|-------|-----|-----|--------|-------|
| AC007-00 | 6 A | 32 A | 1 | 7.4 kW |
| AC011E-01 | 6 A | 16 A | 3 | 11 kW |
| AC22E-01 | 6 A | 32 A | 3 | 22 kW |

### Example

Add as a Modbus Manager device with:

- **Template:** Sungrow AC011E Wallbox
- **Model:** AC011E-01 (or your model)
- **Host:** IP of inverter WiNet-S dongle or Modbus gateway (not the wallbox itself)
- **Port:** 502
- **Slave ID:** 3 (or your wallbox slave ID)

## 📊 Available Entities

### Sensors

| Entity | Description |
|--------|-------------|
| Device Type Code | Model identification (AC007-00, AC011E-01, AC22E-01) |
| Power Phases | Number of power phases |
| Rated Voltage | Rated voltage (V) |
| Phase Switching Status | Three phase / Single phase |
| Minimum / Maximum Charging Power | Power limits (W) |
| Total Energy | Lifetime energy delivered (Wh) |
| Phase A/B/C Voltage & Current | Per-phase measurements |
| Charging Power / Energy | Current session power (W) and energy (Wh) |
| Charging Status | Idle, Standby, Charging, Completed, etc. |
| Charging Start / End Time | Session timestamps |
| Start Mode | Stopped, EMS, Swipe |

### Controls

| Entity | Type | Description |
|--------|------|-------------|
| Output Current | Number | Max charging current (model-dependent: 6–16 A or 6–32 A) |
| Phase Mode | Select | Three phase / Single phase |
| Charger Enable | Switch | Enable / Disable wallbox |
| Mileage per kWh | Number | km/kWh for range calculation (1–500) |
| Working Mode | Select | Network, Plug and Play, EMS |
| Start Charging | Button | Remote start |
| Stop Charging | Button | Remote stop |

### Calculated Sensors

| Entity | Description |
|--------|-------------|
| Charging Start Time | Formatted timestamp (dd.mm.yyyy HH:mm) |
| Charging End Time | Formatted timestamp |
| Charging Duration | Session duration (H:MM) |
| Charged Range | Estimated km from energy × mileage/kWh |

### Binary Sensors

| Entity | Description |
|--------|-------------|
| Charging Active | True when status is Charging |

## 📚 References

- [Sungrow-Wallbox-Modbus-HomeAssistant](https://github.com/Louisbertelsmann/Sungrow-Wallbox-Modbus-HomeAssistant)
- [modbus_wallbox.yaml](https://raw.githubusercontent.com/Louisbertelsmann/Sungrow-Wallbox-Modbus-HomeAssistant/refs/heads/main/modbus_wallbox.yaml)
