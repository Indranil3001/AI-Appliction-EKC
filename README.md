# Carbon Economics: Testing the Environmental Kuznets Curve Hypothesis

An econometric analysis of the relationship between economic growth and greenhouse gas emissions using panel data from the world's 30 largest emitters (1999–2024).

## Overview

The Environmental Kuznets Curve (EKC) proposes that as countries get richer, they initially pollute more — but after reaching a critical income threshold, emissions begin to decline. This project tests that theory using real-world data and finds that the turning point exists at approximately **$46,398 GDP per capita**, a threshold already surpassed by several developed nations.

The analysis also reveals that the EKC was effectively non-existent before 2012 but became statistically meaningful after the Paris Agreement era, suggesting that **policy intervention — not income growth alone** — drives the downward bend.

## Key Findings

- **EKC confirmed** in both Pooled OLS (R² = 0.569) and Fixed Effects Panel (R² = 0.460) models
- **Cubic turning point**: ~$46,398 GDP per capita (full cubic derivative)
- **β₂ is stable** across specifications (−0.064 pooled, −0.060 fixed effects), confirming the inverted-U is robust
- **Within R² = 7%** — GDP growth alone explains very little of within-country emission changes; policy matters far more
- **Time-varying shift**: Turning point dropped from $16.6M (1999–2011) to $88K (2012–2024), aligning with post-Paris Agreement policy intensification
- **Country peak detection**: Nations that have peaked in per-capita emissions are predominantly high-income economies with active climate policies

## Data Sources

| Dataset | Source | Coverage |
|---------|--------|----------|
| GHG Emissions | [EDGAR](https://edgar.jrc.ec.europa.eu/) (European Commission) | 1970–2024, kt CO₂-eq |
| GDP | [World Bank](https://data.worldbank.org/) World Development Indicators | 1960–2024, current USD |
| Population | [United Nations](https://population.un.org/) DESA | 1970–2024 |

## Project Structure

```
├── Code
├── Data
└── README.md
```

## Methodology

### Data Pipeline

1. **Integration**: Three datasets merged on ISO3 country codes and year, producing a panel of 167 countries
2. **Feature Engineering**: Per-capita emissions, per-capita GDP, emission intensity, log transformations, income group classification
3. **Filtering**: Top 30 emitters by latest-year total GHG, yielding a balanced panel of 780 observations (30 × 26 years)

### Econometric Models

**Pooled OLS (Baseline)**
```
ln(GHG/capita) = α + β₁x + β₂x² + β₃x³ + ε
```
Where x = mean-centred ln(GDP per capita). HC3 robust standard errors.

**Fixed Effects Panel (Primary)**
```
PanelOLS with entity (country) + time (year) effects, clustered standard errors
```

### Turning Point Calculation

The cubic turning point is derived by solving the first derivative:
```
β₁ + 2β₂x + 3β₃x² = 0
```
Using the quadratic formula, selecting the root with a negative second derivative (confirmed maximum), and transforming back to dollar scale.

### Robustness Checks

- AIC comparison: Cubic (1171.4) vs Quadratic (1192.6) — cubic wins
- Breusch-Pagan test: Heteroscedasticity confirmed (p = 0.0002), justifying HC3
- F-test for poolability: Fixed effects decisively preferred (F = 286.70, p < 0.0001)
- Time-varying EKC: Sub-period analysis (1999–2011 vs 2012–2024)
- Country-level peak detection with dual threshold (3-year lag + 10% decline)

## Visualisations

The project includes interactive Plotly charts:

- **Pooled EKC scatter** — hover for country, year, GDP/capita, emissions; turning point marked
- **Country trajectories** — USA, China, India, Germany, Brazil, Russia traced over time
- **Animated bubble chart** — all 30 countries moving year-by-year with Play/Pause slider

## Tech Stack

- **Python 3.13** — pandas, numpy, statsmodels, linearmodels, scipy
- **Visualisation** — matplotlib, seaborn, plotly
- **Econometrics** — `statsmodels.OLS` (pooled), `linearmodels.PanelOLS` (fixed effects)
- **Jupyter Notebook**


### Dependencies

```
pandas
numpy
matplotlib
seaborn
statsmodels
linearmodels
scipy
plotly
openpyxl
nbformat
```

## Usage

Run the notebooks in order:

```
1. data_integration.ipynb    → Produces final_merged_data.csv
2. EDA_and_feature_engg.ipynb → Produces emissions_top30.csv
3. EKC.ipynb                  → Regression analysis, turning point, visualisations
```

## References

- Stern, D.I. (2004). Environmental Kuznets Curve. *Encyclopedia of Energy*, Vol. 2, Elsevier.
- Stern, D.I. (2004). The Rise and Fall of the Environmental Kuznets Curve. *World Development*, 32(8), 1419–1439.
- Wang, Q. et al. (2024). Reinvestigating the EKC of carbon emissions in 147 countries. *Humanities and Social Sciences Communications*, 11, 160.
- Wang, Q. et al. (2024). Rethinking the EKC hypothesis across 214 countries. *Humanities and Social Sciences Communications*, 11, 283.
- Li, J. (2024). Environmental Kuznets curve, balanced growth, and influencing factors: evidence from China. *Int. J. Climate Change Strategies and Management*, 16(3), 318–336.


## Author

Infranil (GH1026152)
