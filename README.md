# 1S Power Bank Power Management (SW6208)

This repository documents the power-management section of a single-cell Li-ion power bank design based on the SW6208 integrated power bank SoC.

The SW6208 combines battery charging, boost conversion, and USB fast-charge protocol handling into a single device, making it well suited for compact 1S power bank architectures.

---

## Overview

Key characteristics of this power stage:

- Single-cell Li-ion operation (1S)
- Integrated buck charging and boost conversion
- USB-C and USB-A support
- Fast-charge protocol handling inside the SoC
- External inductor-based power stage
- Minimal external control logic

The design is intended to work downstream of a dedicated battery protection stage and assumes a protected battery node.

---

## Architecture

The SW6208 manages the entire power path:

- Charging the battery from USB input
- Boosting battery voltage to regulated USB output
- Negotiating fast-charge behavior using D+ / D− and CC pins
- Controlling port enable and output current limiting

No external charger IC, boost IC, or PD controller is required.

---

## USB-C Handling

USB-C CC pins are connected directly to the SW6208.

- No external pull-down resistors
- No large CC-to-ground capacitors
- No external PD controller

This follows the intended use of the device and avoids unnecessary loading of the CC lines that could interfere with attach and negotiation behavior.

---

## Power Inductor Selection

The design uses a single shared inductor for both charging and boost operation.

Selection criteria:
- Inductance aligned with reference designs
- High saturation current
- Low DCR
- Shielded construction for EMI control

This component largely defines efficiency and thermal behavior under load.

---

## Output Switching

USB output power is gated using an external N-channel MOSFET controlled by the SW6208.

This provides:
- Controlled port enable
- Reduced leakage when disabled
- Isolation of output connectors from the internal power node

The MOSFET is selected primarily for low RDS(on) at logic-level gate drive rather than absolute current rating.

---

## Design Philosophy

This power stage is designed to be:

- Datasheet-aligned
- Minimal in external components
- Predictable under load
- Easy to integrate with a separate protection BMS

Rather than pushing protocol or current limits, the focus is on stable operation and clean power behavior.

---

## Files

- `schematic/`  
  Power-management schematic (PDF and image).

- `notes/power-path.md`  
  Explanation of how power flows through the system.

- `notes/usb-c-decisions.md`  
  Rationale behind USB-C CC and ESD choices.

- `notes/component-selection.md`  
  Notes on inductor, MOSFET, and passive component selection.

---

## Disclaimer

This design is provided as a reference only.  
It has not been certified and must be validated and tested before use in any product.

---

## License

Open-source hardware, see LICENSE file.
