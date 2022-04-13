# Lecture 17

# Summarization (The Set covering problem)

Consider a dictionary $w$ and document $D_i$ such that 

$$D_1 \subseteq w$$
$$D_2 \subseteq w$$
$$D_3 \subseteq w$$
$$\vdots$$
$$D_n \subseteq w$$

We need to find a subset of the documents such that it covers the whole dictionary. By "cover", we mean the union of the words in the subset equals the whole dictionary.

$$Q \subseteq \{D_1, ..., D_n\} \text{ s.t } \bigcup D_i = w \text{ where } D_i \in Q$$

We can also consider a variant of the question. Say we are provided a group of experts 

$$E =\{e_1, e_2, ..., e_n\}$$

Each of those expert has their price

$$P = \{p_1, p_2, ..., p_n\}$$

There are a set of all possible skills

$$S = \{s_1, s_2, ..., s_n\}$$

Such that each expert has a subset of all the skills.

$$\forall e_i \in E, \hspace{2mm} \exists s_{e_i} \subseteq S$$

Now, consider we have a task at hand, we need to hire a group of experts whose union of their skills convert skills required for the task. We want the chosen group of expert to have the minimum price.

$$\text{Min }|Q| \text{ OR Min}F(Q) = \Sigma_{E_i \in Q} \hspace{2mm}p_i$$

***

#### Formalized definition of sir cover problem

Input:

1. A universe of items $\mathcal{U} = \{u_1, u_2, ..., u_n\}$
2. A collection of subset of the universe $\mathcal{S} = \{s_1, s_2, ..., s_n\}$ such that $s_i \subseteq \mathcal{U}$ and $\bigcup_{s_i \in S} s_i = \mathcal{U}$

Output:

A subset of $Q \subseteq \mathcal{S}$ such that $\bigcup_{s_i \in Q} s_i = \mathcal{U}$ where $|Q|$ is minimized.

Notice that this is a minimize problem and we can assign different weight to $s_i$. The problem the NP hard.

***

Example:

Input:

$$\mathcal{U} = \{a, b, c, d, e\}$$

$$\mathcal{S} = \{s_1, s_2, s_3\}$$

$$s_1 = \{a, b\}$$

$$s_2 = \{b, c, d, e\}$$

$$s_3 = \{c, d, e\}$$

Expected output:

$$Q = \{s_1, s_3\}$$

Algorithm:

(Greedy algorithm for set cover)

1. Select the largest-cardinality set $s$ from $\mathcal{S}$
2. Remove the elements that are in $s$ from $\mathcal{U}$
3. Recompute the size of the remaining sets in $\mathcal{S}$
4. Repeat from the first step.

Algorithm

$X = \mathcal{U}$

$C = \{\}$

While $X$ is not empty do:

$\hspace{6mm} \forall s \in \mathcal{S}$ let $a_s = | s \cap X |$

$\hspace{6mm}$ let $s$ be such that $a_s$ is maximal

$\hspace{6mm} C = C \cup \{s\}$

$\hspace{6mm} X = X \setminus s$

Analysis of the algorithm:

We let the output of the greedy algorithm be $c$. And let the optimal solution to the problem to $o$. Now, we can claim that

$$F(o) \le F(c)$$

Since the greedy algorithm did not consider all possible solutions. We can show that 

$$F(o) \le F(c) \le \alpha F(o) \text{ where } \alpha > 1$$

Here, the $\alpha$ is the approximation factor.

We claim that 

$$F(c) \le O(\log n) F(o)$$

And this is the best approximation possible. 

Proof: (The greedy algorithm is a $O(log n)$ approximation)

Notice that the greedy algorithm is $|S_{\text{max}}|$ factor approximation and also remember that $S = \{s_1, s_2, ..., s_n\}$ such that $s_i \subseteq \mathcal{U}$. In this case, we observe that $S_{\text{max}} = \text{ argmax }_{s_i \in S} |s_i|$

Now, we claim that the product of the algorithm $c$ is such that $F(c) \le |S_{\text{max}}| \cdot F(o)$. Since $F(o) \ge \frac{n}{|S_{\text{max}}|}$, we have $n \le |S_{\text{max}}| F(o)$ and therefore $F(c) \le n \le |S_{\text{max}}| F(o)$

## K-coverage problem

Now, we add a new variable $k$ into the input. Here, the question change into find $Q \subseteq |S|$ such that $|Q| = k$ where $k$ is a fixed cardinality subset and $g(Q) = |\bigcup_{s_i \in Q} S_i|$ is maximized.

Notice that this problem is also NP hard, and it is also a maximization problem.

In this case, we let $Q$ to be the found solution, and $O$ be the optimal solution. Therefore, we can claim that $g(O) \ge g(Q)$ and in this case $g(Q) \ge \alpha \hspace{1mm} g(O)$ where $\alpha < 1$

Algorithm:

$Q = \emptyset$

$x = \mathcal{U}$

for $i = 1 \dots k$
    
$\hspace{6mm} I_s = |S \cap x| \forall s$ pick the $s$ with the greatest $I_s$

$Q = S \cup Q$

$x = x \setminus Q$

Theorem:

The greedy algorithm is $\left(1 - \frac{1}{e}\right)$ approximation

$$g(Q) \ge \left(1 - \frac{1}{e}\right) g(O)$$

And this is the best approximation possible.

### Definition: Sub-modular Function

A function $g$ is a sub-modular function if it is a set function 

$$g: 2^{v} \rightarrow \mathbb{R}$$

and for every two set $s$ and $t$ when $s \subseteq v, t \subseteq v$ and for every $x \not\in v \wedge x \not\in T$ we have

$$S \subseteq T \rightarrow g \left(s \cup \{x\}\right) - g(s) \ge g \left(T \cup \{x\}\right) - g(T)$$

What it is essentially saying is that when a set grows, the marginal gain decreases as the cardinality increases.