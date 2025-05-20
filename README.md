# latchup-test-framework
Automated latch-up and over-voltage test documentation for semiconductor devices, following JEDEC JESD78 guidelines. Developed in C++ with a configuration-driven test flow, support for current/voltage pulsing, power measurement, and structured JSON result logging.

# Latch-Up Test Automation (C++)

This project automates latch-up and over-voltage validation testing for semiconductor DUTs in a lab or ATE environment. It follows JEDEC JESD78 guidelines and is implemented entirely in C++ with SCPI-controlled equipment.

## 📋 Key Features

- Reads **external latch-up and DPS config files**
- Applies both positive and negative current pulses to DUT pins
- Captures **pre/post power and temperature data**
- Automatically judges latch-up conditions and logs to JSON
- Supports pattern-based initialization (`inputs_low`, `inputs_high`)

## 🧪 Testing Overview

1. **Run Setup Pattern** (inputs_low or inputs_high)
2. **Measure Power** (idd/vdd pre-pulse)
3. **Inject Current Pulse** (±100mA)
4. **Measure Power Again** (idd/vdd post-pulse)
5. **Determine Pass/Fail**
6. **Log to Output File**
7. **Repeat for Each Pin**

See [`docs/Test_Procedure.md`](docs/Test_Procedure.md) for full step-by-step detail.

## 🔧 Config Files

### `Latchup Config (.cfg)`
Describes pin-level current and voltage clamp conditions.  
→ See [`docs/Latchup_Config_Format.md`](docs/Latchup_Config_Format.md)

### `DPS Config (.cfg)`
Describes power supply force/measure settings for each DPS pin.  
→ See [`docs/DPS_Config_Format.md`](docs/DPS_Config_Format.md)

## 📦 Data Format

All logs are written in structured `.json` format for easy parsing and visualization. Example:

```json
{
  "BOARD_ID[0]": {
    "pattern": "inputs_low",
    "pulse_current": 0.1,
    "idd_pre": 0.005,
    "idd_post": 0.035,
    "result": "FAIL"
  }
}
```

## ✅ Judgement Criteria

| Pre IDD Condition | Criteria                                   |
|------------------|--------------------------------------------|
| < 25mA           | Fail if post > pre + 10mA                  |
| ≥ 25mA           | Fail if post > 1.4 × pre                   |

→ See [`docs/Judgement_Criteria.md`](docs/Judgement_Criteria.md)

## 🔄 Over Voltage Test Flow

- Force nominal V
- Wait → Measure
- Force OVL V for 10ms
- Return to nominal → Measure again
- Judge using same IDD-based logic

## 🗂 Example Configs

- [`examples/latchup_config_example.cfg`](examples/latchup_config_example.cfg)
- [`examples/dps_config_example.cfg`](examples/dps_config_example.cfg)

