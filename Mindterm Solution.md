# Mindterm Solution

## Question 1

Given that $d(x \cdot x) \rightarrow \R^2$ is a matrix. Show that 

$$D: x \times x \rightarrow \R \text{ where } D(x, y) = \text{ min }(d(x, y), 1)$$

###### Solution:

Case analysis.

## Question 2

given objective function $\Sigma^{l}_{i = 1} \Sigma_{x_j \in l_i} (x_j - \mu_i)^2 + \alpha l \log(n)$ where $\alpha \ge 1$. (The greater the $\alpha$ the greater we penalize the more clusters)

###### Solution:

($1 -D$ k-means clustering)

When we are building a table that has $n$ rows and $k$ columns. $n$ is the number of data points we considering and $k$ are the number of clusters we divide them into. Therefore, $\text{table}[i, k] = \text{ min }_{k \le j \le i - 1} \left(\text{table}[j, k - 1] + \text{ unit }[i, j] + \alpha \log(n)\right)$. Therefore, after the table is finished, the minimum number in the bottom row corresponds to the optimal number of clusters. 

Normally, since there are $n^2$ entires in the table and we have $n$ computation, therefore, we have a runtime of $n^3$.

######  Optimization:

later

## Question 4

Given $F(c_1, c_2, ..., c_k) = \text{ min }_{c_i, c_j} \text{ min }_{x \in c_i, y \in c_j} d(x, y)$. We partition data into clusters that maximum the expression. (single link distance).

###### Solution:

Use hierarchical clustering since it allows us to merge clusters together.