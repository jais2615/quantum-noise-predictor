# Quantum Circuit Noise Prediction

A research project exploring whether the noise sensitivity of quantum circuits can be predicted from circuit structure, hardware topology, calibration parameters, and input-data features.

## Objective

The project follows:

Classical Data
      ↓
Quantum Feature Encoding
      ↓
Circuit Generation
      ↓
Hardware-Aware Noise Simulation
      ↓
Noise Dataset
      ↓
ML Noise Predictor
      ↓
Real Hardware Validation

The main research question is:

> can a two-stage machine learning predictor first estimating a circuit's hardware-level behavior, then translating that into an estimate of real-world classification performance correctly identify, before any hardware execution, which circuit design will best preserve accuracy on a genuine real-world classification task? Further, can this predictor be used not just to select among existing designs, but to actively guide the discovery of improved circuit configurations, validated on real IBM Quantum hardware under a constrained hardware budget?


## Current Progress

### Classical Input

The Blood Cell NNN dataset is used only as a source of realistic numerical feature values.

Five features are encoded into a 5-qubit quantum circuit.

### Quantum Circuits

The project uses Qiskit's `ZZFeatureMap` with:

- 5 qubits
- 3 repetition levels: `1`, `2`, `3`
- 2 entanglement patterns: `linear`, `full`
- 4 hardware layouts

This gives:

200 data rows × 6 configurations × 4 layouts
= 4,800 circuit instances

Each circuit is evaluated with 1024 shots and 5 noisy repetitions.

## Target

The primary noise metric is Total Variation Distance (TVD):

TVD(P,Q) = 1/2 × Σ |P(x) - Q(x)|

where `P` is the ideal output distribution and `Q` is the noisy distribution.

The primary prediction target is `mean_tvd`.

## Features

The dataset includes:

- input features and encoded angles
- abstract circuit features
- transpiled circuit features
- two-qubit gate statistics
- hardware topology and layout information
- CZ error and duration statistics
- T1/T2 coherence features

## Dataset

Current dataset:

- 4,800 rows
- 61 columns

The main dataset was generated using the Qiskit `FakeTorino` calibration snapshot.

The dataset has passed basic integrity checks with no missing values or duplicate rows.

## Validation

A subset of circuits was validated using:

- `FakeKingston`
- real `ibm_kingston`

Results for the 12-circuit validation set:

| Comparison | Correlation | MAE |
|---|---:|---:|
| FakeKingston vs Real Kingston | 0.826 | 0.0229 |
| FakeTorino vs Real Kingston | 0.415 | 0.1294 |

These results suggest that hardware calibration is backend-dependent and that a corresponding fake-backend model can reasonably reproduce its real hardware behavior.

## Repository Structure

quantum-noise-predictor/
├── notebooks/
├── data/
├── results/
├── README.md
├── requirements.txt
└── .gitignore


