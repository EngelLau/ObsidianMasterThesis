This chapter should explain how DynaMiCs works which omptimization steps and parameters were used. 
Additionaly the different Benchmarks and Application steps should be described

## Data
### Preprocessing

### COMBAT
- COVID Data -> Single Cell RNAseq

## DynaMiCs
### Training
Naive Approach: $\hat{Y} = X \hat{C}$
To derive $\hat{C}$ use non-negative Least Square: $\hat{C} = argmin_{C\geq0}{||Y - XC||^2_F}$

More reliable add G as vector  $G = [\sqrt{\gamma_0},\ldots, \sqrt{\gamma_p}]$ , and $a = [a_1, \ldots, a_q]$ and $\Theta$ with $\Theta^{(k)} = [\theta_1^{(k)}, \ldots, \theta_{n_k}^{(k)}]$ 

resulting in the following equation: 
 $\hat{C(G, \Theta, a)} = argmin_{C\geq0}{||G\cdot(Y - [a_1 X^{(1)}\Theta^{(1)}, \ldots,a_q X^{(q)}\Theta^{(q)}]\cdot C)||^2_F}$
with $X^{(k)} \epsilon R^{p x n_k}$

Here G is a weight for each gene, a buffers the biological variance in the references and theta is a weight for each of the training expression profiles for the generation of the reference profile $X_{ref, k} = a_k \cdot X^{(k)} \cdot \Theta^{(k)}$
### Inference
## Ablation Study
remove the dynamics feature to achieve a model that uses only static X_ref

## Benchmarks
Test dynaMiCs against different other models: 
MuSiC (perhaps only 30.000 cell instead of 150.000)
SCDC
BayesPrism
## "Domain Adaptation"
train the model on only subgroup of states in the training data use the other for testing
different more or less strong scenarios
## Real-Life Bulk Data
Use Bulk Data and eval via cytoTOF

## GridSearch
Iterate over different parameters and define the best work via CV?
