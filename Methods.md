This chapter should explain how DynaMiCs works which omptimization steps and parameters were used. 
Additionaly the different Benchmarks and Application steps should be described

## Data
### Preprocessing

### COMBAT
- COVID Data -> Single Cell RNAseq

## DynaMiCs
The model is based on non-negative matrix factorization. So we see the deconvolution problem as linear equation of the bulk expression matrix Y a referene matrix X describing the cell type specific gene expresion and the composition vector C describing the proportion of each cell type within the sample. To derive with this naive approach the cell composition we can solve the linear equtation using an optimization algorithm non-linear Least Squares solving the los function as the frobenius norm of Y-XC. 
Since the gene expression varies between different genes, so some genes are generally highly expressed while other generally lower expressed which does not account for their importance a gene weighting can be added which weighs important genes for solving the problem up while weughing unimportant down. So a gene weight vector G can be introcdued in our problem.

### Training Loss
Let:
- $C \in \mathbb{R}^{n \times d}$ be the ground truth matrix
    
- $\hat{C} \in \mathbb{R}^{n \times d}$ be the predicted matrix
    
- $\lambda \in \mathbb{R}$ be the L2 regularization coefficient
    
- $\mu_C = \frac{1}{d} \sum_{j=1}^d C_{ij}$, the mean across each row of CC
    
- $\mu_{\hat{C}} = \frac{1}{d} \sum_{j=1}^d \hat{C}_{ij}$ , the mean across each row of $\hat{C}$
    
Per-row Pearson correlation as: $r_i = \frac{\sum_{j=1}^d (C_{ij} - \mu_{C_i})(\hat{C}_{ij} - \mu_{\hat{C}_i})}{\sqrt{\left(\sum_{j=1}^d (C_{ij} - \mu_{C_i})^2\right) \left(\sum_{j=1}^d (\hat{C}_{ij} - \mu_{\hat{C}_i})^2\right)} + \epsilon}$

with a small stabilizing constant $\epsilon = 0.0001.$

Now the total loss is:

$\mathcal{L}(C, \hat{C}) = \begin{cases} - \sum_{i=1}^n r_i, & \text{if } \lambda = 0 \text{ or L2 is disabled} \\ - \sum_{i=1}^n r_i + \lambda \cdot \frac{1}{d} \sum_{i=1}^n \sum_{j=1}^d \left( \frac{C_{ij} - \hat{C}_{ij}}{\mu_{C_i}} \right)^2, & \text{otherwise} \end{cases}$
This formula expresses the correlation-based loss (negative sum of per-row Pearson correlations), and optionally adds an L2 penalty scaled by $1 / \mu_{C_i}$, averaged across features.

---


### Training
Naive Approach: $\hat{Y} = X \hat{C}$
To derive $\hat{C}$ use non-negative Least Square: $\hat{C} = argmin_{C\geq0}{||Y - XC||^2_F}$

More reliable add G as vector  $G = [\sqrt{\gamma_0},\ldots, \sqrt{\gamma_p}]$ , and $a = [a_1, \ldots, a_q]$ and $\Theta$ with $\Theta^{(k)} = [\theta_1^{(k)}, \ldots, \theta_{n_k}^{(k)}]$ 

resulting in the following equation: 
 $\hat{C(G, \Theta, a)} = argmin_{C\geq0}{||G\cdot(Y - [a_1 X^{(1)}\Theta^{(1)}, \ldots,a_q X^{(q)}\Theta^{(q)}]\cdot C)||^2_F}$
with $X^{(k)} \epsilon R^{p x n_k}$

Here G is a weight for each gene, a buffers the biological variance in the references and theta is a weight for each of the training expression profiles for the generation of the reference profile $X_{ref, k} = a_k \cdot X^{(k)} \cdot \Theta^{(k)}$
### Inference

## Evaluation
Apply dynamics and measure pearson correlarion of real and predicted abundance
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
