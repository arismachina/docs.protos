# SPMeT Dynamic Load Model <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

[← Models Library](Model-Library) · **SPMeT Dynamic Load**

Simulates a cell under an arbitrary power or C-rate drive cycle, returning full timeseries and energy analysis.

---

Physics-based drive cycle simulation using the Single Particle Model with
Electrolyte (SPMe) and lumped thermal model. Supports arbitrary power or
C-rate drive cycles with custom safety terminations.

## Overview

Simulates a single drive cycle on a cell, returning voltage, current, power,
temperature, SOC, and energy timeseries with summary statistics and energy
analysis.

### Protocol

```
1. Capacity calibration (iterative electrode width adjustment)
2. Build drive cycle step (power_W or c_rate array)
3. Apply custom terminations (anode potential, temperature)
4. Solve SPMe with IDAKLUSolver
5. Extract timeseries, summary, energy analysis
```

## Input Schema

### Cell Parameters (`CellParametersInput`)

Structured nested input that mirrors the `cell_performance.py` schema.
Nested under `cell_parameters` in the top-level input.

#### Electrode Configuration (required)

| Field | Type | Description |
|---|---|---|
| `positive_electrode_mass_loading_mg_cm2` | `float` | Positive mass loading [mg/cm²] |
| `negative_electrode_mass_loading_mg_cm2` | `float` | Negative mass loading [mg/cm²] |
| `positive_electrode_sheet_count` | `float` | Positive sheet count (can be fractional) |
| `negative_electrode_sheet_count` | `float` | Negative sheet count (can be fractional) |
| `jelly_roll_count` | `float` | Number of jelly rolls (default: 1.0) |
| `electrode_coating_side_count` | `int` | Number of coating sides (default: 2) |

#### Dimensions

| Field | Type | Description |
|---|---|---|
| `form_factor` | `Literal` | "Pouch", "Prismatic", "Cylindrical", or "Coin" (default: "Pouch") |
| `cell_width_mm` | `float` or None | Cell width or diameter for cylindrical [mm] |
| `cell_height_mm` | `float` or None | Cell height [mm] |
| `cell_thickness_mm` | `float` or None | Cell thickness [mm] (for pouch/prismatic) |
| `cell_diameter_mm` | `float` or None | Cell diameter [mm] (for cylindrical/coin) |

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

#### Optional (cell_parameters)

| Field | Type | Description |
|---|---|---|
| `electrolyte` | `Electrolyte` or None | Electrolyte composition (optional) |
| `positive_electrode_pore_tortuosity` | `float` or None | Pore tortuosity (optional) |
| `negative_electrode_pore_tortuosity` | `float` or None | Pore tortuosity (optional) |
| `separator_tortuosity` | `float` or None | Separator tortuosity (optional) |

**Note:** `nominal_capacity_Ah` is computed internally from electrode design parameters (mass loading, sheet count, active material specific capacity) and is not required as input.

### Load Cycle (`LoadCycleInput`)

| Field | Type | Description |
|---|---|---|
| `time_s` | `list[float]` | Time points [s] |
| `power_W` | `list[float]` or None | Power values [W] (positive = discharge) |
| `c_rate` | `list[float]` or None | C-rate values (positive = discharge) |
| `label` | `str` | Load cycle label (default: "load_cycle") |

Must specify either `power_W` or `c_rate`, not both. The load cycle is nested
inside `simulation_parameters` as `load_cycle`.

### Simulation Parameters (`SimulationParameters`)

Nested under `simulation_parameters` in the top-level input.

#### Load Cycle (nested)

| Field | Type | Description |
|---|---|---|
| `load_cycle` | `LoadCycleInput` | Load cycle profile (power or C-rate) |

#### Operating Conditions

| Parameter | Default | Description |
|---|---|---|
| `ambient_temperature_K` | 298.15 | Ambient temperature [K] |
| `initial_cell_temperature_K` | None | Initial cell temperature [K]; if None, uses `ambient_temperature_K` |
| `reference_cell_temperature_K` | 298.15 | Reference temperature for performance test [K] |
| `start_soc_pct` | 100.0 | Initial state of charge [%] (0–100); seeded model uses 50% |
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
| `anode_potential_safety_threshold_V` | 0.01 | Anode potential safety limit [V]; terminates if anode potential drops below this; set to `null` to disable |
| `temperature_safety_threshold_K` | 363.15 | Temperature safety limit [K]; terminates if cell temperature exceeds this; set to `null` to disable |

### Load Cycle (`LoadCycleInput`)

| Field | Type | Description |
|---|---|---|
| `time_s` | `list[float]` | Time points [s] |
| `power_W` | `list[float]` or None | Power profile [W] (positive = discharge) |
| `c_rate` | `list[float]` or None | C-rate profile (positive = discharge) |
| `label` | `str` | Metadata label |

## Output Schema

### Timeseries (`TimeseriesOutput`)

All arrays have the same length (one value per `data_sampling_period_s`):

- `time_s` -- simulation time [s]
- `voltage_V` -- terminal voltage [V]
- `current_A` -- current [A] (positive = discharge)
- `power_W` -- terminal power [W]
- `temperature_K` -- cell temperature [K]
- `capacity_Ah` -- discharge capacity [A.h]
- `soc` -- state of charge (0-1)
- `energy_Wh` -- discharge energy [W.h]
- `anode_potential_V` -- anode potential [V]

### Summary (`SummaryOutput`)

| Field | Description |
|---|---|
| `duration_s` | Total simulation time |
| `data_points` | Number of timeseries points |
| `voltage_min/max_V` | Voltage range |
| `current_min/max_A` | Current range |
| `power_min/max_W` | Power range |
| `temperature_min/max_K` | Temperature range |
| `soc_start/end` | SOC at start and end |
| `soc_used_pct` | SOC consumed [%] |

### Energy Analysis (`EnergyAnalysisOutput`)

| Field | Description |
|---|---|
| `energy_discharged_Wh` | Total energy discharged |
| `energy_regenerated_Wh` | Total energy regenerated (regen braking) |
| `energy_net_Wh` | Net energy (discharged - regenerated) |
| `capacity_used_Ah` | Total capacity consumed |
| `soc_change_pct` | SOC change [%] |
| `regeneration_ratio_pct` | Regeneration / discharge ratio [%] |

### Top-level (`SpmetDynamicLoadOutput`)

- `success` -- boolean
- `termination_reason` -- "final time", "Lower voltage cut-off", etc.
- `timeseries` -- `TimeseriesOutput`
- `summary` -- `SummaryOutput`
- `energy_analysis` -- `EnergyAnalysisOutput`
- `config` -- echo of simulation parameters
- `error`, `traceback` -- on failure

## Default Configuration

### Cell Parameters: VW ID3 Pouch 80Ah

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
**Voltage Range:** 2.8V - 4.2V  
**Form Factor:** Pouch (88.97mm x 503.5mm electrodes)  
**Parameter Set:** ORegan2022

### Drive Cycle: Aero Quad Drone

- Duration: 599 seconds (~10 minutes)
- Data points: 600 (1 Hz)
- Power range: 24 - 84 W (discharge only, no regen)
- Input type: power_W (positive = discharge)

## Example Results

```
VW ID3 80Ah NMC, Aero Quad Drone, SOC 80%, 25C

Duration: 599s, Points: 600
Voltage: 3.917 - 3.974 V
Temperature: 298.1 - 298.2 K
SOC: 80.0% -> 77.5% (2.52% used)
Energy: discharged=7.99 Wh, net=7.99 Wh
Capacity: 2.025 Ah

Runtime: ~6 seconds (3.5s calibration + 2.5s simulation)
```

---

*[← Back to Models Library](Model-Library)*
