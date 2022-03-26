# Lecture 14

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