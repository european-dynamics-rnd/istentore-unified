# Implementation Details of Peak Shaving in Demo 1

## Demo Context

Demo 1 corresponds to the i-STENTORE demonstrator **Molten glass thermal storage for an increased uptake of renewable electric energy – Slovenia**.

The demonstrator uses the flexibility of an industrial glass furnace and its thermal inertia to reshape electricity consumption while respecting production and process constraints. In this context, Peak Shaving is implemented as an internal energy-management service that reduces or shifts high electricity demand periods by coordinating furnace operation, PV generation forecasts and process-aware scheduling.

The service is closely linked to the Furnace Monitoring System and the furnace digital model. The scheduling mechanism manipulates the balance between gas consumption and electrical booster usage so that electricity demand can be reduced during peak periods without moving the glass melt temperature outside the predefined operating limits.

---

## Service Objective

The objective of the Peak Shaving service in Demo 1 is to reduce the electricity peak demand of the industrial site or local substation by exploiting furnace flexibility.

The service aims to:

- reduce electricity demand during peak periods;
- use furnace thermal inertia as a flexibility resource;
- modify electric booster usage while respecting furnace constraints;
- coordinate electricity consumption with PV generation forecasts;
- preserve glass melt temperature within predefined limits;
- maintain production requirements and glass quality;
- support a process-aware flexibility service compatible with the i-STENTORE digital ecosystem.

---

## Service Classification

| Field | Description |
|---|---|
| Service name | Peak Shaving |
| Demonstrator | Demo 1 – Slovenia |
| Demonstrator title | Molten glass thermal storage for increased uptake of renewable electric energy |
| Service family | Behind-the-meter / local energy management |
| Main objective | Reduction or shifting of peak electricity demand |
| Main flexible asset | Glass furnace / molten glass thermal storage |
| Supporting asset | PV plant |
| Main monitoring system | Furnace Monitoring System |
| Main optimisation basis | PV forecast, furnace model and substation demand forecast |
| Final readiness level | Field-tested process-coupled furnace flexibility |

---

## Demonstrator Assets

### Main Assets

- Glass furnace;
- Molten glass thermal storage;
- Electric booster system;
- Gas combustion system;
- PV plant;
- Site or substation metering point.

### Digital and Control Assets

- Furnace Monitoring System;
- Furnace digital model;
- Scheduling / optimisation mechanism;
- PV forecasting service;
- Energy forecast for the substation;
- Local monitoring and data storage;
- HMI or dashboard layer;
- i-STENTORE semantic and data model layer.

---

## Actors

| Actor | Role |
|---|---|
| Site Operator | Defines production constraints and acceptable operating boundaries. |
| Furnace Monitoring System | Monitors furnace state and receives feedback from the scheduling mechanism. |
| Furnace Model | Represents the thermal and process behaviour of the furnace. |
| Scheduling Mechanism | Computes the process-aware peak-shaving schedule. |
| PV Forecasting Service | Provides expected PV generation. |
| Substation Forecasting Layer | Provides expected energy demand at the substation. |
| Local Controller / Operator Guidance | Supports implementation of the planned schedule. |
| HMI / Dashboard | Displays forecast, schedule, process status and KPIs. |
| i-STENTORE Middleware | Supports harmonised information exchange and semantic representation. |

---

## Operational Principle

The Peak Shaving service modifies the electricity consumption of the furnace system while maintaining the process within safe and acceptable operating limits.

The key operational principle is that the glass furnace has thermal inertia. This means that, within defined boundaries, the energy input to the furnace can be shifted or modulated without immediately compromising the glass melt temperature or production quality.

The scheduling mechanism uses this flexibility to:

1. identify expected peak electricity demand periods;
2. reduce electric booster usage during those periods where feasible;
3. compensate with gas consumption or shifted operation when required;
4. maintain the glass melt temperature between its lower and upper set points;
5. keep the production schedule and quality constraints respected.

---

## Difference between Peak Shaving and Congestion Management

Demo 1 uses a similar technical configuration for Peak Shaving and Congestion Management, but the service trigger and objective are different.

| Aspect | Peak Shaving | Congestion Management |
|---|---|---|
| Main trigger | Internal energy-management objective or demand peak forecast. | External DSO request or network constraint. |
| Main objective | Reduce site or substation peak demand. | Support external grid constraint management. |
| Main optimisation criterion | Minimise or limit peak demand. | Meet DSO requested power profile or flexibility requirement. |
| Implementation status | Field-tested as process-coupled furnace flexibility. | Conceptual/modelled due to absence of DSO interface. |

For this reason, this file focuses only on the internally driven Peak Shaving service.

---

## Timing Requirements

| Parameter | Value / Description |
|---|---|
| Planning horizon | Day-ahead or intra-day |
| Main timestep | Defined by the scheduling mechanism |
| Reaction time | Process-dependent; furnace flexibility is not a sub-second service |
| Frequency of use | Periodic scheduling according to process and demand conditions |
| Main unit of service | kW / MW peak reduction and kWh / MWh shifted energy |
| Main process constraint | Glass melt temperature limits |
| Main operational limitation | Gas combustion and electric booster operating constraints |

---

## Information and Data Flows

### 1. Data Collection

The service requires data from the furnace, PV plant and local network.

The Furnace Monitoring System and local data infrastructure provide:

- current glass melt temperature;
- minimum glass melt temperature;
- maximum glass melt temperature;
- electric booster limits;
- current or historical gas consumption;
- current or historical electric booster usage;
- production-related parameters;
- current operational status.

The forecasting services provide:

- PV generation forecast;
- expected energy demand at the substation;
- day-ahead electricity price or cost signal, where relevant;
- glass production estimate.

### 2. Peak Identification

The scheduling mechanism analyses the expected site or substation energy profile.

The objective is to identify:

- time intervals where demand is expected to be high;
- possible overlap with low PV generation;
- time intervals where furnace electric booster usage can be reduced;
- feasible compensation periods before or after the peak.

### 3. Process-Aware Optimisation

The scheduling mechanism uses the furnace model to compute an optimised schedule.

The optimisation modifies:

- electric booster usage;
- gas consumption;
- energy input distribution over time.

The optimisation must respect:

- minimum glass melt temperature;
- maximum glass melt temperature;
- maximum booster electric power;
- glass production estimate;
- furnace operational limitations;
- process quality requirements.

### 4. Schedule Communication

The computed schedule is communicated back to the Furnace Monitoring System or local operational layer.

The schedule may include:

- planned booster electric power;
- planned gas consumption;
- expected temperature trajectory;
- expected peak reduction;
- warnings if process constraints are close to their limits.

### 5. Execution and Monitoring

During execution, the system monitors:

- actual furnace temperature;
- actual booster usage;
- actual gas consumption;
- actual site or substation demand;
- deviation between planned and executed operation.

### 6. Result Storage and KPI Calculation

The service stores:

- forecast inputs;
- optimisation outputs;
- executed schedules;
- temperature evolution;
- peak demand before optimisation;
- peak demand after optimisation;
- realised peak reduction;
- process constraint violations, if any.

---

## Implementation Workflow

```text
1. Collect furnace and site data.
   |
   |-- Glass melt temperature
   |-- Minimum and maximum furnace temperature limits
   |-- Electric booster limits
   |-- Gas consumption
   |-- Electric booster usage
   |-- Production-related parameters
   |
2. Collect forecast data.
   |
   |-- PV generation forecast
   |-- Substation energy demand forecast
   |-- Day-ahead price or cost signal
   |-- Glass production estimate
   |
3. Identify expected peak demand periods.
   |
4. Run furnace-aware optimisation.
   |
   |-- Reduce booster usage during peak periods where feasible
   |-- Shift or compensate energy input through the furnace process
   |-- Adjust gas/electricity balance
   |-- Maintain temperature within lower and upper set points
   |
5. Validate process feasibility.
   |
   |-- Check glass melt temperature trajectory
   |-- Check booster power limits
   |-- Check production constraints
   |
6. Send schedule to the Furnace Monitoring System.
   |
7. Monitor execution.
   |
   |-- Track actual furnace temperature
   |-- Track actual booster usage
   |-- Track actual gas consumption
   |-- Track site/substation demand
   |
8. Store results and calculate KPIs.
```

---

## Sequence Diagram

```mermaid
sequenceDiagram
    participant Operator as Site Operator
    participant FMS as Furnace Monitoring System
    participant Furnace as Furnace Model
    participant PV as PV Forecast Service
    participant SUB as Substation Forecast
    participant OPT as Scheduling / Optimisation Mechanism
    participant CTRL as Local Control / Operational Layer
    participant DB as Data Storage
    participant UI as HMI / Dashboard

    Operator->>OPT: Define operational constraints and service objective
    FMS->>OPT: Send current furnace temperature and process status
    FMS->>OPT: Send booster limits and furnace parameters
    PV->>OPT: Send PV generation forecast
    SUB->>OPT: Send substation energy forecast

    OPT->>Furnace: Evaluate feasible furnace operating trajectories
    Furnace-->>OPT: Return thermal feasibility and temperature trajectory

    OPT->>OPT: Identify expected peak periods
    OPT->>OPT: Optimise gas/electricity balance

    OPT-->>FMS: Send peak-shaving schedule
    OPT-->>CTRL: Send operational guidance or setpoints

    CTRL-->>FMS: Execute or support implementation
    FMS-->>DB: Store measured furnace operation
    FMS-->>UI: Display process status and warnings

    OPT-->>DB: Store optimisation inputs and outputs
    DB-->>UI: Display peak reduction and service KPIs
```

---

## Data Inputs

### Metadata and Scheduling Inputs

| Input | Code Reference | Type | Structure | Unit |
|---|---|---|---|---|
| Service identifier | service_id | String | Scalar | n/a |
| Requester identifier | requester_id | String | Scalar | n/a |
| Timestamp | timestamp | DateTime | Scalar | datetime |
| Timestep | timestep | Float | Scalar | h |
| Scheduling horizon | horizon | Integer | Scalar / Set | n/a |

### Furnace Parameters

| Input | Code Reference | Type | Structure | Unit |
|---|---|---|---|---|
| Glass melt temperature | Tbf | Float | Scalar | °C |
| Minimal glass melt temperature | Tbf_min | Float | Scalar | °C |
| Maximal glass melt temperature | Tbf_max | Float | Scalar | °C |
| Maximal booster electric power | P_el_max | Float | Scalar | kW |
| Cullet | Cullet | Integer | List | % |
| Glass production estimate | Glass_out | Float | List | ton/day |

### Forecast and Network Variables

| Input | Code Reference | Type | Structure | Unit |
|---|---|---|---|---|
| Day-ahead market price | lambda_t_dam | Float | Dictionary | €/kWh |
| PV generation forecast | P_t_pv_fr | Float | Dictionary | kW |
| Energy forecast for substation | P_t_sub | Float | List | kWh |

---

## Service Outputs

| Output | Code Reference | Type | Structure | Unit |
|---|---|---|---|---|
| Gas consumption | Gas_cons | Float | List | m³ |
| Booster usage | P_el | Float | List | kW |
| Optimised schedule | optimised_schedule | Object / Array | Time series | n/a |
| Expected peak reduction | expected_peak_reduction | Float | Scalar / Time series | kW |
| Temperature trajectory | temperature_trajectory | Float | Time series | °C |
| Schedule deviation | schedule_deviation | Float | Time series / Scalar | kW or % |
| Realised peak reduction | realised_peak_reduction | Float | Scalar | kW or % |

---

## Optimisation Logic

The Peak Shaving optimisation can be represented as a process-constrained peak minimisation problem.

```text
Minimise:
    Maximum electricity demand over the scheduling horizon
    + operational penalties
    + process constraint penalties
```

Subject to:

```text
Furnace constraints:
    Tbf_min <= Tbf_t <= Tbf_max
    P_el_t <= P_el_max
    Glass production requirement is satisfied
    Cullet-related process assumptions are respected

Energy balance:
    Furnace energy demand is supplied by a combination of gas and electricity
    Booster usage can be reduced or shifted if thermal feasibility is maintained

PV and network constraints:
    PV forecast is considered in the scheduling decision
    Substation demand forecast is considered for identifying peak periods

Operational constraints:
    Gas combustion system limitations are respected
    Process safety and product quality are not compromised
```

---

## Suggested JSON Structure

```json
{
  "service": "PeakShaving",
  "demo": "Demo1",
  "metadata": {
    "service_id": "demo1_peak_shaving",
    "requester_id": "site_operator",
    "timestamp": "YYYY-MM-DDTHH:mm:ssZ"
  },
  "scheduling": {
    "timestep": 1.0,
    "horizon": 24
  },
  "furnace": {
    "glass_melt_temperature": {
      "value": null,
      "unit": "°C"
    },
    "minimal_glass_melt_temperature": {
      "value": null,
      "unit": "°C"
    },
    "maximal_glass_melt_temperature": {
      "value": null,
      "unit": "°C"
    },
    "maximal_booster_electric_power": {
      "value": null,
      "unit": "kW"
    },
    "cullet": {
      "values": [],
      "unit": "%"
    },
    "glass_production_estimate": {
      "values": [],
      "unit": "ton/day"
    }
  },
  "market": {
    "day_ahead_market_price": {
      "values": {},
      "unit": "€/kWh"
    }
  },
  "renewable_generation": {
    "pv_generation_forecast": {
      "values": {},
      "unit": "kW"
    }
  },
  "network": {
    "energy_forecast_for_substation": {
      "values": [],
      "unit": "kWh"
    }
  },
  "outputs": {
    "gas_consumption": {
      "values": [],
      "unit": "m3"
    },
    "booster_usage": {
      "values": [],
      "unit": "kW"
    },
    "expected_peak_reduction": {
      "value": null,
      "unit": "kW"
    },
    "temperature_trajectory": {
      "values": [],
      "unit": "°C"
    }
  },
  "kpis": {
    "maximum_demand_before": {
      "value": null,
      "unit": "kW"
    },
    "maximum_demand_after": {
      "value": null,
      "unit": "kW"
    },
    "realised_peak_reduction": {
      "value": null,
      "unit": "kW"
    },
    "schedule_deviation": {
      "value": null,
      "unit": "%"
    },
    "process_constraint_violations": 0
  }
}
```

---

## HMI and Dashboard Requirements

The Demo 1 Peak Shaving dashboard should display:

- forecasted substation demand;
- expected peak periods;
- PV generation forecast;
- current glass melt temperature;
- minimum and maximum glass melt temperature;
- planned booster electric power;
- planned gas consumption;
- expected temperature trajectory;
- expected peak reduction;
- realised peak reduction;
- warnings if furnace temperature approaches limits;
- comparison between planned and executed schedules.

Historical views should include:

- maximum demand before and after optimisation;
- booster usage over time;
- gas consumption over time;
- PV generation forecast versus actual production;
- substation forecast versus actual demand;
- peak reduction performance;
- process feasibility indicators.

---

## Implementation Status

The Peak Shaving service in Demo 1 reached a field-tested level as a process-coupled furnace flexibility service.

The implementation demonstrates that the furnace and its thermal inertia can be used to reshape the electrical demand profile under real industrial conditions. The final implementation is internal to the site and does not require a formal DSO activation mechanism.

| Criterion | Status |
|---|---|
| Conceptual service definition | Completed |
| Service workflow | Completed |
| Data model | Completed |
| Sequence diagram | Completed |
| API-oriented structure | Completed |
| Local implementation | Field-tested |
| External DSO interface required | No |
| Main limitation | Process constraints and furnace operating limits |
| Final readiness level | Field-tested |

---

## Lessons Learned

The Demo 1 Peak Shaving implementation provides several lessons:

- industrial thermal processes can act as flexible energy resources;
- peak reduction must be process-aware and cannot be treated as a simple load curtailment problem;
- glass melt temperature is the central constraint for service feasibility;
- electric booster usage is the main controllable electrical variable;
- gas consumption provides a compensating energy input;
- PV and substation forecasts are required to schedule the service effectively;
- service performance should be evaluated against both electrical and process indicators;
- the same furnace scheduling framework can support several objectives by changing the optimisation criterion.

---

## Recommended Repository Files

```text
PeakShaving/
└── demo1/
    ├── Description.md
    ├── implementation_files/
    │   ├── demo1_peak_shaving_architecture.png
    │   ├── demo1_peak_shaving_sequence.png
    │   └── demo1_peak_shaving_api_example.json
    └── references.md
```

---

## References

This file is based on the WP3 work developed in i-STENTORE, especially the Demo 1 Peak Shaving implementation details, harmonised data model and final service readiness assessment.
