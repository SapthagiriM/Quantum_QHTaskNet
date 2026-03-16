# QHTaskNet v2.46 — Variational Quantum-Classical Neural Network for Deadline-Aware Task Offloading

[![Python 3.10](https://img.shields.io/badge/Python-3.10-blue)](https://python.org)
[![PennyLane 0.37](https://img.shields.io/badge/PennyLane-0.37.0-purple)](https://pennylane.ai)
[![TensorFlow 2.15](https://img.shields.io/badge/TensorFlow-2.15.0-orange)](https://tensorflow.org)
[![Platform: Apple M1](https://img.shields.io/badge/Platform-Apple%20M1-black)](https://www.apple.com/mac/m1)

A residual hybrid quantum-classical neural network that learns deadline-aware task offloading policies in Edge-IoT environments. An 8-qubit variational quantum circuit (VQC) runs in parallel with a classical skip connection; their fused representations are processed by a dense stack that predicts the cost-benefit of offloading each task.

---

## Model Architecture

```
Input (18 features)
    └─► Dense(8, tanh)          ← pre-processing projection
             │
    ┌────────┴────────┐
    ▼                 ▼
VQC (8 qubits)    Skip (8-dim)   ← quantum branch ‖ classical branch
    └────────┬────────┘
       Concat (16-dim)
       LayerNorm
       Dense(64, relu) → Dropout(0.2)
       Dense(32, relu) → Dropout(0.3)
       Dense(16, relu) → Dropout(0.3)
       Dense(1,  tanh)           ← predicted offloading cost-benefit
```

**Quantum circuit:** 3 variational layers, dual-basis data re-uploading (RY + RZ), depolarizing noise model (p = 0.01).
**Parameters:** 48 quantum + 3,905 classical = **3,953 total**.

---

## Key Results

| Metric | QHTaskNet | Classical NN | SVR (RBF) | RFF |
|---|---|---|---|---|
| MAE | 0.1886 | 0.1133 | 0.0904 | 0.3289 |
| R² | 0.3242 | 0.6991 | 0.8695 | 0.0286 |
| Direction Accuracy | **84.3%** | 76.3% | 74.2% | 39.6% |
| Deadline Met Rate | **95.0%** | 95.0% | 92.0% | 93.7% |

**Monte Carlo cross-validation** (10-fold, 2,500 tasks): mean deadline satisfaction **94.33% ± 1.63%** (95% CI: 93.68–96.06%).

**Noise robustness:** Deadline met rate holds at **95.0%** even at depolarizing noise p = 0.10.

---

## Repository Contents

```
QH-TaskNet_v2.46_M1.ipynb   # Main notebook (Apple M1 build)
results_summary.json         # All numerical results (ground truth)
visualizations/              # Generated figures (PNG)
  ├── training_loss_accuracy.png
  ├── regression_parity.png
  ├── monte_carlo_analysis.png
  ├── ablation_study.png
  ├── boundary_analysis.png
  ├── quantum_embeddings.png
  ├── task_performance.png
  ├── baseline_comparison.png
  ├── network_visualization.png
  ├── QHTasknet_Model_Diagram.png
  └── qh_tasknet_quantum_circuit.png
```

---

## Setup

### Requirements

```
python >= 3.10
pennylane == 0.37.0
tensorflow == 2.15.0        # tensorflow-macos on Apple Silicon
scikit-learn
scipy
pandas
numpy
matplotlib
seaborn
tqdm
```

### Install

```bash
# Apple M1 / Apple Silicon
pip install tensorflow-macos==2.15.0 tensorflow-metal
pip install pennylane==0.37.0
pip install scikit-learn scipy pandas numpy matplotlib seaborn tqdm
```

### Run

Open `QH-TaskNet_v2.46_M1.ipynb` in JupyterLab and run all cells.
Estimated runtime on Apple M1:

| Phase | Description | Time |
|---|---|---|
| 1 | Dataset generation (10K tasks) | ~2 min |
| 2 | Feature engineering | ~1 min |
| 3 | Classical baselines (NN, SVR, RFF) | ~3 min |
| 4 | QHTaskNet training | ~45 min |
| 5 | Evaluation & benchmark | ~5 min |
| 6 | Noise ablation | ~5 min |
| 7 | Monte Carlo cross-validation | ~5 min |
| 8 | Sensitivity analysis | ~10 min |
| 9 | Quantum embeddings analysis | ~5 min |
| 10 | Latency profiling | ~2 min |

---

## Dataset

Synthetically generated Edge-IoT task offloading dataset (10,000 samples).
50% local-biased / 50% offload-biased for class balance.

**18 input features:** task size, CPU cycles, memory requirement, deadline, priority, device CPU/memory/battery/energy, deadline headroom, compute density, server CPU/memory/available CPU/available memory/energy, bandwidth, latency.

**Splits:** 8,500 train / 1,500 validation / 2,500 MC evaluation / 300 final evaluation.

---

## Training Configuration

| Hyperparameter | Value |
|---|---|
| Quantum LR | 0.02 |
| Classical LR | 0.005 |
| Epochs | 75 (early stop patience 20) |
| Batch size | 64 |
| Loss | Hybrid decision loss (regression + deadline penalty) |
| LR schedule | Halved at epochs 25, 50, 75 |
| Backend | `default.qubit` (PennyLane) |
| Noise model | Depolarizing, p = 0.01 |

---

## Citation

If you use this code or results in your work, please cite:

```
Miriyala, S., & Chirra, V. R. (2025). QHTaskNet: A Variational Quantum-Classical
Neural Network for Deadline-Aware Task Offloading in Edge-IoT.
```

---

## License

This project is released for academic and research use.
