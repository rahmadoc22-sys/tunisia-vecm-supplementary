# Data Availability Statement & Replication Package

## Statement (for insertion in the manuscript)

> The data analyzed in this study were constructed exclusively from official primary sources: the Institut National de la Statistique (INS, Tunisia) industrial production index and consumer price index as disseminated through the IMF's *International Financial Statistics* (IFS), the Banque Centrale de Tunisie (BCT) statistical tables (money-market rate and monetary aggregates), and the INS monthly conjuncture bulletin of June 2026. All raw retrievals, construction scripts, derived datasets, estimation scripts, and output tables are provided in the accompanying replication package [repository link / DOI to be inserted at acceptance], licensed under CC-BY 4.0. Every observation in the analysis file can be traced to a named official series code or publication, as documented in the package README. No proprietary or restricted-access data were used.

## Replication package structure

```
replication-package/
├── README.md                      # provenance of every variable, retrieval instructions, license
├── data/
│   ├── raw/
│   │   ├── IPI_06_2026.xlsx       # INS conjuncture bulletin 06-2026 (monthly IPI 2025-01→2026-06)
│   │   ├── bct_tmm_PL203105.html  # BCT stat_mens.jsp raw response, TMM 1987–2026
│   │   ├── bct_agregats_PL203276.html  # BCT raw response, M1/M2/M3 2001-12→2026-07
│   │   ├── bct_ipi_PL203175.html  # BCT raw response, IPI base 100=2000, 2000–2014
│   │   ├── bct_ipi_PL203177.html  # BCT raw response, IPI base 100=2010, 2010–2020
│   │   └── ifs_series.json        # IMF-IFS series via DB.nomics API (codes below)
│   ├── derived/
│   │   ├── 03_IPI_mensuel_Tunisie_officiel_1993_2026.csv
│   │   ├── 04_M2_TMM_mensuel_Tunisie_officiel_1962_2026.csv
│   │   ├── 05_tunisie_officiel_trimestriel_1993_2026.csv
│   │   └── 07_tunisie_analysis_officiel_1994Q1_2020Q4.csv   # analysis file (n = 108)
│   └── comparison/
│       └── 06_tunisie_comparaison_origine_vs_officiel.csv   # legacy series vs official (audit trail)
├── code/
│   ├── build_panel.py             # multi-country annual panel (external-validity checks)
│   └── officiel_v2_analysis.py    # full empirical pipeline: T1–T10 + figures
├── output/
│   ├── T1_descriptives.csv … T10_sous_echantillons.csv
│   ├── fig_series.png
│   └── fig_irf.png
└── docs/
    └── data_note.md               # French data memo: coverage, cross-validations, splice diagnostics
```

## Source provenance (exact)

| Variable | Source | Series code / endpoint | Coverage retrieved |
|---|---|---|---|
| IPI (monthly, base 100=2010) | INS via IMF IFS, mirrored by DB.nomics | `GET https://api.db.nomics.world/v22/series/IMF/IFS/M.TN.AIP_IX?observations=1` | 1993-01 → 2019-04 |
| IPI (monthly, base 100=2010), extension | BCT statistical tables | `POST https://www.bct.gov.tn/bct/siteprod/stat_mens.jsp` with `pilote=PL203177`, `cat='0','V203177001'`, `db=20100131`, `df=20201231` | 2010-01 → 2020-12 |
| IPI (monthly, base 100=2000), cross-check | BCT | same endpoint, `pilote=PL203175`, `cat='0','V203175001'` | 2000-01 → 2014-12 |
| IPI 2025–2026 (context only) | INS bulletin 06-2026 | `IPI_06_2026.xlsx`, sheet Feuil1, row "INDICE ENSEMBLE" | 2025-01 → 2026-06 |
| CPI (monthly) | INS via IMF IFS / DB.nomics | `M.TN.PCPI_IX` | 1987-07 → 2025-06 |
| M2 (monthly, MDT) | BCT monetary aggregates table | `stat_mens.jsp` with `pilote=PL203276`, `cat='0','V203276001','V203276002','V203276003','V203276004'` (M3, M2, M1, currency) | 2001-12 → 2026-07 |
| M2 long segment (money + quasi-money, MDT) | BCT via IMF IFS / DB.nomics | `M.TN.35L___XDC` | 1962-12 → 2018-02 |
| TMM (monthly, %) | BCT | `stat_mens.jsp` with `pilote=PL203105`, `cat='0','V203105001',…,'V203105012'` | 1987-01 → 2026-08 |
| TMM, cross-check | IMF IFS / DB.nomics | `M.TN.FIMM_PA` | 2001-12 → 2018-04 |

## Cross-validations performed (documented in `docs/data_note.md`)

1. IFS IPI vs BCT IPI (base 2010), 112 overlapping months: ratio 1.000 (s.d. 0.003) — identical series.
2. BCT TMM vs IFS money-market rate, 197 overlapping months: correlation 0.984, mean difference +0.011 pp.
3. IFS money-plus-quasi-money vs BCT M2, 195 overlapping months: mean ratio 0.900 (s.d. 0.021) — the measured splice factor applied to the pre-2001-12 segment; log-continuity at the join verified (adjacent monthly log-changes −0.006, +0.012, +0.016; maximum |Δln| over the full series 0.093).
4. BCT IPI base 2000 vs base 2010 (60 overlapping months): ratio 1.356 — consistent rebasing.

## Known gaps (disclosed)

- Official monthly IPI is unavailable for 2021-01 → 2024-12 (BCT stopped republishing after 2020-12; no INS online archive; IFS ends 2019-04). The analysis sample therefore ends in 2020Q4.
- The pre-1993 period has no verifiable official quarterly equivalent and is excluded.

## Software

Python 3.13; statsmodels 0.15.0; arch 8.0.0; pandas/NumPy/Matplotlib as pinned in `requirements.txt`. Single-command replication: `python code/officiel_v2_analysis.py` (regenerates all tables T1–T10 and figures from `data/derived/07_*.csv`).
