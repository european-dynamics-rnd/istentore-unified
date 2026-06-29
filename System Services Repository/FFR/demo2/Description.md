# Implementation Details of FFR in Demo 2

## Demo Context

Demo 2 corresponds to the i-STENTORE demonstrator **Pump Hydro Storage system combined with BESS – Portugal**.

The demonstrator is located in an islanded grid context where fast dynamic response is important due to limited system inertia and the absence of support from a large interconnected grid. Fast Frequency Response (FFR) is represented as a fast active-power service delivered through storage and coordinated with hydro and synchronous-machine resources.

FFR is part of the shared Demo 2 ancillary-service workflow together with Synchronous Inertia, FCR and aFRR.

---

## Service Objective

The objective of FFR in Demo 2 is to provide very fast active-power support following a frequency disturbance.

The service aims to:

- reduce rapid frequency deviations;
- support low-inertia island-grid operation;
- provide fast BESS / HESS active-power response;
- coordinate with inertia and FCR;
- validate the operating point using dynamic assessment;
- preserve system security under the N-1 criterion;
- provide a reusable FFR service model for storage-based grid support.

---

## Service Classification

| Field | Description |
|---|---|
| Service name | Fast Frequency Response |
| Acronym | FFR |
| Demonstrator | Demo 2 – Portugal |
| Service family | Ancillary service / fast frequency service |
| Main objective | Very fast active-power support after a frequency disturbance |
| Main system context | Islanded grid |
| Main resources | Li-ion BESS, hydro and synchronous-machine functionality |
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
- EEM API interface;
- EEM server;
- INESC TEC virtual machine;
- optimisation algorithm;
- dynamic assessment procedure;
- HMI / dashboard;
- i-STENTORE semantic and service data model layer.

---

## Actors

| Actor | Role |
|---|---|
| EEM | Initiates the workflow and applies the accepted operating point. |
| INESC TEC | Hosts optimisation and dynamic assessment. |
| EEM Server | Provides input data and stores results. |
| INESC TEC Virtual Machine | Retrieves data, runs optimisation and stores results. |
| Optimisation Algorithm | Calculates candidate dispatch and available fast-response capability. |
| Dynamic Assessment Procedure | Checks frequency stability and N-1 security. |
| Li-ion BESS | Provides fast active-power support. |
| Hydro Power Plant | Provides dispatchable support. |
| Synchronous Condenser / Machine | Provides additional dynamic support when required. |
| HMI / Dashboard | Displays FFR status, response and KPIs. |

---

## Operational Principle

FFR is triggered by a frequency deviation or a fast frequency-security requirement. The service requires very fast active-power response, usually from the BESS or another power-electronic storage interface.

In Demo 2, FFR is represented within the common optimisation and dynamic-assessment workflow. A candidate operating point is calculated and checked dynamically. If the operating point is stable, it is accepted and applied. If not, synchronous condenser dispatch is used as corrective support and the optimisation is repeated.

---

## Implementation Workflow

```text
1. EEM initiates the FFR workflow through the API.
2. INESC TEC retrieves updated EEM system data.
3. Fast-response availability is estimated.
4. The optimisation algorithm computes a candidate operating point.
5. Dynamic assessment evaluates frequency behaviour and N-1 stability.
6. Stable candidate:
   |-- store result;
   |-- apply accepted operating point.
7. Unstable candidate:
   |-- dispatch synchronous condenser;
   |-- repeat optimisation and assessment.
8. Store final results and display KPIs.
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
    participant BESS as Li-ion BESS
    participant SYN as Synchronous Condenser
    participant UI as HMI / Dashboard
    participant MW as i-STENTORE Middleware

    EEM->>API: Request FFR workflow
    API->>VM: Trigger fast-response optimisation
    VM->>Server: Fetch updated system data
    Server-->>VM: Return generation, storage and hydro data

    VM->>OPT: Run optimisation
    OPT-->>VM: Candidate operating point and FFR capability
    VM->>DYN: Assess dynamic stability and N-1 security

    alt Candidate is stable
        DYN-->>VM: Confirm accepted point
        VM->>Server: Store accepted result
        VM->>BESS: Provide accepted fast-response dispatch context
    else Candidate is unstable
        DYN-->>VM: Reject candidate
        VM->>SYN: Dispatch synchronous condenser
        VM->>OPT: Repeat optimisation
        VM->>DYN: Repeat assessment
    end

    VM->>UI: Display FFR status and KPIs
    VM->>MW: Share harmonised service data
```

---

## Data Inputs

| Input | Description | Unit |
|---|---|---|
| nominal_frequency | Nominal system frequency. | Hz |
| frequency_deviation | Frequency deviation from nominal. | Hz |
| rocof | Rate of change of frequency, where available. | Hz/s |
| ffr_activation_threshold | Frequency or RoCoF threshold. | Hz or Hz/s |
| response_time_requirement | Required fast-response time. | s |
| SoC_LIB | Li-ion battery state of charge. | kWh |
| available_bess_power | Available BESS fast-response power. | MW |
| Reservoir_HPP | Hydro reservoir capacity. | m³ |
| renewable_generation_forecast | Wind, hydro and PV generation forecasts. | kWh |
| dynamic_security_requirement | Dynamic stability requirement. | n/a |
| n_minus_one_requirement | N-1 criterion flag. | Boolean |

---

## Service Outputs

| Output | Description | Unit |
|---|---|---|
| ffr_power_setpoint | Fast active-power response setpoint. | MW |
| delivered_ffr_power | Delivered fast response. | MW |
| response_time | Time to deliver response. | s |
| ffr_availability | Availability of fast response. | status |
| candidate_operating_point | Candidate dispatch from optimisation. | n/a |
| dynamic_security_result | N-1 dynamic assessment result. | status |
| synchronous_machine_state | Synchronous condenser / machine state. | status |
| accepted_operating_point | Final accepted dispatch. | n/a |

---

## Optimisation Logic

```text
Minimise:
    frequency-security risk
    + insufficient fast-response capability
    + generation and storage operation cost
    + penalties for dynamically unsafe dispatch
```

Subject to:

```text
Fast-response constraints:
    FFR capability must be available.
    Response must satisfy time and power requirements.
    BESS SoC and power limits are respected.

Dynamic-security constraints:
    Candidate operating point must pass N-1 dynamic assessment.
    If unstable, synchronous condenser dispatch is activated.

Resource constraints:
    Hydro and storage operating constraints are respected.
    Renewable generation forecasts are considered.
```

---

## Suggested JSON Structure

```json
{
  "service": "FFR",
  "demo": "Demo2",
  "metadata": {
    "service_id": "demo2_ffr",
    "requester_id": "EEM",
    "timestamp": "YYYY-MM-DDTHH:mm:ssZ"
  },
  "frequency": {
    "nominal_frequency": {
      "value": 50.0,
      "unit": "Hz"
    },
    "frequency_deviation": {
      "value": null,
      "unit": "Hz"
    },
    "rocof": {
      "value": null,
      "unit": "Hz/s"
    },
    "ffr_activation_threshold": {
      "value": null,
      "unit": "Hz"
    }
  },
  "storage": {
    "bess_state_of_charge": {
      "value": null,
      "unit": "%"
    },
    "available_fast_response_power": {
      "value": null,
      "unit": "MW"
    },
    "response_time_requirement": {
      "value": null,
      "unit": "s"
    }
  },
  "dynamicAssessment": {
    "n_minus_one_criterion": true,
    "dynamic_security_result": "pending",
    "synchronous_machine_state": "available",
    "accepted_operating_point": {}
  },
  "outputs": {
    "ffr_power_setpoint": {
      "value": null,
      "unit": "MW"
    },
    "delivered_ffr_power": [],
    "response_time": {
      "value": null,
      "unit": "s"
    },
    "ffr_availability": "pending"
  },
  "kpis": {
    "response_time": null,
    "activation_accuracy": null,
    "service_availability": null,
    "dynamic_security_status": null
  }
}
```

---

## HMI and Dashboard Requirements

The Demo 2 FFR dashboard should display:

- frequency deviation;
- RoCoF, where available;
- FFR activation threshold;
- fast-response availability;
- BESS SoC;
- available BESS fast-response power;
- response-time requirement;
- candidate operating point;
- dynamic assessment result;
- synchronous condenser state;
- accepted operating point;
- delivered FFR power and KPIs.

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

---

## Lessons Learned

Demo 2 shows that FFR is highly relevant for low-inertia islanded systems, but it must be coordinated with inertia, FCR and dynamic security. Storage can provide the required fast response, but the accepted operating point must still satisfy N-1 stability requirements.
