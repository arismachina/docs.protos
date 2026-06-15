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
- **1–5 design targets** with typed condition parameters
- **Variable params** as bounds; integer fields (e.g. `positive_electrode_sheet_count`) coerced from float optimizer output
- **Error threshold** 2%: if best design's max relative error across targets exceeds 2%, `success=False`

## Input Schema

### CellOptimizerInput

- `variable_params`: `dict[str, tuple[float, float]]` — Param name → (min, max) bounds; keys must be in `CellParameters`. **Must contain at least one parameter** (empty dict will return error).
- `cell_parameters`: `CellParameters` — Base cell design (VW ID3 style)
- `design_targets`: `dict[str, Any]` — 1–5 targets: `capacity_Ah`, `energy_Wh`, `power_W`, `dcir_mOhm`, `cycle_life_cycles` (each with `target` and conditions)
- `weights`: `dict[str, float]` — Per-target weights (optional)
- `max_iterations`: int (default 5) — NSGA2 generations
- `population_size`: int (default 6) — Population size
- `use_pybamm_parameters`: `str` (default: `"Chen2020"`) — PyBaMM parameter set. Supported values: `"Chen2020"` (default for NMC/graphite), `"Prada2013"` (for LFP), `"custom"` (uses Chen2020 as base with custom overrides), or any PyBaMM parameter set name

### CellParameters (`CellParametersInput`)

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
| `positive_electrode_ocv_model` | str | "interpolant" | OCV model for positive electrode: "polynomial", "interpolant", or "msmr" |
| `negative_electrode_ocv_model` | str | "interpolant" | OCV model for negative electrode: "polynomial", "interpolant", or "msmr" |
| `positive_electrode_ocv_polynomial_degree` | int \| null | null | Polynomial degree for positive electrode (auto-selected based on chemistry if null) |
| `negative_electrode_ocv_polynomial_degree` | int \| null | null | Polynomial degree for negative electrode (auto-selected based on chemistry if null) |
| `positive_electrode_ocv_msmr_n_sites` | int \| null | null | Number of MSMR sites for positive electrode (auto-selected based on chemistry if null) |
| `negative_electrode_ocv_msmr_n_sites` | int \| null | null | Number of MSMR sites for negative electrode (auto-selected based on chemistry if null) |

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
| `rpt_power.power_level_W` | 2000.0 | Power pulse level [W] |
| `rpt_power.power_duration_s` | 300.0 | Power pulse duration [s] |
| `rpt_power.power_direction` | "discharge" | Power pulse direction ("charge" or "discharge") |
| `rpt_power.power_soc_pct` | 50.0 | Power pulse SOC [%] |
| `rpt_power.power_temperature_K` | 298.15 | Power pulse temperature [K] |

RPT parameters are used for reference performance testing and are separate from the optimization targets.

### Design Target Models

| Key                | Model           | Target / Conditions                                                                      |
|--------------------|-----------------|------------------------------------------------------------------------------------------|
| `capacity_Ah`      | CapacityTarget  | `target` [Ah], `c_rate`, `temperature_K`                                                 |
| `energy_Wh`        | EnergyTarget    | `target` [Wh], `c_rate`, `temperature_K`                                                 |
| `power_W`          | PowerTarget     | `target` [W], `direction`, `pulse_duration_s`, `soc_pct`, `temperature_K`                  |
| `dcir_mOhm`        | DCIRTarget      | `target` [mOhm], `c_rate`, `direction`, `pulse_duration_s`, `soc_pct`, `temperature_K`              |
| `cycle_life_cycles`| CycleLifeTarget | `target` [cycles], `charge_c_rate`, `discharge_c_rate`, `temperature_K`, `target_soh_pct` |

## Output Schema

### CellOptimizerOutput

- `success`: True if optimization completed and best design error ≤ 2%
- `summary`:
  - `target_kpis`: Target values per KPI (keys include units: `capacity_Ah`, `energy_Wh`, `power_W`, `dcir_mOhm`, `cycle_life_cycles`)
  - `optimizing_params`: Per-param `{lower, upper, init}`
  - `best_design`: `{kpis, optimized_params}` — `kpis` use unitized keys
  - `pareto_front`: List of `{kpis, optimized_params}` — `kpis` use unitized keys
  - `error`: Max relative error across targets for best design (0–1)
- `num_evaluated`: Number of successful evaluations
- `error`: Error message when `success=False` (e.g. exceeding 2% threshold)
- `traceback`: Optional traceback when run failed

## Pass/Fail Logic

- **`success=True`** when NSGA2 completes and `summary.error ≤ 0.02` (2%)
- **`success=False`** when:
  - NSGA2 raises an exception
  - Best design's max relative error across targets exceeds 2%

## Example Parameters (Seed)

Default uses VW ID3 Pouch 80Ah baseline. Variables: `positive_electrode_mass_loading_mg_cm2`, `positive_electrode_sheet_count`. See migration 036 for full structure.

```json
{
  "variable_params": {
    "positive_electrode_mass_loading_mg_cm2": [20.0, 35.0],
    "positive_electrode_sheet_count": [14, 24]
  },
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
    "lower_voltage_cutoff_V": 2.8
  },
  "simulation_parameters": {
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

The model expects the nested structure shown above, with `simulation_parameters` containing `optimization_targets` and `target_weights`.

---

*[← Back to Models Library](Model-Library)*
