# Cell Optimizer Model <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

[← Models Library](Model-Library) · **Cell Optimizer**

Multi-objective optimisation to find Pareto-optimal cell designs that meet performance targets.

---

Multi-objective optimization using NSGA2 with PyBaMM simulations. Standalone implementation with `CellParameters` and typed target models.

## Features

- **One-time capacity calibration** on the base cell; calibration width factor extracted and reused for all designs in the loop (no per-evaluation calibration cost)
- **Derived c_max**: when `max_lithium_conc_mol_m3` is not provided, max lithium concentration is derived from `specific_capacity_mAh_g` and AM particle density — ensures PyBaMM's volumetric capacity matches the equilibrium calculation
- **NSGA2** (pymoo) for multi-objective Pareto optimization
- **PyBaMM SPMe** for performance (capacity, energy, power, DCIR) and cycle life
- **Up to 5 design targets** (`capacity_Ah`, `energy_Wh`, `power_W`, `dcir_mOhm`, `cycle_life_cycles`) as plain dict entries with condition parameters (each target is `dict[str, float | str]`, not a typed Pydantic class)
- **Variable params** as bounds; integer fields (e.g. `positive_electrode_sheet_count`) coerced from float optimizer output
- **Error threshold** 2%: if best design's max relative error across targets exceeds 2%, `success=False`

## Input Schema

### CellOptimizerInput

The top-level input has exactly two required fields — everything else lives underneath them:

- `cell_parameters`: `CellParametersInput` — Base cell design (VW ID3 style)
- `simulation_parameters`: `SimulationParameters` — Simulation config, optimization targets, and NSGA2 settings (see below). This is where `variable_params`, `optimization_targets`, `target_weights`, `max_iterations`, `population_size`, and `use_pybamm_parameters` actually live — there is no top-level `design_targets` or `weights` field.

Within `simulation_parameters`:

- `variable_params`: `dict[str, tuple[float, float]]` (default `{}`) — Param name → (min, max) bounds; keys must be real `CellParametersInput` field names. **Must contain at least one parameter** — an empty dict returns `success=False` with an error at run time (not a Pydantic validation error).
- `optimization_targets`: `dict[str, dict[str, Any]]` — targets among `capacity_Ah`, `energy_Wh`, `power_W`, `dcir_mOhm`, `cycle_life_cycles` (each a dict with `target` and conditions; see Design Target Entries below)
- `target_weights`: `dict[str, float]` — Per-target weights (optional, defaults to equal weighting)
- `max_iterations`: int (default 20) — NSGA2 generations
- `population_size`: int (default 10) — Population size
- `use_pybamm_parameters`: `str` (default: `"Chen2020"`) — PyBaMM parameter set. Supported values: `"Chen2020"` (default for NMC/graphite), `"Prada2013"` (for LFP), `"custom"` (uses Chen2020 as base with custom overrides), or any PyBaMM parameter set name
- `num_cycles`: int, **required** — total ageing cycles
- `upper_voltage_cutoff_V` / `lower_voltage_cutoff_V`: float, **required** — voltage cutoffs used by `SimulationParameters` (separate from the optional, defaulted fields of the same name on `cell_parameters`)

### CellParameters (`CellParametersInput`)

Structured nested input that mirrors the `cell_performance.py` schema.
Nested under `cell_parameters` in the top-level input.

#### Electrode Configuration (required)

`model_config = ConfigDict(extra="forbid")` — unknown keys are rejected.

| Field | Type | Description |
|---|---|---|
| `positive_electrode_mass_loading_mg_cm2` | `float` | Positive mass loading [mg/cm²] |
| `negative_electrode_mass_loading_mg_cm2` | `float` | Negative mass loading [mg/cm²] |
| `positive_electrode_sheet_count` | `int` | Positive sheet count |
| `negative_electrode_sheet_count` | `int` | Negative sheet count |
| `jelly_roll_count` | `int` | Number of jelly rolls (default: 1) |
| `electrode_coating_side_count` | `int` | Number of coating sides (default: 2) |
| `positive_electrode_specific_heat_capacity_J_kg_K` | `float` | Positive electrode specific heat capacity [J/kg/K] |
| `negative_electrode_specific_heat_capacity_J_kg_K` | `float` | Negative electrode specific heat capacity [J/kg/K] |
| `positive_electrode_thermal_conductivity_W_m_K` | `float` | Positive electrode thermal conductivity [W/m/K] |
| `negative_electrode_thermal_conductivity_W_m_K` | `float` | Negative electrode thermal conductivity [W/m/K] |
| `positive_electrode_electronic_conductivity_S_m` | `float` | Positive electrode electronic conductivity [S/m] |
| `negative_electrode_electronic_conductivity_S_m` | `float` | Negative electrode electronic conductivity [S/m] |
| `positive_coating_thickness_um` | `float` | Positive electrode coating thickness [µm] |
| `negative_coating_thickness_um` | `float` | Negative electrode coating thickness [µm] |
| `positive_electrode_active_materials` | `list[ActiveMaterial]` | Positive formulation: each item needs `name`, `mass_fraction`, `density_g_cm3`, `specific_capacity_mAh_g`, `nominal_voltage_V` |
| `negative_electrode_active_materials` | `list[ActiveMaterial]` | Negative formulation (same shape as above) |
| `positive_electrode_binders` | `list[Binder]` | Each item needs `name`, `mass_fraction`, `density_g_cm3` |
| `negative_electrode_binders` | `list[Binder]` | Each item needs `name`, `mass_fraction`, `density_g_cm3` |
| `positive_electrode_conductive_agents` | `list[ConductiveAgent]` | Each item needs `name`, `mass_fraction`, `density_g_cm3` |
| `negative_electrode_conductive_agents` | `list[ConductiveAgent]` | Optional — defaults to `[]` (only positive-side conductive agents are required) |

**Formulation constraint:** for each electrode, `sum(active_materials.mass_fraction) + sum(binders.mass_fraction) + sum(conductive_agents.mass_fraction)` must equal `1.0` (tolerance `1e-6`), or the model raises a validation error.

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
| `positive_electrode_width_mm` | `float` or `None` | Positive electrode width [mm]. If provided, used directly for area calculation instead of deriving from cell dimensions. Also used for negative electrode if negative-specific dimensions not provided. If `None`, calculated from electrode area (square root of area). |
| `positive_electrode_height_mm` | `float` or `None` | Positive electrode height [mm]. If provided, used directly for area calculation instead of deriving from cell dimensions. Also used for negative electrode if negative-specific dimensions not provided. If `None`, calculated from electrode area (square root of area). |
| `negative_electrode_width_mm` | `float` or `None` | Negative electrode width [mm]. If provided, used directly for area calculation. If not provided, positive electrode width is used. |
| `negative_electrode_height_mm` | `float` or `None` | Negative electrode height [mm]. If provided, used directly for area calculation. If not provided, positive electrode height is used. |

**Note**: When electrode-specific dimensions are not provided, PyBaMM parameter building calculates electrode width/height from the electrode area (using `sqrt(area)`), matching the behavior in `cell_performance.py`. This ensures consistent PyBaMM parameter generation across models.

#### Separator Dimensions (Optional)

| Field | Type | Description |
|---|---|---|
| `separator_sheet_count` | `float` or None | Separator sheet count. If not provided, calculated as `pos_count + neg_count + 1` |
| `separator_sheet_width_mm` | `float` or None | Separator sheet width [mm]. If not provided, uses positive electrode width + overhang |
| `separator_sheet_height_mm` | `float` or None | Separator sheet height [mm]. If not provided, uses positive electrode height + overhang |

**Separator dimension derivation logic:**
- If both `separator_sheet_width_mm` and `separator_sheet_height_mm` are provided, they are used directly.
- Otherwise, separator dimensions are derived from positive electrode dimensions plus overhang (4 × `electrode_overhang_mm` total, accounting for 2 × overhang per side).
- For cylindrical cells, separator area approximates positive electrode area.

### Simulation Parameters (`SimulationParameters`)

Nested under `simulation_parameters` in the top-level input.

#### OCV Model Configuration

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `positive_electrode_ocv_model` | str \| null | "interpolant" | OCV model for positive electrode: "polynomial", "interpolant", or "msmr" |
| `negative_electrode_ocv_model` | str \| null | "interpolant" | OCV model for negative electrode: "polynomial", "interpolant", or "msmr" |

There is no `..._ocv_polynomial_degree` or `..._ocv_msmr_n_sites` input field on this model — polynomial degree and MSMR site count are always auto-selected internally based on chemistry; they cannot be overridden through `CellOptimizerInput`.

**OCV Model Options:**
- **"interpolant"** (default): Linear interpolation of OCV data. Fast, CasADi-compatible, accurate representation of tabulated data.
- **"polynomial"**: Polynomial fit to OCV data. Degree auto-selected based on chemistry (e.g., NMC=10, Graphite=12, Si=16, LFP=18).
- **"msmr"**: Multi-Site Multi-Response thermodynamic model. Sites auto-selected based on chemistry (e.g., Graphite=2, Si=4).

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

RPT parameters are used for reference performance testing and are separate from the optimization targets.

### Design Target Entries

Each entry in `simulation_parameters.optimization_targets` is a plain `dict[str, float | str]` — there are no dedicated `CapacityTarget`/`EnergyTarget`/etc. Pydantic classes in the code. Only `c_rate` (must be a positive finite number) and `direction` (must be `"charge"` or `"discharge"`) are validated at parse time; other keys are read as-is by the evaluator.

| Key                | Target / Conditions                                                                      |
|--------------------|------------------------------------------------------------------------------------------|
| `capacity_Ah`      | `target` [Ah], `c_rate`, `temperature_K`                                                 |
| `energy_Wh`        | `target` [Wh], `c_rate`, `temperature_K`                                                 |
| `power_W`          | `target` [W], `direction`, `pulse_duration_s`, `soc_pct`, `temperature_K`                  |
| `dcir_mOhm`        | `target` [mOhm], `c_rate`, `direction`, `pulse_duration_s`, `soc_pct`, `temperature_K`              |
| `cycle_life_cycles`| `target` [cycles], `charge_c_rate`, `discharge_c_rate`, `temperature_K`, `target_soh_pct` |

## Output Schema

### CellOptimizerOutput

- `success`: True if optimization completed and best design error ≤ 2%
- `summary`:
  - `target_kpis`: Target values per KPI (keys include units: `capacity_Ah`, `energy_Wh`, `power_W`, `dcir_mOhm`, `cycle_life_cycles`)
  - `optimizing_params`: Per-param `{lower, upper, init}`
  - `best_design`: `{kpis, optimized_params}` — `kpis` use unitized keys
  - `pareto_front`: List of `{kpis, optimized_params}` — the top `max(population_size, 10)` designs by weighted error across *all* evaluated designs (not only NSGA2's final population, and not a strict Pareto-optimal set), sorted best-first
  - `error`: Max relative error across targets for best design (0–1)
- `num_evaluated`: Number of successful evaluations
- `error`: Error message when `success=False` (e.g. exceeding 2% threshold, all evaluations failing, or an NSGA2 exception message)
- `traceback`: Declared on the schema but never populated by this model's code path — always `null` in practice

## Pass/Fail Logic

- **`success=True`** when NSGA2 completes, at least one design evaluated successfully, and `summary.error ≤ 0.02` (2%)
- **`success=False`** when:
  - NSGA2 raises an exception
  - Every design evaluation fails (`num_evaluated=0`)
  - Best design's max relative error across targets exceeds 2%

## Example Parameters (Seed)

Default uses VW ID3 Pouch 80Ah baseline. Variables: `positive_electrode_mass_loading_mg_cm2`, `positive_electrode_sheet_count`. See migration 036 for full structure.

```json
{
  "cell_parameters": {
    "positive_electrode_mass_loading_mg_cm2": 25.126,
    "negative_electrode_mass_loading_mg_cm2": 18.0,
    "positive_electrode_sheet_count": 18,
    "negative_electrode_sheet_count": 19,
    "form_factor": "Pouch",
    "cell_width_mm": 535.0,
    "cell_height_mm": 98.0,
    "cell_thickness_mm": 9.012,
    "positive_electrode_width_mm": 503.546,
    "positive_electrode_height_mm": 88.971,
    "upper_voltage_cutoff_V": 4.2,
    "lower_voltage_cutoff_V": 2.8,
    "positive_electrode_specific_heat_capacity_J_kg_K": 1000.0,
    "negative_electrode_specific_heat_capacity_J_kg_K": 1400.0,
    "positive_electrode_thermal_conductivity_W_m_K": 1.5,
    "negative_electrode_thermal_conductivity_W_m_K": 1.0,
    "positive_electrode_electronic_conductivity_S_m": 10.0,
    "negative_electrode_electronic_conductivity_S_m": 100.0,
    "positive_coating_thickness_um": 80.0,
    "negative_coating_thickness_um": 90.0,
    "positive_electrode_active_materials": [
      {
        "name": "NMC811",
        "mass_fraction": 0.96,
        "density_g_cm3": 4.87,
        "specific_capacity_mAh_g": 200.0,
        "nominal_voltage_V": 3.8
      }
    ],
    "positive_electrode_binders": [
      {"name": "PVDF", "mass_fraction": 0.02, "density_g_cm3": 1.78}
    ],
    "positive_electrode_conductive_agents": [
      {"name": "Carbon Black", "mass_fraction": 0.02, "density_g_cm3": 1.8}
    ],
    "negative_electrode_active_materials": [
      {
        "name": "Graphite",
        "mass_fraction": 0.98,
        "density_g_cm3": 2.24,
        "specific_capacity_mAh_g": 344.0,
        "nominal_voltage_V": 0.1
      }
    ],
    "negative_electrode_binders": [
      {"name": "SBR-CMC", "mass_fraction": 0.02, "density_g_cm3": 1.1}
    ]
  },
  "simulation_parameters": {
    "variable_params": {
      "positive_electrode_mass_loading_mg_cm2": [20.0, 35.0],
      "positive_electrode_sheet_count": [14, 24]
    },
    "optimization_targets": {
      "capacity_Ah": {"target": 80.0, "c_rate": 1.0, "temperature_K": 298.15}
    },
    "target_weights": {"capacity_Ah": 1.0},
    "max_iterations": 5,
    "population_size": 8,
    "num_cycles": 10,
    "upper_voltage_cutoff_V": 4.2,
    "lower_voltage_cutoff_V": 2.8,
    "use_pybamm_parameters": "Chen2020"
  }
}
```

Notes on the fields added above versus a minimal "happy path" payload: `positive_electrode_active_materials`/`binders`/`conductive_agents` and their negative-electrode counterparts are required lists (the negative electrode's conductive agents may be omitted — they default to `[]`), and for each electrode `sum(active_materials.mass_fraction) + sum(binders.mass_fraction) + sum(conductive_agents.mass_fraction)` must equal `1.0`. Note also that `variable_params` lives under `simulation_parameters`, not at the top level — `CellOptimizerInput` only has `cell_parameters` and `simulation_parameters`, so a stray top-level `variable_params` key would be silently ignored (not an error) and the run would fail with "variable_params must contain at least one parameter".

---

*[← Back to Models Library](Model-Library)*
