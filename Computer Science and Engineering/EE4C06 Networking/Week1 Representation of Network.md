# Week 1: Representation of Network

[toc]

# 1. Network

Network is:

1. Service $\longrightarrow$ function $\longrightarrow$ software, processes
2. Topology $\longrightarrow$ graph $\longrightarrow$ hardware, structure

# 2. Graph

N nodes (vertices), L links (edges)

## 2.1. Three equivalent representations of a Graph

Topology domain; spectral domain; geometric domain

## 2.2. Topology domain

### 2.2.1. Computer structures:

1. List of neighbors:

   for each node, the set of direct neighbors is listed

2. List of links:

   link L has: L = ( i, j ) = ( j, i ) or L = （ l-, l+）

### 2.2.2. Graph related matrices:

#### 2.2.2.1. Adjacency matrix（A）

Number of neighbors of node i is the degree：
				$d_i=\sum_{k=1}^{N} a_{ik}$

#### 2.2.2.2. Incidence matrix (B)

each row is represent a link, the start point is equal to 1, the target point is equal to -1.

1. size: $N \times L$

2. B specifies the directions of links

3. each row of B sum to zero

   $u=(1,1, ...,1) \quad u^TB=0$ 

#### 2.2.2.3. Laplacian matrix (Q)

   $Q=B \cdot B^T= \Delta - A$

$\Delta=diag(d_1,d_2,d_3,...,d_N) \quad of \quad A $

1. each row/line of Q sum to zero

   $u=(1,1, ...,1) \quad u^TQ=Q \cdot u=0$

   $u \rightarrow eigenvector \quad of \quad Q  \quad for \quad \mu =0$ 

   >  it is easy to prove through the relations between Q and B

2. A, Q, B are all symmetric

   

## 2.3. Weighted graph

### 2.3.1. Unweighted graph

all nodes and all links are of the same type

### 2.3.2. Weighted graph

Each node and/or link can be different

1. A weighted adjacency matrix W specifies the link weights

### 2.3.3. Representations of Weighted graph

![weighted graph representation](.\img\weighted graph representation.png)

## 2.4. Classes of Graphs

### 2.4.1. Trees

$L = N-1$

special types: star, path graph

### 2.4.2. Ring

$L = N$

### 2.4.3. Complete Graph $K_n$

$L = \frac {N \cdot (N-1)}{2}$

![special_connected_graphs](.\img\special_connected_graphs.png)

### 2.4.4. Null graph

### 2.4.5. Complement $G^c$

If there is a link between two nodes in G, there is none in $G^c$

1. if  G is not connected, than $G^c$ will be connected
2. $A^c=J-I-A \quad where \quad J=u \cdot u^T$

### 2.4.6. Line graph I(G)

Link in G $\longrightarrow$ nodes

Two links has a common node $\longrightarrow$ two nodes is connected in I(G)

### 2.4.7. Subgraph

1. How to find the adjacency matrix of a subgraph:

   ![subgraph_matrix](.\img\subgraph_matrix.png)

2. Some property of the new matrix

   $$  A_{N \times N}=\left[ \begin{matrix} A_s & B \\ C & A_{G \setminus S} \end{matrix} \right]=\left [ \begin{matrix} (A_1)_{m \times m} & B_{m \times n} \\ C_{n \times m} & (A_2)_{n \times n} \end{matrix} \right ]$$

   B is interconnection matrix and represents the cut set of graph. Number of links in B equals $u^T\cdot B \cdot u$

### 2.4.8. Turan Graph

Among all graphs with N nodes, the graph with most links that does not contain a complete graph Kr is the **Turan graph**
T(n,r-1). (r-1 means r-1partitions)

1. Turan's Theorem

   Any graph on N nodes with more links than $L= \left [ \frac {r-1}{2r} \cdot N^2 \right]$ has a clique $K_r$

   

### 2.4.9. Lattice

A **lattice graph**, **mesh graph**, or **grid graph**, is a [graph](https://en.wikipedia.org/wiki/Graph_(discrete_mathematics)) whose [drawing](https://en.wikipedia.org/wiki/Graph_drawing), [embedded](https://en.wikipedia.org/wiki/Embedding) in some [Euclidean space](https://en.wikipedia.org/wiki/Euclidean_space) **R***n*, forms a [regular tiling](https://en.wikipedia.org/wiki/Regular_tiling)