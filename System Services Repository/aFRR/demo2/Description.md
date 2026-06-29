# Implementation Details of aFRR in Demo 2

## Demo Context

Demo 2 corresponds to the i-STENTORE demonstrator **Pump Hydro Storage system combined with BESS – Portugal**.

The demonstrator is an islanded grid environment combining hydro resources, Li-ion battery storage, renewable generation and dynamic-stability assessment. In this context, Automatic Frequency Restoration Reserve (aFRR) is represented as an ancillary service that restores system balance after the initial containment of a frequency deviation.

aFRR is part of the Demo 2 common ancillary-service workflow, together with Synchronous Inertia, FFR and FCR. The service is implemented as an API-triggered optimisation and dynamic assessment process between EEM and INESC TEC.

---

## Service Objective

The objective of aFRR in Demo 2 is to automatically restore the system operating balance after a frequency disturbance while ensuring that the resulting dispatch is dynamically secure.

The service aims to:

- provide automatic frequency-restoration capability;
- coordinate hydro, storage and renewable resources;
- define an economically feasible operating point;
- validate the operating point using the N-1 criterion;
- preserve dynamic stability in an islanded system;
- dispatch synchronous condensers if required;
- provide a reusable aFRR data model for future service implementation.

---

## Service Classification

| Field | Description |
|---|---|
| Service name | Automatic Frequency Restoration Reserve |
| Acronym | aFRR |
| Demonstrator | Demo 2 – Portugal |
| Service family | Ancillary service / frequency restoration service |
| Main objective | Automatic restoration of system balance |
| Main system context | Islanded grid |
| Main resources | Hydro, Li-ion BESS, VRFB representation and synchronous-machine functionality |
| Requester / operator | EEM |
| Optimisation provider | INESC TEC |
| Validation procedure | Dynamic assessment with N-1 criterion |
| Final readiness level | Simulated / HIL Validated |
| Final repository service | Yes |

---

## Demonstrator Assets

- pumped hydro storage system;
- hydro power plant;
- Li-ion BESS;
- VRFB-related representation in the shared Demo 2 data model;
- synchronous condenser / synchronous-machine functionality;
- renewable generation resources;
- EEM server and API interface;
- INESC TEC virtual machine;
- optimisation algorithm;
- dynamic assessment procedure;
- HMI / dashboard;
- i-STENTORE semantic and service data model layer.

---

## Actors

| Actor | Role |
|---|---|
| EEM | Initiates the service and applies the accepted operating point. |
| INESC TEC | Hosts the optimisation and dynamic assessment process. |
| EEM Server | Provides input data and stores results. |
| INESC TEC Virtual Machine | Fetches data, runs optimisation and stores results. |
| Optimisation Algorithm | Calculates a candidate restoration dispatch. |
| Dynamic Assessment Procedure | Checks whether the candidate dispatch is stable under N-1. |
| Synchronous Condenser / Machine | Provides corrective dynamic support if the candidate dispatch is not stable. |
| Hydro Power Plant | Provides dispatchable generation and balancing capability. |
| Li-ion BESS | Provides fast active-power flexibility. |
| HMI / Dashboard | Displays service request, dispatch, stability results and KPIs. |

---

## Operational Principle

The aFRR workflow starts when EEM sends an API request. The INESC TEC virtual machine fetches updated EEM data and runs an optimisation algorithm to compute a candidate dispatch point.

The candidate point is then assessed dynamically. If it satisfies the N-1 criterion and is dynamically stable, it is stored and EEM applies it. If not, synchronous condensers are dispatched and the optimisation is repeated.

This process ensures that automatic restoration is not only economically feasible but also compatible with island-grid stability requirements.

---

## Implementation Workflow

```text
1. EEM initiates the aFRR workflow through the API.
2. INESC TEC retrieves updated EEM server data.
3. The optimisation algorithm computes a candidate restoration dispatch.
4. Dynamic assessment evaluates N-1 stability.
5. Stable candidate:
   |-- store results on INESC TEC VM;
   |-- store results on EEM server;
   |-- EEM applies the accepted operating point.
6. Unstable candidate:
   |-- dispatch synchronous condensers;
   |-- repeat optimisation;
   |-- repeat dynamic assessment.
7. Store final results and display KPIs.
```

---

## Sequence Diagram

```mermaid
sequenceDiagram
    participant EEM as EEM
    participant API as API
    participant Server as EEM Server
    participant VM as INESC TEC VM
    participant OPT as Optimisation Algorithm
    participant DYN as Dynamic Assessment
    participant SYN as Synchronous Condenser
    participant GEN as Generation and Storage
    participant UI as HMI / Dashboard
    participant MW as i-STENTORE Middleware

    EEM->>API: Request aFRR workflow
    API->>VM: Trigger optimisation
    VM->>Server: Fetch updated system data
    Server-->>VM: Return generation, storage, hydro and cost data
    VM->>OPT: Run aFRR optimisation
    OPT-->>VM: Candidate restoration dispatch
    VM->>DYN: Run N-1 dynamic assessment

    alt Candidate is stable
        DYN-->>VM: Confirm stable operating point
        VM->>Server: Store accepted result
        EEM->>GEN: Apply accepted operating point
    else Candidate is unstable
        DYN-->>VM: Reject candidate
        VM->>SYN: Dispatch synchronous condenser
        VM->>OPT: Repeat optimisation
        VM->>DYN: Repeat assessment
    end

    VM->>UI: Display restoration status and KPIs
    VM->>MW: Share harmonised service data
```

---

## Data Inputs

| Input | Description | Unit |
|---|---|---|
| service_id | Service instance identifier. | n/a |
| requester_id | EEM or requester identifier. | n/a |
| timestamp | Request timestamp. | datetime |
| timestep | Optimisation timestep. | min |
| horizon | Optimisation horizon. | h |
| afrr_activation_signal | Automatic restoration power requirement. | MW |
| restoration_direction | Upward or downward restoration. | n/a |
| frequency_deviation | Frequency deviation to be restored. | Hz |
| SoC_LIB | Li-ion battery state of charge. | kWh |
| SoC_VRFB | VRFB state of charge, where applicable. | kWh |
| Reservoir_HPP | Hydro reservoir capacity. | m³ |
| renewable_generation_forecast | Wind, hydro and PV generation forecasts. | kWh |
| generation_cost | Renewable generation cost. | €/kWh |
| charging_cost | Battery charging cost. | €/kWh |
| pumping_hydro_cost | Pumped hydro operation cost. | €/kWh |

---

## Service Outputs

| Output | Description | Unit |
|---|---|---|
| afrr_power_setpoint | Active-power restoration setpoint. | MW |
| lib_converter_power | Li-ion battery converter power. | MW |
| vrfb_converter_power | VRFB converter power, where applicable. | MW |
| hydro_power | Hydro generation output. | MW |
| synchronous_machine_state | Synchronous machine / condenser state. | status |
| candidate_operating_point | Candidate dispatch from optimisation. | n/a |
| dynamic_security_result | Result of the N-1 assessment. | status |
| accepted_operating_point | Final accepted dispatch. | n/a |
| restoration_status | Pending, accepted, rejected or completed. | status |

---

## Optimisation Logic

```text
Minimise:
    restoration error
    + generation and storage operation cost
    + charging cost
    + pumped hydro operation cost
    + penalties for infeasible or dynamically unsafe operation
```

Subject to:

```text
Storage constraints:
    SoC and converter power limits are respected.
    Enough upward or downward capability is available.

Hydro constraints:
    Reservoir and hydro dispatch limits are respected.

Restoration constraints:
    The aFRR activation signal is followed as closely as possible.
    The restoration direction is respected.

Dynamic-security constraints:
    Candidate dispatch must pass N-1 dynamic assessment.
    If unstable, synchronous condenser dispatch is activated and the optimisation is repeated.
```

---

## Suggested JSON Structure

```json
{
  "service": "aFRR",
  "demo": "Demo2",
  "metadata": {
    "service_id": "demo2_afrr",
    "requester_id": "EEM",
    "timestamp": "YYYY-MM-DDTHH:mm:ssZ"
  },
  "activation": {
    "afrr_activation_signal": {
      "value": null,
      "unit": "MW"
    },
    "restoration_direction": "upward_or_downward",
    "frequency_deviation": {
      "value": null,
      "unit": "Hz"
    }
  },
  "resources": {
    "liIonBattery": {
      "soc": {
        "values": [],
        "unit": "kWh"
      },
      "converter_power": {
        "values": [],
        "unit": "MW"
      }
    },
    "hydroPowerPlant": {
      "reservoir_capacity": {
        "values": [],
        "unit": "m3"
      },
      "hydro_power": {
        "values": [],
        "unit": "MW"
      }
    },
    "synchronousMachine": {
      "state": "available"
    }
  },
  "dynamicAssessment": {
    "n_minus_one_criterion": true,
    "dynamic_security_result": "pending",
    "accepted_operating_point": {}
  },
  "outputs": {
    "afrr_power_setpoint": {
      "value": null,
      "unit": "MW"
    },
    "restoration_status": "pending"
  }
}
```

---

## HMI and Dashboard Requirements

The Demo 2 aFRR dashboard should display:

- service request status;
- aFRR activation signal;
- restoration direction;
- candidate dispatch point;
- accepted operating point;
- Li-ion BESS SoC;
- hydro reservoir capacity;
- synchronous condenser state;
- N-1 validation status;
- restoration status;
- rejected candidate points;
- service availability and KPIs.

---

## Implementation Status

| Criterion | Status |
|---|---|
| Conceptual service definition | Completed |
| Demo-specific workflow | Completed |
| Sequence diagram | Completed |
| Data model orientation | Completed |
| API-mediated process | Completed |
| Dynamic N-1 assessment | Included |
| Synchronous condenser re-dispatch loop | Included |
| Final readiness level | Simulated / HIL Validated |
| Final repository service | Yes |

---

## Lessons Learned

The Demo 2 aFRR implementation shows that restoration reserve provision in an island-grid context must be assessed together with dynamic security. The common EEM / INESC TEC workflow is reusable across ancillary services, provided that the service-specific activation signal and validation KPIs are clearly separated.
