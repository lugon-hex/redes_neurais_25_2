# Análise de Redes Neurais 
Trabalho sobre Tópicos em IA 3 - 2025.2

# documentation 

# Double Pendulum: AI Project (PINN & LSTM)

## Overview
This project evaluates the performance of Long Short-Term Memory (LSTM) recurrent neural networks—both in standard configurations and augmented with Physics-Informed Neural Network (PINN) losses—for forecasting the dynamics of simple and double pendulum systems. This work explores how physical constraints impact model stability in systems ranging from predictable nonlinear oscillators to complex chaotic systems.

This research is documented in the paper: *"Pendulum Data Analysis – Predictions Using PINN and RNN Models"* by Fábio Luiz Gonçalves Filho, João Pedro da Rosa Mendes, and Paul Neugebauer (November 2025).

## Dataset Generation

### Simple Pendulum
*   **Methodology:** Data was generated using the **4th-Order Runge–Kutta (RK4)** method to ensure numerical stability and precision.
*   **Parameters:** Systematically varies gravity, rod length, damping coefficients, initial angles, and angular velocities.
*   **Robustness:** Includes 972 unique simulations, ranging from harmonic to strongly nonlinear and damped oscillations, including multiple Gaussian noise levels.

### Double Pendulum
*   **Source:** Based on the implementation by [Fajardo et al. (2024)](https://github.com/javierfa98/Double-Pendulum-NN).
*   **Methodology:** Generated using **Euler integration** with a 0.01s time step.
*   **Configuration:** Gravity (9.8 m/s²), masses (1kg), and lengths (1m). Initial angles generated via uniform distribution [0, 360]° and angular velocities [-180, 180]°.
*   **Structure:** 100 CSV trajectories, each containing 500 time steps.

## Methodology: LSTM & PINN
The project utilizes LSTMs to capture long-range temporal dependencies inherent in the systems' differential equations.

*   **Architecture:** 2 hidden layers (128 neurons each), linear output layer.
*   **Optimizer:** ADAM with `ReduceOnPlateau` learning rate scheduler.
*   **PINN Integration:** Physical constraints were incorporated by adding terms for the Lagrangian-derived equations of motion and global energy conservation.
*   **Optimization:** A mixed-loss formulation was employed, utilizing fine-tuned lambda coefficients (e.g., $10^{-6}$ for motion) to balance physical loss against data-driven Mean Squared Error (MSE).

## Key Findings
1.  **System Complexity:** For the simple pendulum, standard LSTMs outperformed PINN-augmented models, as physical constraints provided no practical benefit and hindered convergence speed.
2.  **Chaos Control:** In the double pendulum, PINN constraints significantly improved long-term prediction stability.
3.  **Hyperparameter Sensitivity:** Performance is highly sensitive to architecture. The default configuration (256 Batch Size, 200 Epochs, 128 hidden units, 2 layers) provides the optimal balance between computational feasibility and accuracy.

## Directory Structure
*   `data/`: Scripts for data generation (RK4 for simple, Euler for double).
*   `models/`: Implementation of LSTM architectures and PINN loss classes.
*   `notebooks/`: Training scripts, hyperparameter tuning, and evaluation.
*   `results/`: Plots comparing trajectories and loss evolution for PINN vs. standard LSTM.

## Citation
If you use this code or findings in your research, please refer to our paper:
> Gonçalves Filho, F. L., Mendes, J. P. R., & Neugebauer, P. (2025). *Pendulum Data Analysis – Predictions Using PINN and RNN Models*.
