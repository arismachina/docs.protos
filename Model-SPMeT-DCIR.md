# SPMeT DCIR Model <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

[← Models Library](Model-Library) · **SPMeT DCIR**

Calculates Direct Current Internal Resistance at configurable SOC, temperature, and C-rate combinations.

---

Physics-based Direct Current Internal Resistance (DCIR) simulation using the Single Particle Model with Electrolyte (SPMe) and lumped thermal model. Calculates DCIR at configurable time points from cell design parameters.

## Overview

Simulates DCIR pulses at specified SOC, temperature, and C-rate combinations, returning DCIR values at multiple time points (default: 0.1s, 1s, 10s, 18s, 30s). Supports single-point and parametric sweep simulations with automatic capacity calibration.

### Protocol

```
1. Compute equilibrium KPIs (electrode balancing, stoichiometry windows)
2. Build PyBaMM parameters from cell design
3. Capacity calibration (iterative electrode width adjustment)
4. DCIR sweep across SOC/temperature/C-rate combinations
5. Extract DCIR at specified time points for each condition
```

## Input Schema

### Top-Level Structure

The input uses a nested structure with two top-level keys:

- `cell_parameters`: `CellParametersInput` - Cell design parameters
- `simulation_parameters`: `SimulationParameters` - Simulation configuration including DCIR conditions

### Cell Parameters (`CellParametersInput`)

Structured nested input that mirrors the `cell_performance.py` schema.
Nested under `cell_parameters` in the top-level input.

#### Electrode Configuration (required)

| Field | Type | Description |
|---|---|---|
| `positive_electrode_mass_loading_mg_cm2` | `float` | Positive mass loading [mg/cm²] |
| `negative_electrode_mass_loading_mg_cm2` | `float` | Negative mass loading [mg/cm²] |
| `positive_electrode_sheet_count` | `int` | Positive sheet count |
| `negative_electrode_sheet_count` | `int` | Negative sheet count |
| `jelly_roll_count` | `int` | Number of jelly rolls (default: 1) |
| `electrode_coating_side_count` | `int` | Number of coating sides (default: 2) |

#### Dimensions

| Field | Type | Description |
|---|---|---|
| `form_factor` | `Literal` | "Pouch", "Prismatic", "Cylindrical", or "Coin" (default: "Pouch") |
| `cell_width_mm` | `float` or None | Cell width or diameter for cylindrical [mm] |
| `cell_height_mm` | `float` or None | Cell height [mm] |
| `cell_thickness_mm` | `float` or None | Cell thickness [mm] (for pouch/prismatic) |
| `cell_diameter_mm` | `float` or None | Cell diameter [mm] (for cylindrical/coin) |

#### Electrode Dimensions (Optional)

| Field | Type | Description |
|---|---|---|
| `positive_electrode_width_mm` | `float` or `None` | Positive electrode width [mm]. If provided, used directly for area calculation instead of deriving from cell dimensions. Also used for negative electrode if negative-specific dimensions not provided. |
| `positive_electrode_height_mm` | `float` or `None` | Positive electrode height [mm]. If provided, used directly for area calculation instead of deriving from cell dimensions. Also used for negative electrode if negative-specific dimensions not provided. |
| `negative_electrode_width_mm` | `float` or `None` | Negative electrode width [mm]. If provided, used directly for area calculation. If not provided, positive electrode width is used. |
| `negative_electrode_height_mm` | `float` or `None` | Negative electrode height [mm]. If provided, used directly for area calculation. If not provided, positive electrode height is used. |

#### Coating Thickness (required)

| Field | Type | Description |
|---|---|---|
| `positive_coating_thickness_um` | `float` | Positive electrode coating thickness [μm] |
| `negative_coating_thickness_um` | `float` | Negative electrode coating thickness [μm] |

#### Formulation (required)

Each formulation is a list of structured objects with properties:

**Active Material** (required field: `positive_electrode_active_materials`, `negative_electrode_active_materials`)
- `name` -- Material name (e.g. "NMC811_Generic_v1", "Graphite_Generic_v1")
- `mass_fraction` -- Mass fraction in electrode (0-1)
- `density_g_cm3` -- True density [g/cm³]
- `specific_capacity_mAh_g` -- Reversible specific capacity [mAh/g]
- `nominal_voltage_V` -- Midpoint voltage vs Li/Li⁺ [V]
- `soc_pct` -- State of charge points [%] (optional, for OCV override)
- `ch_ocv_V` -- Charge OCV [V] (optional, paired with soc_pct)
- `dch_ocv_V` -- Discharge OCV [V] (optional, paired with soc_pct)

**Binder** (required field: `positive_electrode_binders`, `negative_electrode_binders`)
- `name` -- Binder name (e.g. "PVDF")
- `mass_fraction` -- Mass fraction in electrode (0-1)
- `density_g_cm3` -- Density [g/cm³]

**Conductive Agent** (required for positive, optional for negative)
- `name` -- Conductive agent name (e.g. "Carbon_Black")
- `mass_fraction` -- Mass fraction in electrode (0-1)
- `density_g_cm3` -- Density [g/cm³]

#### Separators & Foils (optional, have defaults)

| Field | Type | Default | Description |
|---|---|---|---|
| `separator_sheet_count` | `float` or None | None | Separator sheet count. If not provided, calculated as `pos_count + neg_count + 1` |
| `separator_sheet_width_mm` | `float` or None | None | Separator sheet width [mm]. If not provided, uses positive electrode width + overhang |
| `separator_sheet_height_mm` | `float` or None | None | Separator sheet height [mm]. If not provided, uses positive electrode height + overhang |
| `separator_thickness_um` | `float` | 12.0 | Separator thickness [μm] |
| `separator_porosity` | `float` | 0.42 | Separator porosity |
| `separator_density_g_cm3` | `float` | 0.5 | Separator density [g/cm³] |
| `positive_electrode_foil_thickness_um` | `float` | 14.0 | Positive foil thickness [μm] |
| `negative_electrode_foil_thickness_um` | `float` | 6.0 | Negative foil thickness [μm] |
| `positive_electrode_foil_density_g_cm3` | `float` | 2.7 | Positive foil density [g/cm³] |
| `negative_electrode_foil_density_g_cm3` | `float` | 8.96 | Negative foil density [g/cm³] |
| `volume_packing_ratio` | `float` | 0.95 | Volume packing ratio |
| `electrode_overhang_mm` | `float` | 1.0 | Electrode overhang [mm] |
| `jelly_roll_inner_diameter_mm` | `float` | 4.0 | Inner JR diameter [mm] |

**Separator dimension derivation logic:**
- If both `separator_sheet_width_mm` and `separator_sheet_height_mm` are provided, they are used directly.
- Otherwise, separator dimensions are derived from positive electrode dimensions plus overhang (4 × `electrode_overhang_mm` total, accounting for 2 × overhang per side).
- For cylindrical cells, separator area approximates positive electrode area.

#### Optional (cell_parameters)

| Field | Type | Description |
|---|---|---|
| `electrolyte` | `Electrolyte` or None | Electrolyte composition (optional) |
| `positive_electrode_pore_tortuosity` | `float` or None | Pore tortuosity (optional) |
| `negative_electrode_pore_tortuosity` | `float` or None | Pore tortuosity (optional) |
| `separator_tortuosity` | `float` or None | Separator tortuosity (optional) |
| `upper_voltage_cutoff_V` | `float` or None | Upper voltage limit [V] (default: 4.2) |
| `lower_voltage_cutoff_V` | `float` or None | Lower voltage limit [V] (default: 3.0) |

**Note:** `nominal_capacity_Ah` is computed internally from electrode design parameters (mass loading, sheet count, active material specific capacity) and is not required as input.

### Simulation Parameters (`SimulationParameters`)

Nested under `simulation_parameters` in the top-level input.

#### DCIR Conditions (`DCIRConditions`)

Nested under `simulation_parameters.dcir_conditions`. All fields are lists to support parametric sweeps:

| Field | Type | Description |
|---|---|---|
| `dcir_soc_pct` | `list[float]` | SOC values [%] (0-100) for DCIR measurements |
| `dcir_temperature_K` | `list[float]` | Temperature values [K] for DCIR measurements |
| `dcir_c_rate` | `list[float]` | C-rate values for DCIR pulse (positive = discharge, negative = charge) |
| `dcir_direction` | `list[str]` | Pulse direction: "discharge" or "charge" |
| `dcir_pulse_duration_s` | `list[float]` | Pulse duration [s] |
| `dcir_time_points_s` | `list[float]` | Time points [s] at which to extract DCIR (default: [0.1, 1.0, 10.0, 18.0, 30.0]) |

The model performs a full parametric sweep across all combinations of SOC, temperature, and C-rate.

#### Operating Conditions

| Parameter | Default | Description |
|---|---|---|
| `ambient_temperature_K` | 298.15 | Ambient temperature [K] |
| `initial_cell_temperature_K` | None | Initial cell temperature [K]; if None, uses `ambient_temperature_K` |
| `reference_cell_temperature_K` | 298.15 | Reference temperature for performance test [K] |
| `upper_voltage_cutoff_V` | 4.2 | Upper voltage limit [V] |
| `lower_voltage_cutoff_V` | 3.0 | Lower voltage limit [V] |
| `cell_contact_resistance_Ohm` | 1e-4 | Cell contact resistance [Ohm] |

#### Thermal

| Parameter | Default | Description |
|---|---|---|
| `cell_heat_transfer_coefficient_W_m2_K` | 10.0 | Cell heat transfer coefficient [W/(m²·K)] |
| `cell_cooling_surface_area_m2` | 0.02 | Cell cooling surface area [m²] |

#### Solver

| Parameter | Default | Description |
|---|---|---|
| `solver_atol` | 1e-3 | Solver absolute tolerance |
| `solver_rtol` | 1e-3 | Solver relative tolerance |
| `data_sampling_period_s` | 0.1 | Output data sampling period [s] |
| `use_pybamm_params` | "" | PyBaMM parameter set name (e.g. `"Chen2020"`, `"Prada2013"`, `"ORegan2022"`); empty string uses Chen2020 as base with custom overrides |

#### Safety Terminations

| Parameter | Default | Description |
|---|---|---|
| `anode_potential_safety_threshold_V` | None | Anode potential safety limit [V]; terminates if anode potential drops below this; set to `null` to disable |
| `temperature_safety_threshold_K` | None | Temperature safety limit [K]; terminates if cell temperature exceeds this; set to `null` to disable |

#### Capacity Calibration

| Parameter | Default | Description |
|---|---|---|
| `calibration_rate_C` | 0.05 | C-rate for capacity calibration experiment [C] |

#### Reference Performance Test (RPT) Parameters (Optional)

| Parameter | Default | Description |
|---|---|---|
| `perform_rpt` | `false` | Whether to perform reference performance test (RPT) |
| `rpt_dcir.dcir_c_rate` | 2.0 | DCIR pulse C-rate for RPT |
| `rpt_dcir.dcir_direction` | "discharge" | DCIR pulse direction ("charge" or "discharge") |
| `rpt_dcir.dcir_soc_pct` | 50.0 | DCIR pulse SOC [%] |
| `rpt_dcir.dcir_temperature_K` | 298.15 | DCIR pulse temperature [K] |
| `rpt_dcir.dcir_pulse_duration_s` | 10.0 | DCIR pulse duration [s] |
| `rpt_dcir.dcir_rest_s` | 1.0 | Rest time after DCIR pulse [s] |
| `rpt_power.power_level_W` | 2000.0 | Power pulse level [W] |
| `rpt_power.power_duration_s` | 300.0 | Power pulse duration [s] |
| `rpt_power.power_direction` | "discharge" | Power pulse direction ("charge" or "discharge") |
| `rpt_power.power_soc_pct` | 50.0 | Power pulse SOC [%] |
| `rpt_power.power_temperature_K` | 298.15 | Power pulse temperature [K] |

RPT parameters are used for reference performance testing and are separate from the main DCIR sweep simulation.

## Output Schema

### Top-Level (`SpmetDCIROutput`)

| Field | Type | Description |
|---|---|---|
| `success` | `bool` | Whether all simulations completed successfully |
| `results` | `list[dict]` | List of result dicts, one per SOC/temperature/C-rate combination |
| `conditions` | `dict` | Summary of input conditions and thresholds |
| `error` | `str` or None | Concatenated error messages from failed simulations |

### Result Items (`results`)

Each item in the `results` list contains:

| Field | Type | Description |
|---|---|---|
| `success` | `bool` | Whether this specific simulation succeeded |
| `dcir_mOhm` | `dict[str, float]` | DCIR values [mΩ] at each time point (keys are time point strings, e.g. "0.1", "1.0", "10.0") |
| `conditions` | `dict` | Condition for this result: `soc_%`, `temperature_K`, `c_rate` |
| `error` | `str` or None | Error message if simulation failed |

### Conditions Summary (`conditions`)

| Field | Type | Description |
|---|---|---|
| `soc_%` | `list[float]` | Sorted list of SOC values tested |
| `ambient_temperature_K` | `list[float]` | Sorted list of temperature values tested |
| `c_rate` | `list[float]` | Sorted list of C-rate values tested |
| `contact_resistance_Ohm` | `float` | Contact resistance used [Ohm] |
| `pulse_duration_s` | `float` | Pulse duration [s] |
| `dcir_time_points_s` | `list[float]` | Time points at which DCIR was extracted [s] |
| `safety_threshold_anode_potential_V` | `float` or None | Anode potential safety threshold [V] |
| `safety_threshold_jelly_roll_temperature_K` | `float` or None | Temperature safety threshold [K] |
| `use_model_parameters` | `str` | PyBaMM parameter set name used |

## Default Configuration

### Cell Parameters: Pouch 80Ah-style

The default cell configuration uses standalone Pouch 80Ah-style parameters.
Example structure:

```json
{
  "cell_parameters": {
    "positive_electrode_mass_loading_mg_cm2": 25.126,
    "negative_electrode_mass_loading_mg_cm2": 18.0,
    "positive_electrode_sheet_count": 18.0,
    "negative_electrode_sheet_count": 19.0,
    "form_factor": "Pouch",
    "cell_width_mm": 503.546,
    "cell_height_mm": 88.971,
    "cell_thickness_mm": 9.012,
    "positive_coating_thickness_um": 87.3,
    "negative_coating_thickness_um": 115.3,
    "positive_electrode_active_materials": [
      {
        "name": "NMC811_Generic_v1",
        "mass_fraction": 0.89,
        "density_g_cm3": 4.8,
        "specific_capacity_mAh_g": 200.0,
        "nominal_voltage_V": 3.77
      }
    ],
    "negative_electrode_active_materials": [
      {
        "name": "Graphite_Generic_v1",
        "mass_fraction": 0.91,
        "density_g_cm3": 2.26,
        "specific_capacity_mAh_g": 365.0,
        "nominal_voltage_V": 0.1
      }
    ],
    "positive_electrode_binders": [
      {"name": "PVDF_Generic_v1", "mass_fraction": 0.055, "density_g_cm3": 1.78}
    ],
    "negative_electrode_binders": [
      {"name": "CMC_Generic_v1", "mass_fraction": 0.045, "density_g_cm3": 1.6},
      {"name": "SBR_Generic_v1", "mass_fraction": 0.045, "density_g_cm3": 0.94}
    ],
    "positive_electrode_conductive_agents": [
      {"name": "CarbonBlack_Generic_v1", "mass_fraction": 0.055, "density_g_cm3": 1.9}
    ],
    "separator_thickness_um": 18.0,
    "separator_porosity": 0.4,
    "separator_density_g_cm3": 0.95
  }
}
```

**Chemistry:** NMC811 / Graphite  
**Nominal Capacity:** ~80.5 Ah (computed from electrode design)  
**Voltage Range:** 2.5V - 4.2V (configurable)  
**Form Factor:** Pouch (88.97mm x 503.5mm electrodes)

### DCIR Conditions: Default Sweep

```json
{
  "simulation_parameters": {
    "dcir_conditions": {
      "dcir_soc_pct": [50, 80],
      "dcir_temperature_K": [298.15, 318.15],
      "dcir_c_rate": [1.0],
      "dcir_direction": ["discharge"],
      "dcir_pulse_duration_s": [30],
      "dcir_time_points_s": [0.1, 1.0, 10.0, 18.0, 30.0]
    }
  }
}
```

This performs 4 simulations (2 SOC × 2 temperature) with 1 C-rate and 1 direction.

## Example Results

```
Pouch 80Ah NMC811, SOC 50-80%, Temperature 25-45°C, 1C discharge

Results: 4 simulations
- SOC 50%, 25°C: DCIR = {0.1s: 0.85 mΩ, 1.0s: 0.92 mΩ, 10.0s: 1.05 mΩ, 18.0s: 1.12 mΩ, 30.0s: 1.18 mΩ}
- SOC 50%, 45°C: DCIR = {0.1s: 0.78 mΩ, 1.0s: 0.84 mΩ, 10.0s: 0.95 mΩ, 18.0s: 1.01 mΩ, 30.0s: 1.06 mΩ}
- SOC 80%, 25°C: DCIR = {0.1s: 0.72 mΩ, 1.0s: 0.78 mΩ, 10.0s: 0.88 mΩ, 18.0s: 0.94 mΩ, 30.0s: 0.99 mΩ}
- SOC 80%, 45°C: DCIR = {0.1s: 0.66 mΩ, 1.0s: 0.71 mΩ, 10.0s: 0.80 mΩ, 18.0s: 0.85 mΩ, 30.0s: 0.89 mΩ}

Runtime: ~30-60 seconds (calibration + 4 simulations)
```

---

*[← Back to Models Library](Model-Library)*
