# State Estimation for Noisy Dynamic Systems using Kalman Filter

## Project Overview
This project studies the problem of estimating hidden system states in noisy dynamic systems.
When observations are corrupted by noise and the system evolves over time,
naive estimation methods often fail to provide reliable or timely results.

To address this challenge, a Kalman filter is applied as a model-based state estimation method,
explicitly modeling system dynamics and uncertainty.

---
Part I: One-Dimensional Random Walk Model

## Problem Setting
We consider a one-dimensional discrete-time dynamic system.
The true system state evolves over time but is not directly observable.
Instead, noisy measurements are available at each time step.

The objective is to estimate the hidden system state from noisy observations
while accounting for uncertainty in both system dynamics and measurements.

---

## System Model
The system is modeled as a random walk with additive noise:

$$
x_t = x_{t-1} + w_{t-1}, \quad w_{t-1} \sim \mathcal{N}(0, Q)
$$

Observations are generated as noisy measurements of the true state:

$$
y_t = x_t + v_t, \quad v_t \sim \mathcal{N}(0, R)
$$

where:
- $Q$ represents process noise variance,
- $R$ represents measurement noise variance.

---

## Estimation Methods

### Baseline Approaches
Several baseline methods are implemented for comparison:

- **Naive estimation:** directly using noisy observations.
- **Moving average:** smoothing observations over a fixed window.
- **Linear regression:** data-driven prediction using past observations.

These approaches provide partial improvements but suffer from noise sensitivity
or temporal lag.

---

### Kalman Filter
To overcome baseline limitations, a Kalman filter is applied as a model-based estimation method.
The filter recursively estimates the system state through prediction and update steps.

#### Prediction
$$
\hat{x}_{t|t-1} = \hat{x}_{t-1|t-1}
$$

$$
P_{t|t-1} = P_{t-1|t-1} + Q
$$

#### Update
The Kalman gain is computed as:

$$
K_t = \frac{P_{t|t-1}}{P_{t|t-1} + R}
$$

The state estimate and uncertainty are then updated:

$$
\hat{x}_{t|t} = \hat{x}_{t|t-1} + K_t \bigl(y_t - \hat{x}_{t|t-1}\bigr)
$$

$$
P_{t|t} = (1 - K_t) P_{t|t-1}
$$

The Kalman gain $K_t$ determines how much the estimate is corrected toward the observation.

---

## Experimental Results
Experiments show that:

- Baseline methods either overreact to noise or introduce noticeable delay.
- The Kalman filter achieves a better balance between smoothness and responsiveness.
- Compared to baselines, the Kalman filter produces more stable and timely state estimates.

---

## Sensitivity Analysis: Effects of $Q$ and $R$

### Effect of Measurement Noise ($R$)
As $R$ increases:
- The Kalman gain decreases.
- The filter relies more on model predictions.
- State estimates become smoother and less sensitive to noisy observations.

### Effect of Process Noise ($Q$)
As $Q$ increases:
- Predicted uncertainty increases.
- The Kalman gain increases.
- The filter responds more strongly to new observations.

---

## Conclusion
This project demonstrates that model-based filtering provides a principled solution
for state estimation in noisy dynamic systems.
By explicitly modeling uncertainty, the Kalman filter outperforms intuitive
baseline methods and offers interpretable control over estimation behavior.

