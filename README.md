# Detecting Cybersecurity Threats Using Deep Learning

A PyTorch binary-classification project that detects suspicious events in system-log data using the BETH cybersecurity dataset.

## Project overview

This project builds a feed-forward neural network to classify system events as:

- `0` — benign
- `1` — suspicious

The workflow includes feature/label separation, feature scaling, conversion to PyTorch tensors, mini-batch training, validation, and test evaluation.

## Dataset

The project uses preprocessed data derived from the **BETH dataset**, a cybersecurity dataset designed for anomaly-detection research.

Input features used by the model:

- `processId`
- `threadId`
- `parentProcessId`
- `userId`
- `mountNamespace`
- `argsNum`
- `returnValue`

Target:

- `sus_label`

The dataset files are intentionally not included in this repository. See `accreditation.md` for the original dataset source and paper.

## Model architecture

The network is implemented in PyTorch:

```text
7 input features
        ↓
Linear(7, 32)
        ↓
ReLU
        ↓
Linear(32, 16)
        ↓
ReLU
        ↓
Linear(16, 1)
```

Training configuration:

- Loss: `BCEWithLogitsLoss`
- Optimizer: SGD
- Learning rate: `1e-3`
- Weight decay: `1e-4`
- Batch size: `64`
- Epochs: `10`
- Random seed: `42`

## Preprocessing

`StandardScaler` is fit only on the training data. The fitted scaler is then used to transform validation and test features to avoid information leakage.

## Results

Final saved run:

| Metric | Result |
|---|---:|
| Validation accuracy | 1.0000 |
| Test accuracy | 0.1554 |

Training loss decreased from approximately `0.1234` in epoch 1 to `0.0035` in epoch 10.

## Important limitation

The data splits are extremely imbalanced and their class distributions differ substantially. Because of that, raw accuracy alone is not sufficient for judging model quality, and the validation score should not be interpreted as proof of perfect real-world threat detection.

A stronger follow-up version would add:

- confusion matrix
- precision and recall
- F1 score
- ROC-AUC / PR-AUC
- class weighting or resampling
- threshold tuning
- investigation of distribution shift between train, validation, and test sets

## Repository structure

```text
cybersecurity-threat-detection-pytorch/
├── cybersecurity_threat_detection.ipynb
├── README.md
├── requirements.txt
├── accreditation.md
└── .gitignore
```

## Installation

```bash
pip install -r requirements.txt
```

## Run the project

1. Download the BETH data referenced in `accreditation.md`.
2. Place the required processed CSV files in the notebook directory:
   - `labelled_train.csv`
   - `labelled_validation.csv`
   - `labelled_test.csv`
3. Open `cybersecurity_threat_detection.ipynb`.
4. Run the notebook from top to bottom.

## Skills demonstrated

- Python
- pandas
- scikit-learn
- PyTorch
- neural-network classification
- feature scaling
- mini-batch training
- validation and test evaluation
- cybersecurity anomaly detection

## Future improvements

The next iteration should focus on evaluating performance on the minority class rather than relying primarily on accuracy. This would make the project substantially stronger as a machine-learning portfolio example.
