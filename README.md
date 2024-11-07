# Unimelb MAST90106 & MAST90107 Data Science Project - HNN Knapsack

## Acknowledgement
I would like to thank the teaching team and staff in Unimelb Data Science, as well as the Scalene Group, for providing us the opportunity to work on this project and for their valuable guidance throughout.

## Project Introduction
This project is the capston project of Unimelb Master of Data Science program. The project team that I was in was assigned a project called "Solving Floor Space Allocation as a Large-Scale Optimisation Problem", which is in collaboration with the Scalene Group. 

The project is a multi-knapsack problem: Optimizing store space allocation to **Maximize Profit (KPI1)** while **Minimizing Wastage (KPI2)**, with a series of equality and inequality constrains:
* Equality Constrain:
  * Each product can only have one space allocation plan. 
* Inequality constrains:
  * The total space allocated for all products must be less than or equal to the store's total area.
  * Sum of the space value of all products eligible for the special zone should be less than or equal to the total number of special zones in the store.
  * The largest space value for a given product can’t be used until all other products in the store are of equal opt priority.

My team implemented multiple methods for this problem, including: 
* Branch & Bound
* Simulated Annealing
* Greedy Randomised Adaptive Search Procedure
* Hopfield Neural Network

My work in this team project is to implement a **Hopfield Neural Network (HNN) for Mixed Integer Programming**, which is used as an alternative to classical optimization techniques for its ability to handle complex constraints and explore large solution spaces.

## Files Description
The following is an introduction to the paths and files in this repository:
* `/data/`:
  * `data1.csv`: This is a small sample of original project data with only 10 stores. The complete data with nearly 600 stores can be downloaded by the link: https://pan.baidu.com/s/1AZUM0lzmBygVXvWQI5aB9Q?pwd=1234.
  * `data2.json`: This is cleaned and preprocessed data in a dictionary format, after running `Data Process.ipynb`.
  * `data3a.json`: This is the data with solutions for each store, after running `HNN Knapsack - Without Parameter Tunning.ipynb`.
  * `data3b.json`: This is the data with solutions for each store, after running `HNN Knapsack - With Parameter Tunning.ipynb`.
* `Data Process.ipynb`: Data cleaning and preprocessing.
* `HNN Knapsack - Without Parameter Tunning.ipynb`: Implementation of HNN Knapsack model without parameter tunning process.
* `HNN Knapsack - With Parameter Tunning.ipynb`: Implementation of HNN Knapsack model, with parameter tunning process for a better solution.

## Data Description
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


## HNN Knapsack Formulation

This project tackles the **multi-knapsack problem**, focusing on optimizing the use of limited store space to:

- **Maximize profit** (KPI1)
- **Minimize wastage** (KPI2)

Retailers face constraints like limited shelf space, product placement priorities, and designated special zones. The goal is to determine the optimal layout for products across multiple stores, ensuring all constraints are respected.

The **Hopfield Neural Network (HNN)** method offers a unique approach by framing the optimization problem within a neural network, providing an alternative to traditional methods such as binary integer programming.


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


## Conclusion

The **Hopfield Neural Network** provides an innovative way to solve the multi-knapsack problem by encoding constraints directly into the energy function. With proper tuning, this method demonstrates promising results in finding optimal or near-optimal solutions while managing complex constraints.

For detailed experimentation results and comparisons with other optimization methods, please refer to the project report.
