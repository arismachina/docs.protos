# Cell Performance Model <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

[← Models Library](Model-Library) · **Cell Performance**

Computes cell KPIs from electrode design inputs, with optional electrochemical simulation.

---

---

## Overview

The Cell Performance Model computes equilibrium KPIs (capacity, energy, N/P ratio, mass, energy density) from cell design inputs: electrode formulation (active materials, binders, conductive agents), geometry (mass loading, thickness, sheet count), and cell dimensions (Pouch, Cylindrical, Prismatic, or Coin). It derives porosity, volume fractions, and mass breakdown from the formulation without requiring electrochemical simulation.

When electrode area > 0 (and PyBaMM is installed), the model runs a PyBaMM SPMe simulation pipeline producing nominal capacity, DCIR, maximum 300 s power, and arbitrary multi-step/multi-cycle experiments (including drive cycles and EIS). The model supports extensive PyBaMM parameter overrides: electrolyte transport properties, active material diffusivity/particle size/kinetics/thermodynamics (MSMR), electrode tortuosity, double-layer capacitance (for LIC/hybrid cells), and half-cell mode with configurable counter electrode.

---

## Input Structure

`CellPerformanceInput` has two top-level sections:

- **`cell_parameters`** (`CellParametersInput`): Electrode design, formulation, dimensions, foils, separator
- **`simulation_parameters`** (`SimulationParameters`): Solver, mesh, RPT, experiments, half-cell, C_dl

---

## `cell_parameters`

### Required Electrode Fields

| Parameter | Type | Description |
|-----------|------|-------------|
| `positive_electrode_mass_loading_mg_cm2` | float | Positive electrode mass loading [mg/cm²] |
| `negative_electrode_mass_loading_mg_cm2` | float | Negative electrode mass loading [mg/cm²] |
| `positive_electrode_sheet_count` | int | Number of positive electrode sheets |
| `negative_electrode_sheet_count` | int | Number of negative electrode sheets |
| `jelly_roll_count` | int | Number of jelly rolls in the cell |
| `electrode_coating_side_count` | int | Number of coating sides per electrode (1 or 2) |
| `positive_coating_thickness_um` | float | Positive coating thickness [µm] |
| `negative_coating_thickness_um` | float | Negative coating thickness [µm] |
| `positive_electrode_specific_heat_capacity_J_kg_K` | float | Positive electrode specific heat [J/kg/K] |
| `negative_electrode_specific_heat_capacity_J_kg_K` | float | Negative electrode specific heat [J/kg/K] |
| `positive_electrode_thermal_conductivity_W_m_K` | float | Positive electrode thermal conductivity [W/m/K] |
| `negative_electrode_thermal_conductivity_W_m_K` | float | Negative electrode thermal conductivity [W/m/K] |
| `positive_electrode_electronic_conductivity_S_m` | float | Positive electrode electronic conductivity [S/m] |
| `negative_electrode_electronic_conductivity_S_m` | float | Negative electrode electronic conductivity [S/m] |
| `upper_voltage_cutoff_V` | float | Upper voltage cutoff [V] |
| `lower_voltage_cutoff_V` | float | Lower voltage cutoff [V] |

### Formulation

| Parameter | Type | Description |
|-----------|------|-------------|
| `positive_electrode_active_materials` | list[ActiveMaterial] | Positive active material list (**required**) |
| `negative_electrode_active_materials` | list[ActiveMaterial] | Negative active material list (**required**) |
| `positive_electrode_binders` | list[Binder] | Positive binders (**required**) |
| `negative_electrode_binders` | list[Binder] | Negative binders (**required**) |
| `positive_electrode_conductive_agents` | list[ConductiveAgent] | Positive conductive agents (**required**, pass `[]` if none) |
| `negative_electrode_conductive_agents` | list[ConductiveAgent] | Negative conductive agents (**required**, pass `[]` if none) |

#### ActiveMaterial

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | str | Yes | Material name (e.g. NMC811, Graphite, LFP) |
| `mass_fraction` | float | Yes | Mass fraction in electrode (0–1) |
| `density_g_cm3` | float | Yes | Density [g/cm³] |
| `specific_capacity_mAh_g` | float | Yes | Specific capacity [mAh/g] |
| `nominal_voltage_V` | float | Yes | Nominal voltage [V] |
| `soc_pct` | list[float] \| null | No | SOC [0–100%] for OCV |
| `ch_ocv_V` | list[float] \| null | No | OCV charge curve [V] |
| `dch_ocv_V` | list[float] \| null | No | OCV discharge curve [V] |
| `ocv_specific_capacity_mAh_g` | list[float] \| null | No | Specific capacity at each OCV point [mAh/g] |
| `max_lithium_conc_mol_m3` | float \| null | No | Max Li concentration [mol/m³] for PyBaMM |
| `diffusivity_m2_s` | float \| IonicPropertyTable \| null | No | Particle diffusivity [m²/s]; scalar or tabulated |
| `particle_size_distribution` | ParticleSizeDistribution \| null | No | PSD; d50 used for PyBaMM radius = d50/2 |
| `exchange_current_density_A_m2` | float \| null | No | Exchange current density [A/m²] |
| `double_layer_capacitance_F_m2` | float \| null | No | Per-material double-layer capacitance [F/m²] |

#### ParticleSizeDistribution

| Field | Type | Description |
|-------|------|-------------|
| `d50_um` | float | Median diameter [µm]; PyBaMM radius = d50/2 |
| `d10_um` | float \| null | 10th percentile [µm] |
| `d90_um` | float \| null | 90th percentile [µm] |
| `dmin_um` | float \| null | Min diameter [µm] |
| `dmax_um` | float \| null | Max diameter [µm] |

#### Binder / ConductiveAgent

| Field | Type | Description |
|-------|------|-------------|
| `name` | str | Component name |
| `mass_fraction` | float | Mass fraction (0–1) |
| `density_g_cm3` | float | Density [g/cm³] |

### Cell Dimensions

| Parameter | Type | Description |
|-----------|------|-------------|
| `form_factor` | str | **required** — `"Pouch"`, `"Prismatic"`, `"Cylindrical"`, or `"Coin"` |
| `cell_width_mm` | float \| null | Cell width [mm] (Pouch/Prismatic); default null |
| `cell_height_mm` | float \| null | Cell height [mm] (Pouch/Prismatic), cylinder height, or coin thickness; default null |
| `cell_thickness_mm` | float \| null | Cell thickness [mm] (Pouch/Prismatic); default null |
| `cell_diameter_mm` | float \| null | Cell diameter [mm] (Cylindrical/Coin); default null |

**Volume validation** (enforced when `perform_rpt=True` or at least one experiment is configured): Pouch/Prismatic require all three of width/height/thickness; Cylindrical/Coin require both diameter and height. Equilibrium-only runs (no PyBaMM) skip this check.

### Electrode Dimensions (Optional)

If provided, used directly for area calculation; otherwise derived from cell dimensions via `volume_packing_ratio` and `electrode_overhang_mm`.

| Parameter | Type | Description |
|-----------|------|-------------|
| `positive_electrode_width_mm` | float \| null | Positive electrode width [mm] |
| `positive_electrode_height_mm` | float \| null | Positive electrode height [mm] |
| `negative_electrode_width_mm` | float \| null | Negative electrode width [mm] |
| `negative_electrode_height_mm` | float \| null | Negative electrode height [mm] |

### Foils, Separator, Electrolyte

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `positive_electrode_foil_thickness_um` | float | **required** | Positive foil [µm] (Al) |
| `negative_electrode_foil_thickness_um` | float | **required** | Negative foil [µm] (Cu) |
| `positive_electrode_foil_density_g_cm3` | float | **required** | Al foil density [g/cm³] |
| `negative_electrode_foil_density_g_cm3` | float | **required** | Cu foil density [g/cm³] |
| `positive_electrode_foil_electronic_conductivity_S_m` | float | **required** | Al foil electronic conductivity [S/m] |
| `negative_electrode_foil_electronic_conductivity_S_m` | float | **required** | Cu foil electronic conductivity [S/m] |
| `positive_electrode_foil_specific_heat_capacity_J_kg_K` | float | **required** | Al foil specific heat [J/kg/K] |
| `negative_electrode_foil_specific_heat_capacity_J_kg_K` | float | **required** | Cu foil specific heat [J/kg/K] |
| `positive_electrode_foil_thermal_conductivity_W_m_K` | float | **required** | Al foil thermal conductivity [W/m/K] |
| `negative_electrode_foil_thermal_conductivity_W_m_K` | float | **required** | Cu foil thermal conductivity [W/m/K] |
| `separator_thickness_um` | float | **required** | Separator thickness [µm] |
| `separator_porosity` | float | **required** | Separator porosity |
| `separator_density_g_cm3` | float | **required** | Separator density [g/cm³] |
| `separator_specific_heat_capacity_J_kg_K` | float | **required** | Separator specific heat [J/kg/K] |
| `separator_thermal_conductivity_W_m_K` | float | **required** | Separator thermal conductivity [W/m/K] |
| `separator_sheet_count` | float | **required** | Number of separator sheets |
| `separator_sheet_width_mm` | float | **required** | Separator sheet width [mm] |
| `separator_sheet_height_mm` | float | **required** | Separator sheet height [mm] |
| `electrolyte_density_g_cm3` | float | **required** | Electrolyte density [g/cm³] (fallback when no `electrolyte` object) |
| `electrolyte_fill_ratio` | float | **required** | Pore volume fill fraction |
| `electrolyte` | Electrolyte \| null | null | Optional electrolyte with composition + transport properties |
| `casing_thickness_mm` | float | **required** | Casing thickness [mm] |
| `casing_density_g_cm3` | float | **required** | Casing density [g/cm³] |
| `electrode_overhang_mm` | float | **required** | Electrode overhang [mm] |
| `jelly_roll_inner_diameter_mm` | float | **required** | Jelly roll inner diameter [mm] (cylindrical only) |
| `volume_packing_ratio` | float | **required** | Volume packing ratio for jelly roll in cell |

### Electrolyte (Optional)

| Field | Type | Description |
|-------|------|-------------|
| `name` | str | Electrolyte name |
| `composition` | ElectrolyteComposition | Solvents, salts, additives |
| `ionic_conductivity_S_m` | float \| IonicPropertyTable \| null | Ionic conductivity [S/m] |
| `ionic_diffusivity_m2_s` | float \| IonicPropertyTable \| null | Ionic diffusivity [m²/s] |
| `transference_number` | float \| IonicPropertyTable \| null | Cation transference number |
| `activity_coefficient` | float \| IonicPropertyTable \| null | Activity coefficient → Thermodynamic factor |

### Electrode Tortuosity

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `positive_electrode_pore_tortuosity` | float \| null | null | Positive electrode electrolyte tortuosity |
| `negative_electrode_pore_tortuosity` | float \| null | null | Negative electrode electrolyte tortuosity |
| `separator_tortuosity` | float \| null | null | Separator electrolyte tortuosity |
| `positive_electrode_solid_tortuosity` | float \| null | null | Positive solid-phase tortuosity (defaults to pore if unset) |
| `negative_electrode_solid_tortuosity` | float \| null | null | Negative solid-phase tortuosity (defaults to pore if unset) |

When any tortuosity is set, PyBaMM uses `transport efficiency: tortuosity factor`, overriding Bruggeman.

### Cell-Level OCV

| Field | Type | Description |
|-------|------|-------------|
| `cell_ocv` | CellOCVData \| null | Cell-level OCV (soc_pct, specific_capacity_mAh_g, ch_ocv_V, dch_ocv_V) |

Used for OCV100/OCV0 extraction when `use_pybamm_params` is empty.

---

## `simulation_parameters`

### Core Solver

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `solver_atol` | float | 1e-3 | Solver absolute tolerance |
| `solver_rtol` | float | 1e-3 | Solver relative tolerance |
| `calibration_rate_C` | float | **required** | Capacity calibration C-rate |
| `mesh_resolution` | dict | x_n/x_s/x_p/r_n/r_p=10 | Mesh resolution (x = electrode/separator, r = particles) |
| `data_sampling_period_s` | float | 0.1 | Data sampling period [s] |
| `use_pybamm_params` | str | `""` | Base parameter set (e.g. `"Chen2020"`). Empty = use defaults/custom |
| `upper_voltage_cutoff_V` | float | **required** | Upper voltage cutoff [V] |
| `lower_voltage_cutoff_V` | float | **required** | Lower voltage cutoff [V] |
| `cell_contact_resistance_Ohm` | float \| null | **required** | Contact resistance [Ω] (pass `null` to use PyBaMM default) |
| `cell_cooling_surface_area_m2` | float | **required** | Cooling surface area [m²] |
| `cell_heat_transfer_coefficient_W_m2_K` | float | **required** | Heat transfer coefficient [W/(m²·K)] |
| `temperature_safety_threshold_K` | float | **required** | Max power temperature limit [K] |
| `anode_potential_safety_threshold_V` | float | **required** | Max power anode potential limit [V] |
| `first_cycle_coulombic_loss_pct` | float | **required** | First-cycle coulombic loss [%] for initial electrode balancing |
| `ambient_temperature_K` | float | **required** | Ambient temperature [K] |
| `reference_cell_temperature_K` | float | **required** | Reference temperature for performance test [K] |
| `start_soc_pct` | float \| null | **required** | Starting SOC for load cycle simulation [%] (pass `null` to skip) |

### RPT (Reference Performance Test)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `perform_rpt` | bool | false | Run DCIR and max power tests |
| `rpt_dcir` | DCIRParameters \| null | **required** | DCIR pulse parameters (must be non-null when `perform_rpt=true`) |
| `rpt_power` | PowerPulseParameters \| null | **required** | Max power pulse parameters (must be non-null when `perform_rpt=true`) |

**DCIRParameters** (`rpt_dcir`):

| Field | Type | Description |
|-------|------|-------------|
| `dcir_c_rate` | float | Pulse C-rate (**required**) |
| `dcir_direction` | str | `"charge"` or `"discharge"` (**required**) |
| `dcir_soc_pct` | float | SOC for pulse [%] (**required**) |
| `dcir_temperature_K` | float | Temperature [K] (**required**) |
| `dcir_pulse_duration_s` | float | Pulse duration [s] (**required**) |
| `dcir_rest_s` | float | Rest time before pulse [s] (**required**) |

**PowerPulseParameters** (`rpt_power`):

| Field | Type | Description |
|-------|------|-------------|
| `power_level_W` | float | Initial power for bisection search [W] (**required**) |
| `power_duration_s` | float | Pulse duration [s] (**required**) |
| `power_direction` | str | `"charge"` or `"discharge"` (**required**) |
| `power_soc_pct` | float | SOC for pulse [%] (**required**) |
| `power_temperature_K` | float | Temperature [K] (**required**) |

### Experiments

`experiments: list[ExperimentConfig] | None = None`

Arbitrary multi-step, multi-cycle PyBaMM experiments. Independent of `perform_rpt`. Results in `experiment_results[label]` as `ExperimentOutput`.

#### ExperimentConfig

All fields are **required**.

| Field | Type | Description |
|-------|------|-------------|
| `label` | str | Key in `experiment_results` |
| `steps` | list[str \| DriveStepConfig \| EISStepConfig] | Ordered steps for one cycle (min 1) |
| `n_cycles` | int | Number of times to repeat the step sequence (≥1) |
| `terminations` | list[ExperimentTermination] \| null | Custom per-step termination conditions (pass `null` if none) |
| `period_s` | float | Solver output sampling period [s] |
| `temperature_K` | float | Ambient temperature [K] |
| `initial_soc_pct` | float | Initial SOC [%] before experiment begins |

String steps are PyBaMM step expressions:
```
"Discharge at 1C until 3.0 V"
"Charge at 1C until 4.2 V"
"Hold at 4.2 V until C/20"
"Rest for 10 minutes"
"Discharge at 500 mA until 3.0 V"
```

Example — 3-cycle charge/discharge aging:
```json
{
  "label": "cycle_aging",
  "steps": [
    "Discharge at 1C until 3.0 V",
    "Rest for 10 minutes",
    "Charge at 1C until 4.2 V",
    "Hold at 4.2 V until C/20"
  ],
  "n_cycles": 3,
  "terminations": null,
  "period_s": 60.0,
  "temperature_K": 298.15,
  "initial_soc_pct": 100.0
}
```

#### ExperimentTermination

Custom termination evaluated at every solver step. Triggers end-of-step only; experiment continues with subsequent steps.

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `variable` | str | — | PyBaMM variable name, e.g. `"Anode potential [V]"` |
| `threshold` | float | — | Threshold value |
| `comparison` | str | `"less_than"` | `"less_than"` (variable < threshold) or `"greater_than"` (variable > threshold) |
| `name` | str \| null | null | Human-readable name for logging |

Example — protect anode from lithium plating:
```json
{"variable": "Anode potential [V]", "threshold": 0.0, "comparison": "less_than"}
```

#### DriveStepConfig

Time-varying drive cycle step. Exactly one of `current_A`, `power_W`, or `c_rate` must be supplied. Profile length must match `time_s` length.

| Field | Type | Description |
|-------|------|-------------|
| `kind` | `"drive_cycle"` | Fixed discriminator |
| `time_s` | list[float] | Strictly increasing time points [s] (≥ 2 points) |
| `current_A` | list[float] \| null | Current profile [A]. Positive = discharge |
| `power_W` | list[float] \| null | Power profile [W]. Positive = discharge |
| `c_rate` | list[float] \| null | C-rate profile. Positive = discharge |

Example — 30 s power pulse:
```json
{"kind": "drive_cycle", "time_s": [0, 30, 30.01, 60], "power_W": [50.0, 50.0, 0.0, 0.0]}
```

#### EISStepConfig

Frequency-domain impedance sweep at the current battery state (does not advance simulation time). Requires `pybammeis` (`pip install pybammeis`). If absent, the step is skipped and a warning is added to `ExperimentOutput.warnings`. If an experiment contains **only** EIS steps and `pybammeis` is unavailable, an empty `ExperimentOutput` is returned immediately with a warning.

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `kind` | `"eis"` | — | Fixed discriminator |
| `freq_min_hz` | float | 1e-4 | Minimum frequency [Hz] |
| `freq_max_hz` | float | 1e4 | Maximum frequency [Hz] |
| `n_frequencies` | int | 30 | Log-spaced frequency points (≥ 2) |
| `method` | str | `"direct"` | Linear solver: `"direct"`, `"prebicgstab"`, or `"bicgstab"` |

Example — EIS after rest:
```json
{
  "steps": [
    "Rest for 30 minutes",
    {"kind": "eis", "freq_min_hz": 0.01, "freq_max_hz": 10000, "n_frequencies": 50}
  ]
}
```

EIS steps require the calibration pipeline to have run (need `sto_windows`). They are excluded from the PyBaMM fast-path and do not count toward `n_steps_per_cycle`. On cycle failure, EIS snapshots from the failed cycle are rolled back alongside timeseries data.

---

### Double-Layer Capacitance (C_dl / LIC)

Adds a non-Faradaic capacitive current term: `j_total = j_BV + C_dl · d(φ_s − φ_e)/dt`. Required for hybrid lithium-ion capacitor (LIC) simulations (e.g. activated-carbon EDLC positive + hard-carbon intercalation negative).

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `enable_double_layer_capacitance` | bool | false | Enable C_dl in the SPMe model |
| `positive_electrode_double_layer_capacitance_F_m2` | float \| null | null | Positive electrode C_dl [F/m²]. PyBaMM default ~0.2 F/m² when enabled |
| `negative_electrode_double_layer_capacitance_F_m2` | float \| null | null | Negative electrode C_dl [F/m²]. PyBaMM default ~0.2 F/m² when enabled |

Typical values: activated carbon EDLC ~0.1–0.4 F/m²; hard carbon / intercalation electrode ~0.01–0.05 F/m².

C_dl can also be set **per active material** via `ActiveMaterial.double_layer_capacitance_F_m2`; the per-material value takes precedence when `enable_double_layer_capacitance=True`.

---

### Half-Cell Mode

Simulates a single electrode against a configurable counter electrode. To simulate a negative electrode half-cell (e.g. graphite vs Li metal), load the negative electrode parameters into the positive electrode slots.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `working_electrode` | `"positive"` \| null | null | `"positive"` = half-cell mode; null = full-cell |
| `reference_electrode_exchange_current_density_A_m2` | float \| null | null | Counter electrode j₀ [A/m²]. Default: Xu2019 Li-metal function |
| `reference_electrode_ocp_V` | float \| null | null | Counter electrode OCP [V]. Default: 0.0 V (Li metal) |
| `reference_electrode_charge_transfer_coefficient` | float \| null | null | Butler-Volmer α (0–1). Default: 0.5 (symmetric kinetics) |

Constraints:
- `working_electrode="positive"` is incompatible with `perform_rpt=True`
- `reference_electrode_*` fields raise a validation error if set with `working_electrode=None`
- Set `reference_electrode_ocp_V` for non-Li-metal counters (Na metal: ~0.3 V vs Li/Li+)
- When `working_electrode="positive"`, the negative-electrode OCP/material overrides from `negative_electrode_active_materials` are skipped; the counter-electrode OCP injected via `reference_electrode_ocp_V` (or the Xu2019 Li-metal default) is preserved unchanged

---

## Output Structure (`CellPerformanceKPIs`)

### Equilibrium Outputs (always present)

| Field | Type | Description |
|-------|------|-------------|
| `cell_theoretical_capacity_Ah` | float | Equilibrium-derived capacity [Ah] |
| `cell_theoretical_energy_Wh` | float | Equilibrium-derived energy [Wh] |
| `cell_capacity_weighted_voltage_V` | float | Capacity-weighted average voltage [V] |
| `cell_n_p_ratio` | float | N/P capacity ratio |
| `cell_mass_g` | float | Total cell mass [g] |
| `cell_volume_L` | float | Cell volume [L] |
| `cell_theoretical_gravimetric_energy_density_Wh_kg` | float | [Wh/kg] |
| `cell_theoretical_volumetric_energy_density_Wh_L` | float | [Wh/L] |
| `stoichiometry_windows` | StoichiometryWindows | Electrode stoichiometry windows (sto_ca0/100, sto_an0/100) |
| `positive_electrode_porosity` | float | Positive electrode porosity |
| `negative_electrode_porosity` | float | Negative electrode porosity |
| `positive_electrode_volume_fraction` | float | Positive active material volume fraction |
| `negative_electrode_volume_fraction` | float | Negative active material volume fraction |
| `electrolyte_mass_g` | float | Electrolyte mass [g] |
| `electrolyte_mass_fraction` | float | Electrolyte mass fraction |
| `jelly_roll_mass_g` | float | Jelly roll mass [g] |
| `jelly_roll_mass_fraction` | float | Jelly roll mass fraction |
| `casing_mass_g` | float | Casing mass [g] |
| `casing_mass_fraction` | float | Casing mass fraction |
| `bill_of_materials` | BillOfMaterials | Full BOM breakdown (AM, binders, conductive, separator, foils, electrolyte, casing) |

### PyBaMM Scalar Outputs

| Field | Type | Description |
|-------|------|-------------|
| `cell_nominal_capacity_Ah` | float \| null | Nominal capacity from C/3 discharge [Ah] |
| `cell_nominal_energy_Wh` | float \| null | Nominal energy from C/3 discharge [Wh] |
| `cell_nominal_gravimetric_energy_density_Wh_kg` | float \| null | [Wh/kg] |
| `cell_nominal_volumetric_energy_density_Wh_L` | float \| null | [Wh/L] |
| `cell_nominal_max_discharge_power_300s_W` | float \| null | Max 300 s power [W] (absolute value; direction via `power_direction`) |
| `cell_nominal_gravimetric_power_density_W_kg` | float \| null | [W/kg] |
| `cell_nominal_volumetric_power_density_W_L` | float \| null | [W/L] |
| `cell_dcir_10s_mohm` | float \| null | DCIR at configured pulse duration [mΩ] |
| `ocv100_V` | float \| null | Open-circuit voltage at ~100% SOC [V] (measured at SOC=99.9%). `null` if estimation failed |
| `ocv0_V` | float \| null | Open-circuit voltage at 0% SOC [V]. `null` if estimation failed |

### Timeseries Outputs

| Field | Type | Description |
|-------|------|-------------|
| `c3_discharge_timeseries` | TimeseriesOutput \| null | C/3 discharge (RDP-subsampled); excluded from copilot summary |
| `experiment_results` | dict[str, ExperimentOutput] \| null | Multi-cycle experiment results keyed by label |

### TimeseriesOutput

Used by `c3_discharge_timeseries` and `ExperimentOutput.timeseries`.

| Field | Type | Description |
|-------|------|-------------|
| `time_s` | list[float] | Time from step start [s] |
| `voltage_V` | list[float] | Terminal voltage [V] |
| `current_A` | list[float] | Current [A] (positive = discharge) |
| `temperature_K` | list[float] | Spatially-averaged cell temperature [K] |
| `capacity_Ah` | list[float] | Cumulative capacity from step start [Ah] |
| `energy_Wh` | list[float] | Cumulative energy from step start [Wh] |
| `power_W` | list[float] | Instantaneous power [W] |
| `anode_potential_V` | list[float] | Negative electrode surface potential at separator [V] |
| `cc_capacity_Ah` | float \| null | CC-phase capacity [Ah] (CCCV tests only) |
| `total_capacity_Ah` | float \| null | Total CC+CV capacity [Ah] (CCCV tests only) |

### ExperimentOutput

| Field | Type | Description |
|-------|------|-------------|
| `timeseries` | TimeseriesOutput | Concatenated timeseries across all cycles and steps (RDP-subsampled) |
| `cycle_summaries` | list[ExperimentCycleSummary] | Per-cycle capacity and efficiency summaries |
| `n_cycles_completed` | int | Number of full cycles completed before any termination or solver failure |
| `total_capacity_throughput_Ah` | float \| null | Total capacity throughput across all cycles [Ah] |
| `total_energy_throughput_Wh` | float \| null | Total energy throughput across all cycles [Wh] |
| `eis_measurements` | list[EISOutput] | EIS snapshots in execution order. Rolled back on cycle failure |
| `warnings` | list[str] | Non-fatal warnings (e.g. EIS skipped, solver failure on a cycle) |

### ExperimentCycleSummary

| Field | Type | Description |
|-------|------|-------------|
| `cycle_number` | int | 1-indexed cycle number |
| `discharge_capacity_Ah` | float \| null | Discharge throughput [Ah] |
| `charge_capacity_Ah` | float \| null | Charge throughput [Ah] |
| `coulombic_efficiency_pct` | float \| null | 100 × discharge / charge [%] |
| `total_capacity_throughput_Ah` | float \| null | Sum of \|Ah\| across all steps [Ah] |
| `total_energy_throughput_Wh` | float \| null | Sum of \|Wh\| across all steps [Wh] |
| `min_voltage_V` | float \| null | Minimum terminal voltage in this cycle [V] |
| `max_voltage_V` | float \| null | Maximum terminal voltage in this cycle [V] |
| `max_temperature_K` | float \| null | Maximum cell temperature in this cycle [K] |

### EISOutput

| Field | Type | Description |
|-------|------|-------------|
| `frequencies_hz` | list[float] | Frequency points [Hz] |
| `z_real_ohm` | list[float] | Real part of impedance [Ω] |
| `z_imag_ohm` | list[float] | Imaginary part [Ω] (negative = capacitive) |
| `z_magnitude_ohm` | list[float] | \|Z\| magnitude [Ω] |
| `z_phase_deg` | list[float] | Phase angle [°] |
| `cycle_number` | int | 1-indexed cycle in which this was taken |
| `step_index` | int | 0-indexed position of this EIS step in the full step list |
| `soc_pct` | float \| null | Estimated SOC at measurement time [%] |

### BillOfMaterials

Each `BillOfMaterialsItem` has: `name`, `electrode` (`"positive"`, `"negative"`, or null), `mass_g`, `mass_fraction`.

| Field | Description |
|-------|-------------|
| `active_materials` | Active materials per electrode |
| `binders` | Binders per electrode |
| `conductive_agents` | Conductive additives per electrode |
| `separator` | Separator |
| `foils` | Current collectors (Al, Cu) |
| `electrolyte` | Electrolyte |
| `casing` | Casing |

---

## PyBaMM Parameter Overrides

| Source | What it overrides |
|--------|------------------|
| `electrolyte.*` | Ionic conductivity, diffusivity, transference number, activity coefficient |
| `ActiveMaterial.diffusivity_m2_s` | Solid-phase diffusivity |
| `ActiveMaterial.particle_size_distribution.d50_um` | Particle radius (d50/2) |
| `ActiveMaterial.exchange_current_density_A_m2` | Exchange current density |
| `ActiveMaterial.double_layer_capacitance_F_m2` | Per-electrode C_dl (when `enable_double_layer_capacitance=True`) |
| `*_electrode_pore_tortuosity` | Electrolyte-phase tortuosity (overrides Bruggeman) |
| `*_electrode_solid_tortuosity` | Solid-phase tortuosity |
| `ActiveMaterial.{ch,dch}_ocv_V` | OCP function (MSMR fit or interpolant) |
| `working_electrode + reference_electrode_*` | Half-cell counter electrode parameters |
| `enable_double_layer_capacitance + *_F_m2` | C_dl capacitive current term |

---

## Model Configuration

### Parameter Sets

- **Chen2020**: Default for NMC/graphite full-cells
- **Prada2013**: Auto-selected for LFP when `use_pybamm_params` is not specified
- **Custom**: Set `use_pybamm_params=""` and provide electrode/electrolyte data

### Solver

- **IDAKLUSolver** (SUNDIALS-based DAE solver)
- Mesh configurable via `simulation_parameters.mesh_resolution`

---

---

*[← Back to Models Library](Model-Library)*
