NMF based approach with X = AS (X gene expression profile, A = proportuins, S = signature matrix)
 "We solve this weighted non-negative least squares problem for each gene by
 ![[Pasted image 20250802164921.png]]
where the coefficient aki is a scalar parameter between 0 and 1 to represent
the fraction of tissue type. The formulation can then be efficiently solved
by quadratic programming"
![[DeconRNAseq.pdf]]