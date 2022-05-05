# Week2 Graph Matrix

[toc]

# 1. Degree

degree $d_j$ of node j: number of neighbors of j

1. $\sum_{j=1}^{N} d_j=2L$

2. Average degree in G:
   $E[D]=\frac{1}{N} \sum_{j=1}^{N}d_j=\frac{2L}{N}$

3. Bounds:

   $2- \frac{2}{N} \le E[D] \le N-1$

   it is apparent that the left bound of E[D] is considering the situation of trees

## 1.1. Degree Vectors
Degree Vector(row sum A): $d=A \cdot u$

1. $u^T \cdot d = u^T A u = 2L$
2. $d_j=(A^2)_{jj}$

> $A=(A_1,...,A_n),A\cdot A^T= \left [ \begin {matrix} A_1^T A1 & \cdots &A_1^TA_n \\ \vdots & \vdots & \vdots \\ A_n^T A_1 & \cdots & A_n^T A_n\end{matrix} \right]$
>
> $(A^2)_{jj}=A_{j}^T A_j=\sum (a_j)^2=\sum a_j$

 ## 1.2. Properties

1. At least two nodes in G have the same degree

   > $degree \in [0,N-1]$ but 0 and N-1 cannot exists simultaneously

2. The number of nodes with odd degree is even

3. Many complex networks have a power law degree distribution (scale-free networks)

## 1.3. Clustering Coefficient

### 1.3.1. Definition 1

for node v:

  $C_G(v)=\frac {2 y}{d_v(d_v-1)}$

where y is the number of links between neighbors, 

1. $y_{max}= \left ( \begin{matrix} d_v \\ 2\end{matrix}\right)$
2. $d_v=1, C_G(v)=0$
3. $C_G(v) \in [0,1]$ measures the local density around node v

INTUITIVE UNDERSTANDING:

1. 2y is equal to two times of the number of triangles locally
2. more triangle $\longrightarrow$ more density
3. single node $\longrightarrow$ 1/2, local $\longrightarrow$ complete
4. intuitively $C_G(v)$ represents the local complete

### 1.3.2. Definition 2

for graph G:6 times the number of triangles in G divided by the number of connected triples of nodes

$C_G=\frac {3(triangles)}{N_2-W_2}=\frac{W_3}{d^T d - L}=\frac {trace(A^3)}{\sum d_i (d_i-1)}$



Attention: two definitions do not necessarily give the same number



# 2. Walks and Paths

## 2.1. Walk

**walk** of length k from node i to j: succession of k links (arcs)

$$(n_0 \rightarrow n_1)(n_1 \rightarrow n_2) \cdots (n_{k-1} \rightarrow n_k) \quad\ where\quad n_0=i \quad and \quad n_k=j$$

## 2.2. Path

**path**: a walk in which all nodes/vertices are different

## 2.3. Adjacency matrix A and walks

