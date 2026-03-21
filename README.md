[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/g5Rk6CRe)
[![Run Notebook](https://github.com/eisenhauerIO/projects-businss-decisions/actions/workflows/run-notebook.yml/badge.svg)](https://github.com/eisenhauerIO/projects-businss-decisions/actions/workflows/run-notebook.yml)
[![Ruff](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json)](https://github.com/astral-sh/ruff)

# Smart Green Nudging —  Replication

## Paper

**Smart Green Nudging: Reducing Product Returns Through Digital Footprints and Causal Machine Learning**  
*Moritz von Zahn, Kevin Bauer, Cristina Mihale-Wilson, Johanna Jagow, Maximilian Speicher, Oliver Hinz*  
*Marketing Science, Published Online: 8 Aug 2024*

Online appendix and data files are publicly available at:  
https://doi.org/10.1287/mksc.2022.0393

---

## Project

This project replicates the **preliminary estimation stage** of *Smart Green Nudging*, focusing on the paper's use of causal machine learning (CML) to estimate heterogeneous treatment effects of green nudges based on consumers' digital footprint and shopping cart data.

Rather than targeting exact numerical equivalence, the goal is to reproduce the **causal estimation strategy** and the **qualitative heterogeneity patterns** reported in the original study. Results, comparisons to the original outputs, and a full discussion of reproducibility are presented in [`replication_notebook.ipynb`](replication_notebook.ipynb).

---

## Data

The original study uses transaction-level digital footprint and cart data. A simulated version of this data, along with the simulation methodology, is **publicly available** through the paper's online appendix and data repository:

> https://doi.org/10.1287/mksc.2022.0393

All data used in this replication are obtained directly from the authors' publicly released files. Current data files in this repo:
- `data/train_data.csv`
- `data/test_data.csv`

---

## Methodology Overview

The original paper estimates causal effects of green nudges on product return behavior using causal machine learning techniques that allow treatment effects to vary flexibly across individuals.

This replication follows the same identification logic and focuses on:

- Estimating **heterogeneous treatment effects (HTEs)** rather than only average treatment effects (ATEs)
- Leveraging high-dimensional covariates derived from digital footprint and cart characteristics
- Using a **Causal Forest in a Double Machine Learning (DML) framework**, following Athey et al. (2019)

All models are implemented in Python using the `econml` library.

---

## Repository Structure

```
mksc.2022.0393/
├── data/
│   ├── train_data.csv
│   └── test_data.csv
├── model/
│   └── hyperparam.txt
├── plots/
├── src/
│   ├── cml_training.py
│   ├── cml_evaluation.py
│   └── simulate_data.py
├── environment.yaml
├── replication_notebook.ipynb
└── README.md
```

---

## Installation

This project uses a conda environment. To replicate the environment:

```bash
conda env create -f environment.yaml
conda activate mksc
```

---

## Usage

Generate synthetic data:

```bash
python src/simulate_data.py
```

Train the Causal Forest DML model:

```bash
python src/cml_training.py
```

Evaluate model outputs and generate plots:

```bash
python src/cml_evaluation.py
```

Or run the full replication end-to-end in the notebook:

```bash
jupyter notebook replication_notebook.ipynb
```

---

## Environment Notes

This replication was developed using **Python 3.11.15** on Windows. Several compatibility issues were encountered and resolved:

1. **`econml` patch** — `econml/utilities.py` line 1519: `from pkg_resources import parse_version` replaced with `from packaging.version import Version as parse_version` for Python 3.11+ compatibility.
2. **Matplotlib backend** — `matplotlib.use('Agg')` added to `cml_evaluation.py` to force a non-interactive backend, as the default backend fails silently when running from a terminal without a GUI display context on Windows.
3. **LaTeX rendering** — `plt.rcParams['text.usetex'] = False` added after the `scienceplots` style is set, as the `science` style defaults to requiring a LaTeX installation. Setting this to `False` uses matplotlib's built-in math renderer instead.
4. **Missing dependency** — `scienceplots==2.2.0` was added as a missing dependency not included in the original package.

---

## Replication Notes

The replicated IPS curve baseline (≈0.29) differs from the published figure (≈0.265). This reflects a difference in the underlying dataset rather than a failure of replication — the publicly available simulated data differs from the original proprietary experimental data, shifting the curve vertically. The curve shape, policy ordering, and relative improvement over the random baseline are all successfully reproduced.

The causal forest does not set a `random_state`, meaning results vary slightly across runs. This matches the original implementation and is discussed in detail in the notebook.

---

## Citation

```bibtex
@article{vonZahn2024,
  title     = {Smart Green Nudging: Reducing Product Returns through Digital Footprints and Causal Machine Learning},
  author    = {von Zahn, Moritz and Bauer, Kevin and Mihale-Wilson, Cristina and Jagow, Johanna and Speicher, Maximilian and Hinz, Oliver},
  journal   = {Marketing Science},
  year      = {2024},
  doi       = {10.1287/mksc.2022.0393}
}
```
