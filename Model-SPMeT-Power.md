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

### Cell Parameters (`CellParametersInput`)

Flat structure with all electrode, current collector, and separator parameters.
All fields use underscore-separated names as defined in the `CellParametersInput` schema (e.g., `positive_electrode_mass_loading_mg_cm2`). The schema forbids extra/unknown fields.

Key fields:
- `upper_voltage_cutoff_V`, `lower_voltage_cutoff_V` (defaults: 4.2V / 3.0V) — note these also exist in `simulation_parameters` and the values used by the PyBaMM simulation come from `simulation_parameters`, not from `cell_parameters`.
- **Chemistry is auto-detected, not a dedicated field.** There is no `positive_electrode_active_material_name` field. Any entry in `positive_electrode_active_materials[].name` containing `"LFP"` (case-insensitive) triggers LFP-specific handling (Prada2013 base PyBaMM parameter set); otherwise an NMC/Chen2020-style base is used.
- `cell_nominal_capacity_Ah` and `cell_volume_L` are **not** inputs — they are computed output KPIs and are not fields on `CellParametersInput`.
- Electrode thermal/electrical properties (all **required**, no defaults): `positive_electrode_specific_heat_capacity_J_kg_K`, `negative_electrode_specific_heat_capacity_J_kg_K`, `positive_electrode_thermal_conductivity_W_m_K`, `negative_electrode_thermal_conductivity_W_m_K`, `positive_electrode_electronic_conductivity_S_m`, `negative_electrode_electronic_conductivity_S_m`
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
| `power_pulse_duration_s` | Pulse duration(s) [s] — **required**, no schema default (e.g. `[1, 10, 30]`) |
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
| `use_pybamm_params` | Free-form string (not an enum), default `""`. Common values: `"ORegan2022"`, `"Prada2013"`, `"Chen2020"` — loads that named PyBaMM parameter set as the base. `""` or `"custom"` both build from a Chen2020 base (or Prada2013 automatically, if LFP chemistry is detected) and override with user-provided material properties. |

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

## Example Request

Top-level input has two keys: `cell_parameters` (`CellParametersInput`) and `simulation_parameters` (`SimulationParameters`). The payload below is the actual seeded default configuration for this model (46mm cylindrical NMC811/Graphite cell) and is valid as-is against the schemas above — it can be copy-pasted and run directly:

```json
{
  "cell_parameters": {
    "positive_electrode_mass_loading_mg_cm2": 26.58,
    "negative_electrode_mass_loading_mg_cm2": 19.52,
    "positive_electrode_sheet_count": 1,
    "negative_electrode_sheet_count": 1,
    "jelly_roll_count": 1,
    "electrode_coating_side_count": 2,
    "form_factor": "Cylindrical",
    "cell_diameter_mm": 46.0,
    "cell_height_mm": 80.0,
    "positive_electrode_width_mm": 3356.402408,
    "positive_electrode_height_mm": 69.5,
    "positive_electrode_specific_heat_capacity_J_kg_K": 700.0,
    "negative_electrode_specific_heat_capacity_J_kg_K": 700.0,
    "positive_electrode_thermal_conductivity_W_m_K": 1.5,
    "negative_electrode_thermal_conductivity_W_m_K": 1.0,
    "positive_electrode_electronic_conductivity_S_m": 100.0,
    "negative_electrode_electronic_conductivity_S_m": 100.0,
    "positive_coating_thickness_um": 80.0,
    "negative_coating_thickness_um": 126.0,
    "positive_electrode_active_materials": [
      {
        "name": "NMC811_Generic_v2",
        "mass_fraction": 0.9185,
        "density_g_cm3": 4.7,
        "specific_capacity_mAh_g": 200.0,
        "nominal_voltage_V": 3.7
      }
    ],
    "negative_electrode_active_materials": [
      {
        "name": "Graphite_Generic_v2",
        "mass_fraction": 0.86,
        "density_g_cm3": 2.2,
        "specific_capacity_mAh_g": 360.0,
        "nominal_voltage_V": 0.1
      }
    ],
    "positive_electrode_binders": [
      {"name": "PVDF_Generic_v1", "mass_fraction": 0.04075, "density_g_cm3": 1.78}
    ],
    "negative_electrode_binders": [
      {"name": "CMC_Generic_v1", "mass_fraction": 0.07, "density_g_cm3": 1.6},
      {"name": "SBR_Generic_v1", "mass_fraction": 0.07, "density_g_cm3": 1.0}
    ],
    "positive_electrode_conductive_agents": [
      {"name": "CNT_Generic_v1", "mass_fraction": 0.04075, "density_g_cm3": 1.5}
    ],
    "negative_electrode_conductive_agents": [],
    "positive_electrode_foil_thickness_um": 14.0,
    "positive_electrode_foil_electronic_conductivity_S_m": 37700000.0,
    "positive_electrode_foil_density_g_cm3": 2.7,
    "positive_electrode_foil_specific_heat_capacity_J_kg_K": 897.0,
    "positive_electrode_foil_thermal_conductivity_W_m_K": 237.0,
    "negative_electrode_foil_thickness_um": 10.0,
    "negative_electrode_foil_electronic_conductivity_S_m": 59600000.0,
    "negative_electrode_foil_density_g_cm3": 8.96,
    "negative_electrode_foil_specific_heat_capacity_J_kg_K": 1000.0,
    "negative_electrode_foil_thermal_conductivity_W_m_K": 401.0,
    "separator_thickness_um": 16.0,
    "separator_porosity": 0.42,
    "separator_density_g_cm3": 0.95,
    "separator_specific_heat_capacity_J_kg_K": 1000.0,
    "separator_thermal_conductivity_W_m_K": 0.16,
    "electrolyte_density_g_cm3": 1.2,
    "electrolyte_fill_ratio": 0.85,
    "upper_voltage_cutoff_V": 4.2,
    "lower_voltage_cutoff_V": 2.8
  },
  "simulation_parameters": {
    "power_sweep": {
      "power_soc_pct": 50,
      "power_temperature_K": 298.15,
      "power_pulse_duration_s": [1, 10, 30],
      "power_levels_W": [1000, -1000, 2000, 3000, 4000]
    },
    "upper_voltage_cutoff_V": 4.2,
    "lower_voltage_cutoff_V": 2.8,
    "reference_cell_temperature_K": 298.15,
    "cell_contact_resistance_Ohm": 1e-5,
    "cell_heat_transfer_coefficient_W_m2_K": 0.01,
    "cell_cooling_surface_area_m2": 0.0008574453549197741,
    "solver_atol": 1e-4,
    "solver_rtol": 1e-4,
    "use_pybamm_params": "ORegan2022"
  }
}
```

Note: `power_levels_W` mixes positive (discharge) and negative (charge) values in one sweep — the model runs both directions and reports them separately in `results[].direction`.

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

### Copilot Summary

A curated subset used for AI copilot context (`summarize_for_copilot()`), derived from `results`:

- `power_envelope.discharge` / `power_envelope.charge` -- `max_power_W`, `min_power_W`, `mean_power_W` computed from each direction's achieved power values (a direction is omitted if it has no valid data)
- `discharge_vs_charge` -- `max_discharge_power_W`, `max_charge_power_W` (omitted if both are zero)
- `success`, `error`, `conditions` -- passed through from the full result

---

*[← Back to Models Library](Model-Library)*
