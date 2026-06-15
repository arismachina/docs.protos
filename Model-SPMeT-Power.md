# SPMeT Power Envelope Model <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

[← Models Library](Model-Library) · **SPMeT Power**

Finds the maximum feasible power at each SOC, temperature, and pulse duration combination.

---

Physics-based maximum power determination using the Single Particle Model
with Electrolyte (SPMe) with lumped thermal. Sweeps power levels across
SOC, temperature, and pulse duration to find the maximum feasible power.

## Overview

Runs constant-power pulses at various conditions and determines the highest
power that completes without hitting voltage, anode potential, or temperature
limits. Uses binary search refinement for precision.

### Protocol

```
For each (SOC, temperature, pulse_duration, direction):
  For each power_level (ascending):
    1. Set initial conditions (SOC, temperature) — each level is independent
    2. Apply constant power pulse for pulse_duration
    3. Check termination: if solution.termination == "final time" → pass
       (voltage cutoff, anode potential, or temperature limit hit → fail)
    4. If pass: record as valid (success=True), try next power level
    5. If fail: binary search between last valid (or 0W) and this level
       → record best found (success=False, power_achieved_W=<max found>)
       → stop testing further levels for this condition
```

## Input Schema

### Cell Parameters (`CellParameters`)

Flat structure with all electrode, current collector, and separator parameters.
All fields use underscore-separated names as defined in the `CellParameters` schema (e.g., `positive_electrode_coating_density_g_cm_3`).

Key fields:
- `cell_nominal_capacity_Ah`, `cell_volume_L`
- `cell_upper_voltage_cutoff_V`, `cell_lower_voltage_cutoff_V`
- `positive_electrode_active_material_name` (for auto chemistry detection)
- Electrode geometry: height, width, count, jelly roll count (integer)
- Coating: thickness, porosity, active material fraction, density
- Current collectors: thickness, conductivity, density, specific heat
- Separator: thickness, porosity, density, specific heat, optional sheet dimensions

#### Electrode Dimensions (Optional)

| Field | Type | Description |
|---|---|---|
| `positive_electrode_width_mm` | `float` or `None` | Positive electrode width [mm]. If provided, used directly for area calculation instead of deriving from cell dimensions. Also used for negative electrode if negative-specific dimensions not provided. |
| `positive_electrode_height_mm` | `float` or `None` | Positive electrode height [mm]. If provided, used directly for area calculation instead of deriving from cell dimensions. Also used for negative electrode if negative-specific dimensions not provided. |
| `negative_electrode_width_mm` | `float` or `None` | Negative electrode width [mm]. If provided, used directly for area calculation. If not provided, positive electrode width is used. |
| `negative_electrode_height_mm` | `float` or `None` | Negative electrode height [mm]. If provided, used directly for area calculation. If not provided, positive electrode height is used. |

#### Separator Dimensions (Optional)

| Field | Type | Description |
|---|---|---|
| `separator_sheet_count` | `float` or `None` | Separator sheet count. If not provided, calculated as `pos_count + neg_count + 1` |
| `separator_sheet_width_mm` | `float` or `None` | Separator sheet width [mm]. If not provided, uses positive electrode width + overhang |
| `separator_sheet_height_mm` | `float` or `None` | Separator sheet height [mm]. If not provided, uses positive electrode height + overhang |

**Separator dimension derivation logic:**
- If both `separator_sheet_width_mm` and `separator_sheet_height_mm` are provided, they are used directly.
- Otherwise, separator dimensions are derived from positive electrode dimensions plus overhang (4 × `electrode_overhang_mm` total, accounting for 2 × overhang per side).
- For cylindrical cells, separator area approximates positive electrode area.

### Simulation Parameters

#### Sweep Conditions

| Parameter | Description |
|---|---|
| `power_soc_pct` | SOC value(s) to test [0-100] |
| `power_temperature_K` | Temperature(s) to test [K] |
| `power_pulse_duration_s` | Pulse duration(s) [s] (default: [1, 10, 30]) |
| `power_levels_W` | Power levels to sweep [W] (discharge: +ve, charge: -ve) |

#### Operating Conditions

| Parameter | Description |
|---|---|
| `upper_voltage_cutoff_V`, `lower_voltage_cutoff_V` | Voltage limits [V] |
| `reference_cell_temperature_K` | Reference temperature [K] |
| `ambient_temperature_K` | Ambient temperature [K] (optional, defaults to reference) |
| `initial_cell_temperature_K` | Initial cell temperature [K] (optional, defaults to ambient) |
| `cell_contact_resistance_Ohm` | Cell contact resistance [Ohm] |
| `cell_heat_transfer_coefficient_W_m2_K` | Cell heat transfer coefficient [W/(m²·K)] |
| `cell_cooling_surface_area_m2` | Cell cooling surface area [m²] |

#### Solver

| Parameter | Description |
|---|---|
| `solver_atol` | Absolute tolerance |
| `solver_rtol` | Relative tolerance |
| `use_pybamm_params` | "ORegan2022", "Prada2013", "Chen2020", or "" (build from scratch) |

#### Safety Terminations (Optional)

| Parameter | Default | Description |
|---|---|---|
| `anode_potential_safety_threshold_V` | None | Anode potential limit [V] |
| `temperature_safety_threshold_K` | None | Max temperature [K] |

When provided, these add custom terminations using `pybamm.step.CustomTermination`.
When None, only voltage cutoffs are used.

#### Reference Performance Test (RPT) Parameters (Optional)

| Parameter | Default | Description |
|---|---|---|
| `rpt_power.power_level_W` | 2000.0 | Power pulse level [W] |
| `rpt_power.power_duration_s` | 300.0 | Power pulse duration [s] |
| `rpt_power.power_direction` | "discharge" | Power pulse direction ("charge" or "discharge") |
| `rpt_power.power_soc_pct` | 50.0 | Power pulse SOC [%] |
| `rpt_power.power_temperature_K` | 298.15 | Power pulse temperature [K] |

RPT parameters are used for reference performance testing and are separate from the main power sweep simulation.

## Output Schema

### PowerResultPoint

Each sweep point records:

| Field | Description |
|---|---|
| `success` | `true` if the demanded power was sustained for the full pulse without early termination |
| `soc_pct` | SOC tested [0-100] |
| `temperature_K` | Temperature tested [K] |
| `duration_s` | Pulse duration [s] |
| `direction` | `"Discharge"` or `"Charge"` |
| `power_demand_W` | Power level originally demanded [W] |
| `power_achieved_W` | Max power actually achieved [W] (= demand if success, < demand if binary search result) |
| `current_max_A` | Maximum absolute current [A] during the achieved power pulse (null if not available) |
| `error` | Error message if nothing achievable (null otherwise) |

### SpmePowerOutput

- `success` -- overall success
- `results` -- list of `PowerResultPoint`
- `conditions` -- `SimulationParameters` (power_soc_pct [0-100], power_temperature_K, power_pulse_duration_s, power_levels_W, voltage cutoffs, etc.)
- `error` -- error message if failed

---

*[← Back to Models Library](Model-Library)*
