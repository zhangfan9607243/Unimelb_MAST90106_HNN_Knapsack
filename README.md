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


## HNN Knapsack Method
### HNN Formulation
A Hopfield Neural Network can be used to solve the given problem. A Hopfield Neural Network is a single-layer and fully connected network. Every neuron in the network is a binary threshold unit $X_i$ which can only take two different values: 0 (on) or 1 (off).

Every pair of neurons is connected, and each neuron is not connected with itself. The weight of the connection between neuron k and the neuron is $W_{ab} = W_{ba}$ which is negative if the connection is inhibitory, and positive if the connection is excitatory. Therefore, all the connections in the network can be represented as an N × N symmetric matrix $W$ with zeros on the diagonal.

The initial input of the network can be denoted as: $X(0) = [X_1(0), X_2(0), ...,X_n(0)]$. The values of neurons in the network are subjected to update. When updating the network, at each round, we update neurons asynchronously, i.e., we update neurons one by one, and the sequence is usually random. The updating rule for one neuron at round k can be shown as follows.

For neuron being updated with a threshold θ_i:
<img width="774" alt="截屏2024-11-07 15 00 07" src="https://github.com/user-attachments/assets/d11fe958-0ec5-4689-bf02-791aaf77ce6d">

When the neurons in the network stop updating, the final values of each neuron are the output of the network, which can be denoted as: $X(k) = [X_1(k), X_2(k), ..., X_n(k)]$.

The stopping criteria becomes the energy function of the network, which is defined as:
<img width="806" alt="截屏2024-11-07 15 01 26" src="https://github.com/user-attachments/assets/4fba30b4-103b-4c5f-b0a7-b1de9cea31a9">

The updating process will make continuously decrease and converge to a local minimum value, at which the network becomes stable, i.e., the value of neurons won’t change. To reach a global minimum, we should try more initial inputs of the network.

### HNN for Knapsack Problem
The Hopfield neural network algorithm for knapsack problem has two main components:
* Establish the energy function: The energy function is the penalty function of our knapsack problem, which contains the optimisation purpose (combined KPI1 and KPI2) and constraints. We would impose penalization parameters and penalization functions on constraints.
* Minimize the energy function: The minimization process is completed by a mean-field annealing algorithm, where we use a continuous variable between 0 and 1 to represent the solution of the optimisation problem, instead of a 0-1 discrete variable. Then, we solve the continuous variable by mean-field annealing.

The basic requirement of our project is:
<img width="806" alt="截屏2024-11-07 15 03 48" src="https://github.com/user-attachments/assets/2ab78c4b-358e-4d5b-91fc-413ecd1a03f2">
<img width="771" alt="截屏2024-11-07 15 04 03" src="https://github.com/user-attachments/assets/7566fc7e-0d97-4b10-a963-b95522cf1ba9">

To solve this optimization problem, we can construct a Hopfield neuron network which has the energy function:
<img width="767" alt="截屏2024-11-07 15 06 23" src="https://github.com/user-attachments/assets/c7ddfd58-0b1f-43f1-adca-bb2cc5bc0555">

In practice, we use a continuous variable $V_i ∈ [0, 1]$ to replace discrete $X_i ∈ {0, 1}$, and solve for $V_i$ by mean field annealing approach by iteration:
<img width="904" alt="截屏2024-11-07 15 07 42" src="https://github.com/user-attachments/assets/5d9f36bc-0563-490c-a20e-4c025db3dd89">

The iteration should run at least 1000 rounds by α = 0.1 and T(k + 1) = 0.99T(k) with T(0) = 10.

## Conclusion

The **Hopfield Neural Network** provides an innovative way to solve the multi-knapsack problem by encoding constraints directly into the energy function. With proper tuning, this method demonstrates promising results in finding optimal or near-optimal solutions while managing complex constraints.

For detailed experimentation results and comparisons with other optimization methods, please refer to the project report.
