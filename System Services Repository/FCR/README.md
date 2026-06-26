# Frequency Containment Reserve (FCR)

Frequency Containment Reserve (FCR), also known as primary control reserve, is the first automatic response to a frequency disturbance in the power system.

FCR is activated when the grid frequency deviates from its nominal value, typically 50 Hz in the European synchronous area. The service provides an automatic and proportional active-power response to contain the frequency deviation and stabilise the system before subsequent frequency restoration services, such as aFRR and mFRR, take over.

In the i-STENTORE Service Repository, FCR is represented through:

- Demo 2 – Pumped Hydro Storage combined with BESS, Portugal;
- Demo 5 – Green Steel Production Facility with energy storage capabilities based on Hydrogen and BESS Technologies, Sweden.


---

## Service Category

FCR belongs to the following service families:

- Flexibility Services;
- Balancing Services;
- Ancillary Services;
- Frequency Regulation Services;
- Fast Active-Power Response Services;
- Storage-based Grid Stability Services.

---

## Purpose of the Service

The purpose of FCR is to contain frequency deviations immediately after a disturbance.

The service supports:

- stabilisation of grid frequency;
- automatic response to system disturbances;
- containment of frequency deviations before restoration reserves are activated;
- improvement of system reliability and operational security;
- integration of inverter-based storage into frequency-control services;
- participation of storage assets in balancing or reserve markets, where regulatory conditions allow it.

---

## General Description

When a sudden imbalance occurs between generation and demand, the system frequency changes. If generation is lower than demand, frequency drops. If generation is higher than demand, frequency rises.

FCR resources respond automatically to these deviations:

- if frequency decreases, the resource injects active power or reduces consumption;
- if frequency increases, the resource absorbs active power or increases consumption;
- the response is proportional to the measured frequency deviation, typically following a droop curve.

Battery energy storage systems are well suited to FCR because they can change active power rapidly and accurately. Hybrid energy storage systems can also support FCR by combining fast converter-based response with longer-duration energy capacity.

---

## Value Proposition

| Value Stream | Description |
|---|---|
| Frequency Stability | Contains frequency deviations immediately after disturbances. |
| Fast Active-Power Response | Uses storage power electronics to react rapidly to frequency deviations. |
| Grid Reliability | Improves operational security during imbalances. |
| Reserve Provision | Enables storage assets to provide reserve capacity where market access exists. |
| Renewable Integration | Supports grids with increasing shares of inverter-based renewable generation. |
| Service Stacking | Can be coordinated with Energy Arbitrage, FFR, aFRR or other services if reserve margins are preserved. |

---

## Technical Principle

FCR is based on real-time frequency measurement and proportional active-power response.

The generic process is:

1. Measure grid frequency continuously.
2. Calculate the frequency deviation from the nominal value.
3. Apply a droop-control or equivalent response function.
4. Determine the required active-power response.
5. Check BESS / HESS availability and state-of-charge limits.
6. Dispatch charge or discharge power.
7. Monitor delivered power and frequency response.
8. Store activation data and calculate performance indicators.

---

## Typical Time Horizon

| Parameter | Typical Value / Description |
|---|---|
| Activation trigger | Frequency deviation from nominal frequency |
| Response type | Automatic and proportional |
| Main response timeframe | Seconds; full activation typically required within tens of seconds, depending on market rules |
| Service duration | Short to medium duration, depending on the disturbance and market requirements |
| Main service quantity | Reserve capacity and activated energy |
| Main unit of measurement | MW and MWh |
| Main operational constraint | State of charge and reserved power capacity |

---

## Typical Actors

| Actor | Role |
|---|---|
| TSO / System Operator | Defines reserve requirements and receives the service where market or operational rules apply. |
| DSO / Island Grid Operator | May be the operational beneficiary in isolated or weak-grid contexts. |
| Balance Responsible Party / Market Representative | Supports market participation where relevant. |
| Energy Management System | Coordinates service logic, availability and dispatch. |
| BESS Controller | Executes fast active-power response. |
| HESS Controller | Coordinates multiple storage components where applicable. |
| Frequency Measurement Device | Provides real-time frequency input. |
| SCADA / Monitoring System | Records operational status, setpoints and response. |
| HMI / Dashboard | Displays service status, activation and KPIs. |
| i-STENTORE Middleware | Supports harmonised information exchange and semantic representation. |

---

## Demonstrator Implementations

| Demonstrator | Implementation Focus | Repository File |
|---|---|---|
| Demo 2 | Simulated island-grid FCR through the common ancillary-service optimisation and dynamic assessment workflow. | `FCR/demo2/Description.md` |
| Demo 5 | FCR / FCR-D readiness using a Li-ion BESS in an industrial green-steel demonstrator. | `FCR/demo5/Description.md` |

---

## Demo 2 Implementation Summary

In Demo 2, FCR is part of a shared ancillary-service workflow together with Synchronous Inertia, FFR and aFRR.

The process is initiated by EEM through an API request. The INESC TEC virtual machine fetches updated system data from the EEM server and runs the optimisation algorithm. The resulting dispatch is then assessed using a dynamic procedure with the N-1 criterion. If the point of operation is stable, the results are stored and EEM applies the operating point. If the system is not stable, synchronous condensers are dispatched and the process is repeated.

The final status of Demo 2 FCR is simulation / HIL validation, reflecting the island-grid constraints and the absence of a market-based FCR deployment framework.

---

## Demo 5 Implementation Summary

In Demo 5, FCR is associated with the industrial BESS installed in the green-steel production context.

The Li-ion BESS provides the fast active-power response required for FCR / FCR-D technical readiness. The service relies on real-time frequency measurement, BESS availability, state-of-charge management and droop-based response logic. The BESS is coordinated with the HOCS / local control environment and monitored through the site data-acquisition infrastructure.

In the final Demo 5 service scope, FCR is retained as the only balancing / frequency-service repository entry. aFRR and mFRR are not included as active final Demo 5 repository files.

---

## Typical Inputs

FCR requires frequency measurements, storage availability and reserve-capacity information.

### Frequency and Activation Inputs

| Input | Description | Unit |
|---|---|---|
| nominal_frequency | Nominal grid frequency. | Hz |
| measured_frequency | Real-time measured system frequency. | Hz |
| frequency_deviation | Difference between measured and nominal frequency. | Hz or mHz |
| droop_setting | Parameter defining proportional response. | % or MW/Hz |
| activation_deadband | Frequency band with no activation, where applicable. | Hz or mHz |
| activation_direction | Upward or downward response. | n/a |

### Storage Inputs

| Input | Description | Unit |
|---|---|---|
| bess_state_of_charge | Current BESS state of charge. | % |
| bess_available_power | Available charge/discharge power. | kW / MW |
| bess_energy_capacity | Available energy capacity. | kWh / MWh |
| reserved_fcr_capacity | Capacity reserved for FCR provision. | kW / MW |
| bess_operational_status | Availability and operational status of the BESS. | status |
| minimum_soc | Minimum admissible state of charge. | % |
| maximum_soc | Maximum admissible state of charge. | % |

### Market and Service Inputs

| Input | Description | Unit |
|---|---|---|
| reserve_bid | Reserve capacity bid, where market participation is relevant. | MW |
| reserve_block | Product or activation time block. | h |
| service_availability | Status indicating whether the service is available. | Boolean / status |
| market_result | Accepted or rejected reserve-market result, where applicable. | status |
| performance_requirement | Response-time or activation requirement. | s |

---

## Typical Outputs

| Output | Description | Unit |
|---|---|
| fcr_power_setpoint | Active-power setpoint calculated for FCR response. | kW / MW |
| delivered_fcr_power | Measured active power delivered during activation. | kW / MW |
| fcr_energy_activated | Energy delivered or absorbed during activation. | kWh / MWh |
| soc_trajectory | State-of-charge evolution during service provision. | % |
| response_time | Time required to deliver the response. | s |
| activation_status | Status of the FCR activation. | status |
| service_availability_status | Whether the service remains available. | Boolean / status |
| performance_kpis | FCR performance indicators. | n/a |

---

## Generic Implementation Workflow

```text
1. Measure frequency continuously.
   |
   |-- Nominal frequency
   |-- Measured frequency
   |-- Frequency deviation
   |
2. Determine FCR activation.
   |
   |-- Apply deadband, if relevant
   |-- Determine upward or downward activation
   |-- Calculate proportional response using droop settings
   |
3. Check storage availability.
   |
   |-- BESS SoC
   |-- Reserved capacity
   |-- Power limits
   |-- Operational status
   |
4. Dispatch active-power response.
   |
   |-- Charge if frequency is above nominal
   |-- Discharge if frequency is below nominal
   |-- Respect SoC and power limits
   |
5. Monitor response.
   |
   |-- Delivered active power
   |-- Response time
   |-- SoC trajectory
   |-- Frequency evolution
   |
6. Store and report results.
   |
   |-- Activation history
   |-- Delivered reserve
   |-- Performance KPIs
   |-- Service availability
```

---

## Service Interaction

FCR may interact with other services when the same storage assets are used for multiple purposes.

| Related Service | Interaction |
|---|---|
| Energy Arbitrage | Energy Arbitrage schedules must reserve SoC and power margins for FCR. |
| FFR | FFR may respond faster or earlier; coordination avoids double-counting capacity. |
| Synchronous Inertia | Inertia or synthetic inertia supports the initial frequency behaviour before or alongside FCR. |
| aFRR | aFRR restores system balance after FCR contains the frequency deviation. |
| mFRR | mFRR supports longer-duration restoration after FCR and aFRR. |
| Power Smoothing | Smoothing may use the same high-power storage capacity and must respect FCR reserve margins. |

Service stacking requires explicit prioritisation because FCR availability must be guaranteed during the contracted or committed reserve period.

---

## Data Model Orientation

The FCR data model should distinguish between:

- metadata;
- frequency measurements;
- activation parameters;
- reserve commitment;
- storage state and limits;
- control setpoints;
- measured response;
- service availability;
- performance indicators.

The service should be compatible with the i-STENTORE semantic and API framework.

The data model should therefore support:

- explicit frequency, power and energy units;
- high-frequency or real-time measurement time series;
- BESS / HESS state-of-charge representation;
- reserve-capacity representation;
- JSON-based information exchange;
- OpenAPI-compatible request and response structures;
- alignment with the i-STENTORE ontology;
- future integration with NGSI-LD structures.

---

## Suggested Data Model Structure

```text
FCRService

├── Metadata
│   ├── service_id
│   ├── requester_id
│   └── timestamp
│
├── FrequencyMeasurement
│   ├── nominal_frequency
│   ├── measured_frequency
│   ├── frequency_deviation
│   └── measurement_timestamp
│
├── ActivationParameters
│   ├── droop_setting
│   ├── activation_deadband
│   ├── activation_direction
│   ├── performance_requirement
│   └── response_time_limit
│
├── ReserveCommitment
│   ├── reserved_fcr_capacity
│   ├── reserve_bid
│   ├── reserve_block
│   └── market_result
│
├── StorageState
│   ├── bess_state_of_charge
│   ├── bess_available_power
│   ├── bess_energy_capacity
│   ├── minimum_soc
│   ├── maximum_soc
│   └── bess_operational_status
│
├── Outputs
│   ├── fcr_power_setpoint
│   ├── delivered_fcr_power
│   ├── fcr_energy_activated
│   ├── soc_trajectory
│   └── activation_status
│
└── KPIs
    ├── response_time
    ├── activation_accuracy
    ├── availability_ratio
    ├── delivered_energy
    └── service_compliance
```

---

## Suggested JSON Skeleton

```json
{
  "service": "FCR",
  "demo": "DemoX",
  "metadata": {
    "service_id": "example_fcr_service",
    "requester_id": "tso_or_ems",
    "timestamp": "YYYY-MM-DDTHH:mm:ssZ"
  },
  "frequencyMeasurement": {
    "nominal_frequency": {
      "value": 50.0,
      "unit": "Hz"
    },
    "measured_frequency": {
      "value": null,
      "unit": "Hz"
    },
    "frequency_deviation": {
      "value": null,
      "unit": "Hz"
    }
  },
  "activationParameters": {
    "droop_setting": {
      "value": null,
      "unit": "%"
    },
    "activation_deadband": {
      "value": null,
      "unit": "Hz"
    },
    "activation_direction": "upward_or_downward",
    "response_time_limit": {
      "value": null,
      "unit": "s"
    }
  },
  "reserveCommitment": {
    "reserved_fcr_capacity": {
      "value": null,
      "unit": "kW"
    },
    "reserve_bid": {
      "value": null,
      "unit": "kW"
    },
    "reserve_block": {
      "start": "YYYY-MM-DDTHH:mm:ssZ",
      "end": "YYYY-MM-DDTHH:mm:ssZ"
    },
    "market_result": "accepted_or_not_applicable"
  },
  "storageState": {
    "bess_state_of_charge": {
      "value": null,
      "unit": "%"
    },
    "bess_available_power": {
      "value": null,
      "unit": "kW"
    },
    "bess_energy_capacity": {
      "value": null,
      "unit": "kWh"
    },
    "minimum_soc": {
      "value": null,
      "unit": "%"
    },
    "maximum_soc": {
      "value": null,
      "unit": "%"
    },
    "bess_operational_status": "available"
  },
  "outputs": {
    "fcr_power_setpoint": [],
    "delivered_fcr_power": [],
    "fcr_energy_activated": {
      "value": null,
      "unit": "kWh"
    },
    "soc_trajectory": [],
    "activation_status": "idle_or_active"
  },
  "kpis": {
    "response_time": null,
    "activation_accuracy": null,
    "availability_ratio": null,
    "delivered_energy": null,
    "service_compliance": null
  }
}
```

---

## Implementation Requirements

An FCR implementation should include:

- reliable real-time frequency measurement;
- validated BESS / HESS active-power control;
- droop or proportional response logic;
- reserved power and energy capacity;
- state-of-charge management strategy;
- availability monitoring;
- performance validation against response-time requirements;
- logging of activations and delivered response;
- HMI representation of frequency deviation, setpoints and KPIs;
- prioritisation rules for service stacking.

---

## HMI and Dashboard Requirements

The FCR dashboard should display:

- measured grid frequency;
- frequency deviation;
- activation direction;
- BESS state of charge;
- reserved FCR capacity;
- available charge/discharge power;
- calculated FCR setpoint;
- delivered active-power response;
- response time;
- activation accuracy;
- SoC trajectory;
- service availability status;
- activation history.

Historical views should include:

- frequency events;
- delivered FCR response;
- response-time statistics;
- reserve availability;
- SoC evolution;
- service-compliance indicators.

---

## Implementation Status

FCR is represented as a project-wide frequency-containment service with implementations in Demo 2 and Demo 5.

| Demonstrator | Final Repository Status |
|---|---|
| Demo 2 | Simulated island-grid FCR using the common ancillary-service optimisation and dynamic assessment workflow. |
| Demo 5 | FCR / FCR-D technical readiness using the industrial Li-ion BESS in the OVAKO green-steel context. |

---

## Lessons Learned

The FCR service shows that storage systems are technically well suited for fast automatic frequency response, but full field deployment depends on regulatory, market and certification conditions.

Key lessons include:

- BESS assets can provide rapid and accurate active-power response;
- FCR requires guaranteed availability during the reserve period;
- energy-management and market schedules must preserve SoC and power margins for FCR;
- island-grid contexts are technically relevant but may lack formal FCR markets;
- industrial BESS assets can demonstrate FCR-D readiness even when full market participation is not achieved;
- response validation should include reaction time, delivered power and sustained activation capability;



---

## Files in this Directory

```text
FCR/

├── README.md
├── demo2/
│   └── Description.md
└── demo5/
    └── Description.md
```

---
