# Lecture 13

### Multiway Cut Problem

#### Input

1. A graph $G = (V, E)$
2. A set of terminals $S = \{s_1, ..., s_k\}$ such that $S \subseteq V$

#### Output

A set of edges whose removal disconnect the terminals from each other.

#### Objective

Minimize the total weight of such a set of edges.

#### Side node

The multiway cut problem in NP-hard when we have $k > 2$

#### Solution:

1. For each $i = 1, ..., k$ we compute the minimum weight of **isolating cut** for $s_i$ and we name such a cut $C_i$

2. We discard the heaviest of $C_i$ and take the union of the rest.

*Note: definition of isolating cut*

An isolating cut for $s_i$ is defined as the set of edges whose removal disconnects $s_i$ from all the other terminals.

***

### Minimum k-cut problem

A k-cut is defined as a set of edges whose removal leaves $k$ connected components in the graph. In the minimum k-cut problem, as we need to find the k-cut for a given graph with the minimum weight.

### Solution: Gomory-Hu tree 

#### Understanding Gomory-Hu Tree

Let $G = (V, E)$ be a graph and let $T$ be another tree where:

1. The vertex of $T$ is $V$
2. The edges of $T$ need not be $E$

Let $e$ be an edge in $T$ such that if we remove $e$, we will create two connected components with vertex $(S, S')$.

The tree $T$ can be called the Gomory-Hu tree for $G$ if:

1. For each pair of vertices $u, v$ in $V$, the weight of a minimum $u$ - $v$ cut in $G$ is the same as it is in $T$
2. For each edge $e$ in $T$, $w'(e)$ is the eight of the cut associated with $e$ in $G$

***

### Min-cut

#### What does it mean that a set of nodes are well or sparsely interconnected?

First, we first settle the definition of min cut. Which is the minimum number of edges that when removed causes the graph to become disconnected. 

Additionally, a **small min-cut** means implies the graph is sparsely connected and we can express min cut as

$$\text{min}_{\hspace{}1mmU} E (U, V \setminus U) = \Sigma_{i \in U} \Sigma_{j \in V \setminus U} A [i, j]$$

/Screen Shot 2022-03-23 at 9.01.15 PM.png

#### How do we measure how connected a graph is?

First, we have to ask, what does it mean for a set of node to be well interconnected? And when we actually think about it, it is not really a good idea to use the minimum cut to measure the connectivity of a graph. Consider the following example:

/Screen Shot 2022-03-23 at 9.05.45 PM.png

Therefore, we can try to use the following notion.

***

### Graph Expansion

Graph expansion is used to normalize the cut by of a graph by the size of the smallest component.

$$\text{Cut Ratio} := \alpha = \frac{E(U, V \setminus U)}{\text{min} (|U|, |V \setminus U|)}$$

$$\text{Graph Expansion } := \alpha(G) = \text{min }_{U} \frac{E(U, V \setminus U)}{\text{min }(|U|, |V \setminus U|)}$$

***

Spectral Analysis

The **Laplacian Matrix** $L = D - A$ there:

* $A$ is the adjacency matrix
* $D$ is the diagonal matrix such that 
	* $d_i$ is the degree of the node $i$

Therefore, we have the following properties:

* $L(i, i) = d$
* $L(i, j) = -1$ if there is an edge between $i$ and $j$.


## Laplacian Matrix

The laplacian matrix has the following properties:
1. The matrix $L$ is symmetric.
2. The matrix $L$ is positive semi-definite.
	* A matrix is positive semi-definite if all the eigenvalues of that matrix are positive.

3. The matrix $L$ has $0$ as an eigenvalue and the corresponding eigenvector $w_1 = (1, 1, ..., 1)$
	* Here, we use $\lambda_1 = 0$ to demote the smallest eigenvalue

## Second smallest eigenvalue

The second smallest eigenvalue can also be called the **Fielder value** ($\lambda_2$), which satisfies the following

$$\lambda_2 = \text{ min }_{||x|| = 1, x \perp w_1} x^T Lx$$

The vector that minimizes $\\lambda_2$ is called the **Fielder vector**. This vector minimizes the following

$$\lambda_2 = \text{ min }_{x ≠ 0} \frac{\Sigma_{(i, j) \in E} (x_i - x_j)^2}{\Sigma_i x^2_i} \text{ where } \Sigma_i x_i = 0$$

***

## Spectral Ordering

In order to understand the spectral ordering, we need to the following:

* The value of the $x$ which minimize the following:

$$\text{ min }_{x ≠ 0} \frac{\Sigma_{(i, j) \in E} (x_i - x_j)^2}{\Sigma_{i} x^2_i} \hspace{6mm} \Sigma_i x_i = 0$$

* For the weighted matrix we need the following:

$$\text{ min }_{x≠0} \frac{\Sigma_{(i, j)} A [i, j] (x_i - x_j)^2}{\Sigma_{i} x^2_i} \hspace{6mm} \Sigma_{i} x_i = 0$$

If we have those then the transformation will have the following properties:

1. The ordering according to the $x_i$ value will group similar (connected) nodes together.
2. Physical interpretation: The stable state of springs placed on the edge of the graph

## Spectral Partition

Partition the nodes according to the ordering induced by the Fielder vector

* If $u = (u_1, u_2, ..., u_n)$ is the Fielder vector, then we split the nodes according to the value $s$
	* Bisection: $s$ is the median value in $u$
	* Ratio Cut: $s$ is the value that minimize $\alpha$
	* sign: Separate positive and negative values ($s = 0$)
	* gap: Separate according to the largest gap in the values of $u$

## Fielder Value

The value $\lambda_2$ is a good approximation of the graph expansion

$$\frac{\alpha(G)^2}{2d} \le \lambda_2 \le 2 \alpha (G) \text{ where } d = \text{ maximum degree}$$

$$\frac{\lambda_2}{2} \le \alpha (G) \le \sqrt{\lambda_2 (2d - \lambda_2)}$$

If the max degree $d$ is bounded, we obtain a good approximation of the minimum expansion cut.