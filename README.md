# Modelling Animal Movement Dynamics using Hidden Markov Models

A comprehensive implementation of Hidden Markov Models (HMMs) for animal movement analysis using GPS telemetry data. This project reproduces and extends the methodologies from Chapter 18 of *Hidden Markov Models for Time Series* by Zucchini et al., including behavioral state decoding, dwell-time analysis, population heterogeneity, and movement forecasting.

---

# Overview

This project models animal movement patterns using discrete-time Hidden Markov Models (HMMs). The framework analyzes bivariate time series derived from GPS coordinates:

- **Step Lengths** — Euclidean distances between consecutive locations
- **Turning Angles** — Directional changes between movements

The models classify animal behavior into hidden states such as:

- **Encamped / Resting**
- **Exploring / Moving**

The implementation includes:

- 2-State and 3-State HMMs
- Viterbi decoding
- Forward algorithm likelihood estimation
- Dwell-time diagnostics for HSMM justification
- Population heterogeneity analysis with Mixed HMM motivation
- Future movement forecasting

---

# Project Structure

```bash
.
├── HMM_Model.ipynb / HMM_Model.py
├── Report_HMMs_for_animal_movement.pdf
├── haggis_data.RData
├── elk_data.RData
├── haggis_data.csv
├── elk_data.csv
└── README.md
```

---

# Features

## 1. Data Preprocessing

Converts raw GPS coordinates into movement metrics:

- Step lengths
- Absolute headings
- Turning angles

Includes:

- Coordinate differencing
- Angle wrapping to [-π, π]
- Invalid step filtering

---

## 2. Hidden Markov Models

### 2-State HMM

Models behavioral switching between:

| State | Interpretation |
|---|---|
| State 1 | Encamped / Resting |
| State 2 | Exploring / Moving |

Emission distributions:

- Gamma distribution for step lengths
- von Mises distribution for turning angles

### 3-State HMM

Extends the baseline model for additional behavioral complexity.

---

## 3. Model Selection

Compares models using:

- Log-Likelihood
- AIC
- BIC

Example result from the Haggis dataset:

| States | Parameters | Log-Likelihood | AIC | BIC |
|---|---|---|---|---|
| 2 | 10 | -3456.01 | 6932.02 | 6982.87 |
| 3 | 18 | -3456.96 | 6949.92 | 7041.45 |

The 2-state model achieved the best balance between fit and complexity.

---

## 4. Viterbi Decoding

Uses the Viterbi algorithm to recover the most likely hidden behavioral state sequence from observed movement data.

The decoded states are visualized directly on GPS trajectories.

---

## 5. Dwell-Time Analysis (HSMM Motivation)

Evaluates whether geometric dwell-time assumptions are realistic.

The project demonstrates that Elk movement data violate the standard HMM geometric assumption, motivating the use of Hidden Semi-Markov Models (HSMMs).

---

## 6. Population Heterogeneity

Fits separate HMMs to multiple Elk subjects to demonstrate behavioral variability across individuals.

Example:

| Animal | p11 | p22 |
|---|---|---|
| elk-163 | 0.937 | 0.262 |
| elk-287 | 0.914 | 0.672 |

This supports the use of Mixed HMMs with random effects.

---

## 7. Movement Forecasting

The project extends traditional HMM analysis into forecasting by predicting:

- Future behavioral state probabilities
- Expected step lengths
- Expected turning angles
- Future coordinates

---

# Mathematical Framework

## Transition Matrix

For a 2-state model:

\[
\Gamma =
\begin{pmatrix}
p_{11} & 1 - p_{11} \\
1 - p_{22} & p_{22}
\end{pmatrix}
\]

## Emission Distributions

### Step Lengths

\[
L_t \sim \text{Gamma}(\alpha_i, \beta_i)
\]

### Turning Angles

\[
\Theta_t \sim \text{von Mises}(\mu_i, \kappa_i)
\]

---

# Diagnostic Visualizations

The implementation generates:

- LOESS smoothing plots
- Autocorrelation Functions (ACF)
- Marginal and state-dependent distributions
- Viterbi GPS maps
- Dwell-time comparisons

---

# Installation

## Clone the Repository

```bash
git clone https://github.com/yourusername/hmm-animal-movement.git
cd hmm-animal-movement
```

## Install Dependencies

```bash
pip install numpy pandas scipy matplotlib statsmodels pyreadr
```

---

# Usage

## Run the Full Pipeline

```bash
python HMM_Model.py
```

or run the Jupyter Notebook:

```bash
jupyter notebook
```

---

# Required Libraries

```python
import numpy as np
import pandas as pd
import pyreadr
import matplotlib.pyplot as plt
import statsmodels.api as sm

from scipy.stats import gamma, vonmises, geom
from scipy.optimize import minimize
from statsmodels.graphics.tsaplots import plot_acf
```

---

# Datasets

## Haggis Dataset

Synthetic movement data used for baseline HMM model selection.

## Elk Dataset

Real telemetry data used for:

- HSMM justification
- Mixed HMM analysis
- Forecasting experiments

---

# Results

## Haggis Behavioral States

| State | Step Mean | Step Variance | Turn Mean | Kappa |
|---|---|---|---|---|
| Encamped | 1.00 | 0.24 | -3.110 | 1.035 |
| Exploring | 5.02 | 9.25 | -0.308 | 8.734 |

Interpretation:

- Encamped states exhibit short movements and frequent reversals
- Exploring states exhibit long persistent movement

---

# Applications

This framework can be applied to:

- Wildlife ecology
- Animal migration studies
- Conservation biology
- Behavioral state inference
- Spatial forecasting
- Movement ecology research

---

# References

- Zucchini, W., MacDonald, I. L., & Langrock, R.  
  *Hidden Markov Models for Time Series: An Introduction Using R*

- Chapter 18: HMMs for Animal Movement

---

# Authors

- Tran Quoc Bao
- Nguyen Hanh Dan

Course: **Stochastic Modeling**

---

# License

This project is intended for educational and research purposes.
