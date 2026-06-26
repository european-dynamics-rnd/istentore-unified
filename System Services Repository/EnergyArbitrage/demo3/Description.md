# Implementation Details of Energy Arbitrage in Demo 3

## Demo Context

Demo 3 corresponds to the Spanish demonstrator focused on a **Virtual Energy Storage System (VESS) for Renewable Energy Integration**.

The original VESS concept combines renewable generation and storage assets to provide market-oriented and grid-support flexibility services. The Energy Arbitrage service is designed to optimise the participation of the VESS in the day-ahead electricity market by coordinating renewable generation forecasts, storage availability and expected market prices.

---

## Service Objective

The objective of the Energy Arbitrage service in Demo 3 is to optimise the operation of the VESS for participation in the day-ahead market.

The service aims to:

- maximise the economic value of renewable generation;
- define market offers compatible with storage operation;
- schedule battery charging and discharging according to expected market profitability;
- use renewable generation forecasts and market price forecasts to support day-ahead decision-making;
- ensure that the resulting schedules respect the technical constraints of the storage system;
- provide a repeatable optimisation workflow that can be integrated into the i-STENTORE digital ecosystem.

---

## Service Classification

| Field | Description |
|---|---|
| Service name | Energy Arbitrage |
| Demonstrator | Demo 3 – Spain |
| Service family | Generation Support / Bulk Storage / Trading Service |
| Main objective | Day-ahead market optimisation |
| Main asset concept | Virtual Energy Storage System |
| Main market | Day-ahead electricity market |
| Main storage assets | Li-ion BESS and originally planned VRFB |
| Main renewable assets | PV, wind and run-of-river hydro generation |
| Main control layer | HOCS Platform |
| Implementation level | Modelled and simulated for day-ahead arbitrage logic |

---

## Demonstrator Assets

### Renewable Generation Assets

- PV Power Plant;
- Wind Power Plant;
- Run-of-river Hydroelectric Power Plant.

### Storage Assets

- Li-ion Battery Energy Storage System;
- Vanadium Redox Flow Battery, originally considered in the conceptual VESS configuration.

### Digital and Control Assets

- HOCS Platform;
- Forecast Service;
- Cloud-based Optimisation Service;
- HOCS Databases;
- Local Battery Controller;
- Market Representative interface;
- HMI / dashboard layer.

---

## Actors

| Actor | Role |
|---|---|
| PV Power Plant | Non-controllable renewable generation asset. |
| Wind Power Plant | Non-controllable renewable generation asset. |
| Run-of-river Hydroelectric Power Plant | Renewable generation asset whose production depends on river inflow. |
| Local Controller – Li-ion Battery | Executes schedules or control signals for the Li-ion battery. |
| Local Controller – VRFB | Originally planned controller for the VRFB battery. |
| HOCS Platform | Central supervisory platform managing databases, communications and control signals. |
| Forecast Service | Provides weather, renewable generation and market price forecasts. |
| Cloud-based Optimisation Service | Runs the optimisation for the Energy Arbitrage service. |
| Market Representative | Aggregates VESS availability and submits bids to the wholesale market. |
| Market Operator | Operates and clears the day-ahead market. |
| User Interface / HMI | Displays forecasts, schedules, market results and operational indicators. |
| i-STENTORE Middleware | Enables harmonised data exchange and possible integration with the wider digital ecosystem. |

---

## Operational Principle

The Energy Arbitrage service determines the optimal charging and discharging behaviour of the storage assets based on forecasted renewable generation and forecasted market prices.

The service follows the logic below:

1. Forecast renewable generation from PV, wind and hydro assets.
2. Forecast day-ahead electricity market prices.
3. Assess the available flexibility of the VESS.
4. Optimise storage schedules before market commitment.
5. Submit a market offer through the Market Representative.
6. Receive market results from the Market Operator.
7. Update the operating schedule.
8. Communicate setpoints to the local battery controller.
9. Store results and display them through the HMI.

---

## Timing Requirements

| Parameter | Value / Description |
|---|---|
| Main planning horizon | Day-ahead |
| Service reaction time | At least 12 hours before market commitment |
| Market gate closure | 13:00 |
| Frequency of use | Daily |
| Main time resolution | Market-dependent; typically hourly or sub-hourly depending on implementation |
| Output horizon | Day-ahead battery and market schedule |

---

## Implementation Workflow

```text
1. Renewable and storage data are collected by the HOCS Platform.
   |
2. Forecast requests are sent to the Forecast Service.
   |
3. The Forecast Service returns:
   |-- PV generation forecast
   |-- Wind generation forecast
   |-- Hydro generation forecast
   |-- Day-ahead market price forecast
   |
4. Forecasts and BESS status are sent to the Cloud-based Optimisation Service.
   |
5. The optimisation service computes:
   |-- VESS market offer
   |-- battery charge/discharge schedule
   |-- expected SoC trajectory
   |-- expected economic value
   |
6. Optimised schedules are returned to the HOCS Platform.
   |
7. The HOCS Platform updates the Future Database.
   |
8. The Market Representative submits bids to the Market Operator.
   |
9. Market results are received and sent back to the HOCS Platform.
   |
10. The final schedule is transferred to the Real-Time Database.
   |
11. The central controller sends setpoints to the local battery controller.
   |
12. Results are stored in the Past Database and displayed on the HMI.
```

---

## Sequence Diagram

```mermaid
sequenceDiagram
    participant PV as PV Power Plant
    participant W as Wind Power Plant
    participant H as Hydro Power Plant
    participant BESS as Local Battery Controller
    participant HOCS as HOCS Platform
    participant Forecast as Forecast Service
    participant Opt as Cloud-based Optimisation
    participant MR as Market Representative
    participant MO as Market Operator
    participant UI as HMI / Dashboard

    PV->>HOCS: Send generation data
    W->>HOCS: Send generation data
    H->>HOCS: Send generation data
    BESS->>HOCS: Send SoC and availability

    HOCS->>Forecast: Request generation and price forecasts
    Forecast-->>HOCS: Return PV, wind, hydro and price forecasts
    HOCS->>UI: Display forecasts

    HOCS->>Opt: Send forecasts and BESS status
    Opt-->>HOCS: Return optimised schedule and market offer

    HOCS->>MR: Communicate offer / availability
    MR->>MO: Submit day-ahead market bid
    MO-->>MR: Return market results
    MR-->>HOCS: Communicate accepted market results

    HOCS->>BESS: Send final operating schedule
    HOCS->>UI: Display schedules, market results and KPIs
```

---

## Data Inputs

### Static Parameters

| Parameter | Asset | Description | Typical Unit |
|---|---|---|---|
| Rated power | BESS | Maximum charging/discharging power | kW / MW |
| Rated energy capacity | BESS | Available energy storage capacity | kWh / MWh |
| Minimum SoC | BESS | Lower operational state-of-charge limit | % |
| Maximum SoC | BESS | Upper operational state-of-charge limit | % |
| Charging efficiency | BESS | Efficiency during charging | % |
| Discharging efficiency | BESS | Efficiency during discharging | % |
| Market gate closure | Market interface | Deadline for day-ahead market submission | hh:mm |
| Optimisation horizon | Optimisation service | Time horizon used for schedule optimisation | h |

### Runtime Variables

| Variable | Source | Description | Typical Unit |
|---|---|---|---|
| Current SoC | BESS controller | Current state of charge of the battery | % |
| Battery availability | BESS controller | Operational availability of the storage system | Boolean / status |
| PV generation | PV plant | Current PV active power | kW / MW |
| Wind generation | Wind plant | Current wind active power | kW / MW |
| Hydro generation | Hydro plant | Current run-of-river hydro production | kW / MW |
| Current market data | Market data source | Latest available price or market information | EUR/MWh |

### Forecast Variables

| Forecast | Source | Description | Typical Unit |
|---|---|---|---|
| PV generation forecast | Forecast Service | Expected PV generation | kW / MW |
| Wind generation forecast | Forecast Service | Expected wind generation | kW / MW |
| Hydro generation forecast | Forecast Service | Expected run-of-river hydro generation | kW / MW |
| Day-ahead market price forecast | Forecast Service | Expected day-ahead price | EUR/MWh |

---

## Service Outputs

| Output | Description |
|---|---|
| Day-ahead market offer | Proposed offer for market participation. |
| Charging schedule | Planned battery charging power over the optimisation horizon. |
| Discharging schedule | Planned battery discharging power over the optimisation horizon. |
| State-of-charge trajectory | Expected SoC evolution during the planning horizon. |
| Accepted market schedule | Final schedule after market clearing. |
| Expected revenue | Estimated economic outcome from market participation. |
| Schedule deviation | Difference between planned and realised operation. |
| Market results | Accepted quantities, prices and settlement-related indicators. |
| HMI indicators | Visual representation of schedule, SoC, market results and performance. |

---

## Suggested JSON Structure

```json
{
  "service": "EnergyArbitrage",
  "demo": "Demo3",
  "timestamp": "YYYY-MM-DDTHH:mm:ssZ",
  "market": {
    "name": "DayAheadMarket",
    "operator": "OMIE",
    "gateClosureTime": "13:00",
    "currency": "EUR"
  },
  "assets": {
    "renewableGeneration": {
      "pvPowerPlant": { "available": true },
      "windPowerPlant": { "available": true },
      "hydroPowerPlant": { "available": true }
    },
    "storage": {
      "liIonBattery": {
        "stateOfCharge": { "value": null, "unit": "%" },
        "availability": true
      },
      "vrfbBattery": {
        "stateOfCharge": { "value": null, "unit": "%" },
        "availability": false
      }
    }
  },
  "forecasts": {
    "pvGenerationForecast": [],
    "windGenerationForecast": [],
    "hydroGenerationForecast": [],
    "dayAheadMarketPriceForecast": []
  },
  "optimisationOutputs": {
    "marketBid": [],
    "chargingSchedule": [],
    "dischargingSchedule": [],
    "stateOfChargeTrajectory": [],
    "expectedRevenue": { "value": null, "unit": "EUR" }
  },
  "marketResults": {
    "acceptedSchedule": [],
    "acceptedPrice": [],
    "acceptedQuantity": []
  },
  "kpis": {
    "realisedRevenue": null,
    "scheduleDeviation": null,
    "renewableEnergyUtilisation": null,
    "storageUtilisationFactor": null
  }
}
```

---

## Implementation Status

| Criterion | Status |
|---|---|
| Conceptual service definition | Completed |
| Service workflow | Completed |
| Sequence diagram | Completed |
| Data model | Completed |
| API-oriented structure | Completed |
| Full hybrid asset deployment | completed |
| Day-ahead arbitrage execution | Modelled and simulated |


---

## Lessons Learned

- the VESS concept is suitable for integrating renewable generation and storage into a market-oriented flexibility service;
- day-ahead arbitrage requires accurate renewable generation and price forecasts;
- storage availability strongly affects the feasible market offer;
- hardware changes can reduce the flexibility envelope without invalidating the service model;
- a technology-neutral service definition is robust against changes in the final asset configuration;
- the harmonised data model remains applicable even when the full originally planned hybrid configuration is not deployed.

---

