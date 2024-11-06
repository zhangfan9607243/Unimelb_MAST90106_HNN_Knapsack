# Unimelb MAST90106 & MAST90107 Data Science Project - HNN Knapsack

This project aims to solve a multi-knapsack problem, optimizing store space allocation to maximize profit (KPI1) while minimizing wastage (KPI2).

This is a team project, and my work in this project is to implement a **Hopfield Neural Network (HNN) for Mixed Integer Programming**, which is used as an alternative to classical optimization techniques for its ability to handle complex constraints and explore large solution spaces.

---

## Problem Statement

This project tackles the **multi-knapsack problem**, focusing on optimizing the use of limited store space to:

- **Maximize profit** (KPI1)
- **Minimize wastage** (KPI2)

Retailers face constraints like limited shelf space, product placement priorities, and designated special zones. The goal is to determine the optimal layout for products across multiple stores, ensuring all constraints are respected.

The **Hopfield Neural Network (HNN)** method offers a unique approach by framing the optimization problem within a neural network, providing an alternative to traditional methods such as binary integer programming.

---

## Data Structure

The following table summarizes the structure of the input data used for the project:

| **Field**                    | **Description**                                    |
| ---------------------------------- | -------------------------------------------------------- |
| **STORE ID**                 | Unique identifier for each store                         |
| **PRODUCT ID**               | Unique identifier for each product in the store          |
| **SPACE VALUE**              | Space occupied by the product                            |
| **STORE SPACE**              | Total available shelf space in the store                 |
| **KPI 1**                    | Profit metric to maximize                                |
| **KPI 2**                    | Wastage metric to minimize                               |
| **OPT PRIORITY**             | Controls product placement priority                      |
| **SPECIAL ZONE TAG**         | Indicates if the product can be placed in a special zone |
| **SPECIAL ZONE STORE SPACE** | Maximum space available for special zone products        |
| **KPI 2 MAX**                | Maximum threshold for the sum of KPI2 values             |

These elements form the basis for defining the constraints and objectives used in the neural network optimization.

---

## Hopfield Neural Network Method

### Overview

A **Hopfield Neural Network (HNN)** is a recurrent, fully connected network where each neuron influences every other neuron. The goal of the HNN is to minimize an **energy function** that encodes the optimization objectives and constraints of the knapsack problem.

### Key Components

1. **Network Structure**

   - The network consists of a single layer of binary threshold units representing each product.
   - Each neuron \( $X_i$ \) can have two states: **ON (1)** or **OFF (0)**.
   - Neurons are connected via a symmetric weight matrix \( $W$ \), where \( $W_{ij} = W_{ji}$ ) and diagonal elements are zero.
2. **Energy Function**

   The energy function for the network is designed to penalize constraint violations:

   $$
   E = -\frac{1}{2} \sum_{i} \sum_{j} W_{ij} X_i X_j - \sum_i \theta_i X_i
   $$

   Where:

   - \( $W_{ij}$ \): Weight between neurons \( $i$ \) and \( $j$ \)
   - \( $\theta_i$ \): Threshold value for neuron \( $i$ \)
3. **Optimization Process**

   - **Mean-Field Annealing** is used to find the optimal solution:

     - Neuron states are treated as continuous variables between 0 and 1, rather than discrete binary values, for smoother optimization.
     - The annealing process iteratively updates the neurons to reduce the overall energy of the network.
   - **State Update Rule**:

     $$
     X_i(t+1) = 
          \begin{cases}
          1 & \text{if } \sum_{j \neq i} W_{ij} X_j(t) + \theta_i \geq 0 \\
          0 & \text{otherwise}
          \end{cases}
     $$
4. **Parameter Tuning and Convergence**

   - Parameters such as weights and thresholds are tuned through experimentation to ensure the network converges to feasible solutions.
   - Multiple random initial states are tested to avoid local minima and improve the likelihood of finding a global minimum.

---

## Conclusion

The **Hopfield Neural Network** provides an innovative way to solve the multi-knapsack problem by encoding constraints directly into the energy function. With proper tuning, this method demonstrates promising results in finding optimal or near-optimal solutions while managing complex constraints.

For detailed experimentation results and comparisons with other optimization methods, please refer to the project report.
