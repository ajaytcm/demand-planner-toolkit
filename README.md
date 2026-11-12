# Demand Planner Toolkit — Excel

A working demand-planning toolkit in Excel, built the way planners actually run a monthly cycle: forecast the demand, measure how good the forecast is, set the safety stock and reorder point, then roll it into a PSI (Production–Sales–Inventory) plan. Every number in the workbook is a live formula — change an input and the plan re-chains.

**Skills shown:** demand forecasting (moving average, exponential smoothing), forecast accuracy (MAPE, bias), inventory planning (safety stock, reorder point), PSI planning.

## Files

- `demand_planner_toolkit.xlsx` — the workbook (3 sheets, below).
- `data/demand_history.csv` — the raw input: 18 months of demand for one sample SKU (synthetic sample data).

## Sheet 1 — Forecast
18 months of actual demand with two forecasting methods side by side:
- **3-month moving average** — next month = average of the last 3 actuals.
- **Exponential smoothing (α = 0.3)** — next month = α × last actual + (1 − α) × last forecast.

Accuracy is measured, not assumed — MAPE and bias are calculated for both methods over the months each can forecast:

| Method | MAPE | Bias (total A − F) | Next-month forecast (Jul 2026) |
|--------|------|--------------------|-------------------------------|
| Moving average (3M) | 8.2% | +640 (under-forecast) | 1,445 units |
| Exponential smoothing | 8.0% | +929 (under-forecast) | 1,399 units |

Reading: on a rising trend both methods lag and under-forecast (positive bias). ES is slightly more accurate on this history; the PSI plan still uses the higher MA3 figure — the safer choice while demand is climbing.

## Sheet 2 — Safety Stock & Reorder Point
Inputs come straight from the history: average monthly demand 1,313 units, standard deviation 125 units, lead time 2 months, 95% service level (Z = 1.65).

- **Safety stock** = Z × σ × √lead time = **293 units**
- **Demand during lead time** = 1,313 × 2 = **2,625 units**
- **Reorder point** = 2,625 + 293 = **2,918 units** — when on-hand stock falls to this level, order.

## Sheet 3 — PSI Plan (Jul–Dec 2026)
Sales = the MA3 forecast (1,445/month, flat). Production = Sales + Target ending inventory − Beginning inventory, with a target of holding one month of sales as ending inventory.

The plan tells a story: opening inventory of 2,400 covers most of July, so July production is only **490 units**; from August the plan settles into producing **1,445 units a month** and holding inventory steady at 1,445.

## How to talk about this project in an interview
- "I compare forecast methods on MAPE and bias before trusting one — a low MAPE with a big positive bias still stocks you out on a trend."
- "Safety stock isn't a guess: it's service level times demand variability over the lead time. Raising service from 95% to 99% roughly doubles the buffer — and the cash in it."
- "The PSI plan is just the forecast, the inventory position and the production lever chained together — if any one moves, the plan shows where it lands."

## Phase 2 (planned)
The same toolkit rebuilt in Python as a learning project, once the AI-901 Python study is complete — the Excel version stays the reference for what the numbers should be.
