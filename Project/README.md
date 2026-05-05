# ICS Cascade Classifier

A two-stage cascade Random Forest classifier for discriminating cyber attacks from sensor faults in Industrial Control Systems, evaluated on the WaDi A2 water distribution testbed.

## Overview

Most ICS anomaly detection systems produce a binary normal/anomalous output, leaving operators to determine whether a deviation is a hardware fault or an active cyber attack. These scenarios demand different responses (maintenance vs. incident response), and conflating them wastes response resources. This project implements a cascade classifier that explicitly separates the two:

- **Stage 1:** Detects any anomaly (normal vs. attack+fault) using a Random Forest trained on rolling statistical features.
- **Stage 2:** Classifies confirmed anomalies as either cyber attacks or sensor faults.

At a 2% normal false-positive rate budget, the cascade achieves **95.4% attack recall** and **80.9% fault recall** on WaDi A2.

## Dataset

This project uses **WaDi A2 (Water Distribution, November 2019)**, produced by the iTrust Centre for Research in Cyber Security at Singapore University of Technology and Design.

The dataset is not included in this repository. Request access at:
[https://itrust.sutd.edu.sg/itrust-labs_datasets/dataset_info/](https://itrust.sutd.edu.sg/itrust-labs_datasets/dataset_info/)

Place the downloaded files in `Dataset Downloads/WaDi.A2_19 Nov 2019/` before running the notebooks.

## Notebooks

Run in order:

| Notebook | Description |
|---|---|
| `NB1_DataPrep.ipynb` | Load and clean the raw WaDi A2 files, produce `wadi_prepared.parquet` |
| `NB2_FaultInjection.ipynb` | Inject synthetic sensor faults, produce `wadi_faulted.parquet` |
| `NB3_Features.ipynb` | Compute rolling statistical features, produce `features.parquet` |
| `NB4_Classifier.ipynb` | Train and evaluate the two-stage cascade classifier |
| `NB5_ResultsVisualization.ipynb` | Generate all figures and the confusion matrix |

## Fault Types

Five synthetic fault types are injected into normal sensor readings:

- **Bias** — sudden fixed offset proportional to the sensor IQR
- **Drift** — linear trend with superimposed Brownian noise
- **Precision Degradation** — additive Gaussian noise
- **Stuck-at** — sensor output frozen at the first fault-window value
- **Intermittent Dropout** — random NaN signal loss (burst or sparse)

Fault injection is restricted to closed-loop response sensors, with causal propagation to downstream sensors via data-driven models. See `fault_catalog.yaml` for the full injection configuration.

## Results

| FPR Budget | Attack Recall | Fault Recall | Normal FPR |
|---|---|---|---|
| 0.5% | 75.0% | 47.6% | 0.48% |
| 1.0% | 88.5% | 69.3% | 0.99% |
| 2.0% | 95.4% | 80.9% | 1.96% |

Stage 2 evaluated in isolation: **100% attack recall, 98.8% fault recall**.

## Author

George Lake — Idaho State University, CS6678 Advanced Machine Learning
