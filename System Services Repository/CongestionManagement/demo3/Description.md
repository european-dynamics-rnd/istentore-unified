# Implementation Details of Congestion Management in Demo 3

## Demo Context

Demo 3 corresponds to the i-STENTORE demonstrator **Virtual Energy Storage System for Renewable Energy Integration (VESS) – Spain**.

The demonstrator was designed around the aggregation of renewable generation assets and storage assets into a Virtual Energy Storage System capable of providing market-oriented services, balancing services and DSO-oriented flexibility services.

In this context, Congestion Management is represented as a DSO service in which the VESS adjusts the operation of the Li-ion BESS according to network conditions and the remaining availability of the storage system. The service is intended to mitigate distribution-network constraints by absorbing excess renewable generation when network capacity is limited or by supporting local power injection when required by the grid.

The final implementation status must reflect the evolution of Demo 3. The originally planned hybrid VESS included both a Li-ion BESS and a Vanadium Redox Flow Battery. Since the VRFB subsystem could not be installed within the project timeframe, the final configuration relied on the Li-ion BESS. Moreover, the external DSO cooperation required for full congestion-management activation was not available. Therefore, Demo 3 Congestion Management is documented as **planned / modelled**, with the service logic, data flows and database structure retained in the harmonised service repository.

---

## Service Objective

The objective of the Congestion Management service in Demo 3 is to demonstrate how a VESS can provide flexibility to the DSO by modifying storage operation in response to network constraints.

The service aims to:

- mitigate local distribution-network congestion;
- absorb excess renewable generation when grid export capacity is limited;
- reduce curtailment of PV and wind generation;
- use the Li-ion BESS to store energy during congestion events;
- return stored energy to the grid when line capacity becomes available;
- adjust BESS position according to network conditions and remaining service availability;
- provide a standardised DSO-service model compatible with the i-STENTORE Service Repository.

---

## Service Classification

| Field | Description |
|---|---|
| Service name | Congestion Management |
| Demonstrator | Demo 3 – Spain |
| Demonstrator title | Virtual Energy Storage System for Renewable Energy Integration |
| Service family | Distribution infrastructure support / DSO service |
| Main objective | Mitigation of distribution-network congestion |
| Main asset concept | Virtual Energy Storage System |
| Main storage asset | Li-ion BESS |
| Originally planned additional storage | Vanadium Redox Flow Battery |
| Main renewable assets | PV, wind and run-of-river hydro generation |
| Main grid actor | DSO |
| Main control layer | HOCS / EMS and central controller |
| Final implementation level | Planned / Modelled |
| Main limitation | No operational DSO cooperation for congestion-management activation |

---

## Demonstrator Assets

### Renewable Generation Assets

- PV Power Plant;
- Wind Power Plant;
- Run-of-river Hydroelectric Power Plant.

### Storage Assets

- Li-ion Battery Energy Storage System;
- Vanadium Redox Flow Battery, originally planned in the conceptual VESS configuration.

### Digital and Control Assets

- HOCS / EMS platform;
- Cloud-based optimisation service;
- Future Database;
- Real-Time Database;
- Past Database;
- Central controller;
- Local Li-ion BESS controller;
- Local VRFB controller, retained in the conceptual service description;
- SCADA data collection layer;
- HMI / dashboard;
- i-STENTORE Middleware.

---

## Actors

| Actor | Role |
|---|---|
| DSO | Owner/operator of the distribution network and conceptual requester of the DSO service. |
| PV Power Plant | Renewable generation asset contributing to local network flows. |
| Wind Power Plant | Renewable generation asset contributing to local network flows. |
| Run-of-river Hydroelectric Power Plant | Renewable generation asset contributing to the VESS portfolio. |
| Li-ion BESS | Main controllable storage asset in the final configuration. |
| VRFB | Planned storage component of the conceptual VESS, not installed in the final configuration. |
| HOCS / EMS Platform | Coordinates data acquisition, optimisation and schedule management. |
| SCADA System | Provides real-time generation, storage and network-related measurements. |
| Optimisation Service | Computes the BESS position required to support DSO services. |
| Central Controller | Sends final setpoints to controllable assets. |
| HMI / Dashboard | Displays schedules, network status, BESS position and service results. |
| i-STENTORE Middleware | Supports harmonised data exchange and repository-compatible service representation. |

---

## Operational Principle

The Congestion Management service uses the VESS to respond to network constraints.

The general principle is:

1. The platform receives or generates information about network conditions.
2. SCADA systems collect real-time information from renewable plants and the BESS.
3. The optimisation service analyses network conditions and BESS state.
4. The remaining flexibility of the Li-ion BESS is evaluated after considering other services.
5. The BESS position is adjusted to provide DSO services.
6. Schedules are stored in the Future Database.
7. Final setpoints are communicated through the Real-Time Database to the central controller.
8. Historical information is stored in the Past Database after activation or simulation.
9. Results are displayed through the HMI and may be communicated to the i-STENTORE Middleware.

---

## Congestion Management Logic

The service considers two main congestion situations.

### 1. Excess Renewable Generation

When PV, wind or hydro generation exceeds the capacity of the local network to export power, the BESS can be charged.

This action:

- absorbs excess renewable generation;
- reduces curtailment;
- lowers the power flow through the constrained line;
- stores energy for later use.

### 2. Network Capacity Available after Congestion

When the congestion has been alleviated or line capacity becomes available, the BESS can discharge.

This action:

- returns stored renewable energy to the grid;
- improves the utilisation of renewable generation;
- supports delayed delivery of energy that could not be exported during the congestion event.

---

## Service Interaction and Remaining Availability

The Demo 3 VESS is also associated with other services, especially Energy Arbitrage and mFRR.

Therefore, Congestion Management should consider the remaining availability of the Li-ion BESS after other service commitments.

This includes:

- available state of charge;
- available charging power;
- available discharging power;
- scheduled market commitments;
- reserve-related commitments;
- degradation or cycling constraints;
- operational status of the storage system.

Priority rules may be required when Congestion Management conflicts with market-oriented or balancing-service schedules.

---

## Timing Requirements

| Parameter | Value / Description |
|---|---|
| Planning horizon | Day-ahead, intra-day or event-based |
| Discharge duration | Approximately 15 minutes in the conceptual characterisation |
| Reaction time | Milliseconds to seconds for BESS response |
| Frequency of use | Dependent on congestion occurrence and DSO request |
| Main unit of service | MWh |
| Main activation actor | DSO |
| Final activation status | Not field activated due to lack of DSO cooperation |

---

## Information and Data Flows

### 1. Network-Condition Forecasting

The i-STENTORE platform or service layer provides forecasts of network conditions.

These forecasts may include:

- potential congestion scenarios;
- expected network load;
- expected renewable export;
- available line capacity;
- congestion-risk indicators.

### 2. SCADA Data Collection

The SCADA system collects real-time data from the VESS assets, including:

- PV generation;
- wind generation;
- hydro generation;
- Li-ion BESS state of charge;
- Li-ion BESS operating status;
- BESS charge/discharge power;
- relevant network measurements.

### 3. Data Processing

The optimisation service processes:

- forecasts;
- real-time generation values;
- BESS state;
- network-condition information;
- already committed service schedules.

The objective is to determine the remaining flexibility available for DSO services.

### 4. Optimisation Solving

The optimisation service adjusts the Li-ion BESS position to provide congestion-management flexibility.

The optimisation determines:

- whether to charge or discharge the BESS;
- the required active power setpoint;
- the expected energy shifted;
- the expected congestion relief;
- the feasible duration of the response.

### 5. Schedule Logging

The computed schedules are saved in the Future Database.

The Future Database communicates with the Real-Time Database when the service schedule becomes operational.

### 6. Setpoint Execution

The Real-Time Database communicates with the central controller.

The central controller sends the required setpoints to the Li-ion BESS local controller.

### 7. Historical Storage and Middleware Communication

After activation, simulation or execution, historical information is sent from the Real-Time Database to the Past Database.

Results may also be communicated with the i-STENTORE Middleware from both the Real-Time Database and the Past Database.

### 8. Dashboard Display

The HMI displays information from:

- Past Database;
- Future Database;
- Real-Time Database.

The displayed information may include forecasts, schedules, setpoints, BESS SoC, delivered flexibility and service KPIs.

---

## Implementation Workflow

```text
1. Receive or generate network-condition information.
   |
   |-- Congestion forecast
   |-- DSO request
   |-- Network-load information
   |-- Renewable export limitation
   |
2. Collect VESS data through SCADA.
   |
   |-- PV generation
   |-- Wind generation
   |-- Hydro generation
   |-- Li-ion BESS SoC
   |-- BESS operational status
   |-- Network measurements
   |
3. Process data in the optimisation service.
   |
   |-- Analyse network conditions
   |-- Assess BESS availability
   |-- Check remaining flexibility after other services
   |
4. Optimise BESS position for DSO service.
   |
   |-- Charge BESS during excess renewable generation
   |-- Discharge BESS when grid capacity is available
   |-- Respect SoC and power limits
   |
5. Save schedule in the Future Database.
   |
6. Transfer final setpoints to the Real-Time Database.
   |
7. Send setpoints to the central controller.
   |
8. Dispatch setpoints to the Li-ion BESS controller.
   |
9. Store activation or simulation results in the Past Database.
   |
10. Display results on HMI and share with i-STENTORE Middleware where applicable.
```

---

## Sequence Diagram

```mermaid
sequenceDiagram
    participant DSO as DSO
    participant Platform as i-STENTORE Platform / Service Layer
    participant SCADA as SCADA System
    participant HOCS as HOCS / EMS Platform
    participant Opt as Optimisation Service
    participant FDB as Future DB
    participant RTDB as RT DB
    participant CTRL as Central Controller
    participant BESS as Li-ion BESS Controller
    participant PDB as Past DB
    participant UI as HMI / Dashboard
    participant MW as i-STENTORE Middleware

    DSO->>Platform: Provide congestion request or grid constraint
    Platform->>HOCS: Send network-condition forecast

    SCADA->>HOCS: Send PV, wind and hydro generation data
    SCADA->>HOCS: Send BESS SoC and operational status

    HOCS->>Opt: Send forecasts, network conditions and BESS status
    Opt->>Opt: Assess remaining flexibility
    Opt->>Opt: Optimise BESS position for DSO service
    Opt-->>HOCS: Return congestion-management schedule

    HOCS->>FDB: Store future schedule
    FDB->>RTDB: Transfer final operational setpoints
    RTDB->>CTRL: Send setpoints
    CTRL->>BESS: Dispatch charge/discharge command

    BESS-->>RTDB: Return execution status
    RTDB->>PDB: Store historical activation data
    RTDB->>MW: Share real-time service information
    PDB->>MW: Share historical service information
    PDB->>UI: Display historical results
    FDB->>UI: Display future schedules
    RTDB->>UI: Display real-time operation
```

---

## Data Inputs

### Network and DSO Inputs

| Input | Description | Unit |
|---|---|---|
| dso_request | Flexibility request or congestion-management activation signal. | n/a |
| network_condition_forecast | Forecast of expected network condition. | n/a / time series |
| congestion_period | Time interval during which congestion is expected. | datetime |
| available_line_capacity | Available physical line capacity. | kW / MW |
| network_load | Current or forecasted network load. | kW / MW |
| congestion_risk_indicator | Indicator of expected congestion severity. | n/a or % |

### Renewable Generation Inputs

| Input | Description | Unit |
|---|---|---|
| pv_generation | Real-time PV generation. | kW / MW |
| pv_generation_forecast | Forecasted PV generation. | kW / MW |
| wind_generation | Real-time wind generation. | kW / MW |
| wind_generation_forecast | Forecasted wind generation. | kW / MW |
| hydro_generation | Run-of-river hydro generation. | kW / MW |

### BESS Inputs

| Input | Description | Unit |
|---|---|---|
| bess_state_of_charge | Current state of charge of the Li-ion BESS. | % or MWh |
| bess_operational_status | Availability and operating state of the BESS. | status |
| bess_charging_power_max | Maximum BESS charging power. | kW / MW |
| bess_discharging_power_max | Maximum BESS discharging power. | kW / MW |
| bess_energy_capacity | Available BESS energy capacity. | kWh / MWh |
| bess_remaining_availability | Remaining availability after other service commitments. | kW / MW or % |

---

## Service Outputs

| Output | Description | Unit |
|---|---|
| congestion_management_schedule | Planned BESS operation for DSO service. | time series |
| bess_charging_setpoint | BESS charging power during congestion caused by excess generation. | kW / MW |
| bess_discharging_setpoint | BESS discharging power when grid capacity is available. | kW / MW |
| expected_congestion_relief | Expected reduction of constrained network flow. | kW / MW |
| renewable_curtailment_avoided | Renewable energy absorbed instead of curtailed. | kWh / MWh |
| delivered_flexibility | Actual flexibility delivered by the BESS. | kW / MW |
| schedule_deviation | Difference between planned and executed service delivery. | kW / MW or % |
| service_status | Activation, simulation or planning status. | status |

---

## Optimisation Logic

The Congestion Management optimisation can be represented as a network-constrained storage dispatch problem.

```text
Minimise:
    Congestion-related power-flow violation
    + renewable curtailment
    + deviation from DSO requested profile
    + operational penalties
```

Subject to:

```text
BESS constraints:
    SoC_min <= SoC_t <= SoC_max
    P_charge_t <= P_charge_max
    P_discharge_t <= P_discharge_max
    SoC_t = SoC_t-1 + charged_energy - discharged_energy
    Remaining capacity after other services is respected

Network constraints:
    Power flow through constrained element <= available line capacity
    BESS charge/discharge action is aligned with congestion direction

Renewable generation constraints:
    PV, wind and hydro generation are considered
    Excess renewable generation can be stored if BESS capacity is available

Service coordination constraints:
    Energy Arbitrage and mFRR schedules are considered
    DSO-service priority rules are applied where required
```

---

## Suggested JSON Structure

```json
{
  "service": "CongestionManagement",
  "demo": "Demo3",
  "metadata": {
    "service_id": "demo3_congestion_management",
    "requester_id": "dso_or_internal_scenario",
    "timestamp": "YYYY-MM-DDTHH:mm:ssZ"
  },
  "network": {
    "congestion_period": {
      "start": "YYYY-MM-DDTHH:mm:ssZ",
      "end": "YYYY-MM-DDTHH:mm:ssZ"
    },
    "available_line_capacity": {
      "value": null,
      "unit": "kW"
    },
    "network_load": {
      "value": null,
      "unit": "kW"
    },
    "network_condition_forecast": [],
    "congestion_risk_indicator": null
  },
  "renewable_generation": {
    "pv_generation": {
      "value": null,
      "unit": "kW"
    },
    "wind_generation": {
      "value": null,
      "unit": "kW"
    },
    "hydro_generation": {
      "value": null,
      "unit": "kW"
    },
    "pv_generation_forecast": [],
    "wind_generation_forecast": []
  },
  "storage": {
    "li_ion_bess": {
      "state_of_charge": {
        "value": null,
        "unit": "%"
      },
      "operational_status": "available",
      "charging_power_max": {
        "value": null,
        "unit": "kW"
      },
      "discharging_power_max": {
        "value": null,
        "unit": "kW"
      },
      "remaining_availability": {
        "value": null,
        "unit": "kW"
      }
    },
    "vrfb": {
      "included_in_final_configuration": false,
      "status": "not_installed"
    }
  },
  "outputs": {
    "congestion_management_schedule": [],
    "bess_charging_setpoint": [],
    "bess_discharging_setpoint": [],
    "expected_congestion_relief": {
      "value": null,
      "unit": "kW"
    },
    "renewable_curtailment_avoided": {
      "value": null,
      "unit": "kWh"
    }
  },
  "kpis": {
    "requested_flexibility": null,
    "delivered_flexibility": null,
    "service_delivery_accuracy": null,
    "schedule_deviation": null,
    "curtailment_reduction": null
  }
}
```

---

## HMI and Dashboard Requirements

The Demo 3 Congestion Management dashboard should display:

- network-condition forecast;
- congestion-risk indicator;
- available line capacity;
- PV, wind and hydro generation;
- Li-ion BESS state of charge;
- BESS charge/discharge power;
- future congestion-management schedule;
- real-time BESS setpoints;
- historical activation or simulation results;
- delivered flexibility;
- renewable curtailment avoided;
- schedule deviation;
- market or DSO-service interaction information where available.

A dedicated market and flexibility dashboard is recommended for Demo 3, including:

- bids or service requests;
- activations;
- delivered power;
- revenues or settlement indicators, where applicable;
- data export in CSV or JSON for strategic optimisation.

---

## Implementation Status

The Congestion Management service in Demo 3 is documented as a VESS-based DSO service but did not reach full field activation.

The main reasons are:

- the VRFB subsystem was not installed within the project timeframe;
- the final operational configuration relied on the Li-ion BESS;
- no DSO cooperation or operational congestion-management activation was available for full validation;
- the service remained at planned / modelled readiness level.

Nevertheless, the service remains valid in the harmonised i-STENTORE catalogue because the workflow, data model, sequence logic and middleware compatibility were fully defined.

| Criterion | Status |
|---|---|
| Conceptual service definition | Completed |
| Service workflow | Completed |
| Sequence diagram | Completed |
| Data model orientation | Completed |
| API-oriented structure | Completed |
| VESS conceptual architecture | Completed |
| VRFB integration | Not completed |
| DSO field activation | Not available |
| Final readiness level | Planned / Modelled |
| Final repository service | Yes, as Demo 3 DSO-service model |

---

## Lessons Learned

The Demo 3 Congestion Management implementation provides several lessons:

- VESS architectures are technically suitable for DSO-oriented congestion services;
- storage can reduce renewable curtailment by absorbing excess generation during network constraints;
- DSO cooperation is essential for full operational activation of congestion-management services;
- the absence of one planned storage technology reduces the flexibility envelope but does not invalidate the service model;
- the Future DB, RT DB and Past DB structure supports clear separation of schedules, real-time operation and historical results;
- service stacking requires explicit treatment of remaining BESS availability;
- harmonised service models remain useful even where the final implementation is limited by external or site-level constraints;
- Demo 3 is a relevant reference case for future VESS-based DSO flexibility services.

---