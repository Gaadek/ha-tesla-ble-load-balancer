# Tesla BLE Load Balancer for Home Assistant

![Home Assistant](https://img.shields.io/badge/Home%20Assistant-Blueprint-41BDF5?logo=homeassistant\&logoColor=white)
![ESPHome](https://img.shields.io/badge/ESPHome-Tesla%20BLE-000000?logo=esphome\&logoColor=white)
![Tesla](https://img.shields.io/badge/Tesla-BLE-red?logo=tesla)
![License](https://img.shields.io/github/license/Gaadek/ha-tesla-ble-load-balancer)

A [Home Assistant](https://www.home-assistant.io/) blueprint that dynamically controls Tesla charging current through [PedroKTFC/esphome-tesla-ble](https://github.com/PedroKTFC/esphome-tesla-ble), maximizing the available charging power while ensuring your home's total power consumption never exceeds the configured grid power limit.

Unlike cloud-based Tesla integrations, this blueprint communicates directly with the vehicle over Bluetooth Low Energy (BLE) through the Tesla BLE Home Assistant integration, providing fast, fully local, and reliable control.

---

## Features

- ⚡ Dynamic load balancing
- 🚗 Direct Tesla BLE control (no Tesla cloud required)
- ☀️ Solar charging mode
- 🌙 Off-peak charging support
- 🚀 Boost mode for maximum charging speed
- 🔋 Automatically adjusts charging current according to available household power
- 🏠 Prevents overloading your electrical installation by respecting the configured grid power limits
- ⚙️ Highly configurable through Blueprint inputs with no YAML modifications required
- 🔄 Fully local automation

---

## Charging Modes

### Solar

Charges the vehicle only when surplus solar production is available.

Ideal for maximizing self-consumption.

### Eco

Combines:
- Solar surplus
- Off-peak electricity periods

This mode optimizes charging cost while still taking advantage of solar production.

### Boost

Charges the vehicle as fast as possible using all available power while staying below the configured household power limit.

---

## How it Works

The automation continuously monitors your home's power consumption.

When available power changes, it automatically adjusts the Tesla charging current using Tesla BLE.

The charging current is increased or decreased smoothly while respecting:

- Minimum charging current
- Maximum charging current
- Vehicle charging state
- Household contracted power
- User-selected charging mode

---

## Requirements

You will need:

- Home Assistant
- Tesla BLE integration for Home Assistant (using an ESPHome Bluetooth Proxy) [PedroKTFC/esphome-tesla-ble](https://github.com/PedroKTFC/esphome-tesla-ble)
- A Tesla vehicle supporting BLE
- A household power sensor reporting the net grid power (negative values indicate excess solar production exported to the grid).

---

## Blueprint Inputs

### Tesla BLE

| Input                     | Description                                                                                                       |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Tesla BLE status          | Sensor provided by the Tesla BLE integration indicating whether the BLE connection to the vehicle is established. |
| Asleep sensor             | Sensor provided by the Tesla BLE integration indicating whether the vehicle is asleep.                            |
| Charger                   | Switch provided by the Tesla BLE integration used to start or stop charging.                                      |
| Charging state sensor     | Sensor provided by the Tesla BLE integration reporting the current charging state of the vehicle.                 |
| Measured charging current | Sensor provided by the Tesla BLE integration reporting the actual charging current (A).                           |
| Charging current control  | Number entity provided by the Tesla BLE integration used to set the charging current (A).                         |

### Charging Settings

| Input                  | Description                                                                                                                    |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| Charging mode selector | Select entity used to choose the charging mode. Supported values are **Solar**, **Eco**, and **Boost**.                        |
| Max charging current   | Maximum charging current (A) that the automation is allowed to request. The vehicle will never charge above this value.        |
| Min charging current   | Minimum charging current (A) allowed by the automation. If the calculated current falls below this value, charging is stopped. |

### Power Management

| Input             | Description                                                                                                                                                                       |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Home power sensor | Sensor reporting the total instantaneous power consumption of your home (W).                                                                                                      |
| Voltage           | Nominal AC supply voltage (typically 230 V) used to convert between power and charging current.                                                                                   |
| Hysteresis power  | Minimum change in available power required before recalculating the charging current. Helps prevent frequent adjustments caused by measurement noise or small power fluctuations. |

### Grid Limits

| Input                                 | Description                                                                      |
| ------------------------------------- | -------------------------------------------------------------------------------- |
| Max grid power during peak period     | Maximum power that may be imported from the grid during peak tariff periods.     |
| Max grid power during off-peak period | Maximum power that may be imported from the grid during off-peak tariff periods. |

### Off-Peak Schedule

| Input               | Description                                                                |
| ------------------- | -------------------------------------------------------------------------- |
| Off-peak start time | Time at which the off-peak tariff period begins.                           |
| Off-peak stop time  | Time at which the off-peak tariff period ends.                             |
| Off-peak days       | Select the days of the week on which the off-peak tariff schedule applies. |


---

## Benefits of Tesla BLE

- Fully local communication
- No Internet dependency
- No Tesla Fleet API required
- No API rate limits
- Faster response time
- Better privacy
- Lower latency

---

## Installation

1. Download the blueprint.
2. Copy it into:

```
config/blueprints/automation/<your_folder>/
```

3. Import the blueprint into Home Assistant.
4. Create a new automation from the blueprint.
5. Configure the required entities.
6. Enjoy automatic load balancing.

---

## Recommended Setup

```
Home Power Sensor
        │
        ▼
Blueprint Automation
        │
        ▼
Tesla BLE
        │
        ▼
Tesla Vehicle
```

---

## Notes

Designed to minimize unnecessary Tesla BLE commands, reducing vehicle wake-ups while maintaining responsive load balancing.

---

## Compatibility

- Home Assistant
- ESPHome Tesla BLE integration
- Tesla vehicles supporting Bluetooth Low Energy (BLE) charging control

---

## Contributing

Contributions, bug reports and feature requests are welcome!

If you find an issue or have an idea for improvement, feel free to open an issue or submit a pull request.

---

## Disclaimer

This project is not affiliated with or endorsed by Tesla, Inc.

Use at your own risk.
