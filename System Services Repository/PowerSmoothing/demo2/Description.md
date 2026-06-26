# Implementation Details of Power Smoothing in Demo 2

## Demo Context

Demo 2 corresponds to the i-STENTORE demonstrator **Pump Hydro Storage system combined with BESS – Portugal**.

The demonstrator is located in an islanded grid context and combines hydro-based flexibility, battery storage and optimisation routines to support internal system balancing and ancillary-service provision. Within this demonstrator, Power Smoothing is represented as a flexibility service aimed at reducing fluctuations or variations in electrical power output and supporting stable operation of the small island network.

The Power Smoothing process is based on an API-mediated interaction between EEM and INESC TEC. EEM initiates the service request through the API, the INESC TEC virtual machine fetches updated data from the EEM server, and the VRFB optimisation algorithm is executed. Once the optimisation is complete, results are stored both in the INESC TEC virtual machine and on the EEM server. EEM then controls the VRFB accordingly.

---

## Service Objective

The objective of the Power Smoothing service in Demo 2 is to reduce power-output fluctuations and support stable operation of the islanded electricity system.

The service aims to:

- reduce short-term variations in electrical power output;
- support stable operation of the island grid;
- improve the integration of renewable generation;
- use storage flexibility to smooth system power variations;
- provide EEM with optimised storage-control results;
- support coordinated dispatch between hydro, BESS and VRFB-related flexibility;
- provide a reusable Power Smoothing workflow compatible with the i-STENTORE service repository.

---

## Service Classification

| Field | Description |
|---|---|
| Service name | Power Smoothing |
| Demonstrator | Demo 2 – Portugal |
| Demonstrator title | Pump Hydro Storage system combined with BESS |
| Service family | Generation support / grid-support flexibility |
| Main objective | Reduction of power-output fluctuations |
| Main system context | Islanded grid |
| Main storage configuration | Pumped hydro storage combined with BESS and VRFB optimisation routines |
| Main requester / operator | EEM |
| Optimisation provider | INESC TEC |
| Main communication mechanism | API request and EEM server data exchange |
| Final readiness level | Field-tested / validated hydro + storage smoothing |

---

## Demonstrator Assets

### Main Assets

- Pumped hydro storage system;
- Utility-scale BESS;
- VRFB-related optimisation process;
- Thermal resources;
- Islanded grid assets;
- Renewable generation resources;
- EEM system infrastructure.

### Digital and Control Assets

- EEM API interface;
- EEM SFTP server;
- INESC TEC virtual machine;
- VRFB optimisation algorithm;
- Data storage on INESC TEC virtual machine;
- Data storage on EEM server;
- EEM control layer;
- HMI or dashboard representation;
- i-STENTORE semantic and service data model layer.

---

## Actors

| Actor | Role |
|---|---|
| EEM | Initiates the Power Smoothing request, provides system data and controls the VRFB according to the optimisation results. |
| INESC TEC | Hosts the virtual machine and optimisation algorithm used to calculate the Power Smoothing response. |
| EEM Server | Stores updated input data and receives optimisation results. |
| INESC TEC Virtual Machine | Fetches EEM data, runs the VRFB optimisation algorithm and stores results. |
| VRFB Optimisation Algorithm | Computes the storage operation required for the Power Smoothing service. |
| VRFB / Storage Controller | Executes the storage operation according to the optimisation results. |
| Island Grid Operator | Benefits from smoother system operation and improved balancing. |
| HMI / Dashboard | Displays service outputs, storage state and performance indicators. |
| i-STENTORE Middleware | Supports harmonised representation of the service and related data. |

---

## Operational Principle

The Power Smoothing service is initiated by EEM through an API request.

The INESC TEC virtual machine then collects updated data from the EEM server and runs the VRFB optimisation algorithm. The optimisation determines how the storage resource should be operated to reduce variations in power output and support a smoother system profile.

Once the optimisation is complete, the results are stored in both the INESC TEC virtual machine and the EEM server. EEM uses these results to control the VRFB accordingly.

---

## Power Smoothing Logic

The service is designed to smooth fluctuations in the island grid by using storage as a controllable buffer.

The storage system can:

- charge when system generation exceeds the desired smoothed profile;
- discharge when generation or supply falls below the desired smoothed profile;
- reduce the impact of renewable-generation variability;
- contribute to a smoother power exchange profile;
- support the island grid across different operating conditions.

The optimisation must consider the operating limits of the storage resource and the broader island-grid constraints.

---

## Timing Requirements

| Parameter | Value / Description |
|---|---|
| Planning horizon | Operational or optimisation horizon defined by EEM / INESC TEC workflow |
| Reaction time | Milliseconds in the conceptual service characterisation |
| Frequency of use | Several times per day, depending on operating conditions |
| Discharge time | Depends on the HESS component providing the service |
| Main unit of service | MW |
| Application scale | Small island network at 60 kV, 30 kV and 6.6 kV levels |
| Business case | Day-ahead congestion planning / internal system support |
| Remuneration | Not market-based; service deployed by EEM in an isolated grid context |

---

## Information and Data Flows

### 1. Service Request

EEM initiates the Power Smoothing process through the API.

The request indicates that the optimisation process should be executed and that updated system data should be used.

### 2. Data Retrieval

The INESC TEC virtual machine fetches updated data from the EEM server.

The data may include:

- system generation data;
- storage status;
- island-grid operating data;
- renewable generation information;
- demand or load information;
- relevant constraints for the optimisation process.

### 3. Optimisation

The VRFB optimisation algorithm is executed on the INESC TEC virtual machine.

The algorithm determines:

- storage power setpoints;
- charge/discharge actions;
- smoothing profile;
- operational feasibility;
- storage schedule or dispatch recommendation.

### 4. Result Storage

After the optimisation is completed, results are stored in:

- INESC TEC virtual machine;
- EEM server.

### 5. Control Action

EEM controls the VRFB according to the optimisation results.

The control action is expected to reduce power fluctuations and support smoother island-grid operation.

### 6. Monitoring and Reporting

The system stores and displays:

- service request status;
- input data used for optimisation;
- calculated storage schedule;
- executed storage action;
- smoothed power profile;
- service KPIs.

---

## Implementation Workflow

```text
1. EEM initiates a Power Smoothing request through the API.
   |
2. INESC TEC virtual machine receives the request.
   |
3. INESC TEC virtual machine fetches updated data from the EEM server.
   |
   |-- System generation data
   |-- Storage status
   |-- Island-grid operating data
   |-- Renewable generation information
   |-- Demand or load information
   |
4. The VRFB optimisation algorithm is executed.
   |
   |-- Analyse power fluctuations
   |-- Determine required smoothing action
   |-- Compute storage charge/discharge setpoints
   |-- Check storage and system constraints
   |
5. Results are stored on the INESC TEC virtual machine.
   |
6. Results are also stored on the EEM server.
   |
7. EEM controls the VRFB according to the optimisation results.
   |
8. Results are displayed and stored for performance assessment.
```

---

## Sequence Diagram

```mermaid
sequenceDiagram
    participant EEM as EEM
    participant API as API
    participant Server as EEM Server / SFTP
    participant VM as INESC TEC Virtual Machine
    participant OPT as VRFB Optimisation Algorithm
    participant VRFB as VRFB / Storage Controller
    participant UI as HMI / Dashboard
    participant MW as i-STENTORE Middleware

    EEM->>API: Request Power Smoothing optimisation
    API->>VM: Trigger optimisation workflow

    VM->>Server: Fetch updated system and storage data
    Server-->>VM: Provide updated data

    VM->>OPT: Run VRFB optimisation algorithm
    OPT-->>VM: Return smoothing setpoints and schedule

    VM->>VM: Store optimisation results
    VM->>Server: Store results on EEM server

    EEM->>Server: Retrieve optimisation results
    EEM->>VRFB: Control VRFB according to results

    VRFB-->>EEM: Return execution status
    EEM->>UI: Display smoothing results and KPIs
    EEM->>MW: Share harmonised service information where applicable
```

---

## Data Inputs

### API and Request Inputs

| Input | Description | Unit |
|---|---|---|
| service_request | EEM request to run the Power Smoothing optimisation. | n/a |
| request_timestamp | Time of the service request. | datetime |
| optimisation_horizon | Time horizon for the optimisation process. | h or implementation-specific |
| timestep | Time resolution of the optimisation. | min / h |
| requester_id | Identifier of EEM or the requesting service entity. | n/a |

### Island-Grid Inputs

| Input | Description | Unit |
|---|---|---|
| system_generation | Aggregated generation in the islanded grid. | kW / MW |
| renewable_generation | Renewable generation contributing to system variability. | kW / MW |
| system_load | Current or forecasted demand. | kW / MW |
| grid_operating_state | Current island-grid operating condition. | status |
| frequency | System frequency, where relevant. | Hz |
| voltage | Grid voltage, where relevant. | V / kV |

### Storage Inputs

| Input | Description | Unit |
|---|---|---|
| vrfb_state_of_charge | Current state of charge of the VRFB or storage representation. | % |
| vrfb_power_limit | Maximum charge/discharge power. | kW / MW |
| vrfb_energy_capacity | Available energy capacity. | kWh / MWh |
| storage_availability | Availability status of the storage resource. | Boolean / status |
| storage_operating_constraints | Additional storage operation constraints. | n/a |

---

## Service Outputs

| Output | Description | Unit |
|---|---|---|
| smoothing_schedule | Planned storage operation for Power Smoothing. | time series |
| vrfb_power_setpoint | Storage active power setpoint. | kW / MW |
| charge_discharge_profile | Planned charging and discharging profile. | kW / MW |
| smoothed_power_profile | Expected smoothed system power profile. | kW / MW |
| fluctuation_reduction | Reduction in power-output fluctuations. | % |
| optimisation_status | Status of optimisation completion. | status |
| execution_status | Status of EEM control action. | status |
| service_kpis | Power Smoothing performance indicators. | n/a |

---

## Optimisation Logic

The Power Smoothing optimisation can be represented as a storage-dispatch problem.

```text
Minimise:
    Power-output fluctuation
    + deviation from target smoothed profile
    + storage operating penalties
```

Subject to:

```text
Storage constraints:
    SoC_min <= SoC_t <= SoC_max
    P_charge_t <= P_charge_max
    P_discharge_t <= P_discharge_max
    Storage availability is respected

Island-grid constraints:
    System operating limits are respected
    Generation and demand balance is supported
    Voltage and frequency constraints are considered where applicable

Service constraints:
    Smoothing response follows the optimisation horizon and timestep
    Charge/discharge decisions reduce power variation
    Results are stored and made available to EEM
```

---

## Suggested JSON Structure

```json
{
  "service": "PowerSmoothing",
  "demo": "Demo2",
  "metadata": {
    "service_id": "demo2_power_smoothing",
    "requester_id": "EEM",
    "timestamp": "YYYY-MM-DDTHH:mm:ssZ"
  },
  "request": {
    "optimisation_horizon": {
      "value": null,
      "unit": "h"
    },
    "timestep": {
      "value": null,
      "unit": "min"
    },
    "status": "requested"
  },
  "islandGrid": {
    "system_generation": {
      "value": null,
      "unit": "kW"
    },
    "renewable_generation": {
      "value": null,
      "unit": "kW"
    },
    "system_load": {
      "value": null,
      "unit": "kW"
    },
    "frequency": {
      "value": null,
      "unit": "Hz"
    },
    "voltage": {
      "value": null,
      "unit": "kV"
    }
  },
  "storage": {
    "vrfb": {
      "state_of_charge": {
        "value": null,
        "unit": "%"
      },
      "power_limit": {
        "value": null,
        "unit": "kW"
      },
      "energy_capacity": {
        "value": null,
        "unit": "kWh"
      },
      "availability": true
    }
  },
  "outputs": {
    "smoothing_schedule": [],
    "vrfb_power_setpoint": [],
    "charge_discharge_profile": [],
    "smoothed_power_profile": [],
    "fluctuation_reduction": {
      "value": null,
      "unit": "%"
    }
  },
  "kpis": {
    "power_fluctuation_reduction": null,
    "smoothing_error": null,
    "storage_utilisation": null,
    "schedule_deviation": null
  }
}
```

---

## HMI and Dashboard Requirements

The Demo 2 Power Smoothing dashboard should display:

- EEM service request status;
- optimisation execution status;
- storage state of charge;
- raw power-output profile;
- expected smoothed power-output profile;
- VRFB charge/discharge setpoints;
- optimisation results stored on the EEM server;
- fluctuation reduction indicator;
- storage utilisation;
- schedule deviation;
- historical optimisation runs.

Because Demo 2 data may be aggregated or model-based rather than high-frequency live data, the dashboard should clearly distinguish:

- live measurements, where available;
- batch data;
- optimisation results;
- simulated or aggregated values.

---

## Implementation Status

The Power Smoothing service in Demo 2 is represented as a storage-optimisation service initiated by EEM and executed by INESC TEC.

The final readiness status is associated with hydro and storage smoothing in an island-grid context. The service workflow, API-triggered optimisation, data exchange and result-storage process are fully defined in the repository.

| Criterion | Status |
|---|---|
| Conceptual service definition | Completed |
| Service workflow | Completed |
| Sequence diagram | Completed |
| API-mediated request process | Completed |
| VRFB optimisation workflow | Completed |
| EEM / INESC TEC data-exchange workflow | Completed |
| Field validation of hydro + storage smoothing | Field-tested / validated |
| Final repository service | Yes |

---

## Lessons Learned

The Demo 2 Power Smoothing implementation provides several lessons:

- Power Smoothing in an islanded grid requires close coordination between system operation and storage dispatch;
- an API-triggered optimisation process is suitable for recurring storage-control services;
- separation between EEM data storage and INESC TEC optimisation execution provides a clear operational workflow;
- storage smoothing can support system stability even when implemented through batch or optimisation-based processes;
- the islanded grid context creates a strong need for internal flexibility and smoothing services;
- HMI design should clearly distinguish real-time data from aggregated or model-based optimisation results;
- the service model can be reused for future island-grid or weak-grid storage applications.

---

