# Energy Arbitrage

Energy Arbitrage is a generation-support and market-oriented flexibility service based on the use of energy storage to shift energy across time according to electricity price signals, renewable generation availability and operational constraints.

The service aims to increase the economic value of renewable generation and storage assets by charging or increasing flexible consumption during low-price periods and discharging or reducing flexible demand during high-price periods.

In the i-STENTORE project, Energy Arbitrage is mainly associated with the optimisation of storage and hybrid energy systems for participation in day-ahead electricity markets and for the cost-efficient operation of renewable-based energy systems.

---

## Service Category

Energy Arbitrage belongs to the following service families:

- Generation Support Services;
- Bulk Storage Services;
- Trading Services;
- Market-based Flexibility Services;
- Renewable Energy Integration Services.

---

## Purpose of the Service

The purpose of Energy Arbitrage is to optimise the operation of storage assets and flexible energy resources by exploiting temporal differences in electricity prices.

The service supports:

- improved profitability of renewable generation and storage assets;
- better use of surplus renewable generation;
- reduction of energy procurement costs;
- optimisation of battery charging and discharging schedules;
- improved alignment between renewable generation and market commitments;
- enhanced flexibility for hybrid energy systems;
- support to day-ahead market participation.

---

## General Description

Energy Arbitrage makes use of Energy Storage Systems or Hybrid Energy Storage Systems to store energy when electricity prices are low or renewable generation is abundant, and to release energy when prices are high or when market commitments require additional energy.

The optimisation process typically considers electricity market prices, renewable generation forecasts, load forecasts, battery state of charge, storage capacity limits, charging and discharging power limits, efficiency losses, market gate closure times and operational constraints of local assets.

---

## Value Proposition

| Value Stream | Description |
|---|---|
| Market Revenue | Participation in wholesale or day-ahead electricity markets. |
| Renewable Integration | Better use of variable renewable generation. |
| Cost Reduction | Charging or consuming energy during low-price periods. |
| Operational Optimisation | Coordinated dispatch of storage and flexible assets. |
| Service Stacking | Potential combination with other flexibility services, depending on asset availability and constraints. |

---

## Technical Principle

The service follows a forecast-based optimisation approach:

1. Collect renewable generation, storage, load and market data.
2. Generate forecasts for renewable production, electricity prices and relevant demand.
3. Evaluate the available flexibility of the storage system.
4. Optimise charging, discharging or flexible consumption schedules.
5. Submit market offers or operating schedules.
6. Receive market results or dispatch decisions.
7. Update the operation plan according to accepted bids and real-time constraints.
8. Monitor performance and store results for later analysis.

---

## Typical Actors

| Actor | Role |
|---|---|
| Plant Operator | Owns or operates renewable generation, storage or industrial assets. |
| Market Representative / Aggregator | Aggregates flexibility and submits offers to the electricity market. |
| Market Operator | Operates and clears the wholesale or day-ahead market. |
| Energy Management System | Coordinates forecasting, optimisation and scheduling. |
| Forecasting Service | Provides price, generation and demand forecasts. |
| Storage Controller | Executes charging and discharging schedules. |
| User Interface / HMI | Displays forecasts, schedules, market results and operational performance. |
| Data Governance Middleware | Supports data exchange, semantic interoperability and integration with the i-STENTORE digital ecosystem. |

---

## Demonstrator Implementations

| Demonstrator | Implementation Focus | Repository File |
|---|---|---|
| Demo 3 | Virtual Energy Storage System for renewable energy integration and day-ahead market participation. | `EnergyArbitrage/demo3/Description.md` |
| Demo 5 | Industrial hybrid energy optimisation involving BESS, renewable generation, hydrogen production and day-ahead market optimisation. | `EnergyArbitrage/demo5/Description.md` |

---

## Suggested JSON Skeleton

```json
{
  "service": "EnergyArbitrage",
  "demo": "DemoX",
  "timestamp": "YYYY-MM-DDTHH:mm:ssZ",
  "inputParameters": {
    "optimisationHorizon": "PT24H",
    "timeResolution": "PT15M",
    "marketGateClosureTime": "13:00",
    "storageRatedPower": { "value": null, "unit": "kW" },
    "storageEnergyCapacity": { "value": null, "unit": "kWh" },
    "minimumStateOfCharge": { "value": null, "unit": "%" },
    "maximumStateOfCharge": { "value": null, "unit": "%" }
  },
  "runtimeVariables": {
    "currentStateOfCharge": { "value": null, "unit": "%" },
    "assetAvailability": true
  },
  "forecasts": {
    "electricityPriceForecast": [],
    "renewableGenerationForecast": [],
    "loadForecast": []
  },
  "outputs": {
    "chargingSchedule": [],
    "dischargingSchedule": [],
    "marketBid": [],
    "stateOfChargeTrajectory": [],
    "expectedProfit": { "value": null, "unit": "EUR" }
  },
  "kpis": {
    "realisedProfit": null,
    "scheduleDeviation": null,
    "renewableEnergyUtilisation": null
  }
}
```

---
