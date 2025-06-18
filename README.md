# Engineering Analytics - Assignment 3

---

## Short Description of Solution

This repository contains the solutions for Assignment 3 of the Z6003 Engineering Analytics course. The assignment covers two main topics:

1.  **Physics-Informed Neural Networks (PINNs):** A PINN is implemented to estimate cardiac activation times on a 2D domain using sparse data. Its performance is compared against a standard data-driven neural network. The models are trained on sparsely sampled data generated from a synthetic ground truth function.

2.  **Neural Ordinary Differential Equations (Neural ODEs):** A Neural ODE is used for a simple 2D classification task. Its performance and decision boundary are compared with a standard single-hidden-layer neural network to understand the differences between discrete and continuous-depth models.

---

### 2. Running Question 1 (PINNs)

The script will:
* Generate the synthetic ground truth data and 30 sparse training samples.
* Train the standard data-driven neural network (Model 1).
* Train the Physics-Informed Neural Network (PINN) (Model 2).
* Generate and save three plots in the root directory:
    1.  `q1_activation_maps.png`: Compares the ground truth with the predictions from both models.
    2.  `q1_error_maps.png`: Shows the absolute error for each model.
    3.  `q1_loss_curves.png`: Compares the training loss curves.

### 3. Running Question 2 (Neural ODEs)

The script will:
* Create a 2D toy dataset (`make_moons`).
* Train a standard single-hidden-layer neural network and report its accuracy.
* Train a Neural ODE-based classifier and report its accuracy.
* Generate and save two plots in the root directory:
    1.  `q2_decision_boundaries.png`: Visualizes and compares the decision boundaries of both models.
    2.  `q2_accuracy_loss.png`: Compares the training accuracy and loss curves for both models.

---

## Known Issues & Assumptions

* **PINN Training:** The PINN model's performance is sensitive to the weight assigned to the physics loss versus the data loss. The current implementation uses a fixed weight of `0.1`, which yields good results for this specific problem.
* **Neural ODE Solver:** The `dopri5` adaptive step solver is used for the Neural ODE. The training can be slower than the standard network due to the computational cost of solving the ODE.
* **Reproducibility:** Random seeds are set to `42` to ensure that the data generation and model initializations are consistent across runs, making the results reproducible.

---
## Discussion on Neural ODEs (Question 2, Part C)

### Behavioral Differences Between Models

* **Continuous vs. Discrete Transformation**: A standard network with a hidden layer applies a discrete, fixed transformation. A Neural ODE learns a **continuous** transformation that evolves the input data along a trajectory. This makes them naturally suited for modeling physical processes and time-series data with irregular intervals.
* **Adaptive Computation**: A standard network always performs the same number of computations. A Neural ODE with an adaptive solver like `dopri5` can adjust its computational effort, taking more steps for complex data points and fewer for simpler ones, making it more computationally adaptive.
* **Memory Efficiency**: Neural ODEs can be trained with the "adjoint method," which has a constant memory cost with respect to the "depth" (number of evaluation steps). This makes them more memory-efficient to train than very deep standard networks.

### (Optional Bonus) ResNet and Euler's Method Connection

The assignment asks to show how a residual connection resembles Euler's method for solving ODEs.

A **residual connection** in a ResNet is defined by the update rule:
$h_{i+1} = h_i + f(h_i)$

**Euler's method** approximates the solution to an ODE of the form $\frac{dh}{dt} = f(h(t))$ as:
$h(t + \Delta t) \approx h(t) + \Delta t \cdot f(h(t))$

By setting the step size $\Delta t = 1$ in the Euler method, the equation becomes identical in form to the ResNet update rule. This demonstrates that a ResNet can be interpreted as a specific numerical discretization of a continuous differential equation using the forward Euler method with a fixed step size of $\Delta t=1$. This insight is a key motivation behind Neural ODEs.
