# Code and results for "How Precisely Can an Excess-of-Loss Layer Be Priced? A Precision Rule for Peaks-over-Threshold Premiums"

This repository downloads the public data and computes the tables and figures of the paper and its online supplement: Tables 1–19, Table C1 of Appendix C, Tables S1–S5, Figures 1–5, and Figures S1–S2, together with the data checks of Section 4. The few items not computed are listed below. All random seeds are fixed, so a full run always gives the same numbers. The numbers printed in the paper are those of the run in `results/`. `REPRODUCTION_REPORT.md` lists the definitions used where a method needed a precise statement.

## Contents

| Path | What it is |
|---|---|
| `code/xol_tables.py` | Downloads the 12 data files and computes all tables listed below. Self-contained (no other code file is needed). |
| `code/make_paper_figure_1.py` | Draws Figure 1 of the paper in black and white at 300 dpi (EPS, TIFF, PDF, PNG). |
| `code/make_paper_figures_4_5.py` | Draws Figures 4, 5, and S2 in black and white at 300 dpi from `real_grid_bootstrap.csv` (run the tables first). |
| `code/make_paper_figures_2_3_S1.py` | Draws Figures 2, 3, and S1 in black and white at 300 dpi from the CSV files of Tables 7, 8, 13, and S1 (run the tables first). |
| `code/xol_figures.py` | Optional: draws six illustrative figures of the result tables and zips them. These figures are not in the paper. |
| `code/XoL_pricing_precision_reproducible.ipynb` | Jupyter notebook that saves the five scripts and runs everything in order (see `HOW_TO_RUN.txt`). |
| `code/check_files.py` | Confirms that the five scripts are complete and unchanged. |
| `requirements.txt` | Python packages needed. |
| `results/` | The result tables from a full run with the paper's settings (CSV), plus `all_tables.txt` with every table in one readable file and `run_log.txt` with the summaries printed during the run (progress lines removed). |
| `figures/` | Figures 1–5, S1, and S2 from that run (PNG and PDF; the scripts also write EPS and TIFF). |
| `REPRODUCTION_REPORT.md` | Definitions used where a method needed a precise statement, and the few items not computed. |

## Which file gives which result

| Result file (`results/`) | Paper |
|---|---|
| `table1_nR.csv` | Table 1 |
| `table2.csv` | Table 2 |
| `corollary2_poisson.csv` | Corollary 2 (Poisson claim counts): range of κ_P/κ and check of κ_P² = κ² + (1 − q_R) |
| `table3_kappa_body.csv` | Table 3 |
| `tableC1_kappa_any_gpd.csv` | Table C1 (Appendix C) |
| `table4_decomposition.csv` | Table 4 |
| `table5_6_data.csv` | Sample sizes (Tables 5 and 6) and the 0.99 quantiles that set the layers of Table 6 |
| `table7_lomax.csv` | Table 7 and Figure 2 |
| `table8_beyond_lomax.csv` | Table 8 |
| `table8_all_settings.csv` | All 216 settings of Section 6.2 (Figure 3) |
| `table9_S5_gpd_vs_bc.csv` | Table 9 and Supplement Table S5 |
| `table10_real_data.csv` | Table 10 |
| `section4_indexing.csv` | Section 4.3: nominal against indexed (2007) amounts in frecomfire and frebiloss |
| `table11_formula_vs_bootstrap.csv` | Table 11 |
| `table12_subsampling.csv` | Table 12 (Experiment B) |
| `table13_expA.csv` | Table 13 (Experiment A) |
| `table13_designs.csv` | The 24 designs of Experiment A (Figure 3) |
| `table14_grid_stability.csv` | Table 14 |
| `table15_methods.csv` | Table 15 (single mid-grid, naive BMA, equal weight, burning cost) |
| `table16_rules.csv` | Table 16 (single mid-grid, Hill-plateau and MRL-linear rules) |
| `table17_averaging.csv` | Table 17, with TPW (Supplement S3) |
| `table17_paired_differences.csv` | Section 6.6: each rule's mean absolute relative error minus the single threshold's, with 95% intervals |
| `table19_threshold_stability.csv` | Table 19 |
| `real_grid_bootstrap.csv` | Fit and bootstrap results at every threshold of every real dataset; input of Figures 4, 5, and S2 |
| `tableS1_entropy.csv` | Table S1 and the TPW premiums of Supplement S2 |
| `tableS2_bootstrap_fixed_weights.csv` | Table S2 |
| `tableS3_tpw_additional.csv` | Table S3 and the values of ρ in Supplement S4 |
| `tableS4_trimmed_tpw.csv` | Table S4 (Burr and Lomax rows) |
| `figureS1_weights.csv` | Naive BMA and TPW weights (Figure S1) |
| `table18_bootstrap.csv` | Table 18 |
| `check_derivatives.csv` | Check of the Appendix B derivatives against finite differences |
| `check_gpd_fit.csv` | Check of the GPD maximum-likelihood fit against a brute-force likelihood grid |

The Section 4 data checks (the Norwegian 1988 threshold at the 0.92 quantile, policy-limit censoring in lossalae, and unsettled claims in fremarine) are printed in the run log.

The freMTPLsev restricted-grid checks of Section 6.7 and Supplement S4, the Section 4 data checks, and the summaries quoted in the text of Section 6.2 are in `results/run_log.txt`.

Not computed: the descriptive columns of Tables 5 and 6 (periods, currencies, inflation status), which come from the package documentation; and the pilot study of Supplement S5 (AUC values), which was run with earlier code.

## Data

All data are public and are downloaded automatically on the first run (about 30 MB) from the GitHub repositories of the R packages:

- ReIns (version 1.0.16): `secura`, `norwegianfire`
- fitdistrplus (version 1.2-6): `danishuni`
- CASdatasets (version 1.2-1): `freMTPL2sev`, `freMTPLsev`, `fremarine`, `usautoBI`, `frecomfire`, `frebiloss`, `lossalae`, `lossalaefull`, `ausautoBI8999`

The files are read with the pure-Python package `rdata`, so R is not needed. If the download is blocked, the script prints the address of each missing file; put the files in the folder `xol_data` next to the script.

## How to run

Open a terminal in the top folder of the repository. Install the packages once:

```
pip install -r requirements.txt
```

Then move into the `code` folder and run:

```
cd code
python xol_tables.py                  # quick test of everything (1-3 minutes)
python xol_tables.py --paper          # the paper's settings and seeds
python xol_tables.py --paper --only table12 table18     # selected tables only
python make_paper_figure_1.py         # Figure 1
python make_paper_figures_4_5.py      # Figures 4, 5 and S2 (after the tables)
python make_paper_figures_2_3_S1.py   # Figures 2, 3 and S1 (after the tables)
python xol_figures.py                 # optional illustrative figures (run after the tables)
```

In a Jupyter notebook, prefix each command with `!` (for example, `!python xol_tables.py --paper`). The figure scripts can also be pasted whole into one notebook cell and run: they save themselves as a file, draw each figure in a separate Python process so that a crash in a compiled library cannot stop the notebook, and show the finished figures below the cell. Figures 1–5, S1, and S2 are written to the folder `paper_figures`.

### Run time and memory

With the paper's settings, a full run takes about an hour on a Linux workstation (most of it for Tables 17 and 18) and may take several hours on a laptop. Peak memory use is below 1 GB.

The run saves its progress in `xol_out/checkpoints`. If it is interrupted, run the same command again: it resumes where it stopped and gives the same numbers as an uninterrupted run. Use `--fresh` to start over.

## Settings and seeds

| Setting | Quick test | Paper |
|---|---|---|
| Samples per design, Tables 4 and 9 | 300 | 5,000 |
| Samples per design, Table 12 | 100 | 1,000 |
| Samples per design, Table 18 | 30 | 1,000 |
| Bootstrap resamples, Table 18 | 199 | 999 |
| Bootstrap resamples per threshold, Tables 11 and 19 | 50 | 200 |
| Samples per design, Table 7 | 100 | 1,500 |
| Samples per design, Table 8 | 60 | 600 |
| Samples per design, Table 13 | 30 | 300 |
| Samples per design, Table 17 and Table S2 | 20 | 500 |
| Bootstrap resamples per sample, Tables 17, S2, and S4 | 20 | 50 |
| Bootstrap resamples, Tables S1 and S3 | 30 | 200 |
| Samples per design, Table S4 | 10 | 300 |

Seeds: 777 (Tables 4 and 9), 4242 (Table 12), 20261122 and 20261121 (Table 18 at n = 300 and n = 1,000), 20261124 (Tables 11 and 19), 20261201 (Table 7), 20261202 (Table 8), 20261203 (Table 13), 20261204 (Tables 17 and S2), 20261205 (Tables S1 and S3), 20261206 (Table S4).

## Method notes

- The GPD is fitted by maximum likelihood through the profile likelihood in θ = ξ/β (a grid search followed by 60 golden-section steps). Samples are fitted in batches to limit memory; the batch size does not affect the results.
- The layer premium (Equation 1) and the derivatives of Appendix B are computed by 128-point Gauss–Legendre quadrature.
- True layer premiums of the simulation distributions are computed by composite 16-point Gauss–Legendre quadrature (relative accuracy about 1e-13).
- Thresholds are set at sample quantiles, with exceedances taken strictly above the threshold; on a threshold grid, quantiles that give the same threshold (ties) are counted once.
- Naive BMA weights are proportional to exp(−BIC/2), with BIC = −2ℓ + 3 log n and ℓ the GPD log-likelihood plus the binomial log-likelihood of the number of exceedances.
- In the real-data bootstrap (Tables 11 and 19), the threshold is re-set at the same sample quantile in each resample.
- In the simulations (Tables 7, 8, 17, S2, and S4), the threshold at the sample q-quantile is the order statistic leaving round((1 − q)n) exceedances. For real data and for the subsamples of Experiment A, the exceedances are the losses strictly above the sample quantile, because tied amounts are common in these files.
- If a threshold lies above the retention, the part of the layer below the threshold uses the empirical survival function of the sample.
- The mid-grid threshold is the middle quantile of the grid (the upper of the two middle ones when the grid has an even number of quantiles).
- Hill-plateau rule: at 60 candidate numbers of exceedances k (evenly spaced on the log scale from max(15, 0.02n) to n/2), the Hill estimate is computed; the window of six consecutive candidates with the smallest coefficient of variation is the plateau, and the rule takes its third candidate.
- MRL-linear rule: the automated mean-residual-life rule of Langousis et al. (2016, Section 2.2), as in the R function `mev::automrl`, with the same 60 candidates.
- TPW (Supplement S1) uses B bootstrap resamples of the whole sample, reused at every threshold; the trimmed version drops the lowest thresholds until ρ ≤ 1, keeping at least three.
- The mixture of Section 6.2 is spliced: a lognormal body (σ = 1.5) conditioned to lie below 3 with probability 0.95, and a Pareto tail S(x) = 0.05(x/3)^(−1/0.9) above 3.

## Reference for the MRL-linear rule

Langousis, A., A. Mamalakis, M. Puliga, and R. Deidda. 2016. Threshold detection for the generalized Pareto distribution: Review of representative methods and application to the NOAA NCDC daily rainfall database. *Water Resources Research* 52: 2659–2681.

## Outputs of a run

| Folder or file | Contents |
|---|---|
| `xol_data/` | Downloaded data files |
| `xol_out/*.csv` | Result tables |
| `xol_out/all_tables.txt` | All tables in one text file |
| `xol_out/run_log.txt` | Everything printed during the run |
| `xol_out/crash_report.txt` | Written only if Python itself crashes |
