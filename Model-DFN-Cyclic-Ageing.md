# DFN Cyclic Ageing Model <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

[← Models Library](Model-Library) · **DFN Cyclic Ageing**

Physics-based cycle-life simulation with SEI growth, particle cracking, and loss of active material.

---

Physics-based cyclic ageing simulation using the Doyle-Fuller-Newman (DFN)
electrochemical model with SEI growth, particle mechanics, and loss of active
material degradation.

## Overview

The model runs charge/discharge ageing cycles with periodic diagnostic
check-points that measure capacity, energy, DCIR, and degradation state.

### Protocol

```
┌─────────────────────────────────────────────────────────┐
│  BoL Diagnostic (cycle 0)                               │
│    CCCV C/3 charge → rest → C/3 dch → 50% SOC → DCIR   │
├─────────────────────────────────────────────────────────┤
│  Ageing Block 1 (N cycles)                              │
│    Discharge at x C → Charge at y C  (repeated N times) │
├─────────────────────────────────────────────────────────┤
│  Diagnostic 1                                           │
│    CCCV C/3 charge → rest 10s → C/3 dch to Vmin         │
│    → charge to 50% SOC → rest 1h → 2C pulse 30s         │
│    → CCCV recovery                                      │
├─────────────────────────────────────────────────────────┤
│  Ageing Block 2 (N cycles)                              │
│  ...repeat until num_cycles or SoH threshold...         │
└─────────────────────────────────────────────────────────┘
```

### Diagnostic Measurements

At each diagnostic check-point:

| Measurement | Source |
|---|---|
| Discharge capacity [Ah] | C/3 reference discharge |
| Discharge energy [Wh] | C/3 reference discharge |
| SoH [%] | Capacity relative to BoL (beginning of life) |
| FCE capacity | Cumulative ageing Ah / nominal Ah |
| FCE energy | Cumulative ageing Wh / nominal Wh |
| DCIR [mOhm] | At 0.1, 1, 10, 18, 30s into 2C pulse |
| Temperature [K] | Cell temperature at diagnostic |
| SEI thickness [m] | X-averaged negative SEI thickness |
| LLI [%] | Loss of lithium inventory |
| LAM neg/pos [%] | Loss of active material |

## Simulation Parameters

Input is split across two top-level objects: `cell_parameters` (`CellParametersInput`) and `simulation_parameters` (`CycleSimulationParameters`). **Every field in both objects is required at the schema level** — the Pydantic models carry no field-level defaults (Ruff/Pydantic `Field(...)` everywhere, including nullable fields like `soh_threshold_pct`, which must still be sent explicitly, e.g. as `null`, to disable them). All "Default" values below are the platform's seeded defaults (the `dfn_cyclic_ageing` entry in `model_definitions`, currently sourced from `migration_071_add_dfn_cyclic_ageing_cycle_seq.py`) used to prefill new jobs — not schema defaults.

### Cell Parameters (fields relevant to cycling)

`upper_voltage_cutoff_V`, `lower_voltage_cutoff_V`, and `cell_contact_resistance_Ohm` live on `cell_parameters` (`CellParametersInput`), **not** on `simulation_parameters`. See [Model-Cell-Performance](Model-Cell-Performance) for the full `CellParametersInput` schema (mass loading, geometry, formulation, foils, separator, etc.).

| Parameter | Seed default | Description |
|---|---|---|
| `upper_voltage_cutoff_V` | 3.65 | Upper voltage cutoff for simulations [V] |
| `lower_voltage_cutoff_V` | 2.5 | Lower voltage cutoff for simulations [V] |
| `cell_contact_resistance_Ohm` | 1e-4 | Cell contact resistance [Ohm] |

### Simulation Parameters (Canonical, Alias-Free)

#### Cycling Protocol

| Parameter | Seed default | Description |
|---|---|---|
| `num_cycles` | 1000 | Total ageing cycles |
| `diagnostic_cycle_frequency` | 100 | Diagnostic check-point frequency (every N cycles) |
| `discharge_c_rate` | 1.0 | Ageing discharge C-rate (synced into default `cycle_seq` when unchanged) |
| `charge_c_rate` | 0.5 | Ageing charge C-rate (synced into default `cycle_seq` when unchanged) |
| `cycle_seq` | `{"steps": ["Discharge at 1P until {vmin} V", "Charge at 0.5P until {vmax} V"], "period_s": 3600.0}` | Ageing cycle protocol (`ExperimentSequence`); xC/xP tokens expanded at runtime. **Required** — no schema default. |
| `diagnostic_seq` | 3-segment CCCV → 2C pulse → CCCV protocol | Multi-segment diagnostic protocol (`DiagnosticSequence`). **Required** — no schema default. |
| `scale_nominal` | False | When `False`, xC/xP scaling always uses the BoL (diagnostic 0) reference capacity/energy; when `True`, each ageing block rescales off the most recent diagnostic. **Required** — no schema default. |
| `initial_soc_pct` | 100.0 | Initial state of charge [%] |
| `ambient_temperature_K` | 298.15 | Ambient temperature [K] |
| `initial_cell_temperature_K` | 298.15 | Initial cell temperature [K]; nullable, falls back to `ambient_temperature_K` when `null`. **Required field** (must be sent, even as `null`). |
| `reference_cell_temperature_K` | 298.15 | Reference temperature for PyBaMM's thermal model [K]; nullable, falls back to `ambient_temperature_K` when `null`. **Required field.** |
| `soh_threshold_pct` | `null` | Stop at SoH [%]; `null` = no stop. **Required field** (must be sent, even as `null`). |
| `enable_thermal` | True | Enable lumped thermal model (True = lumped, False = isothermal) |
| `anode_potential_safety_threshold_V` | `null` | Anode potential safety limit [V]; `null` = disabled |
| `temperature_safety_threshold_K` | `null` | Temperature safety limit [K]; `null` = disabled |

#### Solver, Calibration & Timeseries Capture

| Parameter | Seed default | Description |
|---|---|---|
| `solver_atol` | 1e-4 | Solver absolute tolerance |
| `solver_rtol` | 1e-4 | Solver relative tolerance |
| `skip_capacity_calibration` | False | Skip calibration |
| `capture_ageing_block_first_cycle_timeseries` | False | When `True`, store V/I/T/P/Q/E vs time for the first cycle of each ageing block (cycles 1, N+1, 2N+1, ...) |
| `ageing_block_timeseries_period_s` | 60.0 | Output sampling period [s] for captured ageing-block timeseries |
| `use_pybamm_parameters` | "Prada2013" | PyBaMM parameter set |
| `mesh_resolution` | {"x_n":10, "x_s":10, "x_p":10, "r_n":10, "r_p":10} | Mesh resolution for simulation |
| `cooling_surface_area_m2` | ~0.0139 | Cell cooling surface area [m²] |
| `total_heat_transfer_coefficient_W_m2_K` | 10.0 | Heat transfer coefficient [W/(m2.K)] |
| `cell_thermal_expansion_coefficient_m_K` | 1.1e-6 | Cell thermal expansion coefficient [m/K] |

#### Ageing Mechanism Toggles

| Parameter | Seed default | Description |
|---|---|---|
| `enable_sei` | True | Enable SEI (solid electrolyte interphase) growth degradation |
| `enable_lam` | True | Enable loss of active material (LAM). Uses stress-driven mode when `enable_swelling` or `enable_particle_cracking` is also on; otherwise reaction-driven. |
| `enable_particle_cracking` | False | Enable particle cracking (Paris' law) degradation |
| `enable_swelling` | False | Enable particle swelling (mechanics). Enables stress-driven LAM and is required for particle cracking; LAM can still run in reaction-driven mode without swelling. |

#### SEI Degradation

| Parameter | Seed default |
|---|---|
| `initial_sei_thickness_m` | 1e-9 |
| `sei_partial_molar_volume_m3_mol` | 5e-5 |
| `sei_resistivity_Ohm_m` | 1000.0 |
| `sei_growth_activation_energy_J_mol` | 5e4 |
| `sei_solvent_diffusivity_m2_s` | 1e-20 |
| `bulk_solvent_concentration_mol_m3` | 2000.0 |
| `sei_reaction_exchange_current_density_A_m2` | 1.5e-11 |
| `sei_open_circuit_potential_V` | 0.4 |
| `ec_diffusivity_m2_s` | 2e-18 |
| `ec_initial_concentration_mol_m3` | 4541.0 |
| `ratio_lithium_moles_to_sei_moles` | 1.0 |
| `initial_sei_on_cracks_thickness_m` | 1e-9 |

#### Particle Mechanics (Swelling)

| Parameter | Seed default |
|---|---|
| `negative_electrode_youngs_modulus_Pa` | 15e9 |
| `positive_electrode_youngs_modulus_Pa` | 375e9 |
| `negative_electrode_poissons_ratio` | 0.3 |
| `positive_electrode_poissons_ratio` | 0.3 |
| `negative_electrode_partial_molar_volume_m3_mol` | 3.1e-6 |
| `positive_electrode_partial_molar_volume_m3_mol` | -7.28e-7 |
| `negative_electrode_reference_concentration_for_free_of_deformation` | 0.0 |
| `positive_electrode_reference_concentration_for_free_of_deformation` | 0.0 |
| `negative_electrode_volume_change` | 10-coefficient polynomial (Ai2020/Rieger2016) |
| `positive_electrode_volume_change` | `[-4.966e-5, 3e-4]` |

Both are volume-change polynomial coefficients `[sto^0..sto^N]` (ascending powers), used when swelling or cracking is enabled; calibrate per chemistry.

#### Particle Cracking (Paris' Law)

| Parameter | Seed default |
|---|---|
| `negative_electrode_initial_crack_length_m` | 1e-9 |
| `positive_electrode_initial_crack_length_m` | 1e-9 |
| `negative_electrode_cracking_rate` | 1.0e-23 |
| `positive_electrode_cracking_rate` | 1.0e-23 |
| `negative_electrode_number_of_cracks_per_unit_area_1_m2` | 3.16e15 |
| `positive_electrode_number_of_cracks_per_unit_area_1_m2` | 3.16e15 |
| `negative_electrode_initial_crack_width_m` | 1e-9 |
| `positive_electrode_initial_crack_width_m` | 1e-9 |
| `negative_electrode_paris_law_constant_b` | 1.0 |
| `positive_electrode_paris_law_constant_b` | 1.0 |
| `negative_electrode_paris_law_constant_m` | 1.0 |
| `positive_electrode_paris_law_constant_m` | 1.0 |

#### Loss of Active Material (LAM)

| Parameter | Seed default |
|---|---|
| `negative_electrode_lam_constant_proportional_1_s` | 3e-8 |
| `positive_electrode_lam_constant_proportional_1_s` | 3e-8 |
| `negative_electrode_lam_constant_exponential` | 2.0 |
| `positive_electrode_lam_constant_exponential` | 2.0 |
| `negative_electrode_critical_stress_Pa` | 60e6 |
| `positive_electrode_critical_stress_Pa` | 60e6 |
| `negative_electrode_reaction_driven_lam_factor_m3_mol` | 0.0 |
| `positive_electrode_reaction_driven_lam_factor_m3_mol` | 0.0 |

Reaction-driven LAM factors are only used by the reaction-driven LAM submodel (`enable_lam` on, `enable_swelling` and `enable_particle_cracking` both off); 0.0 disables reaction-driven LAM. As a guide, ~2e-4 gives roughly 2% SoH loss at 100 cycles on LFP (Prada2013).

## Output Schema

The route calls `calculate_dfn_cyclic_ageing()`, which returns the **Full Result** below. Copilot-facing surfaces instead call `summarize_for_copilot(full_result)`, which returns a much smaller **Copilot Summary**.

### Full Result (`DfnCyclicAgeingOutput`)

| Field | Description |
|---|---|
| `success` | `bool` — whether the simulation completed |
| `stop_reason` | `str` — one of `num_cycles`, `soh_threshold`, `ageing_solver_failure`, `error` |
| `summary` | `CycleSummaryData \| null` — scalar summary (see below) |
| `data` | `CycleDataOutput \| null` — `diagnostics[]` and, when captured, `ageing_block_first_cycle_timeseries[]` |
| `config` | `CycleSimulationParameters \| null` — the resolved simulation config used |
| `error` | `str \| null` — error message, present when `success` is `false` |
| `traceback` | `str \| null` — present only on unexpected failures |

#### Summary (`CycleSummaryData`)

- `num_cycles_completed`, `diagnostic_cycle_count`
- `nominal_capacity_Ah`, `nominal_energy_Wh`
- `initial/final_capacity_Ah`, `initial/final_energy_Wh`
- `capacity_fade_Ah`, `capacity_fade_pct`
- `initial/final_soh_pct`
- `final_fce_capacity`, `final_fce_energy`
- `final_lli_pct`, `final_lam_neg_pct`, `final_lam_pos_pct`

#### Diagnostic Series (`DiagnosticDataPoint`)

Each point in `data.diagnostics[]`:

- `ageing_cycle` - cycle number
- `discharge_capacity_Ah`, `discharge_energy_Wh` - C/3 reference
- `soh_pct` - relative to BoL
- `fce_capacity`, `fce_energy`
- `dcir[]` - list of `{time_s, dcir_mohm}` at 0.1, 1, 10, 18, 30s
- `temperature_K`, `sei_thickness_m`
- `lli_pct`, `lam_neg_pct`, `lam_pos_pct`
- `eis_measurements[]`, `reference_discharge_curve` (V-Q curve from the diagnostic's reference discharge)

### Copilot Summary (`summarize_for_copilot`)

`summarize_for_copilot(full_result)` keeps only `success`, `stop_reason`, `summary`, and `error` — it drops `config`, `data` (diagnostics/timeseries), and `traceback` entirely, and omits any key whose value is `None`/absent. This is the shape Copilot sees, **not** the full result above.

```json
{
  "success": true,
  "stop_reason": "num_cycles",
  "summary": {
    "num_cycles_completed": 1000,
    "diagnostic_cycle_count": 11,
    "nominal_capacity_Ah": 130.8,
    "nominal_energy_Wh": 425.1,
    "initial_capacity_Ah": 130.8,
    "final_capacity_Ah": 123.4,
    "capacity_fade_Ah": 7.4,
    "capacity_fade_pct": 5.7,
    "initial_soh_pct": 100.0,
    "final_soh_pct": 94.3,
    "final_fce_capacity": 1000.0,
    "final_fce_energy": 1000.0,
    "final_lli_pct": 5.9,
    "final_lam_neg_pct": 0.0,
    "final_lam_pos_pct": 0.0
  }
}
```

On failure, only `success`, `stop_reason` (`"error"`), and `error` are present.

## Example Results (Illustrative)

The run below is illustrative of the kind of degradation trend the model produces over an extended ageing schedule; it does not correspond to a literal run of the current seeded defaults (1000 cycles, 1C discharge / 0.5C charge, lumped thermal, diagnostic every 100 cycles — see [Simulation Parameters](#simulation-parameters) above).

```
2000 cycles, 1C/1C, 25C, isothermal, diagnostic every 500

Cycle     SoH    Capacity   SEI         LLI     DCIR@10s
    0   100.0%   130.8 Ah   1.00e-7 m   0.00%   0.534 mΩ
  500    98.0%   129.0 Ah   1.04e-7 m   2.00%   0.540 mΩ
 1000    95.8%   127.2 Ah   1.08e-7 m   4.13%   0.547 mΩ
 1500    93.6%   125.3 Ah   1.13e-7 m   6.39%   0.555 mΩ
 2000    91.1%   123.4 Ah   1.19e-7 m   8.83%   0.566 mΩ

Runtime: ~85 seconds
```

---

*[← Back to Models Library](Model-Library)*
