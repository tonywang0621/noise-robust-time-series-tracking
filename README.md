# State Estimation for Noisy Dynamic Systems using Kalman Filter

## Project Overview
This project studies the problem of estimating hidden system states in noisy dynamic systems.
When observations are corrupted by noise and the system evolves over time,
naive estimation methods often fail to provide reliable or timely results.

To address this challenge, a Kalman filter is applied as a model-based state estimation method,
explicitly modeling system dynamics and uncertainty.

---

## Part I: One-Dimensional Random Walk Model

### Problem Setting
We consider a one-dimensional discrete-time dynamic system.
The true system state evolves over time but is not directly observable.
Instead, noisy measurements are available at each time step.

The objective is to estimate the hidden system state from noisy observations
while accounting for uncertainty in both system dynamics and measurements.

---

### System Model
The system is modeled as a random walk with additive noise:

$$
x_t = x_{t-1} + w_{t-1}, \quad
w_{t-1} \sim \mathcal{N}(0, Q)
$$

Observations are generated as noisy measurements of the true state:

$$
y_t = x_t + v_t, \quad
v_t \sim \mathcal{N}(0, R)
$$

where:
- $Q$ represents the process noise variance,
- $R$ represents the measurement noise variance.

---

### Estimation Methods

#### Baseline Approaches
Several baseline methods are implemented for comparison:

- **Naive estimation:** directly using noisy observations.
- **Moving average:** smoothing observations over a fixed window.
- **Linear regression:** data-driven prediction using past observations.

These approaches provide partial improvements but suffer from noise sensitivity
or temporal lag.

---

#### Kalman Filter (1D)
To overcome baseline limitations, a Kalman filter is applied as a model-based estimation method.
The filter recursively estimates the system state through prediction and update steps.

**Prediction**

$$
\hat{x}_{t|t-1} = \hat{x}_{t-1|t-1}
$$

$$
P_{t|t-1} = P_{t-1|t-1} + Q
$$

**Update**

The Kalman gain is computed as:

$$
K_t = \frac{P_{t|t-1}}{P_{t|t-1} + R}
$$

The state estimate and uncertainty are updated as:

$$
\hat{x}_{t|t}
=
\hat{x}_{t|t-1}
+
K_t \bigl(y_t - \hat{x}_{t|t-1}\bigr)
$$

$$
P_{t|t}
=
(1 - K_t) P_{t|t-1}
$$

The Kalman gain $K_t$ determines how much the estimate is corrected toward the observation.

---

### Experimental Results
Experiments show that:

- Baseline methods either overreact to noise or introduce noticeable delay.
- The Kalman filter achieves a better balance between smoothness and responsiveness.
- Compared to baselines, the Kalman filter produces more stable and timely state estimates,
as confirmed by RMSE and MAE metrics.

---

### Sensitivity Analysis: Effects of $Q$ and $R$

**Effect of Measurement Noise ($R$)**

As $R$ increases:
- The Kalman gain decreases.
- The filter relies more on model predictions.
- State estimates become smoother and less sensitive to noisy observations.

**Effect of Process Noise ($Q$)**

As $Q$ increases:
- Predicted uncertainty increases.
- The Kalman gain increases.
- The filter responds more strongly to new observations.

Local zoom-in analysis is used to better illustrate these effects.

---

## Part II: Extension to a Two-Dimensional Position–Velocity Model

### Motivation
While the one-dimensional random walk model provides a clear and interpretable example,
many real-world tracking problems involve latent variables that are not directly observed.
To better reflect such scenarios, the system is extended to a two-dimensional
position–velocity model.

---

### State Definition
The system state is defined as:

$$
\mathbf{x}_t =
\begin{bmatrix}
p_t \\
v_t
\end{bmatrix}
$$

where $p_t$ denotes position and $v_t$ denotes velocity.

---

### State Transition Model
The system follows a constant-velocity motion model:

$$
\mathbf{x}_t
=
\begin{bmatrix}
1 & 1 \\
0 & 1
\end{bmatrix}
\mathbf{x}_{t-1}
+
\mathbf{w}_{t-1},
\quad
\mathbf{w}_{t-1} \sim \mathcal{N}(\mathbf{0}, \mathbf{Q})
$$

---

### Observation Model
Only the position component is observed:

$$
y_t =
\begin{bmatrix}
1 & 0
\end{bmatrix}
\mathbf{x}_t
+
v_t,
\quad
v_t \sim \mathcal{N}(0, R)
$$

Velocity is treated as a **latent state** and inferred implicitly
from noisy position measurements through the system dynamics.

---

### Kalman Filter (2D)

**Prediction**

$$
\hat{\mathbf{x}}_{t|t-1}
=
\mathbf{F}\,\hat{\mathbf{x}}_{t-1|t-1}
$$

$$
\mathbf{P}_{t|t-1}
=
\mathbf{F}\,\mathbf{P}_{t-1|t-1}\,\mathbf{F}^\top
+
\mathbf{Q}
$$

**Update**

$$
\mathbf{K}_t
=
\mathbf{P}_{t|t-1}\mathbf{H}^\top
\bigl(
\mathbf{H}\mathbf{P}_{t|t-1}\mathbf{H}^\top
+
\mathbf{R}
\bigr)^{-1}
$$

$$
\hat{\mathbf{x}}_{t|t}
=
\hat{\mathbf{x}}_{t|t-1}
+
\mathbf{K}_t
\bigl(
y_t - \mathbf{H}\hat{\mathbf{x}}_{t|t-1}
\bigr)
$$

$$
\mathbf{P}_{t|t}
=
(\mathbf{I} - \mathbf{K}_t\mathbf{H})
\mathbf{P}_{t|t-1}
$$

---

### Interpretation
Despite observing position only, the Kalman filter successfully estimates both
position and velocity. The inferred velocity is smooth and physically consistent,
demonstrating the filter’s ability to recover latent states under partial observability.

---

## Conclusion
This project demonstrates that model-based filtering provides a principled solution
for state estimation in noisy dynamic systems.
Starting from a simple one-dimensional random walk and extending to a two-dimensional
position–velocity model, the Kalman filter not only outperforms intuitive baseline
methods but also enables inference of latent system states by explicitly modeling
dynamics and uncertainty.
