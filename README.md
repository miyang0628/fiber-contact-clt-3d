# Fiber contact counts in 3D random fiber networks — reproducibility code

Code and notebooks reproducing the numerical results of the paper

> **Asymptotic distribution of fiber contact counts in three-dimensional
> random fiber networks** — *anonymous submission, under review.*

Author and venue information is withheld for double-blind review.

## Overview

Fibers are modeled as thin cylinders of radius `r` whose axes are random
segments in the unit cube: uniform midpoints, isotropic directions, and a
finite mixture of lengths. Two fibers are **in contact** when the minimum
distance between their axes is at most `2r`. The contact count is a degree-two
U-statistic, and the code validates:

- the closed-form cube-truncated length moments `λ₁, λ₂, Var(ℓ_C)` and the
  isotropic mean sine `E[sin γ] = π/4`;
- the boundary-corrected mean and the variance formula;
- the leading contact-probability constant and its finite-`r` correction
  `1 + 8r/L̄`;
- convergence of the standardized contact count to the standard normal law.

## Repository layout

```
.
├── data/                     # placeholder for cached inputs (empty)
├── notebooks/
│   ├── 01_truncated_length_moments.ipynb
│   ├── 02_contact_probability_and_rsweep.ipynb
│   ├── 03_variance_validation.ipynb
│   └── 04_asymptotic_normality.ipynb
├── results/
│   ├── figures/              # generated figures (grayscale, 600 dpi, PNG + PDF)
│   └── tables/               # generated LaTeX tables
├── requirements.txt
└── README.md
```

## Notebooks

Each notebook is self-contained (all code in a single cell), writes its figures
and tables into `results/`, and uses a fixed random seed.

| Notebook | Reproduces | Outputs |
|---|---|---|
| `01_truncated_length_moments` | Closed-form vs Monte Carlo moments `λ₁, λ₂, Var(ℓ_C)` and `E[sin γ] = π/4` | `table_moments.tex`, `fig_ellC_hist` |
| `02_contact_probability_and_rsweep` | Pairwise contact probability; `q_obs/q_pred → 1` as `r → 0` with the `1 + 8r/L̄` correction | `table_rsweep.tex`, `fig_rsweep` |
| `03_variance_validation` | U-statistic mean and variance at `r = 0.002`, `n = 600` | `table_variance.tex` |
| `04_asymptotic_normality` | Exhaustive counting board at `2r = 0.02`: skewness decay (log–log), histogram, normal Q–Q | `table_normality.tex`, `fig_skewness`, `fig_normal_hist`, `fig_normal_qq` |

Figures are saved in grayscale at 600 dpi in both PNG and PDF, without titles or
captions (captions live in the manuscript).

## Setup

Requires Python 3.10+.

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter lab      # or: jupyter notebook
```

Run the notebooks in order. Notebooks `03` and `04` are simulation-heavy and
take a few minutes each at the default replicate counts; raise `reps` (notebook
`04`) and `target_hits` (notebook `02`) for publication-quality estimates. The
figures shipped with the paper use 8000 replicates for `n ≤ 400` and 3500 for
`n = 800`.

## Reproducibility notes

- All randomness is seeded (`numpy.random.default_rng`), so a fresh run
  reproduces the reported numbers up to Monte Carlo error.
- The segment–segment distance uses the standard clamped (Lumelsky) scheme,
  vectorized over pairs.
- Closed-form and simulated moments agree to three significant figures; the
  finite-`r` discrepancies in the mean and variance are the `O(r)`
  spherocylinder end term documented in the manuscript appendix.

## Citation

```
@misc{anonymous_fiber_contact_3d,
  title  = {Asymptotic distribution of fiber contact counts in
            three-dimensional random fiber networks},
  author = {Anonymous},
  year   = {2026},
  note   = {Anonymous submission, under review}
}
```

## License

Released for peer review. A license will be added upon publication.
