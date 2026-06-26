# Implementation Details of Synchronous Inertia in Demo 2

## Demo Context

Demo 2 corresponds to the i-STENTORE demonstrator **Pump Hydro Storage system combined with BESS – Portugal**.

The demonstrator is located in an islanded grid context and combines hydro-based resources, battery storage and optimisation routines to support ancillary services and dynamic grid stability. In this context, Synchronous Inertia is implemented as a frequency-stability service that supports the system during dynamic events by coordinating the available hydro, storage and synchronous-machine capabilities.

The service is part of a common Demo 2 ancillary-service workflow that also includes Fast Frequency Response, Frequency Containment Reserve and Automatic Frequency Restoration Reserve. The shared workflow uses an API-mediated interaction between EEM and INESC TEC, followed by optimisation and dynamic assessment of the proposed operating point.

---

## Service Objective

The objective of the Synchronous Inertia service in Demo 2 is to support the dynamic stability of the islanded electricity system by defining an operating point that is both economically optimal and dynamically secure.

The service aims to:

- support system frequency stability;
- reduce the impact of rapid frequency deviations;
- improve dynamic security in an islanded grid;
- coordinate hydro resources, storage and synchronous-machine operation;
- validate the proposed operating point against the N-1 criterion;
- dispatch synchronous condensers when required to ensure system stability;
- provide a reusable inertia-oriented service model compatible with the i-STENTORE Service Repository.

---

## Service Classification

| Field | Description |
|---|---|
| Service name | Synchronous Inertia |
| Demonstrator | Demo 2 – Portugal |
| Demonstrator title | Pump Hydro Storage system combined with BESS |
| Service family | Ancillary service / frequency-stability service |
| Main objective | Dynamic stability and inertia-related support |
| Main system context | Islanded grid |
| Main flexibility resources | Hydro resources, Li-ion battery and synchronous-machine functionality |
| Main requester / operator | EEM |
| Optimisation provider | INESC TEC |
| Main validation procedure | Dynamic assessment with N-1 criterion |
| Final readiness level | Simulated / HIL Validated |
| Final repository service | Yes |

---

## Demonstrator Assets

### Main Assets

- Pumped hydro storage system;
- Hydro power plant;
- Li-ion Battery Energy Storage System;
- VRFB-related data-model structure, where applicable to the common Demo 2 optimisation framework;
- Synchronous machine / synchronous condenser functionality;
- Renewable generation resources;
- Islanded grid infrastructure.

### Digital and Control Assets

- EEM API interface;
- EEM SFTP server;
- INESC TEC virtual machine;
- Optimisation algorithm;
- Dynamic assessment procedure;
- N-1 security assessment;
- EEM generation-control layer;
- EEM data-storage environment;
- INESC TEC data-storage environment;
- HMI or dashboard representation;
- i-STENTORE semantic and service data model layer.

---

## Actors

| Actor | Role |
|---|---|
| EEM | Initiates the service through the API, provides system data and applies the final stable operating point. |
| INESC TEC | Hosts the virtual machine, optimisation algorithm and dynamic assessment workflow. |
| EEM Server | Stores input data and receives optimisation and assessment results. |
| INESC TEC Virtual Machine | Fetches updated EEM data, runs the optimisation and stores results. |
| Optimisation Algorithm | Defines the economically optimal and technically feasible dispatch point. |
| Dynamic Assessment Procedure | Evaluates whether the operating point is dynamically safe under the N-1 criterion. |
| Synchronous Condenser / Synchronous Machine | Provides additional inertia or dynamic stability support when required. |
| Hydro Power Plant | Provides hydro-based generation and dynamic support. |
| Li-ion BESS | Provides fast converter-based active-power support. |
| HMI / Dashboard | Displays operating points, assessment status, accepted dispatch and service KPIs. |
| i-STENTORE Middleware | Supports harmonised service representation and information exchange. |

---

## Operational Principle

The Synchronous Inertia service starts when EEM sends an API request to initiate the ancillary-service optimisation workflow.

The INESC TEC virtual machine retrieves the latest system data from the EEM server and executes the optimisation algorithm. The optimisation defines a general dispatch point for the available technologies, including hydro resources, Li-ion battery resources and synchronous-machine operation.

The resulting operating point is then passed through a dynamic assessment procedure. This procedure evaluates whether the system remains stable under the N-1 criterion.

If the operating point is dynamically safe, the results are stored and EEM applies the defined operating point to its generation technologies.

If the operating point is not dynamically safe, synchronous condensers are dispatched and the optimisation is repeated. The process continues until a stable operating point is found.

---

## Relation with Other Demo 2 Ancillary Services

Synchronous Inertia is implemented in Demo 2 as part of a shared ancillary-service workflow.

| Service | Relation with Synchronous Inertia |
|---|---|
| FFR | Provides very fast active-power response after a frequency event. |
| FCR | Contains frequency deviations after the initial inertial or fast response. |
| aFRR | Restores frequency and balances the system after FCR activation. |
| Power Smoothing | Reduces renewable or system power fluctuations that may contribute to frequency deviations. |

The same optimisation and dynamic assessment framework can be reused by several of these services, but the service objective and validation criteria differ.

---

## Timing Requirements

| Parameter | Value / Description |
|---|---|
| Planning horizon | Operational dispatch horizon defined by the EEM / INESC TEC workflow |
| Dynamic response | Milliseconds to seconds, depending on the resource |
| Frequency of use | Several times per day, depending on operating conditions |
| Main unit of service | MW and dynamic frequency support |
| Application scale | Small island network |
| Main validation criterion | N-1 dynamic security |
| Final validation status | Simulated / HIL Validated |

---

## Information and Data Flows

### 1. Service Request

EEM initiates the service through the API.

The request triggers the ancillary-service optimisation workflow and indicates that the latest available system data should be used.

### 2. Data Retrieval

The INESC TEC virtual machine fetches updated data from the EEM server.

The retrieved data may include:

- renewable generation;
- renewable generation forecasts;
- Li-ion battery state of charge;
- hydro reservoir capacity;
- generation costs;
- charging costs;
- pumped hydro operation costs;
- system operating conditions.

### 3. Optimisation

The optimisation algorithm defines a general technology dispatch.

The objective is to obtain an operating point that is:

- economically optimal;
- compatible with available generation and storage resources;
- technically feasible;
- suitable for subsequent dynamic-security validation.

### 4. Dynamic Assessment

The dynamic assessment procedure evaluates the proposed operating point.

The assessment includes:

- N-1 criterion;
- dynamic stability;
- inertia-related behaviour;
- suitability of the synchronous machine / condenser state;
- system security under disturbance conditions.

### 5. Stability Decision

If the proposed dispatch is safe and stable:

- results are stored in the INESC TEC virtual machine;
- results are stored on the EEM server;
- EEM sets its generation technologies to the defined point of operation.

If the proposed dispatch is not safe and stable:

- synchronous condensers are dispatched;
- the optimisation is repeated;
- the dynamic assessment is repeated until a stable operating point is reached.

### 6. Monitoring and Reporting

The system stores and displays:

- service request status;
- optimisation result;
- dynamic assessment result;
- accepted or rejected dispatch point;
- synchronous-machine state;
- final stable operating point;
- service KPIs.

---

## Implementation Workflow

```text
1. EEM initiates the Synchronous Inertia service through the API.
   |
2. INESC TEC virtual machine receives the request.
   |
3. INESC TEC virtual machine fetches updated data from the EEM server.
   |
   |-- Renewable generation and forecasts
   |-- Li-ion battery SoC
   |-- Hydro reservoir capacity
   |-- Cost parameters
   |-- System operating conditions
   |
4. The optimisation algorithm defines a candidate dispatch point.
   |
   |-- Hydro dispatch
   |-- Li-ion battery converter power
   |-- Renewable generation use
   |-- Synchronous-machine state
   |
5. The dynamic assessment procedure evaluates the candidate point.
   |
   |-- N-1 criterion
   |-- Frequency-stability behaviour
   |-- Dynamic-security result
   |
6. If the point is dynamically safe:
   |
   |-- Store results on INESC TEC virtual machine
   |-- Store results on EEM server
   |-- EEM applies the accepted operating point
   |
7. If the point is not dynamically safe:
   |
   |-- Dispatch synchronous condensers
   |-- Repeat optimisation
   |-- Repeat dynamic assessment
   |
8. Store final results and update the HMI / dashboard.
```

---

## Sequence Diagram

```mermaid
sequenceDiagram
    participant EEM as EEM
    participant API as API
    participant Server as EEM Server / SFTP
    participant VM as INESC TEC Virtual Machine
    participant OPT as Optimisation Algorithm
    participant DYN as Dynamic Assessment Procedure
    participant SYN as Synchronous Condenser / Machine
    participant GEN as Generation Technologies
    participant UI as HMI / Dashboard
    participant MW as i-STENTORE Middleware

    EEM->>API: Request Synchronous Inertia service
    API->>VM: Trigger optimisation workflow

    VM->>Server: Fetch updated system and asset data
    Server-->>VM: Provide generation, storage, hydro and cost data

    VM->>OPT: Run optimisation algorithm
    OPT-->>VM: Return candidate operating point

    VM->>DYN: Submit candidate point for dynamic assessment
    DYN->>DYN: Evaluate N-1 criterion and dynamic stability

    alt Operating point is stable
        DYN-->>VM: Confirm safe and stable operation
        VM->>Server: Store accepted results
        EEM->>Server: Retrieve accepted operating point
        EEM->>GEN: Set generation technologies to accepted point
    else Operating point is not stable
        DYN-->>VM: Reject candidate point
        VM->>SYN: Dispatch synchronous condenser
        SYN-->>VM: Return updated synchronous-machine state
        VM->>OPT: Repeat optimisation
        OPT-->>VM: Return updated operating point
        VM->>DYN: Repeat dynamic assessment
    end

    VM->>UI: Display operating point and assessment result
    VM->>MW: Share harmonised service information where applicable
```

---

## Data Inputs

### Metadata and Scheduling Inputs

| Input | Code Reference | Description | Unit |
|---|---|---|---|
| Service identifier | service_id | Identifier of the service instance. | n/a |
| Requester identifier | requester_id | Identifier of EEM or service requester. | n/a |
| Timestamp | timestamp | Timestamp of the request. | datetime |
| Timestep | timestep | Optimisation timestep. | implementation-specific |
| Horizon | horizon | Optimisation horizon. | implementation-specific |

### Storage and Hydro Inputs

| Input | Code Reference | Description | Unit |
|---|---|---|---|
| Maximum Li-ion battery SoC | SoC_max_LIB | Maximum state of charge of the Li-ion battery. | % |
| Maximum VRFB SoC | SoC_max_VRFB | Maximum state of charge of the VRFB, where applicable. | % |
| Maximum reservoir capacity | Reservoir_max_HPP | Maximum reservoir capacity of the hydro power plant. | % |
| Li-ion battery charging efficiency | eta_ch_LIB | Charging efficiency of the Li-ion battery. | % |
| VRFB charging efficiency | eta_ch_VRFB | Charging efficiency of the VRFB, where applicable. | % |
| Li-ion battery SoC | SoC_LIB | Li-ion battery state of charge. | kWh |
| VRFB SoC | SoC_VRFB | VRFB state of charge, where applicable. | kWh |
| Hydro reservoir capacity | Reservoir_HPP | Hydro reservoir capacity. | m³ |

### Generation and Forecast Inputs

| Input | Description | Unit |
|---|---|---|
| Wind generation | Wind power generation. | kWh |
| Hydro generation | Hydro power generation. | kWh |
| PV generation | PV power generation. | kWh |
| Wind generation forecast | Forecasted wind generation. | kWh |
| Hydro generation forecast | Forecasted hydro generation. | kWh |
| PV generation forecast | Forecasted PV generation. | kWh |
| Renewable generation cost | Cost associated with renewable generation. | €/kWh |
| Li-ion battery charging cost | Cost associated with Li-ion battery charging. | €/kWh |
| VRFB charging cost | Cost associated with VRFB charging. | €/kWh |
| Pumped hydro cost | Cost associated with pumped hydro operation. | €/kWh |

---

## Service Outputs

| Output | Description | Unit |
|---|---|---|
| Li-ion battery converter power | Converter power of the Li-ion battery. | MW |
| VRFB converter power | Converter power of the VRFB, where applicable. | MW |
| Hydro power | Hydro power output. | MW |
| Wind power | Wind power output. | MW |
| PV power | PV power output. | MW |
| Synchronous machine state | State of the synchronous machine or condenser functionality. | Boolean / status |
| HESS operational schedule | Operational schedule for the hybrid storage system. | time series |
| Candidate operating point | Dispatch point proposed by the optimisation algorithm. | n/a |
| Dynamic security result | Result of the dynamic assessment. | status |
| Accepted operating point | Final operating point after dynamic validation. | n/a |

---

## Optimisation Logic

The Synchronous Inertia workflow combines economic dispatch with dynamic-security validation.

The optimisation can be represented as follows.

```text
Minimise:
    Total generation and storage operation cost
    + charging costs
    + pumped hydro operation costs
    + penalties for infeasible operation
```

Subject to:

```text
Storage constraints:
    SoC_LIB_min <= SoC_LIB_t <= SoC_LIB_max
    SoC_VRFB_min <= SoC_VRFB_t <= SoC_VRFB_max
    Converter power limits are respected
    Charging efficiencies are considered

Hydro constraints:
    Reservoir_HPP_t <= Reservoir_max_HPP
    Hydro generation and pumped hydro operation constraints are respected

Renewable generation constraints:
    Wind, hydro and PV generation are considered
    Forecasted renewable generation is included in the dispatch calculation

Dynamic-security constraints:
    Candidate dispatch must pass dynamic assessment
    N-1 criterion must be satisfied
    If not stable, synchronous condenser dispatch is activated and optimisation is repeated
```

---

## Suggested JSON Structure

```json
{
  "service": "SynchronousInertia",
  "demo": "Demo2",
  "metadata": {
    "service_id": "demo2_synchronous_inertia",
    "requester_id": "EEM",
    "timestamp": "YYYY-MM-DDTHH:mm:ssZ"
  },
  "scheduling": {
    "timestep": {
      "value": null,
      "unit": "min"
    },
    "horizon": {
      "value": null,
      "unit": "h"
    }
  },
  "storageAndHydro": {
    "liIonBattery": {
      "maximum_soc": {
        "value": null,
        "unit": "%"
      },
      "charging_efficiency": {
        "value": null,
        "unit": "%"
      },
      "soc": {
        "values": [],
        "unit": "kWh"
      },
      "converter_power": {
        "values": [],
        "unit": "MW"
      }
    },
    "vrfb": {
      "maximum_soc": {
        "value": null,
        "unit": "%"
      },
      "charging_efficiency": {
        "value": null,
        "unit": "%"
      },
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
      "maximum_reservoir_capacity": {
        "value": null,
        "unit": "%"
      },
      "reservoir_capacity": {
        "values": [],
        "unit": "m3"
      },
      "hydro_power": {
        "values": [],
        "unit": "MW"
      },
      "pumped_hydro_cost": {
        "values": [],
        "unit": "€/kWh"
      }
    }
  },
  "renewableGeneration": {
    "wind_generation": {
      "values": [],
      "unit": "kWh"
    },
    "hydro_generation": {
      "values": [],
      "unit": "kWh"
    },
    "pv_generation": {
      "values": [],
      "unit": "kWh"
    },
    "wind_generation_forecast": {
      "values": [],
      "unit": "kWh"
    },
    "hydro_generation_forecast": {
      "values": [],
      "unit": "kWh"
    },
    "pv_generation_forecast": {
      "values": [],
      "unit": "kWh"
    },
    "generation_cost": {
      "values": [],
      "unit": "€/kWh"
    }
  },
  "dynamicAssessment": {
    "n_minus_one_criterion": true,
    "candidate_operating_point": {},
    "dynamic_security_result": "pending",
    "synchronous_machine_state": {
      "values": [],
      "unit": "status"
    },
    "accepted_operating_point": {}
  },
  "kpis": {
    "dynamic_security_status": null,
    "frequency_stability_indicator": null,
    "number_of_rejected_dispatch_points": null,
    "synchronous_condenser_dispatch_count": null,
    "service_availability": null
  }
}
```

---

## HMI and Dashboard Requirements

The Demo 2 Synchronous Inertia dashboard should display:

- service request status;
- optimisation execution status;
- Li-ion battery SoC;
- hydro reservoir capacity;
- renewable generation and forecast values;
- candidate operating point;
- dynamic assessment result;
- N-1 validation status;
- synchronous-machine or condenser state;
- accepted operating point;
- rejected operating points, where applicable;
- number of optimisation iterations;
- service availability indicators.

Historical views should include:

- service requests;
- optimisation outputs;
- stability assessment outcomes;
- synchronous condenser dispatch events;
- accepted and rejected operating points;
- storage and hydro utilisation;
- frequency-stability indicators.

---

## Implementation Status

The Synchronous Inertia service in Demo 2 was fully specified and validated at modelling / simulation level.

The implementation includes:

- API-triggered EEM / INESC TEC workflow;
- optimisation of a candidate operating point;
- dynamic assessment using the N-1 criterion;
- iterative re-dispatch of synchronous condensers if the operating point is not stable;
- harmonised data model;
- repository-compatible JSON structure.

The final readiness level is **Simulated / HIL Validated**, reflecting detailed inertia modelling and dynamic-security validation rather than full operational market deployment.

| Criterion | Status |
|---|---|
| Conceptual service definition | Completed |
| Demo-specific workflow | Completed |
| Sequence diagram | Completed |
| Data model orientation | Completed |
| API-mediated process | Completed |
| Dynamic N-1 assessment | Included |
| Synchronous condenser re-dispatch loop | Included |
| Field market activation | Not applicable / not demonstrated |

---

## Lessons Learned

The Demo 2 Synchronous Inertia implementation provides several lessons:

- isolated grids require explicit dynamic-security assessment because they cannot rely on large interconnected-system inertia;
- economic optimisation must be combined with dynamic validation;
- a candidate dispatch point can be economically optimal but dynamically unsafe;
- the N-1 criterion provides a practical validation layer for service readiness;
- synchronous condensers or synchronous-machine functionality can be used as corrective measures when dynamic assessment fails;
- Li-ion battery support can contribute to fast active-power response but must be coordinated with hydro and synchronous resources;
- the same workflow can support several ancillary services if the service objective and validation criteria are clearly distinguished;
- harmonised data models allow inertia-related services to be represented consistently in the i-STENTORE Service Repository.

---

