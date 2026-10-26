# Fiber contact counts in 3D random fiber networks — reproducibility code

Code and notebooks reproducing the numerical results of the paper

> **Fiber contact counts in three-dimensional random networks: asymptotic
> normality, closed-form moments, and a Monte Carlo study**
> — *anonymous submission, under review.*

Author, affiliation, and venue information is withheld for anonymous review.
Anonymous repository: <https://anonymous.4open.science/r/fiber-contact-clt-3d-0081/>.

## Overview

Fibers are modeled as thin cylinders of radius `r` whose axes are random
segments: midpoints are seeded uniformly on an **enlarged region**
`S = [-lmax/2, 1+lmax/2]^3`, directions are isotropic, and lengths follow a
finite mixture. Two fibers are **in contact** when the minimum distance between
their axes is at most `2r` **and** the closest-point midpoint lies in the unit
cube `C = [0,1]^3` (the boundary-correcting seeding/counting convention of the
paper). The contact count is a degree-two `U`-statistic, and the code validates:

- the closed-form cube-truncated length moments `λ₁, λ₂, Var(ℓ_C)` and the
  isotropic mean sine `E[sin γ] = π/4`;
- the closed-form mean and variance, including the finite-`r` end-term
  correction `1 + 8r/L̄` and its square on the projection variance `ζ₁`;
- the leading contact-probability constant and its approach to `1 + 8r/L̄`;
- convergence of the standardized contact count to the standard normal law,
  the two variance components, and the crossover scale `n* = |C|/(2πr·Var(ℓ_C))`;
- the cube-versus-torus **dichotomy** (equal-length fibers): the boundary makes
  the limit Gaussian (`Var/C(n,2)` grows), the torus is degenerate (`Var/C(n,2)`
  flat);
- finite-sample coverage of closed-form confidence intervals, a
  method-of-moments estimator of `r` with delta-method intervals, robustness to
  anisotropic orientation, computational cost, and an **exact cell-list**
  (linked-cell) acceleration that returns the same count as brute force and runs
  in `O(n)` time in the fixed-density regime.

Every experiment uses the **same enlarged-seeding model**; the sweep reaches
`n = 3200`, past `n*`, so the projection-driven (boundary-induced) regime is
actually observed.

## Repository layout

```
.
├── data/                     # Monte Carlo outputs behind the figures/tables (CSV, NPZ)
├── notebooks/
│   ├── 01_truncated_length_moments.ipynb
│   ├── 02_contact_probability_and_rsweep.ipynb
│   ├── 03_variance_validation.ipynb
│   ├── 04_asymptotic_normality.ipynb
│   ├── 05_boundary_torus_dichotomy.ipynb
│   ├── 06_coverage_and_estimation.ipynb
│   ├── 07_anisotropy_and_timing.ipynb
│   └── 08_cell_list.ipynb
├── results/
│   ├── figures/              # generated figures (grayscale, 600 dpi, PNG + PDF)
│   └── tables/               # generated LaTeX tables
├── requirements.txt
└── README.md
```

## Notebooks

Each notebook is self-contained (all code in a single cell), begins with a
comment giving its name, writes its figures and tables into `results/` and its
raw outputs into `data/`, and uses a fixed random seed.

| Notebook | Reproduces | Main outputs |
|---|---|---|
| `01_truncated_length_moments` | Closed-form vs Monte Carlo `λ₁, λ₂, Var(ℓ_C)`, `E[sin γ] = π/4` | `table_moments`, `fig_ellC_hist` |
| `02_contact_probability_and_rsweep` | Pairwise contact probability with MC standard errors; `q_obs/q_pred → 1 + 8r/L̄` | `table_rsweep`, `fig_rsweep` |
| `03_variance_validation` | `U`-statistic mean and variance at `r = 0.002, n = 600`; crossover scale `n*` | `variance_validation.csv` |
| `04_asymptotic_normality` | Distribution of `N_n` (n = 200…3200); summary table and variance decomposition | `table_normality`, `table_variance_split`, `fig_skewness`, `fig_normal_hist`, `fig_normal_qq`, `fig_varsplit`, `normality_counts.npz` |
| `05_boundary_torus_dichotomy` | Cube vs torus (equal length): `Var/C(n,2)` grows vs flat | `table_dichotomy`, `fig_dichotomy` |
| `06_coverage_and_estimation` | Closed-form-standardized interval coverage; method-of-moments estimator of `r` with delta-method CIs | `table_coverage`, `table_estimator`, `fig_coverage` |
| `07_anisotropy_and_timing` | Isotropic vs oblate directions; wall-clock cost | `table_anisotropy`, `table_timing` |
| `08_cell_list` | Exact cell-list (linked-cell) acceleration; count matches brute force; `Θ(n)` in the fixed-density regime | `table_celllist_fixedwindow`, `table_celllist_fixeddensity`, `fig_celllist` |

Figures are saved in grayscale (seaborn) at 600 dpi in both PNG and PDF, without
titles or captions (captions live in the manuscript).

## Setup

Requires Python 3.10+.

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter lab      # or: jupyter notebook
```

Run the notebooks in order. Notebook `06` reads `data/normality_counts.npz`
written by notebook `04`, so run `04` before `06`.

## Reproducibility notes

- All randomness is seeded (`numpy.random.default_rng`), so a fresh run
  reproduces the reported numbers up to Monte Carlo error.
- The replicate counts committed here are **lighter** than those used for the
  paper, so the notebooks execute in a few minutes each. Each simulation-heavy
  notebook states its publication replicate counts in a comment (e.g.\ notebook
  `04` uses `R = 6000, 5000, 3500, 1800, 700` for `n = 200, 400, 800, 1600,
  3200`; notebook `02` uses `40000` target contacts per radius). Raising the
  replicate counts to those values reproduces the published tables and figures.
- The segment–segment distance uses the standard clamped (Lumelsky) scheme,
  vectorized over pairs.
- `ζ₁` is reported with the finite-`r` enhancement `(1 + 8r/L̄)²`; this is the
  form that matches the observed variance and calibrates the confidence
  intervals.

## Citation

```
@misc{anonymous_fiber_contact_3d,
  title  = {Fiber contact counts in three-dimensional random networks:
            asymptotic normality, closed-form moments, and a Monte Carlo study},
  author = {Anonymous},
  year   = {2026},
  note   = {Anonymous submission, under review}
}
```

## License

Released for peer review. A license will be added upon publication.
