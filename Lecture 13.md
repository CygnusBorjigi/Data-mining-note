# Lecture 13

### Multiway Cut Problem

#### Input

1. A graph $G = (V, E)$
2. A set of terminals $S = \{s_1, ..., s_k\}$ such that $S \subseteq V$
3. A function $w$ such that $\forall e \in E \hspace{1mm}w(e)$ is the weight of edge $e$.

#### Output

A set of edges $C \subseteq E$ whose removal disconnect the terminals from each other.

#### Objective

Minimize the total weight of such a set of edges.

#### Side node

The multiway cut problem in NP-hard when we have $k > 2$

#### Solution:

1. For each $i = 1, ..., k$ we compute the minimum weight of **isolating cut** for $s_i$ and we name such a cut $C_i$

2. We discard the heaviest of $C_i$ and take the union of the rest.

*Note: definition of isolating cut*

An isolating cut for $s_i$ is defined as the set of edges whose removal disconnects $s_i$ from all the other terminals.

#### Theorem:

This isolation cut algorithm is a $\left(2 - \frac{2}{k}\right)$ approximation algorithm to multi-way cut. I.e.

$$w(C) \le \left(2 - \frac{2}{k}\right) \cdot w (A)$$

Where $w(A)$ is the weight of the optimal solution.

##### Proof:

First, we only consider the first step by taking the union and show that $w(c) \le 2 \cdot w (A)$

Since $C$ is the union of all the cuts. Therefore, we have $w(c) \le \Sigma^k_{i = 1} w(c_i)$. Note here that the $\Sigma$ is taking the submission of all the cut.

Now, we let $\bigcup^k_{i = 1} A_i = A$ where $A_i \subset A$. Here, we known that in computing $w(A)$ we can claim that $\Sigma^{k}_{i = 1} w(A_i) = 2 \cdot 2 w(A)$. The reason being, for each addition of two cut, we double count each edge in those cut. 

Now, we move to show that $w(C) \le \Sigma^k_{i = 1} w(c_i) \le \Sigma^k_{i = 1} w(A_i)$ where $c_i$ is the minimum cut for terminal $s_i$. Since $A_i$ is the optimal solution for the overall problem, it cannot be better than the optimal solution for individual terminals. I.e.

$$\forall i \hspace{1mm} w(c_i) \le w(A_i)$$

Therefore, from transitivity, we have $w(c) \le \Sigma^{k}_{i = 1} w(c_i) \le 2w (A)$.

And now, as reasoned above, we can remove the heaviest cut from the collection the still be safe, so we have the following,

$$w(C) \le \Sigma^{k}_{i = 1} w(c_i) - \frac{1}{k} \Sigma^{k}_{i = 1} w(c_i) \le \left(1 - \frac{1}{k}\right) \Sigma^k_{i = 1} w(A_i) = 2 \left(1 - \frac{1}{k}\right) \cdot w(A)$$

$\hspace{120mm} \blacksquare$

Further more:

Notice here the middle term $\Sigma^k_{i = 1} w(c_i) - \frac{1}{k} \Sigma^k_{i = 1} x(c_i)$ can be transformed into $\left(1 - \frac{1}{k}\right) \cdot \Sigma^k_{i = 1} w(c_i)$. Which leads us to the following:

$$w(c) \le \Sigma^{k-1}_{i = 1} w(c_i) \le \Sigma^k_{i = 1} w(c_i) - \frac{1}{k} \Sigma^k_{i = 1} w(c_i)$$ 

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

$$\text{Cut Ratio} \Rightarrow \alpha = \frac{E(U, V \setminus U)}{\text{min} (|U|, |V \setminus U|)}$$

$$\text{Graph Expansion } \Rightarrow \alpha(G) = \text{min }_{U} \frac{E(U, V \setminus U)}{\text{min }(|U|, |V \setminus U|)}$$

***

Spectral Analysis

The **Laplacian Matrix** $L = D - A$ there:

* $A$ is the adjacency matrix
* $D$ is the diagonal matrix such that 
	* $d_i$ is the degree of the node $i$

To construct the matrix $A$, for each entry $(i, j)$ in $A$, if there is an edge between node $i$ and node $j$, we set the entry to $w(i, j)$. Otherwise, we set the entry to $0$.

$$A(i, j) = \begin{cases}w (i, j) \hspace{6mm} \text{if} \hspace{6mm} e(i, j) \in E \\ 0 \hspace{6mm} \text{otherwise}\end{cases}$$

To construct the matrix $D$, we only calculate the diagonal entires and leave all other entries to $0$. Since the entry on the diagonal has $i = j$, we set each entry $(i, i)$ to the degree of node $i$

Now we calculate the Laplacian matrix by $L = D - A$.

Therefore, we have the following properties:

* $L(i, i) =$ the degree of node $i$
* $L(i, j) = -1$ if there is an edge between $i$ and $j$.


## Laplacian Matrix

The laplacian matrix has the following properties:
1. The matrix $L$ is symmetric.
2. The matrix $L$ is positive semi-definite.
	* A matrix is positive semi-definite if all the eigenvalues of that matrix are positive.

3. The matrix $L$ has $0$ as an eigenvalue and the corresponding eigenvector $w_1 = (1, 1, ..., 1)$
	* Here, we use $\lambda_1 = 0$ to demote the smallest eigenvalue

