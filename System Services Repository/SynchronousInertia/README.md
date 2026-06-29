# Synchronous Inertia

Synchronous Inertia is a grid-stability and ancillary flexibility service that supports power-system frequency stability by resisting rapid changes in frequency after a disturbance.

In traditional power systems, inertia is naturally provided by the rotating masses of synchronous generators, such as thermal or hydroelectric power plants. As inverter-based renewable generation increases, the amount of physical synchronous inertia available in the power system decreases. Energy storage systems and hybrid energy storage systems can support the grid by mimicking or supplementing this behaviour through fast control actions. This is often described as **synthetic inertia** or **virtual inertia**.

In the i-STENTORE Service Repository, Synchronous Inertia is represented through:

- Demo 2 – Pumped Hydro Storage combined with BESS, Portugal.

---

## Service Category

Synchronous Inertia belongs to the following service families:

- Flexibility Services;
- Balancing Services;
- Ancillary Services;
- Frequency Stability Services;
- Fast Dynamic Grid-Support Services;
- Hybrid Storage Services.

---

## Purpose of the Service

The purpose of Synchronous Inertia is to support the stability of the electrical system by reducing the rate of change of frequency after a disturbance.

The service supports:

- frequency stability;
- balancing between supply and demand;
- mitigation of rapid frequency deviations;
- improved grid resilience during disturbances;
- support for isolated or weak grids;
- higher renewable energy penetration;
- replacement or supplementation of inertia traditionally provided by synchronous machines.

---

## General Description

Synchronous Inertia is a fundamental property of conventional power systems. Large rotating generators store kinetic energy in their rotating masses. When a disturbance occurs, this stored energy helps slow down the change in system frequency.

Energy storage systems do not provide physical synchronous inertia in the same way as rotating machines. However, through power-electronic converters and fast control algorithms, they can respond to frequency deviations by injecting or absorbing active power. This behaviour can emulate or supplement synchronous inertia and help stabilise the system.

In i-STENTORE, the service is especially relevant for Demo 2 because the demonstrator combines hydro-based flexibility, Li-ion battery storage and island-grid operation. The islanded nature of the system increases the importance of inertia-related services, since frequency stability must be maintained without relying on a large interconnected power system.

---

## Value Proposition

| Value Stream | Description |
|---|---|
| Frequency Stability | Reduces the rate of change of frequency after disturbances. |
| System Security | Supports stable operation during sudden changes in supply or demand. |
| Renewable Integration | Compensates for reduced physical inertia caused by inverter-based generation. |
| Island Grid Support | Provides additional stability in isolated or weak-grid contexts. |
| Hybrid Storage Coordination | Combines hydro, battery and converter-based control capabilities. |
| Service Stacking | Can be coordinated with FCR, FFR and other frequency-response services. |

---

## Technical Principle

The service relies on detecting frequency deviations or dynamic system conditions and dispatching storage or synchronous-machine resources accordingly.

The generic process is:

1. Monitor system frequency and operating conditions.
2. Detect a frequency deviation or system disturbance.
3. Determine whether the response should be provided by synchronous condenser functionality, hydro resources, battery storage or a hybrid combination.
4. Compute the optimal dispatch point from both economic and dynamic perspectives.
5. Validate the dispatch through a dynamic security assessment, including the N-1 criterion.
6. If the dispatch is not dynamically safe, adjust the synchronous-machine or storage dispatch and repeat the assessment.
7. Apply the stable dispatch point to the generation and storage technologies.
8. Monitor the resulting system stability.

---

## Typical Time Horizon

| Parameter | Typical Value / Description |
|---|---|
| Planning horizon | Day-ahead or operational dispatch horizon |
| Dynamic response | Milliseconds to seconds |
| Service duration | Depends on the technology providing the service |
| Frequency of use | Several times per day, depending on system conditions |
| Main service quantity | MW and dynamic frequency support |
| Main operational trigger | Frequency deviation or dynamic-security requirement |
| Main validation criterion | Dynamic stability and N-1 security |

---

## Typical Actors

| Actor | Role |
|---|---|
| System Operator / DSO | Defines system operating requirements and benefits from inertia support. |
| EEM | Operates the island grid and initiates the optimisation workflow in Demo 2. |
| INESC TEC | Hosts the optimisation and dynamic assessment workflow in Demo 2. |
| Energy Management System | Coordinates dispatch and service execution. |
| Optimisation Algorithm | Defines an economically optimal and dynamically viable dispatch. |
| Dynamic Assessment Procedure | Checks whether the system remains stable under the N-1 criterion. |
| Synchronous Condenser / Hydro Unit | Provides rotating-machine or hydro-based support where available. |
| Li-ion Battery | Provides fast converter-based support. |
| HMI / Dashboard | Displays dispatch, stability status and KPIs. |
| i-STENTORE Middleware | Supports harmonised data exchange and semantic representation. |

---

## Demonstrator Implementation

| Demonstrator | Implementation Focus | Repository File |
|---|---|---|
| Demo 2 | Synchronous / synthetic inertia support through hydro, Li-ion battery and optimisation-based dispatch in an island-grid context. | `SynchronousInertia/demo2/Description.md` |

---

## Demo 2 Implementation Summary

In Demo 2, Synchronous Inertia is one of the ancillary services provided through a common optimisation process together with FFR, FCR and aFRR.

The process is initiated by EEM through an API request. The INESC TEC virtual machine fetches updated data from the EEM server and runs the optimisation algorithm. The objective is to define a general technology dispatch that is economically optimal and dynamically viable.

After the optimisation, the result is assessed through a dynamic procedure that includes the N-1 criterion. If the calculated point of operation is safe and stable, the results are stored and EEM sets its generation technologies to the defined point of operation. If the point is not safe, synchronous condensers are dispatched and the optimisation is repeated until a stable operating point is achieved.

---

## Typical Inputs

Synchronous Inertia requires information about storage status, renewable generation, hydro resources, costs and system operating conditions.

### Metadata and Scheduling Inputs

| Input | Description | Unit |
|---|---|---|
| service_id | Identifier of the service instance. | n/a |
| requester_id | Identifier of the requester. | n/a |
| timestamp | Timestamp of the request. | datetime |
| timestep | Optimisation timestep. | n/a |
| horizon | Optimisation horizon. | n/a |

### Storage and Hydro Inputs

| Input | Description | Unit |
|---|---|---|
| maximum_soc_lib | Maximum Li-ion battery state of charge. | % |
| maximum_soc_vrfb | Maximum VRFB state of charge, where applicable. | % |
| maximum_reservoir_capacity | Maximum reservoir capacity of the hydro power plant. | % |
| charging_efficiency_lib | Charging efficiency of the Li-ion battery. | % |
| charging_efficiency_vrfb | Charging efficiency of the VRFB, where applicable. | % |
| lib_soc | Li-ion battery state of charge. | kWh |
| vrfb_soc | VRFB state of charge, where applicable. | kWh |
| hpp_reservoir_capacity | Hydro reservoir capacity. | m³ |

### Generation and Forecast Inputs

| Input | Description | Unit |
|---|---|---|
| wind_generation | Wind power generation. | kWh |
| hydro_generation | Hydro power generation. | kWh |
| pv_generation | PV power generation. | kWh |
| wind_generation_forecast | Forecasted wind generation. | kWh |
| hydro_generation_forecast | Forecasted hydro generation. | kWh |
| pv_generation_forecast | Forecasted PV generation. | kWh |
| generation_cost | Cost associated with renewable generation. | €/kWh |
| charging_cost | Cost associated with charging storage resources. | €/kWh |
| pumping_hydro_cost | Cost associated with pumped hydro operation. | €/kWh |

---

## Typical Outputs

| Output | Description | Unit |
|---|---|
| lib_converter_power | Converter power of the Li-ion battery. | MW |
| vrfb_converter_power | Converter power of the VRFB, where applicable. | MW |
| hydro_power | Hydro power output. | MW |
| wind_power | Wind power output. | MW |
| pv_power | PV power output. | MW |
| synchronous_machine_state | State of the synchronous machine / condenser functionality. | Boolean / status |
| hess_operational_schedule | Operational schedule for the hybrid energy storage system. | time series |
| dynamic_security_status | Result of the dynamic stability check. | status |
| stable_dispatch_point | Final dispatch point accepted after dynamic validation. | n/a |

---

## Generic Implementation Workflow

```text
1. EEM initiates the service through an API request.
   |
2. INESC TEC virtual machine fetches updated data from the EEM server.
   |
   |-- Renewable generation data
   |-- Forecast data
   |-- Li-ion battery SoC
   |-- Hydro reservoir capacity
   |-- Cost parameters
   |-- System operating conditions
   |
3. The optimisation algorithm defines a dispatch point.
   |
   |-- Economically optimal
   |-- Technically feasible
   |-- Compatible with available storage and hydro resources
   |
4. A dynamic assessment procedure evaluates the dispatch.
   |
   |-- N-1 criterion
   |-- Frequency-stability behaviour
   |-- Safe and stable point of operation
   |
5. If the operating point is stable:
   |
   |-- Store results on EEM server
   |-- Store results on INESC TEC virtual machine
   |-- EEM sets generation technologies to the defined point
   |
6. If the operating point is not stable:
   |
   |-- Dispatch synchronous condensers
   |-- Repeat optimisation and dynamic assessment
   |
7. Monitor operation and store results for service assessment.
```

---

## Service Interaction

Synchronous Inertia interacts strongly with other frequency-related services.

| Related Service | Interaction |
|---|---|
| FFR | Both respond to fast frequency deviations; FFR is usually an explicit fast active-power service. |
| FCR | FCR contains frequency deviations after the initial inertial response. |
| aFRR | aFRR restores frequency and balances the system after FCR activation. |
| Power Smoothing | Smoothing reduces rapid renewable fluctuations that may contribute to frequency deviations. |
| Energy Arbitrage | Market or cost optimisation must preserve enough dynamic flexibility for stability services. |

In Demo 2, Synchronous Inertia, FFR, FCR and aFRR are handled by a shared optimisation and dynamic assessment workflow.

---

## Data Model Orientation

The Synchronous Inertia data model should distinguish between:

- metadata;
- scheduling parameters;
- storage limits and states;
- hydro reservoir parameters;
- renewable generation and forecasts;
- cost parameters;
- optimisation outputs;
- dynamic-security outputs;
- synchronous-machine state;
- service KPIs.

The data model should support:

- explicit power, energy and reservoir units;
- time-series representation of generation and forecasts;
- representation of synchronous-machine state;
- JSON-based information exchange;
- OpenAPI-compatible request and response structures;
- alignment with the i-STENTORE ontology;
- future integration with NGSI-LD structures.

---

## Suggested Data Model Structure

```text
SynchronousInertiaService

├── Metadata
│   ├── service_id
│   ├── requester_id
│   └── timestamp
│
├── Scheduling
│   ├── timestep
│   └── horizon
│
├── StorageAndHydroParameters
│   ├── maximum_soc_lib
│   ├── maximum_soc_vrfb
│   ├── maximum_reservoir_capacity
│   ├── charging_efficiency_lib
│   ├── charging_efficiency_vrfb
│   ├── lib_soc
│   ├── vrfb_soc
│   └── hpp_reservoir_capacity
│
├── GenerationAndForecasts
│   ├── wind_generation
│   ├── hydro_generation
│   ├── pv_generation
│   ├── wind_generation_forecast
│   ├── hydro_generation_forecast
│   └── pv_generation_forecast
│
├── Costs
│   ├── generation_cost
│   ├── lib_charging_cost
│   ├── vrfb_charging_cost
│   └── pumped_hydro_cost
│
├── Outputs
│   ├── lib_converter_power
│   ├── vrfb_converter_power
│   ├── hydro_power
│   ├── wind_power
│   ├── pv_power
│   ├── synchronous_machine_state
│   └── hess_operational_schedule
│
└── KPIs
    ├── dynamic_security_status
    ├── frequency_stability_indicator
    ├── stable_dispatch_status
    └── service_availability
```

---

## Suggested JSON Skeleton

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
    "timestep": 15.0,
    "horizon": 24
  },
  "ess": {
    "lib": {
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
      }
    },
    "hpp": {
      "maximum_reservoir_capacity": {
        "value": null,
        "unit": "%"
      },
      "pumping_hydro_cost": {
        "values": [],
        "unit": "€/kWh"
      },
      "reservoir_capacity": {
        "values": [],
        "unit": "m3"
      }
    }
  },
  "res": {
    "generation_cost": {
      "values": [],
      "unit": "€/kWh"
    },
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
    }
  },
  "outputs": {
    "lib_converter_power": {
      "values": [],
      "unit": "MW"
    },
    "vrfb_converter_power": {
      "values": [],
      "unit": "MW"
    },
    "hydro_power": {
      "values": [],
      "unit": "MW"
    },
    "wind_power": {
      "values": [],
      "unit": "MW"
    },
    "pv_power": {
      "values": [],
      "unit": "MW"
    },
    "synchronous_machine_state": {
      "values": [],
      "unit": "status"
    },
    "hess_operational_schedule": []
  },
  "kpis": {
    "dynamic_security_status": null,
    "frequency_stability_indicator": null,
    "stable_dispatch_status": null,
    "service_availability": null
  }
}
```

---

## Implementation Requirements

A Synchronous Inertia implementation should include:

- system-frequency monitoring or dynamic-security assessment;
- available hydro, storage and synchronous-machine data;
- state-of-charge and reservoir-capacity monitoring;
- optimisation of dispatch points;
- dynamic validation using stability criteria;
- fallback logic if the first dispatch point is not stable;
- data storage in both operator and optimisation environments;
- HMI representation of dispatch and stability status;
- performance indicators linked to dynamic security.

---

## HMI and Dashboard Requirements

The Synchronous Inertia dashboard should display:

- service request status;
- current system operating state;
- Li-ion battery SoC;
- hydro reservoir capacity;
- renewable generation and forecasts;
- calculated dispatch point;
- synchronous machine / condenser state;
- dynamic-security assessment result;
- stable or unstable operating-point flag;
- final accepted dispatch;
- historical service runs and stability outcomes.

Historical views should include:

- optimisation runs;
- rejected and accepted dispatch points;
- synchronous condenser dispatch events;
- frequency-stability indicators;
- storage and hydro utilisation;
- service availability.

---

## Implementation Status

In the final i-STENTORE Service Repository, Synchronous Inertia is represented as a Demo 2 ancillary service.

The service was specified with:

- a generic service definition;
- Demo 2 implementation workflow;
- shared optimisation process with FFR, FCR and aFRR;
- dynamic N-1 assessment;
- harmonised data model;
- JSON/API-oriented request structure.

The final readiness assessment classifies Demo 2 Synchronous / Synthetic Inertia as **Simulated / HIL Validated**, reflecting detailed inertia modelling and dynamic validation rather than full market-based or operational field deployment.

| Criterion | Status |
|---|---|
| Conceptual service definition | Completed |
| Service workflow | Completed |
| Data model | Completed |
| API-oriented structure | Completed |
| Dynamic-security assessment | Included |
| N-1 criterion | Included |
| Demo-specific implementation | Demo 2 |

---

## Lessons Learned

The Synchronous Inertia service shows how hybrid energy storage can contribute to frequency stability in systems with increasing renewable penetration.

Key lessons include:

- storage does not provide physical inertia but can emulate inertial response through fast converter control;
- isolated and weak grids have a particularly strong need for inertia-related support;
- hydro, synchronous condenser functionality and batteries can be coordinated for dynamic stability;
- economic dispatch alone is not sufficient; dynamic-security validation is required;
- N-1 assessment is essential when defining safe operating points;
- the service is strongly linked to FFR, FCR and aFRR;
- market or certification frameworks may limit field deployment even where technical modelling is mature;
- the harmonised data model remains reusable for future mainland or island-grid applications.

---

## Files in this Directory

```text
SynchronousInertia/

├── README.md
└── demo2/
    └── Description.md
```

---
