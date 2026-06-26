# Manual Frequency Restoration Reserve (mFRR)

Manual Frequency Restoration Reserve (mFRR) is a frequency-restoration service activated manually or semi-manually after faster balancing services have contained and initially restored system frequency.

In the i-STENTORE Service Repository, mFRR is represented through:

- Demo 3 – Virtual Energy Storage System for Renewable Energy Integration, Spain.

---

## Service Category

mFRR belongs to the following service families:

- Flexibility Services;
- Balancing Services;
- Ancillary Services;
- Manual Frequency Restoration Services;
- Market-based Reserve Services;
- Virtual Energy Storage System Services.

---

## Purpose of the Service

The purpose of mFRR is to restore system balance after a frequency deviation or imbalance by activating reserve capacity manually or through scheduled balancing-market mechanisms.

The service supports:

- restoration of system balance after frequency deviation;
- provision of upward or downward balancing energy;
- participation of aggregated storage and renewable resources in reserve markets;
- use of VESS flexibility for balancing purposes;
- coordination between market bids, storage state and renewable forecasts.

---

## General Description

mFRR is generally activated after FCR and aFRR. It provides additional balancing energy and helps the system operator restore reserve margins. In a VESS context, mFRR requires that the aggregated resource portfolio can provide upward or downward active-power response during a defined activation period.

In Demo 3, mFRR is represented as a service provided by the VESS. The VESS combines renewable generation, BESS flexibility and optimisation routines. The final implementation is documented as planned / modelled because external balancing-market activation and the full VESS asset configuration were not fully available during the project validation period.

---

## Value Proposition

| Value Stream | Description |
|---|---|
| Balancing Energy | Provides upward or downward balancing energy. |
| Market Participation | Enables storage-based participation in balancing markets where allowed. |
| Renewable Integration | Uses storage to manage variability and provide reserve capacity. |
| VESS Aggregation | Aggregates multiple assets into a flexible service provider. |
| Service Stacking | Can be coordinated with Energy Arbitrage and Congestion Management. |
| Replication Potential | Provides a reusable model for VESS-based reserve provision. |

---

## Demonstrator Implementation

| Demonstrator | Implementation Focus | Repository File |
|---|---|---|
| Demo 3 | VESS-based mFRR service using BESS flexibility, renewable generation and market-oriented optimisation. | `mFRR/demo3/Description.md` |

---

## Demo 3 Implementation Summary

Demo 3 mFRR is represented as a balancing-service model for the Spanish VESS. The service estimates available reserve capacity from the VESS portfolio, prepares upward or downward reserve capability, and defines the storage and generation actions required to satisfy a balancing request or market activation.

The final validation status is modelled / planned because the VRFB subsystem could not be installed and external market or system-operator activation was not available for full field validation.

---

## Typical Inputs

| Input | Description | Unit |
|---|---|---|
| mfrR_request | Manual reserve activation or scenario request. | n/a |
| activation_direction | Upward or downward reserve. | n/a |
| activation_period | Requested reserve period. | datetime |
| requested_power | Requested reserve power. | MW |
| bess_state_of_charge | BESS state of charge. | % |
| available_charge_power | Available charging power. | MW |
| available_discharge_power | Available discharging power. | MW |
| renewable_generation_forecast | Forecasted PV, wind or hydro generation. | MW |
| market_price | Balancing or reserve market price. | €/MWh |
| existing_commitments | Energy Arbitrage or other service commitments. | n/a |

---

## Typical Outputs

| Output | Description | Unit |
|---|---|---|
| mfrR_bid | Reserve bid or available capacity offer. | MW |
| accepted_reserve_capacity | Accepted reserve capacity, where applicable. | MW |
| bess_dispatch_schedule | BESS charge/discharge schedule. | MW |
| delivered_balancing_power | Delivered balancing response. | MW |
| activated_energy | Energy delivered or absorbed during activation. | MWh |
| service_status | Planned, activated, simulated or unavailable. | status |
| schedule_deviation | Difference between requested and delivered response. | MW or % |

---

## Generic Workflow

```text
1. Receive mFRR request or balancing-market scenario.
2. Evaluate VESS availability.
3. Check BESS SoC and power limits.
4. Check renewable generation forecast and existing commitments.
5. Calculate upward or downward reserve capability.
6. Prepare bid, schedule or activation response.
7. Dispatch BESS / VESS response if activated.
8. Monitor delivery and calculate KPIs.
```

---

## Service Interaction

| Related Service | Interaction |
|---|---|
| Energy Arbitrage | Market schedules can reduce available reserve capacity. |
| Congestion Management | Network constraints may limit reserve dispatch. |
| FCR / aFRR | Faster frequency services have priority before manual restoration. |
| Power Smoothing | Uses the same storage capacity for local fluctuation management. |

---

## Suggested JSON Skeleton

```json
{
  "service": "mFRR",
  "demo": "Demo3",
  "metadata": {
    "service_id": "demo3_mfrr",
    "requester_id": "market_or_internal_scenario",
    "timestamp": "YYYY-MM-DDTHH:mm:ssZ"
  },
  "activation": {
    "activation_direction": "upward_or_downward",
    "activation_period": {
      "start": "YYYY-MM-DDTHH:mm:ssZ",
      "end": "YYYY-MM-DDTHH:mm:ssZ"
    },
    "requested_power": {
      "value": null,
      "unit": "MW"
    }
  },
  "vess": {
    "bess_state_of_charge": {
      "value": null,
      "unit": "%"
    },
    "available_charge_power": {
      "value": null,
      "unit": "MW"
    },
    "available_discharge_power": {
      "value": null,
      "unit": "MW"
    },
    "renewable_generation_forecast": [],
    "existing_commitments": []
  },
  "outputs": {
    "mfrr_bid": {
      "value": null,
      "unit": "MW"
    },
    "bess_dispatch_schedule": [],
    "delivered_balancing_power": [],
    "activated_energy": {
      "value": null,
      "unit": "MWh"
    },
    "service_status": "planned"
  },
  "kpis": {
    "reserve_availability": null,
    "activation_accuracy": null,
    "schedule_deviation": null,
    "delivered_energy": null
  }
}
```

---

## Implementation Status

| Demonstrator | Final Repository Status |
|---|---|
| Demo 3 | Planned / modelled VESS-based mFRR service. |

---

## Files in this Directory

```text
mFRR/

├── README.md
└── demo3/
    └── Description.md
```
