# lecture 18

#### Definition: Sub-modular Function

A function $g$ is a sub-modular function if it is a set function 

$$g: 2^{v} \rightarrow \mathbb{R}$$

and for every two set $s$ and $t$ when $s \subseteq v, t \subseteq v$ and for every $x \not\in v \wedge x \not\in T$ we have

$$S \subseteq T \rightarrow g \left(s \cup \{x\}\right) - g(s) \ge g \left(T \cup \{x\}\right) - g(T)$$

#### Alternative definition: Sub-modular Function

A set function $f$ is sub-modular function is 

$$\forall A, B \subseteq E \hspace{6mm} F(A) + F(B) \ge F(A + B) + F(A \cap B)$$

#### Definition: Monotome Function

For a sub-modular function $f: 2^E \rightarrow \mathbb{R}$. The function $f$ is monotone if 

$$\forall A \subseteq B \subseteq E \hspace{3mm} f(A) \le f(B)$$

## Sub-modular Function (Optimization problem)

Assume we have a ground set $E$ and a function $f: 2^E \rightarrow \mathbb{R}$ where $2^{E}$ is the power set of $E$. We need to find a subset $Q \subseteq E$ such that $|Q| \le k$ and $F(Q)$ is maximized.

For example, in the real world application, we may have a restaurant with reviews. Say we have set $E$ which is a set of reviews of that restaurant. And we use $e \in E$ to represent each individual reviews. Each $e$ is associated with a set of attributes of the restaurant. We want to find a subset $Q \subseteq E$ such that $\bigcup \{e \in Q\}$ is equal to all the attributes of that restaurant.

#### Definition: Marginal Value

For all set $A$ and $E$ such that $A \subseteq E$ and $i \in E \setminus A$. the marginal value of $i$ with respect to $A$ is defined as 

$$f_A(i) = f(A \cup \{i\}) - f(A)$$

In other word, it is essentially saying how much does the value of the function change if we add $i$ to $A$.

#### Definition in terms of marginal value: Sub-modularity

$$\forall A \subset B \subset E, \forall i \in E \setminus B, \hspace{2mm} f_A(i) \ge f_B(i)$$

$$\text{Alternatively}$$

$$\forall A \subseteq E, \forall i, j \in E \setminus A, \hspace{2mm} f_A(i) \ge f_{A + j}(i)$$

#### Definition in terms of marginal value: Monotone

For a sub-modular function $f: 2^E \rightarrow \mathbb{R}$ is monotone if

$$\forall A \subseteq B \subseteq E, \hspace{2mm} f(A) \le f(B)$$

#### Lemma:

$\forall \alpha, \beta \in \mathbb{R} \wedge \alpha, \beta > 0$. If function $f$ and $g$ are sub-modular, then $\alpha f + \beta g$ is also sub-modular.

Recall the greedy algorithm discussed in last lecture, for every iteration, it only choose the node that maximizes the marginal gain at that iteration.

#### Theorem:

Let $c$ be the optimal solution and $s$ be the solution from the greedy algorithm. Then we have

$$F(s) \ge \left(1 - \frac{1}{e}\right) F(c)$$

