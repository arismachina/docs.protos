# DFN Calendar Ageing Model <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

[← Models Library](Model-Library) · **DFN Calendar Ageing**

Simulates battery capacity fade during storage using the Doyle-Fuller-Newman electrochemical model.

---

## Overview

The DFN Calendar Ageing Model simulates battery degradation during storage (calendar aging) for Li-ion batteries using the Doyle-Fuller-Newman (DFN) electrochemical model. This model predicts capacity fade and state-of-health (SoH) degradation over extended periods (days, months, years) without any charging or discharging cycles.

## Input Parameters

### Required Input Structure

```jsonc
{
  "cell_design": {
    // Cell design parameters (flat dictionary)
  },
  "simulation_parameters": {
    // Simulation configuration parameters
  }
}
```

### Cell Design Parameters

The `cell_design` dictionary contains physical cell properties. Key required parameters include:

- `nominal_capacity_Ah` (float, > 0): Target capacity in Ampere-hours
- `cell_volume_L` (float, > 0): Cell volume in liters
- `upper_voltage_cutoff_V` (float, > 0): Upper voltage limit [V]
- `lower_voltage_cutoff_V` (float, > 0): Lower voltage limit [V]

Additional cell design parameters include electrode geometries, material properties, separator properties, thermal properties, and more. See the full schema for complete details.

### Simulation Parameters

#### Simulation Parameters

The following simulation parameters correspond to the `SimulationParameters` schema. Parameters that have schema defaults are optional and will fall back to those defaults when omitted.

| Parameter | Type | Description |
| --- | --- | --- |
| `initial_soc` | float | Initial state of charge (0-1) |
| `upper_voltage_cutoff_V` | float | Upper voltage cutoff [V] |
| `lower_voltage_cutoff_V` | float | Lower voltage cutoff [V] |
| `reference_temperature_K` | float | Reference temperature [K] |
| `ambient_temperature_K` | float | Ambient temperature [K] |
| `initial_temperature_K` | float | Initial cell temperature [K] |
| `contact_resistance_Ohm` | float | Contact resistance [Ω] (optional; defaults to 0 Ω if omitted) |
| `total_heat_transfer_coefficient_W.m2.K-1` | float | Heat transfer coefficient [W·m⁻²·K⁻¹] |
| `cooling_surface_area_m2` | float | Cooling surface area [m²] (optional; defaults to 0.1 m² if omitted) |
| `solver_atol` | float | Absolute tolerance for solver |
| `solver_rtol` | float | Relative tolerance for solver |
| `soh_threshold` | float | Optional SoH threshold [%] (0-100) used to determine the `stop_reason` (e.g. degradation-driven stop); the simulation always runs to `calendar_time_days`; defaults to 0 (threshold effectively disabled) if omitted |
| `skip_capacity_calibration` | boolean | Optional flag to skip the capacity calibration step; if omitted, the schema default is used |
| `use_model_parameters` | string | Optional; one of `""`, `"OKane2022"`, `"Prada2013"`, `"ORegan2022"`. If omitted or set to `""`, the model defaults to the `OKane2022` parameter set |

#### Optional SEI Parameters

All SEI parameters are **optional** (can be `None`). If not provided, the model uses defaults from the specified parameter set (`use_model_parameters`). If provided (not `None`), they override the parameter set defaults.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `initial_sei_thickness_m` | float \| null | 2e-9 | Initial SEI thickness [m] |
| `sei_partial_molar_volume_m3_mol` | float \| null | 9.585e-5 | SEI partial molar volume [m³·mol⁻¹] |
| `sei_resistivity_Ohm_m` | float \| null | 200000.0 | SEI electrical resistivity [Ω·m] |
| `sei_growth_activation_energy_J_mol` | float \| null | 0.0 | SEI growth activation energy [J·mol⁻¹] |
| `sei_solvent_diffusivity_m2_s` | float \| null | 5.0e-22 | Solvent diffusivity through SEI [m²·s⁻¹] |
| `bulk_solvent_concentration_mol_m3` | float \| null | 2636.0 | Bulk solvent concentration [mol·m⁻³] |
| `sei_reaction_exchange_current_density_A_m2` | float \| null | 3.0e-7 | SEI reaction exchange current density [A·m⁻²] |
| `sei_open_circuit_potential_V` | float \| null | 0.4 | SEI open circuit potential [V] |
| `ec_diffusivity_m2_s` | float \| null | 2e-18 | Ethylene carbonate diffusivity [m²·s⁻¹] |
| `ec_initial_concentration_mol_m3` | float \| null | 4541.0 | EC initial concentration [mol·m⁻³] |

**Parameter Handling Logic**:
- When `use_model_parameters` is set (e.g., "OKane2022"), it provides defaults for all SEI parameters
- If a parameter is provided (not `None`), it overrides the parameter set default
- If a parameter is `None` or omitted, the parameter set default is used
- This allows users to override specific parameters while using defaults for others

#### Optional Mesh Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `var_pts` | object \| null | See below | Variable point distribution for mesh |

Default `var_pts` structure:
```json
{
  "x_n": 20,
  "x_s": 20,
  "x_p": 20,
  "r_n": 20,
  "r_p": 20
}
```

## Output Structure

### Success Response

    ```json
    {
      "success": true,
      "stop_reason": "completed",
      "data": {
        "time_days": [0.0, 40.9, 81.8],
        "capacity_Ah": [50.0, 49.8, 49.6],
        "soh_pct": [100.0, 99.6, 99.2],
        "sei_thickness_m": [2e-9, 2.1e-9, 2.2e-9],
        "dcir_0_1s_Ohm": [0.02, 0.021, 0.022],
        "dcir_1s_Ohm": [0.018, 0.019, 0.02],
        "dcir_10s_Ohm": [0.015, null, 0.017],
        "dcir_18s_Ohm": [0.014, 0.015, 0.016],
        "dcir_30s_Ohm": [0.013, 0.014, 0.015]
      },
      "summary": {
        "calendar_time_days": 365.0,
        "initial_capacity_Ah": 50.0,
        "final_capacity_Ah": 48.5,
        "capacity_fade_Ah": 1.5,
        "capacity_fade_pct": 3.0,
        "initial_soh_pct": 100.0,
        "final_soh_pct": 97.0,
        "LLI_pct": 3.0,
        "Q_SEI_total_Ah": 1.5,
        "porosity_neg_initial": 0.253,
        "porosity_neg_final": 0.251,
        "porosity_neg_change": -0.002,
        "porosity_pos_initial": 0.216,
        "porosity_pos_final": 0.216,
        "porosity_pos_change": 0.0,
        "sei_thickness_initial_m": 2e-9,
        "sei_thickness_final_m": 3.2e-9
      },
      "config": {}
    }
```

### Output Fields

#### `success` (boolean)
Indicates if the simulation completed successfully.

#### `stop_reason` (string)
Reason indicating why the simulation stopped:
- `"completed"`: Simulation ran for the full requested duration
- `"soh_threshold"`: Simulation terminated early because SoH dropped below `soh_threshold` during simulation
- `"batch_failed"`: Simulation stopped due to a failed storage batch (solver error, convergence issues)
- `"error"`: Simulation failed with an error

> **Note**: The summary field `calendar_time_days` reflects the **actual simulated duration**, which may be less than the requested duration if the simulation terminates early. Compare `calendar_time_days` (actual) with `requested_calendar_time_days` (requested) to determine if the simulation completed fully.
#### `data` (object | null)
Minimal timeseries data with **10 evenly spaced points** over the simulation period:

- `time_days` (array): Time points [days]
- `capacity_Ah` (array): Capacity calculated from LLI [A·h]
- `soh_pct` (array): State of Health calculated from LLI [%]
- `sei_thickness_m` (array): X-averaged negative SEI thickness [m] at each timeseries point
- `dcir_0_1s_Ohm` (array): DCIR at 0.1 s [Ω] from 2C pulse at 50% SOC
- `dcir_1s_Ohm` (array): DCIR at 1 s [Ω]
- `dcir_10s_Ohm` (array): DCIR at 10 s [Ω]
- `dcir_18s_Ohm` (array): DCIR at 18 s [Ω]
- `dcir_30s_Ohm` (array): DCIR at 30 s [Ω]

**Note**: Timeseries is sampled at 10 evenly spaced times over `calendar_time_days`. DCIR values are computed per point via a 2C discharge pulse simulation.

#### `summary` (object)
Scalar summary values:

| Field | Type | Description |
|-------|------|-------------|
| `calendar_time_days` | float | Actual calendar aging time simulated [days] (may be less than requested if simulation terminates early) |
| `requested_calendar_time_days` | float \| null | Originally requested calendar aging time [days] (null if not available) |
| `initial_capacity_Ah` | float | Initial capacity [A·h] |
| `final_capacity_Ah` | float | Final capacity [A·h] |
| `capacity_fade_Ah` | float | Capacity fade [A·h] |
| `capacity_fade_pct` | float | Capacity fade [%] |
| `initial_soh_pct` | float | Initial state of health [%] |
| `final_soh_pct` | float | Final state of health [%] |
| `LLI_pct` | float \| null | Loss of lithium inventory [%] |
| `Q_SEI_total_Ah` | float \| null | Total SEI capacity loss [A·h] |
| `porosity_neg_initial` | float \| null | Initial negative electrode porosity |
| `porosity_neg_final` | float \| null | Final negative electrode porosity |
| `porosity_neg_change` | float \| null | Change in negative electrode porosity |
| `porosity_pos_initial` | float \| null | Initial positive electrode porosity |
| `porosity_pos_final` | float \| null | Final positive electrode porosity |
| `porosity_pos_change` | float \| null | Change in positive electrode porosity |
| `sei_thickness_initial_m` | float \| null | Initial X-averaged negative SEI thickness [m] |
| `sei_thickness_final_m` | float \| null | Final X-averaged negative SEI thickness [m] |

### Error Response

```json
{
  "success": false,
  "stop_reason": "error",
  "error": "Error message describing what went wrong"
}
```

## Model Configuration

### Model Options

The model uses the following PyBaMM model options:

- `"SEI": "solvent-diffusion limited"`: SEI growth mechanism
- `"SEI porosity change": "true"`: Track porosity changes due to SEI
- `"SEI on cracks": "false"`: No SEI growth on cracks
- `"particle mechanics": "none"`: No particle mechanics
- `"loss of active material": "none"`: No LAM mechanism
- `"thermal": "lumped"`: Lumped thermal model
- `"contact resistance": "true"`: Include contact resistance

### Solver

- **Solver**: IDAKLUSolver (Implicit Differential-Algebraic solver)
- **Time Integration**: Adaptive time stepping with specified tolerances
- **Mesh Resolution**: Configurable via `var_pts` parameter

### Parameter Sets

The model supports PyBaMM parameter sets via `use_model_parameters`. Allowed values:
- `""`: Empty string defaults to OKane2022
- `"OKane2022"`: OKane2022 parameter set (default)
- `"Prada2013"`: Prada2013 parameter set
- `"ORegan2022"`: ORegan2022 parameter set

## Usage Examples

### Basic Usage

```python
parameters = {
    "cell_design": {
        "nominal_capacity_Ah": 50.0,
        "cell_volume_L": 0.1,
        # ... other cell design parameters
    },
    "simulation_parameters": {
        "calendar_time_days": 365.0,
        "initial_soc": 0.8,
        "upper_voltage_cutoff_V": 4.2,
        "lower_voltage_cutoff_V": 2.8,
        "reference_temperature_K": 298.15,
        "ambient_temperature_K": 298.15,
        "initial_temperature_K": 298.15,
        "contact_resistance_Ohm": 1e-5,
        "total_heat_transfer_coefficient_W.m2.K-1": 0.01,
        "cooling_surface_area_m2": 0.1,
        "solver_atol": 1e-4,
        "solver_rtol": 1e-4,
        "soh_threshold": 80.0,
        "skip_capacity_calibration": False,
        "use_model_parameters": "OKane2022"
    }
}

result = calculate_dfn_calendar_ageing(parameters)
```

### Override Specific SEI Parameters

```python
simulation_parameters = {
    # ... required parameters ...
    "use_model_parameters": "OKane2022",
    # Override specific SEI parameters
    "sei_solvent_diffusivity_m2_s": 1.0e-21,  # Faster growth
    "sei_reaction_exchange_current_density_A_m2": 6.0e-7,  # Faster reaction
    # Other SEI parameters will use OKane2022 defaults
}
```

### Use All Defaults (No Overrides)

```python
simulation_parameters = {
    # ... required parameters ...
    "use_model_parameters": "OKane2022",
    # All SEI parameters omitted or set to None - uses parameter set defaults
}
```

## Technical Details

### Capacity Calculation

For calendar aging simulations, capacity is calculated from Loss of Lithium Inventory (LLI):

```
capacity_Ah = nominal_capacity_Ah × (1 - LLI_pct / 100)
```

### State of Health Calculation

```
soh_pct = 100 - LLI_pct
```

### Timeseries Sampling

The output timeseries uses **10 evenly spaced points** over the simulation period:
1. Target times are generated via `linspace(0, max_time_days, 10)`
2. For each target time, the closest actual simulation time point is selected
3. The last data point is always included
4. All timeseries arrays are sampled using the same indices
5. Per point, DCIR values are computed via a 2C discharge pulse simulation at 50% SOC

### Capacity Calibration

If `skip_capacity_calibration` is `False`, the model performs an initial capacity calibration:
1. Charges the cell to the upper voltage cutoff
2. Discharges at C/10 rate
3. Scales cell design parameters to match the measured capacity

This ensures the simulated capacity matches the specified `nominal_capacity_Ah`.

### Stopping Criteria

The simulation may terminate early in the following cases:
- **SoH threshold**: If `soh_threshold` > 0 and SoH drops below the threshold during simulation
- **Failed batch**: If a storage batch fails (solver error, convergence issues), the simulation stops with partial results

When the simulation completes (either normally or early):
- `calendar_time_days` in the summary reflects the **actual simulated duration** (matches `cumulative_time_days` in logs)
- `requested_calendar_time_days` in the summary reflects the **originally requested duration**
- `stop_reason` indicates why the simulation stopped:
  - `"completed"`: Simulation ran for the full requested duration
  - `"soh_threshold"`: Simulation terminated early because SoH dropped below threshold
  - `"batch_failed"`: Simulation stopped due to a failed batch

## Limitations

1. **SEI Only**: This model only includes SEI growth degradation. Other mechanisms (particle cracking, LAM) are not modeled.

2. **Calendar Aging Only**: The model simulates storage conditions only. No charge/discharge cycles are included.

3. **Lumped Thermal Model**: Uses a simplified lumped thermal model. Spatial temperature gradients are not captured.

4. **Timeseries Sampling**: Output timeseries uses 10 evenly spaced points. Full-resolution data is not available in the output.

---

*[← Back to Models Library](Model-Library)*
