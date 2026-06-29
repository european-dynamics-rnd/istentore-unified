# Fast Frequency Response (FFR)

Fast Frequency Response (FFR) is a fast active-power response service that supports power-system stability immediately after a frequency disturbance.

FFR is particularly relevant in power systems with high shares of inverter-based renewable generation, where the reduction of synchronous inertia can lead to faster frequency deviations. Storage systems and hybrid energy storage systems can provide FFR because they are able to change active power rapidly through power-electronic converters.

In the i-STENTORE Service Repository, FFR is represented through:

- Demo 2 – Pumped Hydro Storage combined with BESS, Portugal.

---

## Service Category

FFR belongs to the following service families:

- Flexibility Services;
- Balancing Services;
- Ancillary Services;
- Fast Frequency Services;
- Dynamic Grid-Support Services;
- Storage-based Active-Power Response Services.

---

## Purpose of the Service

The purpose of FFR is to provide very fast active-power response immediately after a frequency disturbance.

The service supports:

- rapid containment of frequency deviations;
- mitigation of high rate-of-change-of-frequency events;
- support to low-inertia systems;
- improved system resilience;
- coordination with Synchronous Inertia and FCR;
- use of BESS and HESS assets for dynamic grid support.

---

## General Description

FFR acts faster than traditional restoration reserves and may complement inertia and FCR. It is usually triggered by a frequency deviation or rate-of-change-of-frequency condition.

Storage systems are well suited to FFR because they can quickly inject or absorb active power. In an island-grid context, this response can be essential to prevent rapid frequency excursions after a disturbance.

In Demo 2, FFR is part of the common ancillary-service workflow with Synchronous Inertia, FCR and aFRR. The workflow includes API-triggered optimisation and dynamic assessment with the N-1 criterion.

---

## Value Proposition

| Value Stream | Description |
|---|---|
| Low-Inertia Support | Provides fast active-power support where synchronous inertia is limited. |
| Frequency Security | Reduces severity of rapid frequency deviations. |
| Island Grid Stability | Supports isolated systems with limited interconnection support. |
| Storage Utilisation | Uses fast BESS converter capability. |
| Dynamic Security | Can be assessed jointly with N-1 stability criteria. |
| Service Stacking | Complements inertia, FCR and aFRR. |

---

## Demonstrator Implementation

| Demonstrator | Implementation Focus | Repository File |
|---|---|---|
| Demo 2 | FFR through the common EEM / INESC TEC ancillary-service optimisation and dynamic assessment workflow. | `FFR/demo2/Description.md` |

---

## Demo 2 Implementation Summary

In Demo 2, FFR is implemented as a fast-response ancillary service in the island-grid context. The service uses the same common workflow as Synchronous Inertia, FCR and aFRR.

EEM initiates the workflow through the API. INESC TEC retrieves updated data, runs the optimisation algorithm and checks the resulting operating point through dynamic assessment. If the dispatch is stable, EEM applies it. If not, synchronous condensers are dispatched and the process is repeated.

---

## Typical Inputs

| Input | Description | Unit |
|---|---|---|
| frequency_deviation | Difference from nominal frequency. | Hz |
| rocof | Rate of change of frequency, where available. | Hz/s |
| ffr_activation_threshold | Threshold for fast response. | Hz or Hz/s |
| bess_state_of_charge | BESS state of charge. | % |
| available_power | Available fast active-power response. | MW |
| response_time_requirement | Required FFR response time. | s |
| system_operating_state | Island-grid operating condition. | status |
| dynamic_security_requirement | N-1 and dynamic stability requirement. | n/a |

---

## Typical Outputs

| Output | Description | Unit |
|---|---|---|
| ffr_power_setpoint | Fast active-power response setpoint. | MW |
| delivered_ffr_power | Measured delivered response. | MW |
| response_time | Time to deliver fast response. | s |
| activation_status | Idle, active, completed or unavailable. | status |
| dynamic_security_result | Result of dynamic assessment. | status |
| service_availability | Whether FFR capability is available. | status |

---

## Generic Workflow

```text
1. Detect frequency deviation or FFR need.
2. Check BESS / HESS fast-response availability.
3. Calculate FFR active-power response.
4. Validate operating point through dynamic assessment.
5. Dispatch fast active-power response.
6. Monitor delivered response and response time.
7. Store activation results and KPIs.
```

---

## Service Interaction

| Related Service | Interaction |
|---|---|
| Synchronous Inertia | Both support the initial frequency behaviour after a disturbance. |
| FCR | FCR contains frequency after or alongside FFR activation. |
| aFRR | aFRR restores frequency after containment. |
| Power Smoothing | Reduces fluctuations that may trigger fast frequency events. |
| Energy Arbitrage | Must preserve SoC and power margins for fast response. |

---

## Suggested JSON Skeleton

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
    "available_power": {
      "value": null,
      "unit": "MW"
    },
    "response_time_requirement": {
      "value": null,
      "unit": "s"
    }
  },
  "outputs": {
    "ffr_power_setpoint": {
      "value": null,
      "unit": "MW"
    },
    "delivered_ffr_power": [],
    "activation_status": "idle"
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

## Implementation Status

| Demonstrator | Final Repository Status |
|---|---|
| Demo 2 | Simulated / HIL validated fast frequency response model in the island-grid ancillary-service workflow. |

---

## Files in this Directory

```text
FFR/

├── README.md
└── demo2/
    └── Description.md
```
