# License and terms of use — data files

The code in this package is released under the MIT License (see `LICENSE`).
The **data files are covered by this notice instead**, because they are not our
original creation: they are redistributed from public official sources.

## Files covered

- `data/analysis/ecuador_oil_fiscal_monthly_2015_2025.csv`
- `data/raw/IEM-22-e.xlsx`

## Sources and rights

| Series | Producer | Terms |
|---|---|---|
| WTI crude oil spot price (`wti_usd_bbl`) | U.S. Energy Information Administration (EIA) | U.S. Government work; not subject to copyright protection in the United States. Freely reproducible. |
| NFPS investment and fiscal balance (`nfps_investment_musd`, `nfps_balance_musd`), and `IEM-22-e.xlsx` | Banco Central del Ecuador (BCE), *Boletín de Información Estadística Mensual* | Official statistics published for public use. Redistributed here unmodified in substance, for the sole purpose of reproducing the results of the accompanying article. |

We claim no ownership over the underlying series. To the extent that the
compilation, transcription and formatting of the analysis dataset constitute a
protectable contribution, we release that contribution under the
**Creative Commons Attribution 4.0 International (CC BY 4.0)** license:
<https://creativecommons.org/licenses/by/4.0/>

## How to cite the data

Cite the original producers, not this package, when citing the underlying
series. Cite this package (see `CITATION.cff`) when referring to the compiled
dataset or the analysis code.

## Accuracy and vintage

BCE fiscal series are subject to later revision. The figures distributed here
are the vintage accessed in January 2026 and are the vintage on which the
published results rest. A reader downloading the series today may obtain
revised values that do not reproduce the published tables exactly. This is a
property of the source, not an error in this package.
