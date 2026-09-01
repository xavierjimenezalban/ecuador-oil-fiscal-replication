# Replication package

### Oil Price Shocks, Fiscal Balance, and Public Investment in Ecuador, 2015–2025

**Xavier Jiménez-Albán**¹ — ORCID [0000-0002-7227-9392](https://orcid.org/0000-0002-7227-9392)
**Luis Urquiza-Aguiar**² — ORCID [0000-0002-6405-2067](https://orcid.org/0000-0002-6405-2067)

¹ Independent Researcher, Quito 170000, Ecuador
² Carrera de Software, Facultad de Ingeniería y Ciencias Aplicadas, Universidad de las Américas (UDLA), Quito 170000, Ecuador

Manuscript under review at MDPI *Economies*.

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22233458.svg)](https://doi.org/10.5281/zenodo.22233458)

---

## Overview

This package reproduces **every table, figure and statistic** reported in the
article. The analysis estimates a three-variable VAR on monthly Ecuadorian data
(WTI crude oil price, Non-Financial Public Sector fiscal balance, NFPS public
investment) for January 2015 – November 2025, and complements it with Local
Projections estimated with Newey–West HAC standard errors, in symmetric and
asymmetric form.

Everything is contained in a single R Markdown document. Knitting that document
in full, in one R session, regenerates the results; no other program needs to be
run and no step is performed by hand. Expected run time is **under two minutes**
on a current laptop.

## Contents

```
.
├── README.md                    This file
├── CITATION.cff                 Citation metadata (GitHub "Cite this repository")
├── LICENSE                      MIT License — applies to the code
├── LICENSE-DATA.md              Terms for the data files — applies instead of MIT
├── .zenodo.json                 Archive metadata for Zenodo
├── .gitignore                   Keeps knit by-products out of the archive
├── ecuador-oil-fiscal-replication.Rproj   RStudio project; open this first
│
├── code/
│   ├── oil_price_shocks_analysis.Rmd   The analysis. The only file to run.
│   ├── references.bib                  Bibliography for the analysis document
│   └── session-info.txt                R and package versions used
│
├── data/
│   ├── analysis/
│   │   └── ecuador_oil_fiscal_monthly_2015_2025.csv   Analysis dataset
│   └── raw/
│       ├── IEM-22-e.xlsx               Source document, as downloaded
│       └── README.md                   What it is and what it supports
│
└── output/
    └── oil_price_shocks_analysis.pdf   Knitted document: the expected result
```

## Data availability

All data used in this article are **publicly available** and are redistributed
here. No proprietary, licensed or confidential data are involved, and no data
are withheld.

| Series | Producer | Access |
|---|---|---|
| WTI crude oil price, monthly average spot, USD/barrel | U.S. Energy Information Administration | <https://www.eia.gov>; also FRED series `DCOILWTICO` |
| NFPS public investment and overall fiscal balance, millions of USD | Banco Central del Ecuador | *Boletín de Información Estadística Mensual*, Table 2.2.1, <https://www.bce.fin.ec> |

Both fiscal series were accessed in **January 2026**. BCE series are subject to
later revision; this package fixes the vintage on which the published results
rest. A reader downloading the series today may obtain revised values.

### Provenance of the analysis dataset

`data/analysis/ecuador_oil_fiscal_monthly_2015_2025.csv` is the file the code
reads, and it is distributed in the exact form the code reads it. It was
assembled from successive monthly issues of the BCE bulletin, which the Bank
publishes one issue at a time, and its formatting was then normalized: ISO
dates, quoted thousands separators removed (fourteen values were being imported
as text), and English column names carrying units. **The compilation from
individual bulletin issues was performed manually and is not reproduced by a
script in this package**; we state this plainly rather than imply an automated
pipeline that does not exist.

`data/raw/IEM-22-e.xlsx` is a source document included in its original form. It
backs the oil-revenue share reported in the introduction — not the monthly
series. See `data/raw/README.md`.

## Data dictionary

`data/analysis/ecuador_oil_fiscal_monthly_2015_2025.csv` — 131 rows, 4 columns.

| Column | Type | Units | Description |
|---|---|---|---|
| `date` | ISO date | — | First day of the observation month, `2015-01-01` to `2025-11-01` |
| `wti_usd_bbl` | numeric | USD per barrel | West Texas Intermediate crude oil, monthly average spot price |
| `nfps_investment_musd` | numeric | Millions of USD | NFPS investment, accrual basis (*devengado*), expenditure on non-financial assets |
| `nfps_balance_musd` | numeric | Millions of USD | NFPS overall fiscal balance (*Resultado Global*) |

The series are complete: 131 consecutive monthly observations, no missing
values, no duplicated periods, no gaps.

## Computational requirements

**Software.** R ≥ 4.0. Rendering to PDF additionally requires pandoc and a
LaTeX distribution with XeLaTeX; installing RStudio supplies pandoc.

**R packages.** `vars`, `tidyverse`, `knitr`, `kableExtra`, `bookdown`,
`rmarkdown`, `sandwich`, `tseries`, `urca`, `broom`, `purrr`, `readr`,
`ggplot2`. `sandwich` supplies the Newey–West HAC covariance used by the Local
Projections.

```r
install.packages(c("vars", "tidyverse", "knitr", "kableExtra", "bookdown",
                   "rmarkdown", "sandwich", "tseries", "urca", "broom"))
```

**Versions used.** The published results were produced with R 4.3.3 on
`x86_64-pc-linux-gnu`. Exact package versions are recorded in
`code/session-info.txt`.

**Run time.** Under two minutes, dominated by the bootstrap confidence
intervals and the Monte Carlo power analysis.

## Instructions to replicators

1. Open `ecuador-oil-fiscal-replication.Rproj` in RStudio. This sets the working
   directory correctly; the code reads its input by a path relative to
   `code/`, so opening the `.Rmd` on its own also works.
2. Knit `code/oil_price_shocks_analysis.Rmd`, or equivalently:

   ```r
   rmarkdown::render("code/oil_price_shocks_analysis.Rmd")
   ```

3. Compare the result against `output/oil_price_shocks_analysis.pdf` and against
   the *Expected output* table below.

**Knit the complete document, in order, in a single R session.** The random
seed is set once (`set.seed(123)`, chunk `power-analysis`) and the bootstrap
confidence intervals reported downstream inherit that generator state. Running
individual chunks in isolation, or re-ordering them, produces different
bootstrap intervals. Executed as a whole document the results are
deterministic and reproduce the published tables exactly, including from a cold
cache.

## Expected output

| Model / sub-period | Granger (p) | Peak Month | Peak Response | 95% CI at peak | Eff. obs. |
|---|---|---|---|---|---|
| VAR(2) — baseline | 0.1618 | 3 | 0.1194 | [0.0153, 0.1966] | 128 |
| VAR(2) — COVID dummy | 0.7271 | 3 | 0.0904 | — | 128 |
| VAR(12) — sensitivity | 0.0004 | 2 | 0.0859 | — | 118 |
| Lenín Moreno (2017–2021) | 0.1141 | 1 | 0.1534 | [0.0143, 0.2385] | 45 |
| Lasso / Noboa (2021–2025) | 0.251 | 1 | −0.121 | [−0.2252, −0.0076] | 51 |

Local Projections, Newey–West HAC standard errors at bandwidth *h*+1:

| Quantity | Month | Value |
|---|---|---|
| Symmetric LP, peak response (rescaled to a one-SD WTI innovation) | 3 | 0.1393, 95% CI [0.0587, 0.2199], *p* = 0.0009 |
| Asymmetric LP, response to a positive shock | 3 | −0.027 |
| Asymmetric LP, response to a negative shock | 3 | −0.261 |
| Wald test of symmetry (H₀: β⁺ = β⁻) | 3 | *p* = 0.193 — does **not** reject |
| Wald test of symmetry, only horizon rejecting at 5% | 13 | *p* = 0.0194 |

If your run reproduces both tables, the environment is correct.

Note that the symmetry test does not reject at month 3: the asymmetry visible
in the point estimates at that horizon is a descriptive feature of the
estimated responses, not a statistically established one. The article states it
that way.

## List of tables and figures

Every exhibit in the article is produced by a named chunk of
`code/oil_price_shocks_analysis.Rmd`. Table and figure numbers are those of the
published article.

| Exhibit | Title | Chunk |
|---|---|---|
| Table 1 | Variables, sources, and transformations | `tabla-variables` |
| Table 2 | Descriptive statistics of transformed series | `tabla-descriptiva` |
| Table 3 | Pairwise correlations of VAR-transformed variables | `tabla-correlacion` |
| Table 4 | FEVD: share of forecast variance of log public investment | `fevd-table` |
| Table 5 | Model comparison: peak response to a WTI shock | `tabla-comparacion-chunk` |
| Table 6 | Presidential sub-periods for heterogeneity analysis | `tabla-subperiodos` |
| Table 7 | Fiscal response heterogeneity across administrative regimes | `tabla-resultados-sub-chunk` |
| Table A1 | Unit-root analysis summary | `prueba-adf`, `kpss-test`, `za-test`, `adf-vals` |
| Table A2 | VAR(2) vs VAR(12) diagnostics | `diagnostico-var` |
| Figure 1 | Conceptual transmission channel | `fig-transmission` |
| Figure 2 | WTI price, fiscal balance and public investment, 2015–2025 | `fig-series-plot` |
| Figure 3 | IRF: public investment to a WTI shock | `irf-wti-plot` |
| Figure 4 | IRF: public investment to a fiscal balance shock | `irf-bal-plot` |
| Figure 5 | Symmetric Local Projections | `lp-symmetric-plot` |
| Figure 6 | Asymmetric Local Projections | `lp-asymmetric-plot` |
| Figure 7 | IRF, VAR(2) with COVID-19 dummy | `irf-covid-fig-plot` |
| Figure 8 | IRF, VAR(12) sensitivity specification | `irf-var12-fig-plot` |
| Figure 9 | IRF, Moreno administration | `irf-sub2-plot` |
| Figure 10 | IRF, Lasso/Noboa administrations | `irf-sub3-plot` |
| Figure A1 | Indicative IRF, Correa administration | `irf-sub1-plot-appendix` |

Supporting chunks that compute quantities quoted in the prose rather than
producing an exhibit: `power-analysis` (Monte Carlo power), `estimacion-var`,
`irf-base-vals`, `lp-estimation`, `lp-asym-summary`, `seleccion-rezagos`,
`irf-covid-vals`, `irf-12-vals`, `subperiodos-setup`.

## Conventions used in the article

**Peak Month** follows a one-based convention: Month 1 denotes the impact month
(horizon *h* = 0). The baseline peak at Month 3 therefore corresponds to
*h* = 2.

**Peak Response** is the change in log public investment, in log points,
following a one-standard-deviation WTI shock, measured at the month of greatest
absolute magnitude.

**Effective observations** are those available to the VAR(2) after
first-differencing and lag initialization: 128 for the full sample, and
26 / 45 / 51 for the Correa / Moreno / Lasso–Noboa sub-periods respectively.

## How to cite

Version 1.0.0 of this package is archived on Zenodo and is the version that
produced the results reported in the article:

> Jiménez-Albán, X., & Urquiza-Aguiar, L. (2026). *Replication package for
> "Oil Price Shocks, Fiscal Balance, and Public Investment in Ecuador,
> 2015-2025"* (Version 1.0.0) [Software]. Zenodo.
> <https://doi.org/10.5281/zenodo.22233458>

Please cite the article as well when you use this package, and cite the original
data producers — the U.S. Energy Information Administration and the Banco Central
del Ecuador — when you use the underlying series. Machine-readable citation
metadata is in `CITATION.cff`.

The archived version is fixed; this repository may receive later corrections, so
the two can differ. Cite the DOI above for the exact package behind the
published results.

## License

**Code** — MIT License, see `LICENSE`.
**Data** — see `LICENSE-DATA.md`. The data are redistributed from public
official sources and are not ours to license; cite the original producers.
