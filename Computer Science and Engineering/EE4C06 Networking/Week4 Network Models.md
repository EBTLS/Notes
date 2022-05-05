# Week4： Network Models

[toc]

# 1. Deterministic Networks

## 1.1. Complete Graph Kn on N 

Complete Graph:

a **complete graph** is a simple undirected graph in which every pair of distinct vertices is connected by a unique edge.

1. any graph is a subgraph of $K_n$

## 1.2. Lattice in N-dimensions

each node has 2N neighbors (except for the nodes on the boundary)

## 1.3. Multipartite graphs

In graph theory, a part of mathematics, a **k-partite graph** is a graph whose vertices are or can be partitioned into k different independent sets. Equivalently, it is a graph that can be **colored with k colors, so that no two endpoints of an edge have the same color.** When k = 2 these are the **bipartite graphs**, and when k = 3 they are called the **tripartite graphs**.

## 1.4. Regular Graphs

In graph theory, a **regular graph** is a graph where each vertex has the same number of neighbors.

1. Regular graphs can be random as well: Random Regular Graphs


# 2. Erdös-Rényi random graph (ER-Graph)

Basic Definition: The ER random graph $G_p(n)$ is a graph with N nodes and each node pair is connected independently with probability p

## 2.1. Two Variants

1. $G_p(n)$ each link has existence probability p, independently of all the other links (similar with Basic Definition)
2. $G(N,L)$ has precisely L links and each link has the same probability p. Equivalently, all graphs with *n* nodes and *M* edges have equal probability of $p^M (1-p)^{\tbinom{n}{2}-M}$

## 2.2. Implement

1. Variants 1:  for each link, generate a random value from a distribution f and compare it with the corresponding quantile value of f
2. Variants 2: 
   1. numbered all links, random choose L numbers from $[1,\frac {N(N-1)}{2}]$
   2. give all links a value, sort them and choose extremum L

## 2.3. Adjacency Matrix

$A=(a_{ij})_n$ with $a_{ij}$ $(i \neq j)$ is is a Bernoulli random variable with mean p

## 2.4. Complement Graph

The complement graph of $G_p(N)$ is $G_{1-p}(N)$

$p=1 \longrightarrow G_1(N)$ is the complete graph $K_n$ 

$p=0 \longrightarrow G_0(N) $ is the empty or null graph

## 2.5. Parameters

1. **Average number of links**

   $E[L]=p \cdot \frac {N(N-1)}{2}$

2. **Link Density**

   $p=\frac {E[L]}{L_{max}}$

3. **Average Clustering coefficient**

   $E[C_{G_p(N)}]=p$

## 2.6. Degree distribution of Binomial Distribution

Supposing the degree distribution is Binomial distribution

$P[D=k]=\binom{N-1}{k}p^k(1-p)^{N-1-K} \approx \frac{1}{\sigma \sqrt{2\pi}} e^{-\frac {(k-\mu)^2}{2 \sigma ^2}}$

Attention: because for N nodes graph, for a specific point we only need to consider other N-1 nodes' possibility to connect with it. So it is $\binom{N-1}{k}$  not  $\binom{N}{k}$

![D_Distribution_B_Distribution](.\img\D_Distribution_B_Distribution.png)

1. $\mu=E[D]=(n-1)p \quad , \quad \sigma^2=Var[D]=(N-1)p(1-p)$
2. Limit Behavior for Large N

* p constant: $\frac{\mu}{\sigma}=\sqrt{\frac{(N-1)p}{1-p}} \rightarrow 0$

  that means with larger N, $P[D=k] \rightarrow \frac {1}{\sigma \sqrt{2\pi}}$, the graph tends toward a regular graph

* if we keep E[D] constant, that means p towards smaller when N get larger, then 

  $P[D=k] \rightarrow \frac{\mu^k}{k!}e^{-\mu}$

## 2.7. Phase Transition Property

$p_c \propto logN/N$ is a critical link density, that means:

For large N and $p=logN/N+x/N$ (x here is to say this point is sensitive), and we have 

$P[G_p(N) \quad is \quad connected] \rightarrow \left\{
\begin{aligned}
0 \quad if \quad x<0 \\
1 \quad if \quad x>1 
\end{aligned}
\right.$

![ER_critical point](.\img\ER_critical point.png)

## 2.8. Distribution of Eigenvalues of Adjacency Matrix of $G_p(N)$

For large N, Here is a histogram approximates the probability density function $f_{\lambda}(x)$ of eigenvalues of the adjacency matrix A of a ER-Graph $G_p(N)$

![D_eig_A_matrix](.\img\D_eig_A_matrix.png)

and we will have (Observations, do not need prove):

$E[\lambda]=0 \longrightarrow E[\lambda^k]=E[(\lambda-E[\lambda])^k]=\frac{1}{N} \sum_{j=1}^N {\lambda}_j^k$

1. **Skewness** $S_{\lambda}$ measures the lack of symmetry of the distribution around the mean, and is defined as the normalized third moment.

![Skewness](.\img\Skewness.png)



# 3. Small-World Graph of Watts-Strogatz

## 3.1. Small-World Graph

A **small-world network** is a type of mathematical graph in which most nodes are not neighbors of one another, but the neighbors of any given node are likely to be neighbors of each other and most nodes can be reached from every other node by a small number of hops or steps. 

Specifically, a small-world network is defined to be a network where the typical distance L between two randomly chosen nodes (the number of steps required) grows proportionally to the logarithm of the number of nodes N in the network, that is $L \propto Log(n)$

## 3.2. Small-World Experiment

The small-world experiment comprised several experiments conducted by Stanley Milgram and other researchers examining the average path length for social networks of people in the United States. The research was ground-breaking in that it suggested that **human society is a small-world-type network characterized by short path-lengths**. The experiments are often associated with the phrase "six degrees of separation", although Milgram did not use this term himself.

## 3.3. Watts-Strogatz model

The **Watts–Strogatz model** is a random graph generation model that produces graphs with small-world properties, including short average path lengths and high clustering

### 3.3.1. Advantages

It is aimed to address two limitations of ER-Graphs:

1. They do not generate local clustering and <u>triadic closures</u>. Instead, because they have a constant, random, and independent probability of two nodes being connected, ER graphs have a low clustering coefficient.
2. They do not account for the formation of hubs. Formally, the degree distribution of ER graphs converges to a Poisson distribution, rather than a power law observed in many real-world, scale-free networks.[3]

> tradic_closures: Triadic closure is the property among three nodes A, B, and C (representing people, for instance), that if the connections A-B and B-C exist, there is a tendency for the new connection A-C to be formed

### 3.3.2. Algorithms

![watts_algorithms](.\img\Watts_algorithms.png)

### 3.3.3. Example

![Small_World_Example](.\img\Small_world_example.png)

### 3.3.4. Eigenvalues Spectrum Analysis

![spectrum_small_world_graph](.\img\spectrum_small_world_graph.png)

1. In $f_{\lambda}(x)$

* peak $\longrightarrow$ specific structure/pattern
* border, bell-shape around origin (x=0) $\longrightarrow$ more randomness 

2. $f_{\lambda}(-x) = f_{\lambda}(x) \longleftrightarrow tree \quad or \quad tree \quad like$

* A bipartite graph has a symmetric spectrum (for each $\lambda \rightarrow \exist -\lambda$ )
* $\forall tree \longrightarrow$ can be represented as a bipartite graph

> bipartite graph:
>
> In the mathematical field of graph theory, a bipartite graph (or bigraph) is a graph whose vertices can be divided into two disjoint and independent sets U and V such that every edge connects a vertex in U to one in V. Vertex sets U and V are usually called the parts of the graph. 
>
> Equivalently, a bipartite graph is a graph that does not contain any odd-length cycles.

# 4. Barabasi-Albert Graph

## 4.1. Power Law Graph (Scale-Free network)

A **scale-free network** is a network whose degree distribution follows a power law, at least asymptotically. That is, the fraction P(k) of nodes in the network having k connections to other nodes goes for large values of k as

$P(D=k) = c \cdot k^{-\gamma} $

where $\gamma$ is a parameter whose value is typically in the range $2<\gamma<3$ , although occasionally it may lie outside these bounds

1. Many real-world networks have approximately a power law degree distribution

2. ER-graph is not appropriate the definition of scale-free network

3. Power law graph do not have characteristic length:

   $P[D=ak]=c \cdot a^{-\tau} \cdot k^{-\tau}=c \cdot a^{-\tau} \cdot P[D=k]$ 

   > **characteristic length**:
   >
   > 
   >
   > In physics, a **characteristic length** is an important dimension that defines the scale of a physical system. Often, such a length is used as an input to a formula in order to predict some characteristics of the system, and it is usually required by the construction of a dimensionless quantity, in the general framework of dimensional analysis and in particular applications such as fluid mechanics.
   >
   > 
   >
   > Usually, if a object has the characteristic length and its characteristic length keep constant, then the characteristic of this object will not change.

4. A scale-free distribution is self-similar

   > **self-similar:**
   >
   > 
   >
   > In mathematics, a self-similar object is exactly or approximately similar to a part of itself

## 4.2. Power law degree distribution

$P(D=k) = c \cdot k^{-\tau} \quad $ &  $\sum_{k=1}^{\infin} P[D=k] =1$

then    $c=\frac {1}{\sum_{k=1}^{\infin} k^{-\tau}}=\frac {1}{\zeta(s)} \quad (for \quad Re(s)>1)$

> **Riemann-Zeta function**:
>
> The Riemann zeta function or Euler–Riemann zeta function, ζ(s), is a function of a complex variable s that analytically continues the sum of the Dirichlet series
>
> $\zeta(s)=\sum_{n=1}^{\infin} \frac{1}{n^s}$
>
> which converges when the real part of s is greater than 1

1. **Moments**:

   $E[D^m]=\sum_{k=1}^{\infin} k^m \cdot P[D=k]=\frac {\zeta(\tau-m)}{\zeta(\tau)}$

   only exists for $\tau>m+1$

   > **moment**:
   >
   > The **n-th moment** of a real-valued continuous function f(x) of a real variable about a value c is **$ \mu_n=\int_{-\infin}^{\infin} (x-c)^nf(x)dx$**
   >
   > 
   >
   > It is possible to define moments for random variables in a more general fashion than moments for real values—see moments in metric spaces. The moment of a function, without further explanation, usually refers to the above expression with **c = 0**. 
   >
   > 
   >
   > For the second and higher moments, the central moment (moments about the mean, with c being the mean) are usually used rather than the moments about zero, because they provide they provide clearer information about the **distribution's shape**.

## 4.3. Power parameter of power law graphs

![power law graph critical points](.\img\power law graph critical points.png)

> **Critical Points**:
>
> **Critical point** is a wide term used in many branches of mathematics. When dealing with functions of a real variable, a critical point is a point in the domain of the function where the function is either not differentiable or the derivative is equal to zero.

## 4.4. Barabasi-Albert Model

The **Barabási–Albert (BA) model** is an algorithm for generating random scale-free networks using a preferential attachment mechanism. 

### 4.4.1. Algorithm

![BA-Algorithm](.\img\BA-Algorithm.png)

### 4.4.2. Meaning

The BA construction is called **"preferential attachment"**, a manifestation of **"rich get richer"**.

# 5. Observed common properties of real-world complex networks

![Real World Graphs](.\img\Real World Graphs.png)

## 4.1. small-world property

average length/hop count of a path is short compared to the size N of the network.

$E[H]=O(logN)$

## 4.2. scale-free degree distribution

heavy tails (non-Gaussian behavior）

## 4.3. Clustering and community structure

network of networks

## 4.4. Robustness to random node failure

## 4.5. Vulnerability to targeted hub attacks and cascading failures

> **hub**:
>
> In the context of a network, a **hub** is a node with a large degree, meaning it has connections with many other nodes.

# 5. Questions

1. How to generate a G(N,L) and guarantee all of it's links have possible p to exist?