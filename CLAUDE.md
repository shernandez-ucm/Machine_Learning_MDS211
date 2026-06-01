# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Course repository for **MDS211 - Machine Learning** at Universidad Católica del Maule (UCM). Contains lecture PDFs in `clases/` and interactive Jupyter lab notebooks in `laboratorios/`. Each notebook is a standalone lab exercise.

## Environment setup

```bash
# Activate the included virtual environment (Python 3.12)
source env/bin/activate

# Or install dependencies globally
pip install -r requirements.txt
```

Dependencies: `numpy`, `matplotlib`, `scikit-learn`, `jupyter`

## Running notebooks

```bash
source env/bin/activate
jupyter lab          # or: jupyter notebook
```

To execute a notebook non-interactively:

```bash
jupyter nbconvert --to notebook --execute laboratorios/<notebook>.ipynb --output <notebook>_out.ipynb
```

## Repository structure

- `laboratorios/` — Lab notebooks (one per topic, self-contained)
- `clases/` — PDF lecture slides (introducción, feature engineering, regularización)
- `MDS211(Machine Learning).pdf` — Full course reference PDF
- `env/` — Local virtual environment (not committed in full; `env/` is gitignored except `pyvenv.cfg`)

## Notebook conventions

Labs follow a numbered section structure (1. Import → 2. Generate data → 3. Train → 4. Visualize → 5. Evaluate). Each section has a markdown cell explaining its purpose before the code cell. Plots use `matplotlib` with `plt.show()` at the end of each visualization block. Models are trained with `random_state=42` and data split 70/30 train/test using `stratify=y`.
