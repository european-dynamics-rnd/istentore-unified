# Implementation Details of FCR in Demo 2

## Demo Context

Demo 2 corresponds to the i-STENTORE demonstrator **Pump Hydro Storage system combined with BESS – Portugal**.

The demonstrator is located in an islanded grid context and combines hydro-based resources, battery storage and optimisation routines to support ancillary services and dynamic grid stability. In this context, Frequency Containment Reserve (FCR) is represented as a primary frequency-control service that supports the system immediately after a frequency disturbance.

FCR in Demo 2 is part of a common ancillary-service workflow that also includes Synchronous Inertia, Fast Frequency Response and Automatic Frequency Restoration Reserve. The workflow uses an API-mediated interaction between EEM and INESC TEC, followed by optimisation and dynamic assessment of the proposed operating point.

---

## Service Objective

The objective of the FCR service in Demo 2 is to contain frequency deviations in the islanded electricity system by coordinating the available hydro, storage and synchronous-machine resources.

The service aims to:

- support primary frequency control in the island grid;
- contain frequency deviations immediately after a disturbance;
- use HESS resources to provide fast active-power response;
- define a dispatch point that is economically optimal and dynamically viable;
- validate the operating point through dynamic assessment;
- apply the N-1 criterion before accepting the dispatch;
- dispatch synchronous condensers when needed to maintain system stability;
- provide a reusable FCR service model compatible with the i-STENTORE Service Repository.

---

## Service Classification

| Field | Description |
|---|---|
| Service name | Frequency Containment Reserve |
| Acronym | FCR |
| Demonstrator | Demo 2 – Portugal |
| Demonstrator title | Pump Hydro Storage system combined with BESS |
| Service family | Ancillary service / frequency regulation service |
| Main objective | Primary frequency containment |
| Main system context | Islanded grid |
| Main flexibility resources | Hydro resources, Li-ion battery, VRFB representation and synchronous-machine functionality |
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
| Synchronous Condenser / Synchronous Machine | Provides additional dynamic support if the initial operating point is not stable. |
| Hydro Power Plant | Provides hydro-based generation and dynamic support. |
| Li-ion BESS | Provides fast converter-based active-power support. |
| HMI / Dashboard | Displays operating point, assessment status, accepted dispatch and FCR KPIs. |
| i-STENTORE Middleware | Supports harmonised service representation and information exchange. |

---

## Operational Principle

FCR is the primary frequency-control service. In Demo 2, it is represented through the same optimisation and dynamic assessment workflow used for the other ancillary services.

The process starts when EEM sends an API request to run the optimisation. The INESC TEC virtual machine fetches the latest data from the EEM server and executes the optimisation algorithm. The algorithm defines a general dispatch point for the available technologies.

The candidate dispatch point is then assessed through a dynamic procedure. If the point of operation leads to a safe and stable system, the results are stored and EEM applies the dispatch to the generation technologies. If the point is not stable, synchronous condensers are dispatched and the optimisation is repeated until a stable point of operation is achieved.

Although the workflow is not a direct market-based FCR activation, it provides the technical foundation for ensuring that enough dynamic capability is available for primary frequency containment in the islanded system.

---

## Relation with Other Demo 2 Ancillary Services

FCR is implemented in Demo 2 as part of a shared ancillary-service workflow.

| Service | Relation with FCR |
|---|---|
| Synchronous Inertia | Supports the initial system response and reduces the rate of change of frequency. |
| FFR | Provides very fast active-power support after a frequency disturbance. |
| FCR | Contains the frequency deviation through automatic primary control. |
| aFRR | Restores system balance after the frequency deviation has been contained. |
| Power Smoothing | Reduces renewable or system power fluctuations that may trigger frequency deviations. |

The same optimisation and dynamic assessment architecture supports these services, but the service objective and validation metrics are different.

---

## Timing Requirements

| Parameter | Value / Description |
|---|---|
| Activation trigger | Frequency deviation or primary-control requirement |
| Reaction time | Milliseconds in the conceptual service characterisation |
| Discharge time | Depends on the HESS component providing the service |
| Frequency of use | Several times per day, depending on operating conditions |
| Main unit of service | MW |
| Application scale | Small island network at 60 kV, 30 kV and 6.6 kV levels |
| Business case | Day-ahead congestion planning / internal system support |
| Remuneration | Not market-based; service deployed by EEM in an isolated grid context |
| Final validation status | Simulated / HIL Validated |

---

## Information and Data Flows

### 1. Service Request

EEM initiates the FCR-related ancillary-service optimisation workflow through the API.

The request triggers the optimisation and indicates that the latest available EEM system data should be used.

### 2. Data Retrieval

The INESC TEC virtual machine fetches updated data from the EEM server.

The retrieved data may include:

- renewable generation;
- renewable generation forecasts;
- Li-ion battery state of charge;
- VRFB state of charge, where applicable to the common data model;
- hydro reservoir capacity;
- generation costs;
- charging costs;
- pumped hydro operation costs;
- system operating conditions;
- relevant frequency or dynamic-stability parameters.

### 3. Optimisation

The optimisation algorithm defines a general technology dispatch.

The objective is to obtain an operating point that is:

- economically optimal;
- compatible with available generation and storage resources;
- technically feasible;
- suitable for primary frequency containment;
- suitable for subsequent dynamic-security validation.

### 4. Dynamic Assessment

The dynamic assessment procedure evaluates the proposed operating point.

The assessment includes:

- N-1 criterion;
- dynamic stability;
- frequency containment behaviour;
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
- candidate operating point;
- dynamic assessment result;
- accepted or rejected dispatch point;
- synchronous-machine state;
- final stable operating point;
- FCR service KPIs.

---

## Implementation Workflow

```text
1. EEM initiates the FCR service through the API.
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
4. The optimisation algorithm defines a candidate operating point.
   |
   |-- Hydro dispatch
   |-- Li-ion battery converter power
   |-- Renewable generation use
   |-- Synchronous-machine state
   |-- FCR-related reserve capability
   |
5. The dynamic assessment procedure evaluates the candidate point.
   |
   |-- N-1 criterion
   |-- Frequency-containment behaviour
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

    EEM->>API: Request FCR-related optimisation
    API->>VM: Trigger ancillary-service workflow

    VM->>Server: Fetch updated system and asset data
    Server-->>VM: Provide generation, storage, hydro and cost data

    VM->>OPT: Run optimisation algorithm
    OPT-->>VM: Return candidate operating point and FCR capability

    VM->>DYN: Submit candidate point for dynamic assessment
    DYN->>DYN: Evaluate N-1 criterion and frequency-containment behaviour

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

    VM->>UI: Display FCR status, dispatch and assessment result
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

### Frequency and Dynamic Inputs

| Input | Description | Unit |
|---|---|---|
| nominal_frequency | Nominal system frequency. | Hz |
| measured_frequency | Measured or simulated system frequency. | Hz |
| frequency_deviation | Deviation from nominal frequency. | Hz or mHz |
| fcr_capacity_requirement | FCR capacity required by the system model. | MW |
| dynamic_security_requirement | Stability requirement used in the dynamic assessment. | n/a |
| n_minus_one_requirement | Whether the N-1 criterion must be satisfied. | Boolean |

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
| fcr_available_capacity | Available capacity for FCR provision. | MW |
| fcr_power_setpoint | Active-power response assigned to FCR. | MW |
| Li-ion battery converter power | Converter power of the Li-ion battery. | MW |
| VRFB converter power | Converter power of the VRFB, where applicable. | MW |
| Hydro power | Hydro power output. | MW |
| Wind power | Wind power output. | MW |
| PV power | PV power output. | MW |
| Synchronous machine state | State of the synchronous machine or condenser functionality. | Boolean / status |
| Candidate operating point | Dispatch point proposed by the optimisation algorithm. | n/a |
| Dynamic security result | Result of the dynamic assessment. | status |
| Accepted operating point | Final operating point after dynamic validation. | n/a |

---

## Optimisation Logic

The FCR workflow combines economic dispatch with a frequency-containment and dynamic-security validation step.

The optimisation can be represented as follows.

```text
Minimise:
    Total generation and storage operation cost
    + charging costs
    + pumped hydro operation costs
    + penalties for insufficient FCR capability
    + penalties for infeasible operation
```

Subject to:

```text
Storage constraints:
    SoC_LIB_min <= SoC_LIB_t <= SoC_LIB_max
    SoC_VRFB_min <= SoC_VRFB_t <= SoC_VRFB_max
    Converter power limits are respected
    Charging efficiencies are considered
    Enough headroom is preserved for FCR response

Hydro constraints:
    Reservoir_HPP_t <= Reservoir_max_HPP
    Hydro generation and pumped hydro operation constraints are respected

Frequency-control constraints:
    FCR capability must be available at the accepted operating point
    Frequency-containment behaviour must satisfy dynamic assessment
    Reserve capability must be compatible with storage and hydro dispatch

Dynamic-security constraints:
    Candidate dispatch must pass dynamic assessment
    N-1 criterion must be satisfied
    If not stable, synchronous condenser dispatch is activated and optimisation is repeated
```

---

## Suggested JSON Structure

```json
{
  "service": "FCR",
  "demo": "Demo2",
  "metadata": {
    "service_id": "demo2_fcr",
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
  "frequency": {
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
    },
    "fcr_capacity_requirement": {
      "value": null,
      "unit": "MW"
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
  "outputs": {
    "fcr_available_capacity": {
      "value": null,
      "unit": "MW"
    },
    "fcr_power_setpoint": {
      "value": null,
      "unit": "MW"
    }
  },
  "kpis": {
    "dynamic_security_status": null,
    "fcr_availability": null,
    "frequency_containment_indicator": null,
    "number_of_rejected_dispatch_points": null,
    "synchronous_condenser_dispatch_count": null
  }
}
```

---

## HMI and Dashboard Requirements

The Demo 2 FCR dashboard should display:

- service request status;
- optimisation execution status;
- nominal frequency and measured / simulated frequency;
- frequency deviation;
- available FCR capacity;
- Li-ion battery SoC;
- hydro reservoir capacity;
- renewable generation and forecast values;
- candidate operating point;
- dynamic assessment result;
- N-1 validation status;
- synchronous-machine or condenser state;
- accepted operating point;
- number of optimisation iterations;
- service availability indicators.

Historical views should include:

- service requests;
- optimisation outputs;
- stability assessment outcomes;
- accepted and rejected operating points;
- synchronous condenser dispatch events;
- storage and hydro utilisation;
- FCR availability indicators.

---

## Implementation Status

The FCR service in Demo 2 was fully specified through the common ancillary-service optimisation workflow.

The implementation includes:

- API-triggered EEM / INESC TEC workflow;
- optimisation of a candidate operating point;
- dynamic assessment using the N-1 criterion;
- iterative re-dispatch of synchronous condensers if the operating point is not stable;
- FCR capability representation in the dispatch model;
- harmonised data model;
- repository-compatible JSON structure.

The final readiness level is **Simulated / HIL Validated**, reflecting the island-grid context and the technical validation of the FCR-related dynamic response rather than formal market-based FCR deployment.

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
| Final readiness level | Simulated / HIL Validated |
| Final repository service | Yes |

---

## Lessons Learned

The Demo 2 FCR implementation provides several lessons:

- FCR in isolated grids must be considered together with dynamic stability and system security;
- a technically feasible economic dispatch may not provide sufficient frequency-containment capability;
- N-1 dynamic assessment is essential before accepting the operating point;
- synchronous-machine or condenser functionality can be required to ensure a stable operating point;
- BESS and hydro resources should be coordinated to preserve enough headroom for FCR response;
- the same API and optimisation workflow can represent several ancillary services if their objectives are clearly separated;
- the absence of a formal market does not prevent technical validation of FCR-like functionality in an islanded grid;
- harmonised service representation supports future replication in weak-grid or island-grid contexts.

---
