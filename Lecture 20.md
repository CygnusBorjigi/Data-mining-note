# Lecture 20

## Ranking aggregation

Ranking aggression is the process of the determining which is best suited for a given task. We usually do this using an objective function.

For example, consider two rankings $R_1$ and $R_2$ for elements $x_1, x_2, x_3, x_4, x_5$

$$R_1 \hspace{2mm} : \hspace{2mm} x_1, x_2, x_3, x_4, x_5$$

$$R_2 \hspace{2mm} : \hspace{2mm} x_5, x_4, x_3, x_2, x_1$$

One of the ways we can take measurement of those two rankings to calculate the agreement between those two rankings. This is called the disagreement distance:

$$I_{R_1, R_2}(x_i, x_j) \begin{cases}1 \hspace{2mm} \text{ if } \hspace{2mm} \begin{cases}R_1(x_i) < R_1(x_j) \wedge R_2(x_i) > R_2(x_j) \\ R_1(x_i) > R_1(x_j) \wedge R_2(x_i) < R_2(x_j)\end{cases} \\ 0 \hspace{2mm} \text{ otherwise}\end{cases}$$

And we can normalize the solution by $D_{R_1, R_2} = \frac{1}{\begin{pmatrix}n \\ 2\end{pmatrix}} \Sigma_{(x+i, x_j)} I_{R_1, R_2} (x_i, x_j)$ where $n$ is the total number of elements. 