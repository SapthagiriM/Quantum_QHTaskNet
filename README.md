# Quantum_QHTaskNet
A novel quantum-classical hybrid neural network for intelligent task offloading in edge computing environments, combining quantum machine learning with classical deep learning to optimize resource allocation in IoT systems

# Overview
QH-TaskNet addresses the critical challenge of dynamic task offloading in edge computing by leveraging quantum computing advantages for feature processing combined with classical neural networks for decision-making. The system optimizes multiple objectives including execution time, energy consumption, and deadline satisfaction.

🌟 Key Features
Quantum-Classical Hybrid Architecture: Integrates PennyLane quantum circuits with TensorFlow for enhanced feature representation
Regression-Based Decision Making: Predicts cost differentials between local and edge execution
Comprehensive Ablation Studies: Systematic evaluation of quantum layers, noise resilience, and hyperparameters
Statistical Validation: Monte Carlo cross-validation with confidence intervals
GPU Acceleration: Optimized for NVIDIA GPUs with mixed precision training
Strong Classical Baselines: Comparison against SVR, Random Fourier Features, and parameter-matched neural networks

# Requirements
pip install numpy pandas matplotlib pennylane tensorflow scikit-learn networkx tqdm joblib scipy psutil

Minimum Hardware Requirements:

GPU: NVIDIA GPU with CUDA support (8GB+ VRAM recommended)
CPU: Multi-core processor (6+ cores recommended)
RAM: 16GB+ recommended

# Dataset
filename: final_training_dataset

Dataset Sizes:
Training tasks: 10,000
Validation tasks: 2,500
Test tasks (Monte Carlo): 2,500
Final held-out tasks: 2,500

📊 Model Architecture
Input Layer: 18-dimensional feature vector (task + server characteristics)
Feature Projection: Learned compression to quantum dimension
Quantum Layer: Variational quantum circuit with data re-uploading
Classical Post-Processing: Dense layers with dropout and skip connections
Output: Cost delta prediction (local_cost - server_cost)

🔬 Experimental Design
Ablation Studies
Quantum Components: Qubit scaling (4, 6, 8 qubits)
Noise Resilience: Quantum noise levels (0.0, 0.01, 0.05, 0.1)
Architecture: Variational layers (1, 2)
Hyperparameters: Learning rate, batch size optimization

# Evaluation Metrics
Deadline satisfaction rate
Average execution time
Energy consumption
Offloading decision accuracy
Statistical significance (p-values, confidence intervals)

📈 Results Highlights
Deadline Satisfaction: Outperforms local-only and random baselines
Quantum Advantage: Demonstrated improvement over classical architectures
Noise Robustness: Maintains performance under realistic quantum noise
Scalability: Efficient training on 10K+ task samples

#Project Structure

QH-TaskNet/
├── QH-TaskNet_v2.453_GPU.ipynb  # Main notebook
├── visualizations/               # Generated plots
│   ├── ablations/               # Ablation study results
│   ├── training_history.png
│   ├── decision_boundary_analysis.png
│   └── quantum_comparison.png
├── performance_logs/            # Training metrics
├── experiment_config.json       # Initial configuration
└── optimized_config.json        # Best hyperparameters


🎯 Key Components
Environment Simulation
Edge servers with heterogeneous resources
IoT devices with battery constraints
Network topology with bandwidth/latency modeling
Realistic noise models (packet drops, jitter)
Task Generation
Two-phase balanced dataset creation
Guaranteed 50/50 local/offload distribution
Diverse task characteristics (size, deadline, priority)
Decision Framework
Multi-criteria cost function
Safety-aware offloading rules
Battery-conscious scheduling

📊 Visualization Suite
The notebook automatically generates:
Training convergence plots
Decision boundary analysis
Feature importance rankings
Network topology visualization
Monte Carlo statistical distributions
Quantum vs. classical comparisons

Configuration
Key hyperparameters in Config class:

num_qubits = 4              # Quantum circuit size
num_layers = 2              # Variational layers
learning_rate = 0.005       # Optimizer learning rate
batch_size = 128            # Training batch size
epochs = 30                 # Training epochs
