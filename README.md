# Fleet Telemetry Ingestion & Analytics Service

## Overview
This project implements the core ingestion and analytics layer for a large-scale EV Fleet
management platform. The system processes real-time telemetry data coming from
Smart Meters (grid side) and Electric Vehicles / Chargers (vehicle side), correlates them,
and provides analytical insights into power efficiency and vehicle performance.

The platform is designed to handle **10,000+ devices**, each sending data **every 60 seconds**,
while supporting fast dashboards and efficient analytical queries.

---

## Problem Statement
Fleet operators are billed based on **AC energy consumed from the grid**, while vehicles
store **DC energy in batteries**. Due to power conversion and heat loss, AC consumption
is always higher than DC delivery.

This service helps:
- Track AC vs DC energy usage
- Detect power inefficiencies
- Monitor vehicle battery health
- Provide 24-hour performance analytics per vehicle

---

## Architecture Summary
The system follows a **hot + cold data strategy**:

- **Hot Store (Operational Data)**
  - Stores only the latest state of each meter and vehicle
  - Used for dashboards and real-time status
  - Updated using UPSERT operations

- **Cold Store (Historical Data)**
  - Stores every telemetry event (append-only)
  - Used for reporting and analytics
  - Optimized for large-scale time-series data

---

## Data Sources

### 1. Smart Meter (Grid Side)
Reports AC power drawn from the grid.

```json
{
  "meterId": "string",
  "kwhConsumedAc": "number",
  "voltage": "number",
  "timestamp": "ISO-8601"
}
