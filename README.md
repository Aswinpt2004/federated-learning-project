# Federated Learning Project (NSL-KDD)

This project implements a full cybersecurity intrusion-detection workflow using the NSL-KDD dataset, with both a centralized baseline and a federated learning simulation using Flower.

## Classification Task

Primary task: binary classification.
- Normal traffic -> 0
- Attack traffic -> 1

This enables direct comparison between:
- centralized baseline model metrics
- global federated model metrics

## Main Notebook

- intial_spec_fl_project.ipynb

The notebook contains the full pipeline:
1. Data loading and preprocessing
2. Centralized baseline training
3. Non-IID client partitioning
4. Federated simulation (FedAvg)
5. Held-out test evaluation and comparison

## Dataset Files (expected in project root)

The workflow uses NSL-KDD TXT files (no header row):
- KDDTrain+.txt
- KDDTrain+_20Percent.txt
- KDDTest+.txt
- KDDTest-21.txt

ARFF files are optional reference files and are not required for Python training.

## Preprocessing Summary

- Manual assignment of 43 NSL-KDD columns
- Drop difficulty column
- Label binarization (normal vs attack)
- One-hot encoding for protocol_type, service, and flag
- Train/val split: 80/20, stratified
- StandardScaler fit on training split only
- Macro metrics reported: precision, recall, F1, plus accuracy and AUC

## Model and Training

Shared MLP architecture:
- Input: 112 features (after encoding)
- Hidden layers: 256 -> 128 -> 64
- ReLU activations, BatchNorm and Dropout in first two hidden blocks
- Output: single logit

Training setup:
- Loss: BCEWithLogitsLoss with pos_weight
- Optimizer: Adam
- Centralized: up to 30 epochs with early stopping

## Federated Setup (Current)

-Current notebook configuration is optimized for practical single-machine execution:
- Total clients: 5
- Clients per round: 3
- Rounds: 20
- Local epochs: 3
- Strategy: FedAvg

## Generated Artifacts

Typical generated outputs:
- scaler.pkl
- centralized_baseline.pt
- baseline_metrics.json
- client_data.pkl
- global_federated_model.npy
- results_comparison.csv

## Environment

Recommended Python packages:
- torch
- flwr[simulation]
- scikit-learn
- pandas
- numpy
- matplotlib
- seaborn

## Run Instructions

1. Create and activate a virtual environment.
2. Install required packages.
3. Open and run intial_spec_fl_project.ipynb from top to bottom.
4. Review final comparison metrics and plots.

## Git Notes

The repository includes a project-specific .gitignore that excludes:
- virtual environments
- checkpoints and caches
- generated model/plot artifacts
- raw NSL-KDD dataset files

Keep local data files in the workspace when running the notebook, but do not commit them to Git.
