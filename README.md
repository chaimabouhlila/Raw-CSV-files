# Raw-CSV-files — HVAC Operational Dataset

Real-world HVAC measurement dataset collected from  smart building , released in support of the paper:

> **"Adaptation of HVAC systems to improve the energy efficiency of smart buildings"**  
> Chaima Bouhlila · Fatma Achour · Olfa Jemai — 2026

---

## Files

| File | Description | Resolution | Size |
|---|---|---|---|
| `Consommation_clean.csv` | Cumulative energy consumption (kWh) | 1 hour | 3.8 KB |
| `Puissance_clean.csv` | Active power demand (kW) | 15 minutes | 13.8 KB |
| `Tension_clean.csv` | Supply voltage (V) | 15 minutes | 13.5 KB |

**Period covered**: September 7, 2025 → November 3, 2025 (~52 days)

---

## Column Descriptions

### `Consommation_clean.csv`

| Column | Type | Unit | Description |
|---|---|---|---|
| `Time` | datetime (ISO 8601) | — | Timestamp, hourly resolution |
| `conso_value` | float | kWh | Cumulative energy meter index (monotonically increasing) |

> To obtain 1-hour consumption increments, compute `diff()` between consecutive rows.

---

### `Puissance_clean.csv`

| Column | Type | Unit | Description |
|---|---|---|---|
| `Time` | datetime (ISO 8601) | — | Timestamp, 15-minute resolution |
| `puissance_value` | float | kW | Instantaneous active power draw |

---

### `Tension_clean.csv`

| Column | Type | Unit | Description |
|---|---|---|---|
| `Time` | datetime (ISO 8601) | — | Timestamp, 15-minute resolution |
| `tension_value` | float | V | RMS supply voltage (nominal: 400 V three-phase) |

---

## How to Load

```python
import pandas as pd

conso   = pd.read_csv("Consommation_clean.csv", parse_dates=["Time"])
power   = pd.read_csv("Puissance_clean.csv",    parse_dates=["Time"])
voltage = pd.read_csv("Tension_clean.csv",      parse_dates=["Time"])

# Compute hourly consumption increments
conso["delta_kWh"] = conso["conso_value"].diff().fillna(0)

print(conso.head())
print(power.describe())
print(voltage.describe())
