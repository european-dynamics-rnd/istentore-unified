# Automatic Frequency Restoration Reserve (aFRR)

Automatic Frequency Restoration Reserve (aFRR) is a frequency-restoration service used after the initial frequency containment action. It is activated automatically to restore the system balance and support frequency recovery after a disturbance.

In the i-STENTORE Service Repository, aFRR is represented through:

- Demo 2 – Pumped Hydro Storage combined with BESS, Portugal.

Earlier conceptual descriptions may include aFRR in other demonstrators, but in the final service repository aFRR is retained only for Demo 2. Demo 5 is intentionally not included under aFRR because the final Demo 5 service scope is limited to Energy Arbitrage and FCR.

---

## Service Category

aFRR belongs to the following service families:

- Flexibility Services;
- Balancing Services;
- Ancillary Services;
- Frequency Restoration Services;
- Automatic Active-Power Control Services;
- Storage-based Grid Stability Services.

---

## Purpose of the Service

The purpose of aFRR is to automatically restore system balance after a frequency deviation has been contained by FCR.

The service supports:

- automatic restoration of frequency;
- balancing between supply and demand;
- recovery of reserve margins after FCR activation;
- improved operational security;
- integration of storage and hydro resources into balancing workflows;
- coordinated operation of energy storage, hydro and renewable generation in an island-grid context.

---

## General Description

aFRR is activated after a frequency deviation has occurred and the system has been stabilised by FCR or other fast-response services. While FCR contains the frequency deviation, aFRR contributes to restoring the frequency and system balance automatically.

In storage-based implementations, aFRR may require active-power setpoints to be dispatched to storage or generation resources. The response must respect storage state-of-charge limits, hydro reservoir constraints, power limits and dynamic-security requirements.

In Demo 2, aFRR is implemented as part of a common ancillary-service optimisation workflow together with Synchronous Inertia, FFR and FCR.

---

## Value Proposition

| Value Stream | Description |
|---|---|
| Frequency Restoration | Supports frequency recovery after containment. |
| Automatic Balancing | Provides automatic active-power adjustment. |
| Island Grid Stability | Supports system balance in isolated or weak-grid contexts. |
| Hybrid Resource Coordination | Coordinates hydro, BESS and synchronous-machine resources. |
| Operational Security | Ensures dispatch points are compatible with dynamic-security constraints. |
| Service Stacking | Can be coordinated with FCR, FFR and Synchronous Inertia. |

---

## Technical Principle

The generic aFRR process is:

1. Detect the need for automatic frequency restoration.
2. Determine the required active-power restoration signal.
3. Check available hydro and storage flexibility.
4. Optimise the operating point using generation, storage, forecast and cost data.
5. Assess the candidate operating point against dynamic-security constraints.
6. Dispatch the accepted operating point.
7. Monitor frequency restoration and service performance.
8. Store results for validation and reporting.

---

## Demonstrator Implementation

| Demonstrator | Implementation Focus | Repository File |
|---|---|---|
| Demo 2 | aFRR through the common EEM / INESC TEC ancillary-service optimisation and dynamic assessment workflow. | `aFRR/demo2/Description.md` |

---

## Demo 2 Implementation Summary

In Demo 2, aFRR is part of the shared ancillary-service workflow. EEM initiates the process through an API request. The INESC TEC virtual machine retrieves updated data from the EEM server and runs the optimisation algorithm. The calculated point of operation is then assessed through a dynamic procedure including the N-1 criterion.

If the point of operation is safe and stable, the results are stored and EEM sets the generation technologies to the defined point. If the point is not dynamically safe, synchronous condensers are dispatched and the optimisation is repeated until a stable point is achieved.

---

## Typical Inputs

| Input | Description | Unit |
|---|---|---|
| service_id | Identifier of the aFRR service instance. | n/a |
| requester_id | Identifier of EEM or the requesting entity. | n/a |
| timestamp | Timestamp of the request. | datetime |
| timestep | Optimisation timestep. | implementation-specific |
| horizon | Optimisation horizon. | implementation-specific |
| afrr_activation_signal | Automatic restoration signal or activation requirement. | MW |
| frequency_deviation | Frequency deviation requiring restoration. | Hz or mHz |
| lib_soc | Li-ion battery state of charge. | kWh |
| vrfb_soc | VRFB state of charge, where applicable. | kWh |
| hpp_reservoir_capacity | Hydro reservoir capacity. | m³ |
| renewable_generation_forecast | Forecasted renewable generation. | kWh |
| generation_cost | Renewable generation cost. | €/kWh |
| charging_cost | Storage charging cost. | €/kWh |
| pumping_hydro_cost | Pumped hydro operation cost. | €/kWh |

---

## Typical Outputs

| Output | Description | Unit |
|---|---|---|
| afrr_power_setpoint | Active-power setpoint for automatic restoration. | MW |
| lib_converter_power | Li-ion battery converter power. | MW |
| vrfb_converter_power | VRFB converter power, where applicable. | MW |
| hydro_power | Hydro generation output. | MW |
| synchronous_machine_state | Synchronous machine / condenser state. | status |
| accepted_operating_point | Final dynamically safe operating point. | n/a |
| restoration_status | Status of restoration process. | status |
| dynamic_security_result | Result of N-1 dynamic assessment. | status |

---

## Generic Workflow

```text
1. EEM initiates the aFRR workflow through the API.
2. INESC TEC retrieves updated EEM data.
3. The optimisation algorithm calculates a candidate operating point.
4. The dynamic assessment procedure evaluates the candidate point.
5. If stable, the operating point is accepted and stored.
6. If not stable, synchronous condensers are dispatched and the process is repeated.
7. EEM applies the accepted operating point.
8. Results are stored and displayed.
```

---

## Service Interaction

| Related Service | Interaction |
|---|---|
| FCR | FCR contains the frequency deviation before aFRR restores balance. |
| FFR | FFR provides very fast response and may reduce the magnitude of restoration required. |
| Synchronous Inertia | Supports the initial dynamic behaviour of the grid. |
| Power Smoothing | Reduces power fluctuations that may trigger balancing needs. |
| Energy Arbitrage | Must not compromise reserve availability when restoration capability is required. |

---

## Suggested JSON Skeleton

```json
{
  "service": "aFRR",
  "demo": "Demo2",
  "metadata": {
    "service_id": "demo2_afrr",
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
  "activation": {
    "afrr_activation_signal": {
      "value": null,
      "unit": "MW"
    },
    "frequency_deviation": {
      "value": null,
      "unit": "Hz"
    },
    "restoration_direction": "upward_or_downward"
  },
  "storageAndHydro": {
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
    "vrfb": {
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
  },
  "kpis": {
    "restoration_accuracy": null,
    "service_availability": null,
    "dynamic_security_status": null,
    "number_of_rejected_dispatch_points": null
  }
}
```

---

## Lessons Learned

The aFRR service shows that automatic frequency restoration in storage-rich systems requires more than a power setpoint. The operating point must also be feasible from storage, hydro and dynamic-security perspectives. In island-grid contexts, N-1 validation and synchronous condenser dispatch may be necessary before accepting a restoration schedule.

---

## Files in this Directory

```text
aFRR/

├── README.md
└── demo2/
    └── Description.md
```
