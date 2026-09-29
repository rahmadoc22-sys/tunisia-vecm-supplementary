# Supplementary Material — Public
## Money, Prices, and Industrial Production in Tunisia: A VECM in Log Levels on Fully Official Data, 1994–2020

**Author:** Rahma Tahri — Faculty of Economics and Management of Sfax (FSEGS), University of Sfax, Tunisia · ORCID [0000-0002-4381-9724](https://orcid.org/0000-0002-4381-9724) · rahmatahri@fsegs.u-sfax.tn

**License:** CC-BY 4.0 (see `LICENSE`). This supplementary material is open and may be shared and adapted with attribution.

---

## What this is

This is the **public supplementary material** accompanying the manuscript. It contains the
results, figures, and data documentation a reader needs to follow and assess the paper. It does
**not** contain the raw data files or the analysis code; those live in the separate (private)
**replication package**, available from the author on request.

## Contents

| Path | Content |
|---|---|
| `figures/` | Figures 1–3 of the paper (official level series; impulse responses with bootstrap bands; CUSUM) |
| `tables/` | Result tables (CSV): main system in log levels (T1–T11), CUSUM (T12), provenance exhibit on the growth-rate system (T13–T14) |
| `data_note.md` | Full documentation of every official source, retrieval codes, construction choices (splices, rescaling, coverage gaps), and the v3 levels addendum |
| `data_availability_statement.md` | Data availability statement as submitted to the journal |

## Table guide

| File | Description |
|---|---|
| `T1_v3_descriptives.csv` | Descriptive statistics of the four log-level series (N = 108) |
| `T2_v3_unit_roots.csv` | ADF / Phillips–Perron / KPSS, levels and first differences |
| `T3_v3_zivot_andrews.csv` | Zivot–Andrews tests with one endogenous break; minimum-t break dates |
| `T4_v3_lag_order.csv` | AIC / BIC / HQIC / FPE, levels VAR, lags 1–8 |
| `T5_v3_johansen.csv` | Johansen trace and maximum-eigenvalue, 2 deterministic specs × 3 lags, Osterwald–Lenum critical values |
| `T6_v3_vecm_alpha_beta.csv` | Cointegrating vector (normalized on log M2), adjustment coefficients with s.e./t, LR weak-exogeneity tests |
| `T7_v3_diagnostics.csv` | Ljung–Box, Jarque–Bera, ARCH-LM per equation |
| `T8_v3_granger_ty.csv` | Toda–Yamamoto F-tests (predictive content) |
| `T9_v3_fevd.csv` | Variance decomposition, h = 1/4/8/20: main Cholesky ordering + bootstrap CI + generalized + 2 alternative orderings |
| `T10_v3_breaks.csv` | LR test on break dummies; Chow tests at 2011Q1 and 2020Q1 |
| `T11_v3_subsamples.csv` | Sub-sample cointegration and conditional adjustment of money |
| `T12_v3_cusum.csv` | CUSUM of the money equation |
| `T13_provenance_exhibit.csv` | Growth-rate system, official vs legacy series (manuscript §3.10) |
| `T14_legacy_diagnostics.csv` | Correlations and interpolation signature of the legacy series |

## Note on the two systems

The manuscript's main system is estimated in **log levels** (T1–T12). The provenance exhibit
(T13–T14) concerns a **growth-rate** specification of the same four concepts, retained solely to
document how results move with data vintage. The full replication package (data + code,
proprietary license) is available from the author on request.
