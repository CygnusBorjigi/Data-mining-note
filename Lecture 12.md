# Lecture 12 Graph Clustering

## Definitions:

***

#### Definition: s-t cut

Given:
1. A graph $G = (V, E)$ 
2. A node $s$
3. A node $t$.

An s-t cut denoted $C = (S, T)$ of the graph is a partition of $V$ in to $S$ and $T$ such that $s \in S$ and $t \in T$

***

#### Definition: Min s-t cut

A special case of s-t cut where the input graph is weighted and we are given an additional cost function. In this case, we need to minimize th following:

$$\text{cost}(c) = \Sigma_{e(u, v) \text{ where } u \in S \hspace{1mm} \wedge \hspace{1mm} v \in T \hspace{1mm}} w(e)$$

I.e. The cost is calculated by adding up the weight of all the edges connecting the sub-graph $S$ and the sub-graph $T$

***

#### Definition: Flow Network

This can be seen as an abstraction for material following through the edges.

Given 

1. A directed graph $G = (V, E)$ with no parallel edges. 
2. A function $c$ such that $c(e)$ gives the capacity of and edge $e \in E$.

In this case, we can calculate the flow of each edge in the graph using the function $c$. And, if we are given a subgraph of $G$, we can calculate the flow in and out that sub graph by adding up the individual capacity of each edge connecting that sub-graph to the rest of the graph.

***

#### Definition: s-t flow

Given a graph $G = (V, E)$ and a s-t cut of that graph $C = (S, T)$

An s-t flow is a function that satisfies the following:

1. $\forall e \in E \hspace{3mm} 0 \le f(e) \le c(e)$
2. $\forall v \in V \hspace{3mm} \Sigma_{e \text{ in to } v}f(e) = \Sigma_{e \text{ out of } v} f(e)$

The second rule as also known as the conservation property of s-t flow.

***

#### Flow value lemma
Let $f$ be any flow and let $(S, T)$ be any s-t cut. Then, the net flow sent across the cut is equal to the amount flow leaving $s$

$$\Sigma_{e \text{ out of } s} f(e) - \Sigma_{e \text{ into } s} f(e) = v(f)$$

***

#### Weak Duality
Let $f$ be any flow and let $(S, T)$ be any s-t cut. Then the value of the flow is at most the capacity of the cut defined by $(S, T)$

$$v(f) \le \text{cap}(S, T)$$

***

#### Certificate of Optimality

Let: 

1. $f$ be any flow
2. $(S, T)$ be ay cut

Then:

If $v(f) = \text{cap }(S, T)$ then $f$ is a max flow and $(S, T)$ is a min cut.

***

## Basic Problems

### The min s-t problem

##### Input: 
1. A undirected potentially weighted graph, $G = (V, E, \omega)$ 
2. A pair of nodes $s \in V$ and $t \in V$ such that $s ≠ t$.

##### Output:

$V_1$ and $V_2$ such that $V_1 \cup V_2 = V$ and $V_1 \cap V_2 = \empty$. I.e. a partition of $V$.

##### Objective:

Minimize the following expression:

$$\Sigma_{e(u, v)} w (e) \text{ where } u \in V_1, v \in V_2$$

##### Solution:

To find the s-t cut with the minimum capacity, we have use the **flow techniques** to resolve the problem in polynomial time.

An s-t flow us a function that satisfies the following:

1. $\forall e \in E$ we have $0 \le f(e) \le c(e)$
2. $\forall v \in V - \{s, t\}$ we have $\Sigma_{e \text{ into } v} f(e) = \Sigma_{e \text{ out of } v} f(e)$ i.e conservation

In which case, the value of the flow $f$ is $v(f) = \Sigma_{e \text{ out of } s} f(e)$

One of the varient of the problem can be that the algorithm is not given the $s$ and the $t$.

Note: Both of those problem can be solved in polynomial time.

***

### Randomized min-cut algorithm

The algorithm does the following:

1. It first pick an edge uniformly at random.
2. Take the selected edge and two nodes on either end, merge those two nodes into a super node.
3. Remove the selected so that we do not have edges within a super node.
4. Repeat steps 1 to 3 until there are only two nodes left.
5. Set the edges connecting the remaining two super nodes as a candidate of the min-cut.

Since the algorithm is random, we run it several times and compare the expected output. We do this by compute the probability that during the execution, non of the edges of the min-cut is left untouched. I.e. not contracted.

Since we contract one edge at every iteration, we will reach the result after $n-1$ iterations.

***

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

* $A$ is 