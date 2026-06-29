# Power Smoothing

Power Smoothing is a renewable-generation support and grid-stability flexibility service based on the use of storage or hybrid energy storage systems to reduce short-term power fluctuations caused by variable renewable energy sources.

The service is particularly relevant for non-dispatchable renewable generation, such as photovoltaic and wind power, where output power can change rapidly due to weather conditions, solar irradiation variability, cloud transients or local load dynamics.

In the i-STENTORE Service Repository, Power Smoothing is represented through:

- Demo 2 – Pumped Hydro Storage combined with BESS, Portugal;
- Demo 4 – Hybrid SuperCap / Li-NMC HESS for Smart DC Microgrids and e-mobility, Italy.

---

## Service Category

Power Smoothing belongs to the following service families:

- Generation Support Services;
- Renewable Energy Support Services;
- Power Quality Services;
- Grid Support Services;
- Hybrid Storage Services;
- Fast Local Flexibility Services.

---

## Purpose of the Service

The purpose of Power Smoothing is to reduce short-term fluctuations in active power and, where relevant, voltage caused by variable generation or local consumption.

The service supports:

- smoother renewable generation output;
- reduced grid stress caused by rapid power variations;
- improved local power quality;
- increased hosting capacity for renewable generation;
- reduced wear and stress on grid-connected equipment;
- better coordination between renewable generation, storage and local demand;
- improved lifetime of technologies connected to variable renewable sources.

---

## General Description

Power Smoothing uses storage as an intermediate buffer between a variable generation source and the grid or local microgrid.

When renewable generation suddenly increases, the storage system can absorb part of the surplus power. When renewable generation suddenly decreases, the storage system can inject power to compensate for the drop. In this way, the output power seen by the grid or local system becomes smoother than the raw renewable generation profile.

The same concept can be implemented using different storage technologies:

- batteries for energy buffering over minutes to hours;
- supercapacitors for very fast short-duration fluctuations;
- pumped hydro or hydro-based storage for larger energy shifts;
- hybrid storage systems combining high-power and high-energy capabilities.

---

## Value Proposition

| Value Stream | Description |
|---|---|
| Renewable Integration | Reduces the variability seen by the grid and enables higher renewable penetration. |
| Power Quality Improvement | Smooths active power and supports voltage stability. |
| Asset Lifetime Improvement | Reduces stress on converters, storage systems and grid-connected equipment. |
| Grid Stability | Reduces rapid power-flow variations that may affect local grid operation. |
| Hybrid Storage Optimisation | Allocates fast and slow fluctuations to the most suitable storage technology. |
| Service Stacking | Can be coordinated with congestion management, frequency response or local EMS objectives. |

---

## Technical Principle

The service uses measured or forecasted renewable generation and local power-flow data to determine a smoothing setpoint.

The typical process is:

1. Measure renewable generation output and local power flow.
2. Identify rapid fluctuations or deviations from a target smoothed profile.
3. Calculate the compensation power required from storage.
4. Allocate the compensation signal to the appropriate storage component.
5. Dispatch charge or discharge setpoints.
6. Monitor the resulting grid or microgrid power output.
7. Calculate smoothing performance and update the control strategy.

---

## Typical Time Horizon

| Parameter | Typical Value / Description |
|---|---|
| Planning horizon | Real-time to intra-day, depending on the implementation |
| Response time | Immediate or fast response for short-term smoothing |
| Duration | Seconds to hours, depending on storage technology |
| Main service quantity | Active power and voltage smoothing |
| Main unit of measurement | kW, MW, V |
| Activation trigger | Renewable generation fluctuation, local power-flow variation or EMS request |

---

## Typical Actors

| Actor | Role |
|---|---|
| Renewable Plant Operator | Provides PV, wind or hydro generation data. |
| Energy Management System | Coordinates smoothing logic and dispatch decisions. |
| Storage Controller | Executes charging or discharging setpoints. |
| HESS Controller | Allocates response between different storage technologies. |
| Forecasting Service | Provides renewable generation and weather forecasts where used. |
| DSO / Aggregator | May benefit from smoother grid injection or request smoothing-related support. |
| SCADA System | Monitors generation, storage and grid variables. |
| HMI / Dashboard | Displays power profiles, storage status and service KPIs. |
| i-STENTORE Middleware | Supports harmonised information exchange and semantic representation. |

---

## Demonstrator Implementations

Power Smoothing is represented in the repository through two demo-specific implementations.

| Demonstrator | Implementation Focus | Repository File |
|---|---|---|
| Demo 2 | Power smoothing using the VRFB optimisation process within the pumped hydro and BESS island-grid demonstrator. | `PowerSmoothing/demo2/Description.md` |
| Demo 4 | Power smoothing using Hybrid SuperCap / Li-NMC HESS connected to PV, EV charging and a smart DC microgrid. | `PowerSmoothing/demo4/Description.md` |

---

## Demo 2 Implementation Summary

In Demo 2, Power Smoothing is associated with a pumped hydro storage system combined with battery storage.

The process is initiated by EEM through an API request. The INESC TEC virtual machine fetches updated data from the EEM server and runs the VRFB optimisation algorithm. Results are stored in both the INESC TEC virtual machine and the EEM server. EEM then controls the VRFB according to the optimisation results.

The service focuses on smoothing renewable or system power variations in an islanded grid context.

---

## Demo 4 Implementation Summary

In Demo 4, Power Smoothing is provided by the Hybrid Energy Storage System connected to the PV plant and smart DC microgrid.

The HESS absorbs or injects power to reduce the fluctuations generated by the photovoltaic plant due to solar irradiation and weather variability. The service aims to smooth both output power and voltage for the electricity grid and Demo 4 technologies.

The hybrid configuration is particularly relevant because:

- the supercapacitor can address fast short-duration fluctuations;
- the Li-NMC battery can support longer-duration energy balancing;
- the EMS can coordinate the two storage technologies with EV charging and PV generation.

---

## Typical Inputs

Power Smoothing requires renewable generation measurements, storage status and local grid or microgrid variables.

### Renewable and Grid Inputs

| Input | Description | Unit |
|---|---|---|
| renewable_generation | Current renewable generation output. | kW / MW |
| renewable_generation_forecast | Forecasted renewable generation. | kW / MW |
| pv_output_power | PV plant active power. | kW / MW |
| pv_output_voltage | PV plant or point-of-connection voltage. | V |
| grid_power_flow | Power exchanged with the grid or microgrid. | kW / MW |
| microgrid_voltage | Local microgrid voltage. | V |
| weather_forecast | Weather conditions used for smoothing prediction. | n/a |

### Storage Inputs

| Input | Description | Unit |
|---|---|---|
| storage_state_of_charge | Current storage SoC. | % |
| storage_power_limit | Maximum charge/discharge power. | kW / MW |
| storage_energy_capacity | Available storage energy capacity. | kWh / MWh |
| hess_availability | Availability status of the HESS. | Boolean / status |
| battery_state_of_charge | Li-NMC battery SoC, where applicable. | % |
| supercapacitor_state_of_charge | Supercapacitor SoC, where applicable. | % |
| vrfb_state_of_charge | VRFB SoC, where applicable. | % |

### Demo-Specific Inputs

| Demonstrator | Specific Inputs |
|---|---|
| Demo 2 | EEM API request, EEM server data, VRFB optimisation inputs, island-grid operating data. |
| Demo 4 | PV forecast, EV charging demand, HESS status, battery SoC, supercapacitor SoC, weather forecast and DC microgrid measurements. |

---

## Typical Outputs

| Output | Description | Unit |
|---|---|---|
| smoothing_setpoint | Storage power setpoint used to smooth renewable output. | kW / MW |
| battery_setpoint | Battery charge/discharge setpoint. | kW / MW |
| supercapacitor_setpoint | Supercapacitor setpoint for fast fluctuations. | kW / MW |
| vrfb_setpoint | VRFB setpoint, where applicable. | kW / MW |
| smoothed_power_output | Resulting smoothed power profile. | kW / MW |
| voltage_smoothing_indicator | Indicator of voltage variation reduction. | V or % |
| power_fluctuation_reduction | Reduction of power variability. | % |
| service_status | Activation, simulation or execution status. | n/a |
| service_kpis | Performance indicators for smoothing performance. | n/a |

---

## Generic Implementation Workflow

```text
1. Data Acquisition
   |
   |-- Collect renewable generation measurements
   |-- Collect PV output power and voltage
   |-- Collect grid or microgrid power-flow data
   |-- Collect storage SoC and availability
   |-- Collect weather and generation forecasts where required
   |
2. Fluctuation Detection
   |
   |-- Detect rapid changes in renewable output
   |-- Compare measured output with target smoothed profile
   |-- Estimate required compensation power
   |
3. Storage Allocation
   |
   |-- Allocate fast fluctuations to high-power storage
   |-- Allocate longer fluctuations to high-energy storage
   |-- Respect SoC and power limits
   |
4. Dispatch
   |
   |-- Send charge/discharge setpoints
   |-- Coordinate battery, supercapacitor, VRFB or HESS components
   |
5. Monitoring
   |
   |-- Measure resulting grid or microgrid output
   |-- Compare raw and smoothed power profiles
   |-- Monitor storage state
   |
6. Reporting
   |
   |-- Store service results
   |-- Calculate smoothing KPIs
   |-- Display results on the HMI
```

---

## Service Interaction

Power Smoothing may interact with other services when the same storage assets are used for multiple functions.

| Related Service | Interaction |
|---|---|
| Congestion Management | Smoothing may reduce local fluctuations that contribute to congestion. |
| Energy Arbitrage | Energy arbitrage schedules may limit available SoC for smoothing. |
| FCR / FFR | Fast frequency response may compete with power smoothing for high-power storage capacity. |
| Peak Shaving | Local peak reduction may use the same battery energy capacity. |
| EV Charging Optimisation | In Demo 4, EV charging demand affects the available HESS flexibility. |

Service stacking requires allocation rules to ensure that short-term smoothing does not conflict with higher-priority grid or market services.

---

## Data Model Orientation

The Power Smoothing data model should distinguish between:

- metadata;
- renewable generation measurements;
- renewable generation forecasts;
- grid or microgrid measurements;
- storage states;
- smoothing setpoints;
- storage allocation outputs;
- service KPIs.

The service should be compatible with the i-STENTORE semantic and API framework.

The data model should therefore support:

- explicit power and voltage units;
- high-frequency or near-real-time time-series data;
- representation of hybrid storage components;
- alignment with the i-STENTORE ontology;
- JSON-based information exchange;
- OpenAPI-compatible interfaces;
- future integration with NGSI-LD structures.

---

## Suggested Data Model Structure

```text
PowerSmoothingService

├── Metadata
│   ├── service_id
│   ├── requester_id
│   └── timestamp
│
├── RenewableGeneration
│   ├── measured_power
│   ├── measured_voltage
│   ├── generation_forecast
│   └── weather_forecast
│
├── GridOrMicrogridState
│   ├── grid_power_flow
│   ├── microgrid_voltage
│   ├── point_of_connection_power
│   └── fluctuation_indicator
│
├── StorageState
│   ├── storage_state_of_charge
│   ├── battery_state_of_charge
│   ├── supercapacitor_state_of_charge
│   ├── vrfb_state_of_charge
│   ├── storage_power_limits
│   └── availability
│
├── Outputs
│   ├── smoothing_setpoint
│   ├── battery_setpoint
│   ├── supercapacitor_setpoint
│   ├── vrfb_setpoint
│   └── smoothed_power_output
│
└── KPIs
    ├── power_fluctuation_reduction
    ├── voltage_fluctuation_reduction
    ├── smoothing_error
    ├── storage_utilisation
    └── schedule_deviation
```

---

## Suggested JSON Skeleton

```json
{
  "service": "PowerSmoothing",
  "demo": "DemoX",
  "metadata": {
    "service_id": "example_power_smoothing_service",
    "requester_id": "ems_or_service_platform",
    "timestamp": "YYYY-MM-DDTHH:mm:ssZ"
  },
  "renewableGeneration": {
    "measuredPower": {
      "value": null,
      "unit": "kW"
    },
    "measuredVoltage": {
      "value": null,
      "unit": "V"
    },
    "generationForecast": [],
    "weatherForecast": []
  },
  "gridOrMicrogridState": {
    "gridPowerFlow": {
      "value": null,
      "unit": "kW"
    },
    "microgridVoltage": {
      "value": null,
      "unit": "V"
    },
    "fluctuationIndicator": null
  },
  "storageState": {
    "storageStateOfCharge": {
      "value": null,
      "unit": "%"
    },
    "batteryStateOfCharge": {
      "value": null,
      "unit": "%"
    },
    "supercapacitorStateOfCharge": {
      "value": null,
      "unit": "%"
    },
    "vrfbStateOfCharge": {
      "value": null,
      "unit": "%"
    },
    "storagePowerLimit": {
      "value": null,
      "unit": "kW"
    },
    "availability": true
  },
  "outputs": {
    "smoothingSetpoint": [],
    "batterySetpoint": [],
    "supercapacitorSetpoint": [],
    "vrfbSetpoint": [],
    "smoothedPowerOutput": []
  },
  "kpis": {
    "powerFluctuationReduction": null,
    "voltageFluctuationReduction": null,
    "smoothingError": null,
    "storageUtilisation": null,
    "scheduleDeviation": null
  }
}
```

---

## Implementation Requirements

A Power Smoothing implementation should include:

- renewable generation measurement;
- local power-flow and voltage measurement;
- storage SoC and availability monitoring;
- smoothing algorithm or target-output definition;
- storage allocation strategy;
- dispatch interface to storage controllers;
- monitoring of raw and smoothed output;
- performance calculation against a baseline;
- HMI representation of power and voltage smoothing performance.

---

## HMI and Dashboard Requirements

The Power Smoothing dashboard should display:

- raw renewable generation output;
- smoothed power output;
- PV output voltage or point-of-connection voltage;
- storage SoC;
- HESS charge/discharge setpoints;
- battery and supercapacitor contribution where applicable;
- VRFB setpoint where applicable;
- smoothing performance indicators;
- fluctuations before and after smoothing;
- warnings related to storage availability or SoC limits.

Historical views should include:

- raw versus smoothed renewable power;
- voltage variation before and after smoothing;
- storage cycling;
- service activation periods;
- smoothing KPIs over time.

---

## Implementation Status

Power Smoothing is represented as a project-wide renewable support and grid-stability service.

| Demonstrator | Final Repository Status |
|---|---|
| Demo 2 | Represented through VRFB optimisation in the pumped hydro and BESS island-grid context. |
| Demo 4 | Represented through Hybrid SuperCap / Li-NMC HESS smoothing in the smart DC microgrid and e-mobility context. |


---

## Lessons Learned

The Power Smoothing service shows that storage and hybrid storage systems can support renewable integration by reducing the variability of renewable power output.

Key lessons include:

- power smoothing is strongly dependent on the dynamic performance of the storage technology;
- hybrid storage can allocate fast fluctuations to supercapacitors and longer variations to batteries;
- smoothing requires continuous monitoring of renewable generation and local power flow;
- SoC margins are needed to ensure availability for both upward and downward smoothing;
- Demo 2 shows the relevance of storage optimisation in island-grid contexts;
- Demo 4 shows the importance of hybrid storage for PV and e-mobility applications;
- the service can be reused across different demonstrators through a common data-model structure;
- semantic harmonisation is essential for comparing smoothing performance across technologies and sites.

---

## Files in this Directory

```text
PowerSmoothing/

├── README.md
├── demo2/
│   └── Description.md
└── demo4/
    └── Description.md
```

---
