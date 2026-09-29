# Data Note: Provenance, Construction, and Cross-Validation

## 1. Sources and retrieval

All series were retrieved from official public sources. Retrieval date: September 2026.

### 1.1 Banque Centrale de Tunisie (BCT) statistical tables

The BCT website exposes queryable statistical tables through the endpoint
`https://www.bct.gov.tn/bct/siteprod/stat_mens.jsp` (HTTP POST; parameters `pilote`, `cat`,
`db`, `df`, `la`; dates in `YYYYMMDD` format). Raw responses are stored in `data/raw/`.

| Table (pilote) | Content | Variables used | Coverage retrieved |
|---|---|---|---|
| `PL203105` | Taux moyen du marché monétaire (TMM), monthly, % | TMM | 1987-01 → 2026-08 (476 obs) |
| `PL203276` | Agrégats de monnaie et leurs contreparties, monthly, MDT | M3 (`V203276001`), M2 (`V203276002`), M1 (`V203276003`), currency (`V203276004`) | 2001-12 → 2026-07 (296 obs) |
| `PL203177` | Industrial production index, base 100 = 2010, monthly | IPI (var. `V203177001`) | 2010-01 → 2020-12 (132 obs) |
| `PL203175` | Industrial production index, base 100 = 2000, monthly | cross-check (var. `V203175001`) | 2000-01 → 2014-12 (180 obs) |

### 1.2 IMF International Financial Statistics (IFS), as reported by INS and BCT

Retrieved via the DB.nomics public API v22
(`https://api.db.nomics.world/v22/series/IMF/IFS/<code>?observations=1`); the complete raw
responses are stored in `data/raw/ifs_series.json`.

| Series | Content | Coverage |
|---|---|---|
| `M.TN.AIP_IX` | Industrial production index (INS), base 100 = 2010 | 1993-01 → 2019-04 (316 obs) |
| `M.TN.AIP_PC_CP_A_PT` | Industrial production, y/y % change | 1994-01 → 2019-04 |
| `M.TN.PCPI_IX` | Consumer price index (INS) | 1987-07 → 2025-06 (456 obs) |
| `M.TN.35L___XDC` | Money + quasi-money (BCT), MDT | 1962-12 → 2018-02 (658 obs) |
| `M.TN.FMB_XDC` | Broad money (BCT), MDT | 2001-12 → 2025-06 (283 obs) |
| `M.TN.FIMM_PA` | Money-market rate (BCT), % | 2001-12 → 2018-04 (197 obs) |

### 1.3 INS conjuncture bulletin

`IPI_06_2026.xlsx` (INS, June 2026): monthly industrial production index, aggregate
("INDICE ENSEMBLE"), 2025-01 → 2026-06. Used for post-sample context only; the estimation
sample ends in 2020-12.

## 2. Cross-validations between independent official sources

1. **IFS IPI vs BCT IPI (base 2010)**, 112 overlapping months (2010-01 → 2019-04):
   mean ratio 1.000, s.d. 0.003 — the two publications carry the same series.
2. **BCT TMM vs IFS money-market rate**, 197 overlapping months (2001-12 → 2018-04):
   correlation 0.984, mean difference +0.011 pp, max absolute difference 0.91 pp (one month).
3. **IFS money + quasi-money vs BCT M2**, 195 overlapping months (2001-12 → 2018-02):
   mean ratio 0.900, s.d. 0.021. The IFS aggregate runs systematically ~11% above the
   current-definition BCT M2 (definition difference); see Section 3 for the splice.
4. **BCT IPI base 2000 vs base 2010**, 60 overlapping months: ratio 1.356 — consistent
   rebasing between the two index bases.

## 3. Construction of the analysis series

Quarterly values are three-month averages of the official monthly series.

- **IPI (%)**: year-on-year change of the quarterly average of the official index
  (IFS 1993-01 → 2019-04; BCT PL203177 2019-05 → 2020-12). The y/y construction also
  neutralizes the weak residual seasonality of the monthly index (seasonal-dummy F-test on
  levels, 1993–2020: F = 1.61, p = 0.095).
- **INF (%)**: year-on-year change of the quarterly average of the official CPI.
- **LM2**: natural log of M2 in million TND, spliced as follows: IFS `M.TN.35L___XDC`
  (1993-01 → 2001-11), rescaled by the measured overlap factor 0.900, then BCT table
  PL203276 M2 (2001-12 → 2020-12). Log-continuity at the join: adjacent monthly log-changes
  are −0.006, +0.012, +0.016; the maximum absolute monthly log-change over the full series
  is 0.093. Both the raw and the spliced series are provided in
  `data/derived/04_M2_TMM_mensuel_Tunisie_officiel_1962_2026.csv`.
- **r (%)**: quarterly average of the official monthly TMM (BCT PL203105).

## 4. Known gaps and exclusions

- Official monthly IPI is unavailable for **2021-01 → 2024-12**: the BCT stopped
  republishing the index after 2020-12, the IFS series ends in 2019-04, and the INS monthly
  bulletin archive is not available online. The estimation sample therefore ends in 2020Q4.
- The pre-1993 period has no verifiable official monthly equivalent for IPI and is excluded.
- `data/comparison/06_tunisie_comparaison_origine_vs_officiel.csv` aligns, quarter by quarter,
  a legacy set of the same four series (construction undocumented) against the
  official series. Cross-checks (output `T13_legacy_diagnostics.csv`) show the legacy
  industrial-production series correlates only weakly with official y/y growth (0.55 overall;
  0.22–0.65 depending on sub-period), the legacy money series carries the signature of
  interpolation between annual anchors (22.6% of quarterly growth rates repeat within the
  calendar year) and is not reproducible from official M2 (sub-period level shifts of
  −58%, −41%, +9%). The legacy series are used **only** in the provenance exhibit
  (`code/robustness_r1_r2_legacy.py`, outputs `T12_provenance_exhibit.csv` and
  `T13_legacy_diagnostics.csv`); they enter no main estimation.

## 5. Derived files

| File | Content |
|---|---|
| `03_IPI_mensuel_Tunisie_officiel_1993_2026.csv` | monthly IPI, official, with source column per observation |
| `04_M2_TMM_mensuel_Tunisie_officiel_1962_2026.csv` | monthly M1/M2/M3 (BCT), long M2 (IFS), TMM (BCT + IFS), raw and splice-adjusted M2 |
| `05_tunisie_officiel_trimestriel_1993_2026.csv` | quarterly official series (IPI, INF, M2, LM2, LM2 adjusted, TMM) |
| `07_tunisie_analysis_officiel_1994Q1_2020Q4.csv` | analysis file used in the paper (n = 108) |
| `panel_annual_30pays_1976_2024.csv` | auxiliary multi-country annual panel (World Bank API) for external-validity checks; not used in the paper's tables |

---

## Addendum v3 (2026-09-29) — système principal en NIVEAUX

Le système principal du manuscrit est désormais estimé en **log niveaux** :
{log IPI (base 2010), log CPI, log M2, TMM}, 1994Q1–2020Q4 (n = 108), construit par
`code/officiel_v3_levels.py` (une commande, depuis la racine du paquet).

- **Fichier dérivé** : `data/derived/08_tunisie_levels_officiel_1994Q1_2020Q4.csv`
  (moyennes trimestrielles des séries mensuelles officielles ; log pour IPI, CPI, M2).
- **Déterministes** : dummies saisonnières Q2–Q4 (saisonnalité résiduelle de l'IPI mensuel :
  F = 1.61, p = 0.095) et dummies de rupture 2011Q1 / 2020Q1 (LR = 22.08, p = 0.005).
- **Rang** : Johansen trace + max-eigenvalue, 2 specs × 3 lags, critiques Osterwald–Lenum
  (fournies par statsmodels) ; rang 1 préféré, rang 2 non exclu à k = 2 (constante seule).
- **VECM** : ML complet (rank 1, k_ar_diff = 2) ; α conditionnels au β ML avec s.e. OLS
  équation par équation ; tests LR d'exogénéité faible χ²(1).
- **IRF/FEVD** : représentation MA du VECM ; Cholesky (lipi, lcpi, lm2, r) + 2 ordres
  alternatifs + FEVD généralisées (Pesaran–Shin) ; bandes 95 % bootstrap (500 réplications,
  récursion VECM).
- Les tables `output_v3/T1–T11` + figures `fig1–3_v3` sont les sorties du système principal.
  L'ancien dossier `output/` (T1–T13, système en croissances) est conservé comme
  **exhibit de provenance** (T12/T13 cités dans le manuscrit §3.10).
