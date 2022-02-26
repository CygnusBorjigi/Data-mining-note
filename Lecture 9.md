# Lecture 9 

## K-means problem

Input: 

$\hspace{6mm}$ A set of $n$ $d$ -dimensional points 

$$X = \{x_1, x_2, …, x_n\} \text{ where } x_i \in \R^{d}$$

Output: 

$\hspace{6mm}$ A partition of $X$ into $k$ groups 

$$G_1, G_2, …, G_k$$

$\hspace{6mm}$ And the representative $r_{\mathcal{l}}$ for each group $G_{\mathcal{l}}$

Objective: 

$\hspace{6mm}$ Develop a function that minimize 

$$\Sigma^{k}_{\mathcal{l} = 1} \Sigma_{G_i \in G_{\mathcal{l}}} ||x_i - x_{\mathcal{l}}||^{2}_{2}$$

Note, the k-mean problem is $NP$ hard for $d \ge 2$

The focus of this lecture is on the case where $d = 1$ when the problem can be optimally solved in polynomial time.

#### Example:

Consider an example where the input to a function are just points on the real number line.

Remember the objective is to minimize $\Sigma^{k}_{\mathcal{l} = 1} \Sigma_{G_i \in G} (x_i - r_{\mathcal{l}})^2$

In this case, the difficulty is to find the grouping and the representative for each group will be easy to find.

Observe that in $1$ dimensional space, the clusters consists of continuous points. Therefore, in this case the partitions are the continuous. Which means the only thing we need to do is to pick the position of the boundaries.

##### Brut-force solution:

To begin, we can use brut-force method to find the optimal solution by calculating the cost of all possible combination of boundary points. Which will result in $\begin{pmatrix}n - 1 \\ k - 1\end{pmatrix} \approx n^k$ Number of calculations. Although it works, but we can do much better than this.

##### Dynamic Programming Solution

For the optimal solution, we can use dynamic programming. First, we need to define the following:

* $\text{Unit}(i, j)$ : The cost of placing points $x_i, x_{i + 1}, ..., x_j$ in to a cluster. The cost can be calculated by $\mu_{i, j} = \frac{\Sigma^j_{\mathcal{l} = i} x_{\mathcal{l}}}{j - i + 1}$. Therefore, $\text{Unit}(i, j) = \Sigma^{j}_{\mathcal{l} = i} (x_{\mathcal{l}} - \mu_{i, j})^2$
* $\text{Cost}(i, j)$: the cost of partitioning points $1, ..., i$ into $\mathcal{l}$ clusters

After those definition, we can say that for $k = 2$, where we only need to place one boundary point, we have

$$\text{cost}(n, 2) = \text{min }\{\text{ cost }(1, i) + \text{ unit }(i + 1, n)\}$$

And for general $k$, we have

$$\text{cost }(j, d) = \text{ min }_{1 \le i \le j} \{\text{ cost }(i, \mathcal{l} - 1) + \text{ unit } (i + 1, j)\}$$

$$\text{ cost } (n, k) = \text{ min }_{1 \le i \le n} \{\text{cost}(i, k - 1) + \text{unit }(i + 1, n)\}$$

Note: It is easy to observe that this algorithm is recursive.

Now, we can start building our dynamic programming algorithm by building a table.

The table will be $n \times k$ where the cost displayed in the $(n, k)$ entry of the table being the cost of the optimal solution.

##### Run time analysis

Note here we have to compute the cost of $(j, \mathcal{l})$ for every entry which has the cost of $n \cdot (\text{ lookup cost } + \text{ unit computation})$. With in that expression, the lookup cost is $O(n^3 + k)$ and the unit computation cost is $\text{ compute mean } + \text{ compute the associated error}$. Therefore, the cost of this whole algorithm is 

$$\mu_{i, j} = \frac{\Sigma^{j}_{\mathcal{l} = i} x_i}{j - i + 1} \Rightarrow \text{ unit }(i, j) = \Sigma^{j}_{\mathcal{l} = i}(x_{\mathcal{l} - \mu_{i, j}})^2$$

##### The recursion

The base case here is the first row of the table:

* $\text{cost }(1, 1)$ : Partition the first point into its own cluster $\text{ unit }(1, 1) = 0$
* $\text{cost }(2, 1)$ : Partition the first two points to a cluster $\text{unit }(1, 2)$
* $\dots$
* $\text{cost } (i, 1)$ : $\text{unit }(1, i)$

Following this procedure, we can fill in the first column of the table. Then we can start the recursion.

##### In implementation

To better reconstruct the decision tree we have to arrive at the $(i, j)$ entry, we can construct another table that store the decisions we have made for each computation. 

After the table is completely filled, we can trace back and reconstruct the solution. When doing so, we step back one column each time and only need to determine the correct row to trace to.

##### Improving the algorithm

First, we try to identify the bottle neck of the current algorithm. Notice that for every entry in the table $(j, \mathcal{l})$ we need to do the following computation

$$\text{cost }(j, \mathcal{l}) = \text{ min } \{\text{ cost } (i, \mathcal{l} - 1) + \text{ unit }(i + 1, j)\}$$

Notice that in this calculation, the first term $\text{ cost } (i, \mathcal{l} - 1)$ is a lookup in the table, which is an operation with constant complexity. However, for the operation $\text{ unit } (i + 1, j)$ we need to compute the mean of all the clusters and their distance to all the point in the cloud. This operation is of complexity $O(n^2)$. Therefore, the only way to improve the algorithm, we need to improve the complexity for the calculation of $\text{ unit }(i + 1, j)$. First, observe the following pseudo code:

	unit[i, j]
	s = 0
	
	for w = i ... j do
		s = s + x_w
		mean = s (j - i + 1)
		error = 0
		
		for w = i ... j do
			error = error + (x_w - mean)^2
			return error
		EndFor
	EndFor
	
Therefore, we aim to improve this process to constant complexity. Express the computation of unit as the following:

$$\Sigma^{j}_{w = i} (x_w - \text{ mean })^2$$

$$\Rightarrow \Sigma^{j}{w = i} (x^2_w - 2 \cdot \text{ mean } \cdot x_w + \text{ mean}^2)$$

$$\Rightarrow \Sigma^{j}_{w = i} x^2_w + \Sigma^{j}_{w = i} \text{ mean}^2 - 2 \cdot \text{ mean } \cdot \Sigma^{j}_{w = i} x_w$$

Since we know the mean can be calculated using the expression $\text{ mean } = \frac{1}{j - i + 1} \cdot \Sigma^{j}_{w = i} x_w$

Therefore, we can substitute the calculation of the mean into the calculation of the unit

$$\Rightarrow \text{ unit } \Sigma^{j}_{w = i} x^2_w + (j - i + 1) = \text{ mean}^2 + \left(\Sigma^{j}_{w - i} x_w\right)^2 - \frac{1}{j - i + 1} \cdot \left(\Sigma^{j}_{w = i}\right)^2$$

Observe that we can have a sub-routine to calculate all the means before the start of the algorithm. In that case, the calculation of unit also become a lookup operation and therefore, constant time.