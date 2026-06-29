# Implementation Details of mFRR in Demo 3

## Demo Context

Demo 3 corresponds to the i-STENTORE demonstrator **Virtual Energy Storage System for Renewable Energy Integration – Spain**.

The demonstrator was designed around the aggregation of renewable generation assets and storage resources into a Virtual Energy Storage System. Within this VESS concept, Manual Frequency Restoration Reserve (mFRR) is represented as a balancing service in which the aggregated resource portfolio provides upward or downward reserve capacity.

---

## Service Objective

The objective of the mFRR service in Demo 3 is to define how the VESS can provide manually activated balancing reserve by coordinating BESS flexibility and renewable generation forecasts.

The service aims to:

- estimate available upward and downward balancing capacity;
- prepare mFRR bids or internal activation scenarios;
- use BESS flexibility to deliver balancing energy;
- coordinate reserve provision with Energy Arbitrage and Congestion Management;
- monitor delivered response and activation accuracy;
- provide a reusable VESS-based mFRR model.

---

## Service Classification

| Field | Description |
|---|---|
| Service name | Manual Frequency Restoration Reserve |
| Acronym | mFRR |
| Demonstrator | Demo 3 – Spain |
| Service family | Balancing service / manual frequency restoration |
| Main objective | Provision of upward or downward reserve capacity |
| Main asset concept | Virtual Energy Storage System |
| Main storage asset | Li-ion BESS |
| Originally planned additional storage | VRFB |
| Main renewable assets | PV, wind and hydro generation |
| Final implementation level | Planned / modelled |
| Main limitation | No full external balancing-market activation and incomplete VESS asset configuration |

---

## Assets and Actors

| Asset / Actor | Role |
|---|---|
| VESS | Aggregated flexibility provider. |
| Li-ion BESS | Main final controllable storage asset. |
| VRFB | Planned storage component, not installed in the final configuration. |
| PV / Wind / Hydro Assets | Renewable generation resources contributing to the portfolio. |
| HOCS / EMS | Coordinates forecasts, optimisation and schedules. |
| Market Representative / System Operator | Represents the external mFRR activation or bid context. |
| SCADA | Provides generation and storage measurements. |
| BESS Controller | Executes charge/discharge response. |
| HMI / Dashboard | Displays bids, schedules, activations and KPIs. |
| i-STENTORE Middleware | Supports harmonised data exchange. |

---

## Operational Principle

The mFRR service determines whether the VESS can provide upward or downward reserve during a requested activation window.

For upward reserve, the VESS must be able to increase net injection or reduce net consumption. This can be achieved by discharging the BESS or modifying other controllable resources.

For downward reserve, the VESS must be able to reduce net injection or increase net consumption. This can be achieved by charging the BESS or absorbing surplus renewable generation.

The service must account for BESS state of charge, power limits, renewable generation forecasts and existing commitments from other services.

---

## Implementation Workflow

```text
1. Receive mFRR request, bid opportunity or internal scenario.
2. Retrieve VESS asset data.
   |-- BESS SoC
   |-- available charge/discharge power
   |-- renewable generation forecast
   |-- existing schedules
3. Determine reserve direction.
   |-- upward reserve
   |-- downward reserve
4. Estimate available reserve capacity.
5. Prepare bid, schedule or activation plan.
6. If activated, dispatch BESS response.
7. Monitor delivered balancing power.
8. Store activation result and calculate KPIs.
```

---

## Sequence Diagram

```mermaid
sequenceDiagram
    participant SO as System Operator / Market
    participant HOCS as HOCS / EMS
    participant SCADA as SCADA
    participant OPT as Optimisation Service
    participant BESS as Li-ion BESS Controller
    participant DB as Database
    participant UI as HMI / Dashboard
    participant MW as i-STENTORE Middleware

    SO->>HOCS: Send mFRR request or bid opportunity
    SCADA->>HOCS: Send BESS and renewable data
    HOCS->>OPT: Send request, forecasts and existing commitments
    OPT->>OPT: Calculate upward/downward reserve capacity
    OPT-->>HOCS: Return mFRR schedule or bid

    alt Activation received
        HOCS->>BESS: Dispatch charge/discharge command
        BESS-->>HOCS: Return delivered power and status
    else No activation
        HOCS->>DB: Store available reserve and bid status
    end

    HOCS->>DB: Store service results
    DB->>UI: Display mFRR status and KPIs
    DB->>MW: Share harmonised service data
```

---

## Data Inputs

| Input | Description | Unit |
|---|---|---|
| mfrR_request | Manual reserve request or bid opportunity. | n/a |
| activation_direction | Upward or downward reserve. | n/a |
| activation_period | Reserve activation window. | datetime |
| requested_power | Requested balancing power. | MW |
| bess_state_of_charge | Current BESS SoC. | % |
| bess_power_limit | Maximum BESS charge/discharge power. | MW |
| bess_energy_capacity | BESS energy capacity. | MWh |
| renewable_generation_forecast | Forecasted PV, wind and hydro output. | MW |
| existing_commitments | Energy Arbitrage, Congestion Management or other schedules. | n/a |
| market_price | Balancing-market price or scenario value. | €/MWh |
| availability_status | VESS availability status. | status |

---

## Service Outputs

| Output | Description | Unit |
|---|---|---|
| available_upward_reserve | Available upward mFRR capacity. | MW |
| available_downward_reserve | Available downward mFRR capacity. | MW |
| mfrR_bid | Bid or reserve offer. | MW |
| bess_dispatch_schedule | BESS charge/discharge plan. | time series |
| delivered_balancing_power | Delivered power during activation. | MW |
| activated_energy | Energy delivered or absorbed. | MWh |
| service_status | Planned, available, activated, completed or unavailable. | status |
| schedule_deviation | Difference between requested and delivered response. | MW or % |

---

## Optimisation Logic

```text
Maximise:
    reserve availability and market value

Minimise:
    deviation from requested mFRR activation
    + conflicts with existing commitments
    + BESS cycling and operational penalties
```

Subject to:

```text
BESS constraints:
    SoC_min <= SoC_t <= SoC_max
    charge/discharge power limits are respected
    sufficient energy is available for the activation window

VESS constraints:
    renewable generation forecasts are considered
    existing service commitments are respected
    upward/downward reserve direction is feasible

Market/service constraints:
    requested activation period is respected
    reserve bid or activation product rules are followed
```

---

## Suggested JSON Structure

```json
{
  "service": "mFRR",
  "demo": "Demo3",
  "metadata": {
    "service_id": "demo3_mfrr",
    "requester_id": "market_or_internal_scenario",
    "timestamp": "YYYY-MM-DDTHH:mm:ssZ"
  },
  "activation": {
    "activation_direction": "upward_or_downward",
    "activation_period": {
      "start": "YYYY-MM-DDTHH:mm:ssZ",
      "end": "YYYY-MM-DDTHH:mm:ssZ"
    },
    "requested_power": {
      "value": null,
      "unit": "MW"
    }
  },
  "vess": {
    "bess_state_of_charge": {
      "value": null,
      "unit": "%"
    },
    "bess_power_limit": {
      "value": null,
      "unit": "MW"
    },
    "renewable_generation_forecast": [],
    "existing_commitments": [],
    "availability_status": "available"
  },
  "outputs": {
    "available_upward_reserve": {
      "value": null,
      "unit": "MW"
    },
    "available_downward_reserve": {
      "value": null,
      "unit": "MW"
    },
    "mfrr_bid": {
      "value": null,
      "unit": "MW"
    },
    "bess_dispatch_schedule": [],
    "delivered_balancing_power": [],
    "activated_energy": {
      "value": null,
      "unit": "MWh"
    },
    "service_status": "planned"
  },
  "kpis": {
    "reserve_availability": null,
    "activation_accuracy": null,
    "schedule_deviation": null,
    "delivered_energy": null
  }
}
```

---

## HMI and Dashboard Requirements

The Demo 3 mFRR dashboard should display:

- mFRR request or bid opportunity;
- upward and downward reserve availability;
- BESS SoC and power limits;
- renewable generation forecast;
- existing service commitments;
- bid or schedule status;
- activated response, where applicable;
- delivered balancing power;
- activated energy;
- deviation between requested and delivered response.

---

## Implementation Status

| Criterion | Status |
|---|---|
| Conceptual service definition | Completed |
| Demo-specific workflow | Completed |
| Sequence diagram | Completed |
| Data model orientation | Completed |
| VESS service model | Completed |
| External market activation | Not field validated |
| VRFB integration | Not completed |
| Final readiness level | Planned / modelled |

---

## Lessons Learned

Demo 3 shows that VESS-based mFRR requires coordinated treatment of market activation, storage availability and renewable generation uncertainty. The service remains technically relevant, but full validation depends on external market participation, asset availability and operational activation by a system operator or market framework.
