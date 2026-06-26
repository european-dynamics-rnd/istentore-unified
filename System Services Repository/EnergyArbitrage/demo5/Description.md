# Implementation Details of Energy Arbitrage in Demo 5

## Demo Context

Demo 5 corresponds to the i-STENTORE demonstrator focused on a **green steel production facility with energy storage capabilities based on hydrogen and BESS technologies**.

The final demonstrator scope is centred on the industrial energy system located in Sweden, where the flexibility service is implemented around the coordinated operation of renewable energy inputs, a Li-ion Battery Energy Storage System, hydrogen production and local industrial energy requirements.

---

## Service Objective

The objective of the Energy Arbitrage service in Demo 5 is to optimise the operation of the industrial hybrid energy system according to electricity market signals and renewable energy availability.

The service aims to:

- minimise the cost of electricity procurement for hydrogen production and industrial operation;
- maximise the use of renewable electricity for hydrogen production;
- optimise the charging and discharging schedule of the Li-ion BESS;
- coordinate BESS operation with electrolyser operation and hydrogen storage constraints;
- support day-ahead energy scheduling;
- reduce the cost of hydrogen production when market conditions are favourable;
- provide a reusable service model for industrial multi-energy sites.

---

## Service Classification

| Field | Description |
|---|---|
| Service name | Energy Arbitrage |
| Demonstrator | Demo 5 – Sweden |
| Final demonstrator context | Green steel production facility with hydrogen and BESS technologies |
| Service family | Generation Support / Bulk Storage / Trading Service |
| Main objective | Day-ahead cost optimisation and energy scheduling |
| Main market orientation | Day-ahead electricity market |
| Main storage asset | Li-ion BESS |
| Main flexible asset | Electrolyser and hydrogen production system |
| Additional flexibility | Hydrogen storage and industrial energy management |
| Main control layer | HOCS / supervisory optimisation platform |
| Final repository scope | Energy Arbitrage and FCR only |

---

## Final Scope Correction

The final repository scope for Demo 5 should not include Congestion Management, aFRR or mFRR as active Demo 5 service entries.

The retained Demo 5 services are:

- Energy Arbitrage;
- Frequency Containment Reserve.

Accordingly, this Energy Arbitrage file should be used together with:

```text
FCR/demo5/Description.md
```

The following Demo 5 service files should not be created as final active services:

```text
CongestionManagement/demo5/Description.md
aFRR/demo5/Description.md
mFRR/demo5/Description.md
```

If legacy diagrams or tables from earlier deliverables include those services, they should be treated as historical design-stage elements and not as final repository entries.

---

## Demonstrator Assets

The Energy Arbitrage service is based on the coordination of storage, renewable electricity, hydrogen production and industrial energy demand.

### Main Assets

- Li-ion Battery Energy Storage System;
- Electrolyser;
- Hydrogen storage system;
- Industrial hydrogen demand;
- Industrial electricity demand;
- Renewable electricity supply or renewable electricity procurement;
- Grid connection and market interface.

### Digital and Control Assets

- HOCS / supervisory control platform;
- HOCS database layer;
- Optimisation solver;
- Forecasting module;
- Local BESS controller;
- Local electrolyser controller;
- HMI / dashboard;
- Market interface or market representative.

---

## Actors

| Actor | Role |
|---|---|
| Plant Operator | Operates the industrial site and defines operational constraints. |
| Market Representative | Interfaces with the electricity market and supports day-ahead scheduling or market participation. |
| Market Operator | Operates and clears the relevant day-ahead electricity market. |
| HOCS / Supervisory Platform | Hosts the optimisation logic, forecasting routines and coordination of control signals. |
| HOCS Database | Stores forecast data, schedules, market results, real-time measurements and historical performance. |
| BESS Local Controller | Executes the battery charging and discharging schedule. |
| Electrolyser Local Controller | Executes the hydrogen production schedule. |
| Hydrogen Storage System | Provides operational buffer for hydrogen production and consumption. |
| Forecast Service | Provides electricity price forecasts, renewable generation forecasts and demand forecasts. |
| User Interface / HMI | Displays forecasts, schedules, asset status, market results and KPIs. |
| i-STENTORE Middleware | Supports harmonised data exchange with the wider i-STENTORE digital ecosystem. |

---

## Operational Principle

The Energy Arbitrage service determines an optimised operating schedule for the BESS and electrolyser using forecasted electricity prices, renewable availability and hydrogen demand.

The service follows the principle that the industrial system should increase electricity consumption or charge the BESS when electricity is cheaper or renewable energy availability is higher, and reduce consumption or discharge the BESS when electricity is more expensive.

For hydrogen production, the service seeks to schedule electrolyser operation during favourable market and renewable conditions while respecting hydrogen demand and storage constraints.

---

## Energy Arbitrage Logic

The service coordinates three main forms of flexibility.

### 1. Battery Flexibility

The Li-ion BESS can be charged during low-price periods and discharged during high-price periods or when local optimisation requires support.

Battery operation is constrained by:

- rated power;
- available energy capacity;
- state-of-charge limits;
- charging and discharging efficiency;
- battery availability;
- possible reserve capacity allocation for FCR.

### 2. Electrolyser Flexibility

The electrolyser can shift hydrogen production according to electricity prices, renewable availability and hydrogen storage level.

Electrolyser operation is constrained by:

- rated power;
- minimum and maximum operating levels;
- hydrogen production targets;
- hydrogen demand;
- storage level;
- operational start-up or ramping constraints;
- green hydrogen requirements.

### 3. Hydrogen Storage Flexibility

Hydrogen storage provides a buffer between electricity-driven hydrogen production and industrial hydrogen consumption.

Storage operation is constrained by:

- minimum and maximum storage level;
- pressure-related constraints;
- hydrogen demand profile;
- safety and operational limits.

---

## Timing Requirements

| Parameter | Value / Description |
|---|---|
| Main planning horizon | Day-ahead |
| Main optimisation objective | Cost reduction / market-oriented scheduling |
| Frequency of use | Daily or periodically updated |
| Typical reaction time | Hours |
| Output horizon | Day-ahead operating schedule |
| Main energy unit | MWh |
| Main market signal | Day-ahead electricity price |
| Relevant operational indicator | Cost of hydrogen production / energy cost |

---

## Information and Data Flows

### 1. Data Acquisition

The supervisory platform collects:

- BESS state of charge;
- BESS availability;
- BESS power limits;
- electrolyser status;
- hydrogen storage level;
- hydrogen demand;
- industrial energy demand;
- renewable generation or renewable procurement data;
- electricity market prices;
- site operational constraints.

### 2. Forecasting

The forecasting layer provides:

- day-ahead electricity price forecast;
- renewable generation or renewable electricity availability forecast;
- hydrogen demand forecast;
- industrial demand forecast;
- optional grid or local energy constraints.

### 3. Optimisation

The optimisation solver receives forecasts and asset constraints.

It computes:

- BESS charging schedule;
- BESS discharging schedule;
- electrolyser operating schedule;
- hydrogen production profile;
- hydrogen storage trajectory;
- expected electricity cost;
- expected cost reduction;
- expected renewable electricity utilisation.

### 4. Schedule Validation

The HOCS / supervisory platform validates the optimised schedule against:

- BESS operating limits;
- electrolyser operating limits;
- hydrogen storage constraints;
- industrial production requirements;
- FCR reserve allocation, where applicable;
- safety and availability constraints.

### 5. Dispatch Preparation

The validated schedule is stored and prepared for dispatch.

The system may generate:

- BESS setpoints;
- electrolyser setpoints;
- hydrogen production schedule;
- market-related schedules;
- dashboard outputs.

### 6. Execution

The local controllers execute the schedule.

The BESS controller receives charging and discharging instructions.

The electrolyser controller receives hydrogen production instructions.

### 7. Monitoring and Reporting

The system stores and displays:

- planned schedule;
- executed schedule;
- BESS SoC;
- electrolyser load;
- hydrogen storage level;
- electricity price;
- energy cost;
- cost savings;
- renewable electricity utilisation;
- deviations from the planned operation.

---

## Implementation Workflow

```text
1. The HOCS / supervisory platform collects asset status and site data.
   |
   |-- BESS state of charge and availability
   |-- Electrolyser availability
   |-- Hydrogen storage level
   |-- Industrial demand
   |-- Hydrogen demand
   |-- Renewable energy availability
   |-- Electricity market data
   |
2. Forecasting modules provide day-ahead input data.
   |
   |-- Electricity price forecast
   |-- Renewable generation or availability forecast
   |-- Hydrogen demand forecast
   |-- Industrial demand forecast
   |
3. The optimisation solver computes the energy arbitrage schedule.
   |
   |-- BESS charge/discharge schedule
   |-- Electrolyser operating schedule
   |-- Hydrogen production profile
   |-- Hydrogen storage trajectory
   |-- Expected electricity cost
   |
4. The HOCS validates the schedule.
   |
   |-- Check BESS constraints
   |-- Check electrolyser constraints
   |-- Check hydrogen storage constraints
   |-- Check reserve allocation for FCR if needed
   |-- Check industrial constraints
   |
5. The final schedule is stored in the database.
   |
6. Setpoints are sent to local controllers.
   |
   |-- BESS local controller
   |-- Electrolyser local controller
   |
7. The HMI displays schedules, asset status and KPIs.
   |
8. Historical data are stored for performance assessment.
```

---

## Sequence Diagram

```mermaid
sequenceDiagram
    participant Plant as Plant Operator
    participant HOCS as HOCS / Supervisory Platform
    participant DB as HOCS Database
    participant Forecast as Forecast Service
    participant Opt as Optimisation Solver
    participant Market as Market Interface
    participant BESS as BESS Local Controller
    participant ELY as Electrolyser Controller
    participant H2 as Hydrogen Storage
    participant UI as HMI / Dashboard

    Plant->>HOCS: Provide operational constraints
    BESS->>HOCS: Send SoC, power limits and availability
    ELY->>HOCS: Send electrolyser status and limits
    H2->>HOCS: Send hydrogen storage level
    Market->>HOCS: Provide day-ahead price data

    HOCS->>Forecast: Request price, renewable and demand forecasts
    Forecast-->>HOCS: Return forecasts

    HOCS->>Opt: Send forecasts, constraints and asset status
    Opt-->>HOCS: Return optimised arbitrage schedule

    HOCS->>DB: Store optimised schedule
    HOCS->>Market: Prepare or update market-related schedule

    HOCS->>BESS: Send BESS charge/discharge setpoints
    HOCS->>ELY: Send electrolyser operating schedule

    BESS-->>HOCS: Return execution status
    ELY-->>HOCS: Return execution status
    H2-->>HOCS: Return updated hydrogen storage level

    HOCS->>DB: Store measured operation and KPIs
    HOCS->>UI: Display schedules, asset status and performance
```

---

## Data Inputs

### Static Parameters

| Parameter | Asset | Description | Typical Unit |
|---|---|---|---|
| BESS rated power | BESS | Maximum charging/discharging power | kW / MW |
| BESS energy capacity | BESS | Available energy storage capacity | kWh / MWh |
| Minimum SoC | BESS | Lower operational SoC limit | % |
| Maximum SoC | BESS | Upper operational SoC limit | % |
| Charging efficiency | BESS | Charging efficiency | % |
| Discharging efficiency | BESS | Discharging efficiency | % |
| Electrolyser rated power | Electrolyser | Maximum electrical input power | kW / MW |
| Electrolyser hydrogen production rate | Electrolyser | Hydrogen production per energy input | kg/h or kg/MWh |
| Hydrogen storage capacity | H2 storage | Maximum hydrogen storage capacity | kg |
| Minimum hydrogen storage level | H2 storage | Minimum operational reserve | kg |
| Market gate closure | Market interface | Deadline for day-ahead scheduling | hh:mm |
| Optimisation horizon | Optimisation solver | Scheduling horizon | h |

### Runtime Variables

| Variable | Source | Description | Typical Unit |
|---|---|---|---|
| Current BESS SoC | BESS controller | Current state of charge | % |
| BESS availability | BESS controller | Availability status of the BESS | Boolean / status |
| Current BESS power | BESS controller | Measured active power | kW / MW |
| Electrolyser status | Electrolyser controller | Current operating state | status |
| Electrolyser power | Electrolyser controller | Current electrical consumption | kW / MW |
| Hydrogen storage level | Hydrogen storage system | Current stored hydrogen | kg |
| Hydrogen demand | Industrial process | Required hydrogen consumption | kg/h |
| Industrial demand | Site metering / EMS | Electricity demand of the industrial site | kW / MW |
| Electricity price | Market data source | Current or latest market price | EUR/MWh |

### Forecast Variables

| Forecast | Source | Description | Typical Unit |
|---|---|---|---|
| Day-ahead electricity price forecast | Forecast Service | Expected electricity prices | EUR/MWh |
| Renewable electricity availability forecast | Forecast Service | Expected renewable generation or availability | kW / MW |
| Hydrogen demand forecast | Forecast Service / industrial planning | Expected hydrogen demand | kg/h |
| Industrial demand forecast | Forecast Service / EMS | Expected electricity demand | kW / MW |

---

## Service Outputs

| Output | Description |
|---|---|
| BESS charging schedule | Planned BESS charging power profile. |
| BESS discharging schedule | Planned BESS discharging power profile. |
| Electrolyser schedule | Planned electrolyser operation. |
| Hydrogen production profile | Expected hydrogen production over the planning horizon. |
| Hydrogen storage trajectory | Expected hydrogen storage level. |
| Market-related schedule | Planned consumption, generation or market position. |
| Expected energy cost | Estimated energy cost for the planning horizon. |
| Expected cost reduction | Difference between baseline and optimised operation. |
| Renewable utilisation indicator | Share of hydrogen production supplied by renewable electricity. |
| Schedule deviation | Difference between planned and realised operation. |
| HMI indicators | Operational dashboard indicators and historical trends. |

---

## Optimisation Logic

The optimisation problem can be represented as a cost-minimisation problem.

```text
Minimise:
    Total electricity procurement cost
    + operational penalties
    + schedule deviation penalties
    + hydrogen shortage penalties
```

Subject to:

```text
BESS constraints:
    SoC_min <= SoC_t <= SoC_max
    P_charge_t <= P_charge_max
    P_discharge_t <= P_discharge_max
    SoC_t = SoC_t-1 + charged_energy - discharged_energy

Electrolyser constraints:
    P_ely_min <= P_ely_t <= P_ely_max
    Hydrogen_production_t = f(P_ely_t)
    Ramp limits and availability constraints respected

Hydrogen storage constraints:
    H2_min <= H2_storage_t <= H2_max
    H2_storage_t = H2_storage_t-1 + production_t - demand_t

Market / scheduling constraints:
    Day-ahead price signals considered
    Market gate closure respected where applicable
    Final schedules compatible with site operation

FCR interaction:
    If FCR is active, sufficient BESS power and energy capacity must remain reserved.
```

---

## Interaction with FCR

Energy Arbitrage and FCR may rely on the same BESS asset.

Therefore, the Energy Arbitrage schedule must consider possible reservation of battery capacity for FCR provision.

This means that:

- the full BESS capacity may not always be available for arbitrage;
- minimum SoC margins may be required;
- power capacity must be reserved when FCR is scheduled;
- arbitrage optimisation should not compromise FCR readiness;
- priority rules are needed when both services are active.

The repository therefore treats Energy Arbitrage and FCR as separate service files but recognises that they may be operationally coupled through the BESS.

---

## Suggested JSON Structure

```json
{
  "service": "EnergyArbitrage",
  "demo": "Demo5",
  "timestamp": "YYYY-MM-DDTHH:mm:ssZ",
  "site": {
    "name": "GreenSteelProductionFacility",
    "country": "Sweden"
  },
  "market": {
    "type": "DayAheadMarket",
    "currency": "EUR",
    "priceForecast": []
  },
  "assets": {
    "bess": {
      "stateOfCharge": {
        "value": null,
        "unit": "%"
      },
      "ratedPower": {
        "value": null,
        "unit": "kW"
      },
      "energyCapacity": {
        "value": null,
        "unit": "kWh"
      },
      "availability": true,
      "reservedForFCR": {
        "value": null,
        "unit": "kW"
      }
    },
    "electrolyser": {
      "status": "available",
      "power": {
        "value": null,
        "unit": "kW"
      },
      "hydrogenProductionRate": {
        "value": null,
        "unit": "kg/h"
      }
    },
    "hydrogenStorage": {
      "storageLevel": {
        "value": null,
        "unit": "kg"
      },
      "storageCapacity": {
        "value": null,
        "unit": "kg"
      }
    }
  },
  "forecasts": {
    "electricityPriceForecast": [],
    "renewableAvailabilityForecast": [],
    "hydrogenDemandForecast": [],
    "industrialDemandForecast": []
  },
  "optimisationOutputs": {
    "bessChargingSchedule": [],
    "bessDischargingSchedule": [],
    "electrolyserSchedule": [],
    "hydrogenProductionProfile": [],
    "hydrogenStorageTrajectory": [],
    "expectedEnergyCost": {
      "value": null,
      "unit": "EUR"
    },
    "expectedCostReduction": {
      "value": null,
      "unit": "EUR"
    }
  },
  "kpis": {
    "realisedEnergyCost": null,
    "costReduction": null,
    "renewableElectricityUtilisation": null,
    "hydrogenProductionCost": null,
    "scheduleDeviation": null
  }
}
```

---

## HMI and Dashboard Requirements

The Demo 5 Energy Arbitrage dashboard should display:

- day-ahead electricity price forecast;
- BESS SoC;
- BESS charging and discharging schedule;
- electrolyser schedule;
- hydrogen storage level;
- hydrogen production forecast;
- industrial demand forecast;
- expected energy cost;
- realised energy cost;
- cost reduction indicators;
- renewable electricity utilisation;
- warnings related to BESS or electrolyser availability;
- interaction with FCR reservation, where relevant.

Historical views should include:

- planned versus executed BESS operation;
- planned versus executed electrolyser operation;
- hydrogen storage evolution;
- electricity price and consumption correlation;
- energy cost savings;
- deviations from planned schedules.

---

## Implementation Status

The Energy Arbitrage service in Demo 5 is represented in the final repository as a day-ahead and market-oriented optimisation service for an industrial hydrogen and BESS-based energy system.

The implementation is focused on modelling and validating the operation of:

- BESS scheduling;
- electrolyser scheduling;
- hydrogen production optimisation;
- hydrogen storage flexibility;
- electricity cost reduction;
- green hydrogen-oriented operation.

The final service status can be summarised as follows:

| Criterion | Status |
|---|---|
| Conceptual service definition | Completed |
| Service workflow | Completed |
| Sequence diagram | Completed |
| Data model orientation | Completed |
| API-oriented structure | Completed |
| Demonstrator alignment | Corrected to final Sweden green steel use case |
| Final active repository service | Yes |
| Coupling with FCR | Relevant through BESS capacity reservation |

---

## Lessons Learned

The Demo 5 Energy Arbitrage implementation provides several lessons for industrial multi-energy systems:

- energy arbitrage in industrial sites is not only a battery scheduling problem, but a multi-vector optimisation problem;
- electrolyser flexibility can significantly increase the value of energy scheduling;
- hydrogen storage provides an additional buffer that increases operational flexibility;
- green hydrogen production requires alignment between market optimisation and renewable electricity availability;
- BESS capacity may need to be shared between arbitrage and frequency response services;
---

