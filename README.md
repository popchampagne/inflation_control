# Story 2: Can the FED Control Inflation and Maintain Full Employment?

## 1. Executive Summary & Core Research Question
The central purpose of this analytical narrative is to evaluate the historical efficacy of the Federal Reserve’s monetary policy decisions through a data-driven visual investigation.

* **Central Question:** Has the Federal Reserve successfully fulfilled the dual mandate assigned to it by Congress?
* **Analysis Focus:** Policy trade-offs, transmission lags, and economic tensions between price stability (controlling inflation) and maximum sustainable employment (minimizing unemployment).
* **Temporal Scope:** 25-Year Window (2001–2026).

---

## 2. Storyline & Narrative Architecture
This presentation follows a structured three-part arc (Hook -> Evidence -> Conclusion) meeting the 4-to-5 slide requirement:

* **Slide 1: Opening / Hook (The Dual Mandate Dilemma)**
  * *Headline Message:* One steering wheel (FEDFUNDS) to manage two countervailing economic forces.
  * *Context:* Introducing the trade-off between dampening consumer price spikes and preventing severe labor market contraction.
* **Slide 2: 25-Year Historical Regimes (2001–2026)**
  * *Headline Message:* Every modern crisis triggers aggressive cuts to the zero-bound, followed by delayed, protracted normalization.
  * *Policy Regimes Covered:*
    * Post-Dot-Com Easing (Early 2000s)
    * Great Recession & ZLB (2007–2008)
    * COVID-19 Shock (2020)
    * Post-Pandemic Tightening (2022–2024)
* **Slide 3: Structural Transmission Lags**
  * *Headline Message:* Monetary policy is fundamentally backward-looking and operates with persistent 12–18 month transmission lags.
  * *Evidence:* Rate adjustments consistently trail inflation breakouts, while labor market cooling lags rate hikes.
* **Slide 4: Policy Trade-Offs & The Phillips Curve Reality**
  * *Headline Message:* Raising borrowing costs curbs demand-pull pressures but cannot resolve supply-chain bottlenecks without generating labor slack.
  * *Evidence:* Cross-regime comparison of CPI YoY versus Unemployment Rate distributions.
* **Slide 5: Decisive Verdict / Conclusion**
  * *Headline Message:* The Federal Reserve functions effectively as an emergency crisis backstop, but cannot finely tune long-run equilibrium in isolation.
  * *Core Finding:* Monetary levers only address aggregate nominal demand; true macroeconomic stability requires coordinated counter-cyclical fiscal intervention.

---

## 3. Data Sources & Provenance (2001–2026)
All data covers monthly time series from January 2001 to 2026:

| Indicator | FRED Series ID | Sponsoring Source | Economic Role |
| :--- | :--- | :--- | :--- |
| **Consumer Price Index (CPI-U)** | `CPIAUCSL` | Bureau of Labor Statistics (BLS) | Quantifies price stability via Year-over-Year % change |
| **Federal Funds Effective Rate** | `FEDFUNDS` | Federal Reserve Board / FRED | Tracks central bank monetary policy shifts |
| **Civilian Unemployment Rate** | `UNRATE` | Bureau of Labor Statistics (BLS) | Evaluates labor market capacity and dislocation |

### Inflation Transformation Formula
Raw monthly Consumer Price Index data ($CPI_t$) is transformed into a 12-month percentage change:

$$\text{YoY Inflation}_t = \left( \frac{\text{CPI}_t - \text{CPI}_{t-12}}{\text{CPI}_{t-12}} \right) \times 100$$

---

## 4. Programmatic FRED API Pipeline (Python)
Extract and clean all three required indicators programmatically:

```python
import pandas as pd
from fredapi import Fred

# 1. Initialize API client
fred = Fred(api_key="YOUR_FRED_API_KEY")

# 2. Extract 25-Year Time Series
start_date = "2001-01-01"
end_date = "2026-03-01"

fedfunds = fred.get_series("FEDFUNDS", observation_start=start_date, observation_end=end_date)
cpi = fred.get_series("CPIAUCSL", observation_start="2000-01-01", observation_end=end_date)
unrate = fred.get_series("UNRATE", observation_start=start_date, observation_end=end_date)

# 3. Calculate 12-month YoY Inflation
cpi_yoy = (cpi.pct_change(periods=12) * 100).loc[start_date:end_date]

# 4. Harmonize into Tidy Dataframe
df = pd.DataFrame({
    "FEDFUNDS": fedfunds,
    "CPI_YoY": cpi_yoy,
    "UNRATE": unrate
}).dropna()

# 5. Export dataset
df.to_csv("fed_dual_mandate_data.csv")
print("Data extracted successfully. Total rows:", len(df))
