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

### Simulation Parameters (Canonical, Alias-Free)

#### Cycling Protocol

| Parameter | Default | Description |
|---|---|---|
| `num_cycles` | 1000 | Total ageing cycles |
| `diagnostic_cycle_frequency` | 500 | Diagnostic check-point frequency (every N cycles) |
| `discharge_c_rate` | 1.0 | Ageing discharge C-rate |
| `charge_c_rate` | 1.0 | Ageing charge C-rate |
| `initial_soc_pct` | 100.0 | Initial state of charge [%] |
| `ambient_temperature_K` | 298.15 | Ambient temperature [K] |
| `upper_voltage_cutoff_V` | 3.65 | Upper voltage cutoff [V] |
| `lower_voltage_cutoff_V` | 2.5 | Lower voltage cutoff [V] |
| `contact_resistance_Ohm` | 1e-4 | Contact resistance [Ohm] |
| `soh_threshold_pct` | 80.0 | Stop at SoH [%]; None = no stop |
| `solver_atol` | 1e-4 | Solver absolute tolerance |
| `solver_rtol` | 1e-4 | Solver relative tolerance |
| `skip_capacity_calibration` | False | Skip calibration |
| `use_pybamm_parameters` | "Prada2013" | PyBaMM parameter set |
| `mesh_resolution` | {"x_n":10, ...} | Mesh resolution for simulation |
| `cooling_surface_area_m2` | 0.02 | Cell cooling surface area [m²] |
| `total_heat_transfer_coefficient_W_m2_K` | 10.0 | Heat transfer coefficient [W/(m2.K)] |
| `cell_thermal_expansion_coefficient_m_K` | 1.1e-6 | Cell thermal expansion coefficient [m/K] |
| `anode_potential_safety_threshold_V` | None | Anode potential safety limit [V]; None = disabled |
| `temperature_safety_threshold_K` | None | Temperature safety limit [K]; None = disabled |
| `enable_thermal` | True | Enable lumped thermal model (True = lumped, False = isothermal) |

#### Ageing Mechanism Toggles

| Parameter | Default | Description |
|---|---|---|
| `enable_sei` | True | Enable SEI (solid electrolyte interphase) growth degradation |
| `enable_lam` | True | Enable loss of active material (LAM) degradation |
| `enable_particle_cracking` | True | Enable particle cracking (Paris' law) degradation |
| `enable_swelling` | True | Enable particle swelling (mechanics) degradation |

#### SEI Degradation

| Parameter | Default |
|---|---|
| `initial_sei_thickness_m` | 1e-9 |
| `sei_partial_molar_volume_m3_mol` | 5e-5 |
| `sei_resistivity_Ohm_m` | 1000.0 |
| `sei_growth_activation_energy_J_mol` | 5e4 |
| `sei_solvent_diffusivity_m2_s` | 2.5e-26 |
| `bulk_solvent_concentration_mol_m3` | 2000.0 |
| `sei_reaction_exchange_current_density_A_m2` | 1.5e-11 |
| `sei_open_circuit_potential_V` | 0.4 |
| `ec_diffusivity_m2_s` | 2e-18 |
| `ec_initial_concentration_mol_m3` | 4541.0 |
| `ratio_lithium_moles_to_sei_moles` | 1.0 |
| `initial_sei_on_cracks_thickness_m` | 1e-9 |

#### Particle Mechanics (Swelling)

| Parameter | Default |
|---|---|
| `negative_electrode_youngs_modulus_Pa` | 15e9 |
| `positive_electrode_youngs_modulus_Pa` | 375e9 |
| `negative_electrode_poissons_ratio` | 0.3 |
| `positive_electrode_poissons_ratio` | 0.3 |
| `negative_electrode_partial_molar_volume_m3_mol` | 3.1e-6 |
| `positive_electrode_partial_molar_volume_m3_mol` | -7.28e-7 |
| `negative_electrode_reference_concentration_for_free_of_deformation` | 0.0 |
| `positive_electrode_reference_concentration_for_free_of_deformation` | 0.0 |

#### Particle Cracking (Paris' Law)

| Parameter | Default |
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

| Parameter | Default |
|---|---|
| `negative_electrode_lam_constant_proportional_1_s` | 3e-8 |
| `positive_electrode_lam_constant_proportional_1_s` | 3e-8 |
| `negative_electrode_lam_constant_exponential` | 2.0 |
| `positive_electrode_lam_constant_exponential` | 2.0 |
| `negative_electrode_critical_stress_Pa` | 60e6 |
| `positive_electrode_critical_stress_Pa` | 60e6 |

## Output Schema

### Summary (`CycleSummaryData`)

- `num_cycles_completed`, `num_diagnostics`
- `nominal_capacity_Ah`, `nominal_energy_Wh`
- `initial/final_capacity_Ah`, `initial/final_energy_Wh`
- `capacity_fade_Ah`, `capacity_fade_pct`
- `initial/final_soh_pct`
- `final_fce_capacity`, `final_fce_energy`
- `final_lli_pct`, `final_lam_neg_pct`, `final_lam_pos_pct`

### Diagnostic Series (`DiagnosticDataPoint`)

Each point in `data.diagnostics[]`:

- `ageing_cycle` - cycle number
- `discharge_capacity_Ah`, `discharge_energy_Wh` - C/3 reference
- `soh_pct` - relative to BoL
- `fce_capacity`, `fce_energy`
- `dcir[]` - list of `{time_s, dcir_mohm}` at 0.1, 1, 10, 18, 30s
- `temperature_K`, `sei_thickness_m`
- `lli_pct`, `lam_neg_pct`, `lam_pos_pct`

## Example Results (Default Parameters)

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
