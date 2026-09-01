# Raw source documents

Files in this directory are **as downloaded from the producer**. They are not
modified, not renamed beyond their original filename, and are not read by the
analysis code. They are included so that a reader can check a reported figure
against the document it came from.

## `IEM-22-e.xlsx`

Banco Central del Ecuador, *Boletín de Información Estadística Mensual*,
**Table 2.2 — Operaciones del Sector Público No Financiero (base devengado)**,
in millions of USD. One sheet, 73 rows by 32 columns. Columns are annual
(2019–2023), quarterly (2019 Q1 – 2024 Q3) and monthly (October and November
2024). Downloaded from <https://www.bce.fin.ec>.

**What it supports.** The oil-revenue share reported in the introduction of the
article. Dividing *Ingresos Petroleros* (row code 11) by *Total Ingresos* (row
code 1) across the annual columns gives:

| Year | 2019 | 2020 | 2021 | 2022 | 2023 |
|---|---|---|---|---|---|
| Oil share of total NFPS revenue (%) | 31.4 | 25.1 | 34.1 | 37.3 | 33.3 |

which is the 25–37 percent range quoted there; the mean across the five years is
32.2 percent.

**What it does not support.** This file is *not* the source of the monthly
analysis dataset. That series is monthly and runs from January 2015 to November
2025; this table covers neither that frequency nor that span. See the
*Data availability* section of the top-level `README.md` for the provenance of
the analysis dataset.
