# Lecture 8

**Lemma:** If that distance function is metric, then

$$\Sigma^n_{i = 1} d(x_i, \bar{x}) \le 2 \cdot \Sigma^n_{i = 1} d (x_i, x^*)$$

Proof:

Assume that we are given a set of points $X = \{x_1, ..., x_n\}$ where $\bar{x} \in X$ and $x^*$ is in the same domain. Let $y$ be a iterator.

Let $x' \in X$ such that $x' = \text{ arg min }_{y \in X} d(x^*, y)$. 

Therefore, $\forall x_i \in X \hspace{2mm} d(x_i, x^*) \ge d(x', x^*)$. Since $x'$ is defined to always be the closest point to $x^*$

Since $\bar{x}$ is the sample representative, $\Sigma^n_{i=1} d(x_i, \bar{x}) \le \Sigma^n_{i=1} d(x_i, x')$

Here, we can apply the triangle inequality

$$\Sigma^n_{i=1} d(x_i, \bar{x}) \le \Sigma^n_{i=1} [d(x_i, x^*) + d(x^*, x')]$$

distribute the submission $\Sigma^n_{i=1} d(x_i, \bar{x}) \le \Sigma^n_{i=1} d(x_i, x^*) + \Sigma^n_{i=1}d(x^*, x')$

Again since $x'$ is defined to always be the closest point to $x*$ we say that $\Sigma^n_{i = 1} d(x_i, x^*)$ is greater than $\Sigma^n_{i=1} d (x', x^*)$. 

$$\Sigma^n_{i=1} d(x_i, \bar{x}) \le \Sigma^n_{i=1} d(x_i, x^*) + \Sigma^n_{i=1} d(x_i, x^*)$$

Simplify the terms and we get $\Sigma^n_{i = 1} d(x_i, \bar{x}) \le 2 \Sigma^n_{i = 1} d(x_i, x^*)$

$\hspace{100mm} \blacksquare$

## Partition Clustering

Remember that we define representative as given $X = \{x_1, x_2, ..., x_n\}$ where $x_i \in \mathcal{U}$ an the distance function $d$, we find $x^*$ such that it minimize

$$\Sigma^n_{i=1} d(x^*, x_i)$$

Now, we can define the concept of partition clustering

Given $X = \{x_1, x_2, ..., x_n\}$ where $x_i \in \mathcal{U}$ and a distance function $d$. Find a partition of $X$ into $k$ groups $G_1, G_2, ..., G_3$ such that is minimize the following

$$\Sigma^k_{\mathfrak{l} = 1} \Sigma_{x_i \in G_{\mathfrak{l}}} d(x_i, r_{\mathfrak{l}})$$

Where $r_{\mathfrak{l}}$ is the representative of group $G_{\mathfrak{l}}$.

### $k$ means algorithm

The $k$ means algorithm is a particular case in partition clustering where the distance function $d$ is the sum of squares of the Euclidean distance

$$x_i \in \R^d, r_{\mathcal{U}} \in \R^d$$

$$d(x_i, x_j) = \Sigma^d_{w = 1} (x_i^{(d)} - x_j^{(d)})^2$$

Ask about if it should be $x_i(d)$ or $x_i^{(d)}$

Therefore, if $d$ is $L_i$ then in this case

$$d(x_i, x_j) = \Sigma^d_{w = 1} |x_i^{(w)} - x_j^{(w)}|$$

