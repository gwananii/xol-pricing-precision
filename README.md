# Code and results for "How Precisely Can an Excess-of-Loss Layer Be Priced? A Precision Rule for Peaks-over-Threshold Premiums"

Supplemental material for review. This package downloads the public data and reproduces the computations behind Tables 2, 3, 4, 9, 10, 12, and 18 of the paper, Table S5 of the online supplement, the data checks of Section 4.3, and Figure 1. All random seeds are fixed, so a full run reproduces the reported numbers exactly.

## Contents

| Path | What it is |
|---|---|
| `code/xol_tables.py` | Downloads the 12 data files and computes all tables listed below. Self-contained (no other code file is needed). |
| `code/make_paper_figure_1.py` | Draws Figure 1 of the paper in black and white at 300 dpi (EPS, TIFF, PDF, PNG). |
| `code/xol_figures.py` | Optional: draws six illustrative figures of the result tables and zips them. These figures are not in the paper. |
| `code/requirements.txt` | Python packages needed. |
| `results/` | The result tables from a full run with the paper's settings (CSV), plus `all_tables.txt` with every table in one readable file. |

## Which file gives which result

| Result file (`results/`) | Paper |
|---|---|
| `table2.csv` | Table 2 |
| `corollary2_poisson.csv` | Corollary 2 (Poisson claim counts): range of κ_P/κ and check of κ_P² = κ² + (1 − q_R) |
| `table3_kappa_body.csv` | Table 3 |
| `table4_decomposition.csv` | Table 4 |
| `table9_S5_gpd_vs_bc.csv` | Table 9 and Supplement Table S5 |
| `table10_real_data.csv` | Table 10 |
| `section4_indexing.csv` | Section 4.3: nominal against indexed (2007) amounts in frecomfire and frebiloss |
| `table12_subsampling.csv` | Table 12 (Experiment B) |
| `table18_bootstrap.csv` | Table 18 |
| `check_derivatives.csv` | Check of the Appendix B derivatives against finite differences |
| `check_gpd_fit.csv` | Check of the GPD maximum-likelihood fit against a brute-force likelihood grid |

The Section 4 data checks (the Norwegian 1988 threshold at the 0.92 quantile, policy-limit censoring in lossalae, and unsettled claims in fremarine) are printed in the run log.

Not reproduced by this package: Tables 1, 5–8, 11, 13–17, and 19; Supplement Tables S1–S4; Figures 2–5; and Supplement Figures S1 and S2. Tables 1, 5, and 6 are derived directly from Table 2 and the data descriptions.

## Data

All data are public and are downloaded automatically on the first run (about 30 MB) from the GitHub repositories of the R packages:

- ReIns (version 1.0.16): `secura`, `norwegianfire`
- fitdistrplus (version 1.2-6): `danishuni`
- CASdatasets (version 1.2-1): `freMTPL2sev`, `freMTPLsev`, `fremarine`, `usautoBI`, `frecomfire`, `frebiloss`, `lossalae`, `lossalaefull`, `ausautoBI8999`

The files are read with the pure-Python package `rdata`, so R is not needed. If the download is blocked, the script prints the address of each missing file; put the files in the folder `xol_data` next to the script.

## How to run

Open a terminal in the `code` folder. Install the packages once:

```
pip install -r requirements.txt
```

Then run:

```
python xol_tables.py                  # quick test of everything (1-3 minutes)
python xol_tables.py --paper          # the paper's settings and seeds
python xol_tables.py --paper --only table12 table18     # selected tables only
python make_paper_figure_1.py         # Figure 1
python xol_figures.py                 # optional illustrative figures (run after the tables)
```

In a Jupyter notebook, prefix each command with `!` (for example, `!python xol_tables.py --paper`). Running the scripts as separate programs keeps a crash in a compiled library from stopping the notebook.

### Run time and memory

With the paper's settings, a full run took about 30 minutes on a Linux workstation and 158 minutes on a Windows 11 laptop. Most of the time is spent on Table 18 (1,000 samples per design, each with 999 bootstrap refits). Peak memory use was about 360 MB.

The run saves its progress in `xol_out/checkpoints`. If it is interrupted, run the same command again: it resumes where it stopped and gives the same numbers as an uninterrupted run. Use `--fresh` to start over.

## Settings and seeds

| Setting | Quick test | Paper |
|---|---|---|
| Samples per design, Tables 4 and 9 | 300 | 5,000 |
| Samples per design, Table 12 | 100 | 1,000 |
| Samples per design, Table 18 | 30 | 1,000 |
| Bootstrap resamples, Table 18 | 199 | 999 |

Seeds: 777 (Tables 4 and 9), 4242 (Table 12), 20261122 and 20261121 (Table 18 at n = 300 and n = 1,000).

## Method notes

- The GPD is fitted by maximum likelihood through the profile likelihood in θ = ξ/β (a grid search followed by 60 golden-section steps). Samples are fitted in batches to limit memory; the batch size does not affect the results.
- The layer premium (Equation 1) and the derivatives of Appendix B are computed by 128-point Gauss–Legendre quadrature.
- True layer premiums of the simulation distributions are computed by composite 16-point Gauss–Legendre quadrature (relative accuracy about 1e-13).
- Thresholds are set at sample quantiles, with exceedances taken strictly above the threshold.

## Outputs of a run

| Folder or file | Contents |
|---|---|
| `xol_data/` | Downloaded data files |
| `xol_out/*.csv` | Result tables |
| `xol_out/all_tables.txt` | All tables in one text file |
| `xol_out/run_log.txt` | Everything printed during the run |
| `xol_out/crash_report.txt` | Written only if Python itself crashes |
