# Peak Shaving

Peak Shaving is a behind-the-meter and local energy-management flexibility service based on the use of energy storage or process flexibility to reduce the maximum power demand observed by a site, network segment or connection point.

The service is activated when the expected demand approaches a predefined threshold or when the system operator, aggregator or local energy manager aims to reduce peak power consumption. By shifting or reducing demand during high-consumption periods, Peak Shaving can reduce energy access costs, alleviate stress on the local grid and support a more efficient integration of renewable energy sources.

In the i-STENTORE Service Repository, Peak Shaving is documented as a functional service associated with **Demo 1 – Molten glass thermal storage for increased uptake of renewable electric energy in Slovenia**.

---

## Service Category

Peak Shaving belongs to the following service families:

- Generation Support Services;
- Bulk Storage Services;
- Behind-the-meter Energy Management Services;
- Local Flexibility Services;
- Demand Reduction / Load Shifting Services;
- Industrial Process Flexibility Services.

---

## Purpose of the Service

The purpose of Peak Shaving is to reduce the maximum power demand of a site or local network segment by modifying the operation of flexible assets.

The service supports:

- reduction of peak electricity demand;
- reduction of energy access costs;
- improved use of local renewable generation;
- reduction of stress on local electrical infrastructure;
- smoother interaction between industrial demand and grid constraints;
- increased flexibility of industrial processes;
- improved energy management at site level.

---

## General Description

Peak Shaving involves the controlled use of storage, flexible loads or process-related energy buffers to avoid or reduce demand peaks.

In conventional battery-based implementations, the storage device is charged during low-demand or low-price periods and discharged during peak-demand periods. In industrial thermal applications, the same functional principle can be implemented by modifying the electricity and fuel balance of a process while maintaining production quality and respecting process constraints.

In Demo 1, Peak Shaving is linked to the molten glass furnace system. The furnace and its thermal inertia provide flexibility by allowing the scheduling mechanism to manipulate electricity and gas consumption while keeping the glass melt temperature within predefined limits.

---

## Value Proposition

| Value Stream | Description |
|---|---|
| Peak Demand Reduction | Reduction of the maximum demand observed at the site or substation. |
| Cost Avoidance | Potential reduction of demand-related grid charges or access costs. |
| Renewable Integration | Better alignment between local PV generation and industrial consumption. |
| Process Flexibility | Use of industrial thermal inertia as an energy flexibility resource. |
| Grid Support | Reduced stress on local network infrastructure during high-demand periods. |
| Service Stacking | Potential coordination with congestion management or energy optimisation services. |

---

## Technical Principle

The service relies on forecast-based scheduling and process-aware optimisation.

The general principle is:

1. Forecast the expected electricity demand of the site or substation.
2. Forecast local renewable generation.
3. Estimate process requirements and production needs.
4. Identify expected peak-demand periods.
5. Optimise flexible asset operation to reduce the peak.
6. Validate that the resulting operation respects process constraints.
7. Dispatch the schedule to the relevant controller or monitoring system.
8. Monitor execution and calculate peak reduction performance.

---

## Typical Time Horizon

| Parameter | Typical Value / Description |
|---|---|
| Planning horizon | Day-ahead or intra-day |
| Update frequency | Periodic, depending on process and forecast availability |
| Response time | Minutes to hours, depending on the asset and process constraints |
| Main optimisation target | Reduction of maximum demand |
| Main operational constraint | Asset availability and process quality |
| Main unit of measurement | kW, MW, kWh or MWh |

---

## Typical Actors

| Actor | Role |
|---|---|
| Site Operator | Defines operational constraints and production requirements. |
| Energy Management System | Coordinates data acquisition, forecasting and optimisation. |
| Furnace Monitoring System | Monitors furnace operation and receives scheduling feedback. |
| Forecasting Service | Provides PV generation, demand and market-related forecasts. |
| Optimisation Engine | Computes the peak-reduction schedule. |
| Local Controller | Executes or supports implementation of the schedule. |
| DSO / Aggregator | May request or benefit from peak reduction when the service is externally activated. |
| HMI / Dashboard | Displays forecasts, schedules, execution status and KPIs. |
| i-STENTORE Middleware | Supports harmonised data exchange and semantic alignment. |

---

## Demonstrator Implementation

| Demonstrator | Implementation Focus | Repository File |
|---|---|---|
| Demo 1 | Peak reduction through process-coupled furnace flexibility and scheduling of gas/electricity consumption. | `PeakShaving/demo1/Description.md` |

In the final repository structure, Peak Shaving is documented as a Demo 1 service. Other demonstrators may include load shifting or energy-management behaviours in a broader sense, but the service-specific repository entry is associated with Demo 1.

---

## Demo 1 Implementation Summary

Demo 1 uses molten glass thermal storage and furnace process flexibility to provide internal flexibility.

The service uses:

- PV generation forecast;
- furnace model;
- glass melt temperature limits;
- booster electric power constraints;
- gas consumption flexibility;
- glass production estimate;
- energy forecast for the substation;
- scheduling horizon and timestep.

The optimisation modifies the balance between gas and electricity consumption in the furnace process to reduce electricity demand during peak periods while ensuring that the glass melt temperature remains within the predefined lower and upper bounds.

---

## Typical Inputs

Peak Shaving requires a combination of scheduling parameters, furnace parameters, market or cost signals, renewable generation forecasts and network-level demand forecasts.

### Metadata and Scheduling Inputs

| Input | Description | Unit |
|---|---|---|
| service_id | Identifier of the requested service. | n/a |
| requester_id | Identifier of the actor requesting the service. | n/a |
| timestamp | Timestamp of the request. | datetime |
| timestep | Time resolution of the optimisation. | h or implementation-specific |
| horizon | Number of timesteps in the scheduling horizon. | n/a |

### Furnace and Process Inputs

| Input | Description | Unit |
|---|---|---|
| glass_melt_temperature | Current glass melt temperature. | °C |
| minimal_glass_melt_temperature | Minimum allowed glass melt temperature. | °C |
| maximal_glass_melt_temperature | Maximum allowed glass melt temperature. | °C |
| maximal_booster_electric_power | Maximum electric booster power. | kW |
| cullet | Cullet share used in the production process. | % |
| glass_production_estimate | Expected glass production. | ton/day |

### Forecast and Market Inputs

| Input | Description | Unit |
|---|---|---|
| day_ahead_market_price | Day-ahead electricity price signal. | €/kWh |
| generation_forecast | PV generation forecast. | kW |
| energy_forecast_for_substation | Expected energy demand at the substation. | kWh |

---

## Typical Outputs

| Output | Description | Unit |
|---|---|---|
| gas_consumption | Planned gas consumption profile. | m³ |
| booster_usage | Planned electric booster usage. | kW |
| peak_reduction | Expected reduction in peak demand. | kW / MW |
| optimised_schedule | Final process-aware energy schedule. | n/a |
| temperature_trajectory | Expected glass melt temperature trajectory. | °C |
| schedule_deviation | Difference between planned and executed operation. | kW / MW or % |
| peak_shaving_kpi | Performance indicator describing achieved peak reduction. | % or kW |

---

## Generic Implementation Workflow

```text
1. Data Acquisition
   |
   |-- Collect furnace temperature
   |-- Collect minimum and maximum temperature limits
   |-- Collect electric booster limits
   |-- Collect gas and electricity consumption data
   |-- Collect PV generation forecast
   |-- Collect site or substation demand forecast
   |-- Collect production forecast
   |
2. Peak Identification
   |
   |-- Analyse expected site or substation demand
   |-- Identify forecasted peak periods
   |-- Define target peak-reduction objective
   |
3. Optimisation
   |
   |-- Use furnace model and PV forecast
   |-- Modify gas/electricity balance
   |-- Respect temperature and production constraints
   |-- Compute booster usage and gas consumption schedule
   |
4. Schedule Validation
   |
   |-- Check furnace temperature remains within limits
   |-- Check production requirements are respected
   |-- Check booster electric power limits
   |
5. Dispatch
   |
   |-- Send schedule to the Furnace Monitoring System
   |-- Update local control or operational guidance
   |
6. Monitoring
   |
   |-- Track execution
   |-- Compare planned and realised demand
   |-- Store results and calculate KPIs
```

---

## Service Interaction

Peak Shaving may interact with other services when the same flexible asset is used for several objectives.

| Related Service | Interaction |
|---|---|
| Congestion Management | Peak reduction and congestion relief may use similar scheduling mechanisms but have different activation triggers. |
| Energy Arbitrage / Cost Optimisation | Price-driven operation may conflict with peak reduction if high-price and high-demand periods do not coincide. |
| Power Smoothing | Short-term smoothing may require additional flexibility margins. |
| Demand Response | Peak Shaving may be implemented as part of a broader demand response strategy. |

In Demo 1, Peak Shaving and Congestion Management share similar modelling and scheduling foundations, but they differ in the service trigger and optimisation objective. Congestion Management may be externally requested by a DSO, whereas Peak Shaving can be internally triggered by the site energy-management objective.

---

## Data Model Orientation

The Peak Shaving data model should distinguish between:

- metadata;
- scheduling parameters;
- furnace parameters;
- market and forecast variables;
- network demand forecasts;
- optimisation outputs;
- performance indicators.

The service should be compatible with the i-STENTORE semantic and API framework.

The data model should therefore support:

- consistent naming of furnace and process parameters;
- explicit units;
- time-series representation of forecasts and schedules;
- alignment with the i-STENTORE ontology;
- JSON-based information exchange;
- OpenAPI-compatible interfaces;
- future integration with NGSI-LD data structures.

---

## Suggested Data Model Structure

```text
PeakShavingService

├── Metadata
│   ├── service_id
│   ├── requester_id
│   └── timestamp
│
├── Scheduling
│   ├── timestep
│   └── horizon
│
├── FurnaceParameters
│   ├── glass_melt_temperature
│   ├── minimal_glass_melt_temperature
│   ├── maximal_glass_melt_temperature
│   ├── maximal_booster_electric_power
│   ├── cullet
│   └── glass_production_estimate
│
├── Forecasts
│   ├── day_ahead_market_price
│   ├── pv_generation_forecast
│   └── energy_forecast_for_substation
│
├── Outputs
│   ├── gas_consumption
│   ├── booster_usage
│   ├── optimised_schedule
│   ├── expected_peak_reduction
│   └── temperature_trajectory
│
└── KPIs
    ├── realised_peak_reduction
    ├── schedule_deviation
    ├── maximum_demand_before
    ├── maximum_demand_after
    └── process_constraint_violations
```

---

## Suggested JSON Skeleton

```json
{
  "service": "PeakShaving",
  "demo": "Demo1",
  "metadata": {
    "service_id": "example_service_id",
    "requester_id": "example_requester_id",
    "timestamp": "YYYY-MM-DDTHH:mm:ssZ"
  },
  "scheduling": {
    "timestep": 1.0,
    "horizon": 24
  },
  "ESS": {
    "FURNACE": {
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
    }
  },
  "MARKET": {
    "day_ahead_market_price": {
      "values": {},
      "unit": "€/kWh"
    }
  },
  "RES": {
    "PVPP": {
      "generation_forecast": {
        "values": {},
        "unit": "kW"
      }
    }
  },
  "Network": {
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
    "realised_peak_reduction": null,
    "schedule_deviation": null,
    "maximum_demand_before": null,
    "maximum_demand_after": null
  }
}
```

---

## Implementation Requirements

A Peak Shaving implementation should include:

- reliable demand or substation energy forecast;
- process model of the flexible asset;
- asset limits and safety constraints;
- definition of the peak-reduction objective;
- scheduling horizon and timestep;
- validation of process feasibility;
- mechanism for dispatching or communicating the schedule;
- monitoring of actual demand and process variables;
- performance assessment against the baseline.

---

## HMI and Dashboard Requirements

The Peak Shaving dashboard should display:

- forecasted site or substation demand;
- identified peak periods;
- PV generation forecast;
- glass melt temperature;
- minimum and maximum temperature limits;
- planned booster electric power;
- planned gas consumption;
- expected peak reduction;
- realised peak reduction;
- schedule deviations;
- process constraint warnings.

Historical views should include:

- maximum demand before and after optimisation;
- planned versus realised booster operation;
- planned versus realised gas consumption;
- temperature trajectory;
- peak reduction performance over time.

---

## Implementation Status

In the final service repository, Peak Shaving is represented as a process-coupled industrial flexibility service.

For Demo 1, the service is associated with furnace flexibility and has a field-tested readiness status in the final assessment, reflecting the operational use of process-aware flexibility to reduce or shift demand without compromising production constraints.

| Criterion | Status |
|---|---|
| Conceptual service definition | Completed |
| Service workflow | Completed |
| Data model | Completed |
| API-oriented structure | Completed |
| Demo-specific implementation | Demo 1 |
| Final repository service | Yes |
| Final readiness level | Field-tested for Demo 1 process-coupled flexibility |

---

## Lessons Learned

The Peak Shaving service shows that industrial process flexibility can be represented using the same service-oriented structure as conventional storage-based flexibility.

Key lessons include:

- peak reduction can be achieved using process-coupled flexibility, not only batteries;
- furnace thermal inertia provides an operational buffer for shifting electricity use;
- temperature limits are the central feasibility constraint for the service;
- PV forecasts and demand forecasts are essential for planning the schedule;
- the service requires close integration with the local monitoring system;
- the same scheduling mechanism can support different objectives depending on the optimisation criterion;
- semantic harmonisation is important to describe process variables such as glass melt temperature and booster power consistently.

---

## Files in this Directory

```text
PeakShaving/

├── README.md
└── demo1/
    └── Description.md
```

---

## References

This service description is based on the i-STENTORE WP3 deliverables and Task 3.5 work on flexibility services based on energy storage technologies.

Main reference material:

- D3.4 – Initial definition of Peak Shaving as a system service;
- D3.5 – Implementation details and harmonised data model for Peak Shaving;
- D3.6 – Final Service Repository structure, semantic alignment and service readiness assessment;
- WP3 Final Review Meeting material for Task 3.5.
