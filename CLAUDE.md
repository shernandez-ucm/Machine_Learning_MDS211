# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Course repository for **MDS211 - Machine Learning** at Universidad Católica del Maule (UCM). It holds three kinds of teaching material: interactive Jupyter lab notebooks in `laboratorios/`, the Beamer/LaTeX sources for the lecture slides in `latex/`, and the compiled lecture PDFs (plus the full course reference) in `clases/`. Each notebook is a standalone lab exercise; each `.tex` in `latex/` compiles to a numbered PDF in `clases/`. All content is in Spanish.

## Environment setup

```bash
# Activate the included virtual environment (Python 3.14)
source env/bin/activate

# Or install dependencies globally
pip install -r requirements.txt
```

Dependencies: `numpy`, `matplotlib`, `scikit-learn`, `jupyter`, plus `jax[cuda12]`, `flax`, and `optax` for the JAX-based labs (4 & 5). Note `requirements.txt` is otherwise minimal — `scipy` is also imported (e.g. `scipy.stats.loguniform` in lab 2) but pulled in transitively as a scikit-learn dependency. Labs 4–5 run on CPU if no GPU is available (JAX prints a harmless CUDA OOM warning when a GPU is present but busy).

## Running notebooks

```bash
source env/bin/activate
jupyter lab          # or: jupyter notebook
```

To execute a notebook non-interactively:

```bash
jupyter nbconvert --to notebook --execute laboratorios/<notebook>.ipynb --output <notebook>_out.ipynb
```

## Building the LaTeX slides

Slides are Beamer documents; the assignment handout (`tarea_pjud_prediccion.tex`) is an `article`. Compile from inside `latex/`:

```bash
cd latex
pdflatex introduccion.tex      # run twice if frame numbers / refs look stale
./clean.sh                     # delete .aux/.log/.nav/.snm/.toc/... aux files (pass a dir to scope it)
```

The Beamer decks pull in `tikz`, `pgfplots`, `tcolorbox`, and `babel`(spanish), so build with a full TeX distribution (TeX Live / MacTeX). After compiling, move/copy the PDF into `clases/` under its course-order number (e.g. `introduccion.tex` → `clases/1.-introduccion.pdf`).

## Repository structure

- `laboratorios/` — Lab notebooks (one per topic, self-contained, numbered):
  - `1.-laboratorio_bayes_optimal.ipynb` — Bayes-optimal classifier via per-class `GaussianMixture`, decision boundaries, KNN comparison
  - `2.-laboratorio_regularizacion_feature_engineering.ipynb` — high-dimensional regression, `Pipeline` feature engineering, Ridge/Lasso, `RandomizedSearchCV`, SVD multicollinearity diagnosis
  - `3.-laboratorio_ensambles.ipynb` — ensemble methods (bagging/boosting/random forests)
  - `4.-laboratorio_redes_neuronales_mlp.ipynb` — MLP neural networks (hand-rolled in JAX)
  - `5.-laboratorio_optimizadores_deep_learning.ipynb` — deep-learning optimizers (GD/SGD/momentum/Nesterov/AdaGrad/RMSProp/Adam + LR schedules) with JAX + `flax.nnx` + Optax; 2D trajectory demo and a two-moons MLP
- `latex/` — Beamer slide sources (`.tex`) plus the `tarea_pjud_prediccion.tex` assignment handout and `clean.sh` (removes LaTeX aux files). Each slide deck compiles to the matching PDF in `clases/`.
- `clases/` — Compiled lecture PDFs, numbered by course order (`1.-introduccion`, `2.-feature_engineering`, … `6.-redes_neuronales`).
- `MDS211(Machine Learning).pdf` — Full course reference PDF
- `env/` — Local virtual environment (not committed in full; `env/` is gitignored except `pyvenv.cfg`)

## Notebook conventions

Labs follow a numbered section structure (1. Import → 2. Generate data → 3. Train → 4. Visualize → 5. Evaluate), each preceded by a Spanish markdown cell explaining its purpose. Content (markdown, comments, variable framing) is in Spanish. Plots use `matplotlib` with `plt.show()` at the end of each visualization block.

Reproducibility: fix the seed to `42` on every stochastic call. Lab 1 inlines `random_state=42`; lab 2 defines a `RANDOM_STATE` constant and threads it through `make_regression`, `train_test_split`, `RandomizedSearchCV`, and the estimators — prefer the constant pattern in new notebooks. Data is split train/test with `train_test_split`, using `stratify=y` for classification labs.

Preprocessing in regression labs is wrapped in an sklearn `Pipeline` (`StandardScaler` → `PolynomialFeatures` → estimator) so feature engineering and the model are tuned together; hyperparameters are searched with `RandomizedSearchCV`.

## Slide (LaTeX) conventions

Every Beamer deck shares the same preamble: `\documentclass[aspectratio=169, 10pt]{beamer}`, `\usetheme{default}`, navigation symbols disabled, frame-number footline. The UCM palette is defined inline at the top of each file — `UCMBlue` (RGB 4,171,226) drives frame titles, structure, and block titles; `UCMNavy` and the `Stanford*`/`Accent*` colors are also declared. When adding or restyling a deck, copy this preamble block verbatim rather than inventing new colors or themes so the decks stay visually consistent. Slide body text is Spanish.
