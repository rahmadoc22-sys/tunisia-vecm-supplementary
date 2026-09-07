# Supplementary Material — Public
## Money, Inflation, and Industrial Production in Tunisia: A VECM Analysis on Fully Official Data, 1994–2020

**Author:** Rahma Tahri — Faculty of Economics and Management of Sfax (FSEGS), University of Sfax, Tunisia · ORCID [0000-0002-4381-9724](https://orcid.org/0000-0002-4381-9724) · rahmatahri@fsegs.u-sfax.tn

**License:** CC-BY 4.0 (see `LICENSE`). This supplementary material is open and may be shared and adapted with attribution.

---

## What this is

This is the **public supplementary material** that accompanies the manuscript submitted to the
*Journal of International Money and Finance*. It contains the results, figures, and data
documentation a reader needs to follow and assess the paper. It does **not** contain the raw data
files or the analysis code; those live in the separate (private) **replication package**, available
from the authors and released under a commercial license.

## Contents

| Path | Content |
|---|---|
| `figures/` | Figures 1–3 of the paper (official series; VAR inverse roots; impulse responses) |
| `tables/` | Result tables T1–T13 (CSV): descriptives, unit roots, Zivot–Andrews, lag order, Johansen, VECM, diagnostics, Granger causality, FEVD, robustness, provenance exhibit, legacy diagnostics |
| `data_note.md` | Full documentation of every official source, retrieval codes, and construction choices (splices, rescaling, coverage gaps) |
| `data_availability_statement.md` | Data availability statement as submitted to the journal |

## Table guide

| File | Description |
|---|---|
| `T1_descriptives.csv` | Descriptive statistics, 1994Q1–2020Q4 |
| `T2_unit_roots.csv` | ADF / Phillips–Perron / KPSS unit-root tests |
| `T3_zivot_andrews.csv` | Zivot–Andrews tests with one endogenous break |
| `T4_lag_order.csv` | Lag-order information criteria (levels VAR) |
| `T5_johansen.csv` | Johansen trace tests |
| `T6_vecm_beta_alpha.csv` | Cointegrating vector and adjustment coefficients |
| `T7_diagnostics.csv` | VECM residual diagnostics |
| `T8_granger.csv` | Granger-causality tests (rows = causing, columns = caused) |
| `T9_fevd.csv` | Forecast-error variance decomposition |
| `T10_sous_echantillons.csv` | Pre-/post-2011 sub-samples |
| `T11_robustness_r1_r2.csv` | BCT-only money and alternative-window robustness |
| `T12_provenance_exhibit.csv` | Official vs legacy-data provenance exhibit |
| `T13_legacy_diagnostics.csv` | Legacy-data diagnostics (correlations, interpolation signature) |

## Relationship to the replication package

The full replication package (data + code + one-command reproduction) is maintained privately by the
authors under a proprietary/commercial license and is provided to reviewers and the editorial office
on request. Contact: rahmatahri@fsegs.u-sfax.tn.
