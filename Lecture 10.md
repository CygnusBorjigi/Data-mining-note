# Lecture 10

## Local $k$ means algorithm

A local search algorithm that makes local improvements.

The process of the algorithm is as follows

First, puck a random center. Then, we change it a little bit. After that we check if the change made the cost better. If it indeed made it better, we keep the change, otherwise we discard the change. Then we make another change. Repeat the process and stop when the result converges.

Since this is beyond the premise of this class, we would just accept that this algorithm converges in polynomial time.

## $k$ center problem

Note that there is no notion of representative in this problem.

Consider the input of a set of points $X = \{x_1, x_2, ..., x_n\} \text{ where } x_i \in \R^d$ and $k \in \Z$

We need to generate as output a partition of $x$ into $k$ clusters $C_1, ..., C_k$ such that the maximum diamater is minimized. Similar to the $k$ means problem, the $k$ center problem is also $NP$ hard when $d > 1$. Therefore, for this lecture, we only consider the case when $d = 1$.

*Definition: Diameter*

Let $S$ be a group of points. The diameter of $S$ is defined as 

$$\text{max } d(x, x') \text{ where } x, x' \in S \times S$$

#### Solution

Assume $x_i \in \R$ as discussed before, the clustered will contain continuous points. And in this case there is actually a linear time algorithm.

Think of the following, we can introduce a threshold $\tau$ to the solution. Such that we never produce any cluster that has a diameter greater than $\tau$.

Notice that $\tau$ can be set to the distance between any two points in the input. Therefore, since there are $n$ points in the input set, there are at most $n^2$ values available for $\tau$.

Also notice, once we determine the value of $\tau$ the clustering becomes very simple. We can start from the fist point in the input, and initiate a cluster, or each of the following point we check the distance between it and the first point in the cluster. If the distance is less than $\tau$, we grow the cluster by including that point, else we finish the cluster and initiate a new cluster with that point at its beginning.

In this way, even in the worst case we try the all $n^2$ of the $\tau$ s. However, there is a much better way.

Notice that there exist a linear relation between the value of $\tau$ and the number $k$ clusters are created. In that, the greater the $\tau$ value is, the smaller $k$ number of clusters are created. Therefore, we can sort the $\tau$ s linearly and do a binary search.

Therefore, the run time of this algorithm is $n \cdot \log n^2 \Rightarrow O(n \log n)$

## Clustering Aggression

One of the most important step in any clustering problem is choosing a proper $k$. And one of the best way to approach this problem is through eyeballing.

We can plot the a graph with the $k$ values on the $x$ axis and the value of the objective on the $y$ axis and look for a significant drop in the line.

/Screen Shot 2022-02-25 at 7.01.59 PM.png

What clustering aggression does is that it take as input a set of clustering of the same set of point. I.e. different partition of the $C_1. C_2, ..., C_m$ data. Notice that the number of cluster each clustering produce may be different.

Then, it output a clustering $p$ such that is minimizes

$$\Sigma^{m}_{i = 1} D(P, c_i)$$

I.e. it minimizes the distance between it can all other clustering. Essentially, it is just a sample representation problem.

Now, the problem become how can we find a good distance function for this case.

To solve this, we can represent all the clusters in a table, where each row of the table represent a point in the data cloud, and each column in the table represent a cluster. In the way, each entry in the table will denote the assignment of cluster for each point.

|  | $$C_1$$ | $$C_2$$ | $$\dots$$ | $$\dots$$ |
|:--|:--|:--|:--|:--|
| $$x_1$$ | $$1$$ | $$1$$ | $$\dots$$ | $$\dots$$ |
| $$x_2$$ | $$1$$ | $$2$$ | $$\dots$$ | $$\dots$$ |
| $$\dots$$ | $$\dots$$ | $$\dots$$ | $$\dots$$ | $$\dots$$ |
| $$x_n$$ | $$4$$ | $$5$$ | $$\dots$$ | $$\dots$$ |

Here, using the able, we can measure the number of agreement between any two clusterings in terms of their assignment of points.

$$d_{x_i, x_j}(C_1, C_2) = \begin{cases}1 \text{ if } C_1(x_i) = C_1(x_j) \wedge C_2(x_i) ≠ C_2(x_j) \text{ OR } C_2(x_i) ≠ C_2(x_j) \wedge C_2(x_i) = C_2(x_j)\\ 0 \text{ otherwise}\end{cases}$$

$$D(C_1, C_2) = \Sigma_{x_i, x_j} d_{x_i, x_j}(C_1, C_2)$$

How, we show that $D$ is a metric. The non-negative, symmetry and isolation are obvious. The following is the proof for the triangular inequality.

show $\forall P, Q, C$

$$D(P, Q) \le D(P, C) + D(C, Q)$$

$$\Downarrow$$

$$\Sigma_{x_i, x_j} d_{x_i, x_j} (P, Q) \le \Sigma_{x_i, x_j} d_{x_i, x_j}(P, C) + \Sigma_{x_i, x_j} d_{x_i, x_j} (C, Q)$$

Here, we can fix a pair of $x_i$ and $x_j$

$$d_{x_i, x_j} (P, Q) \le d_{x_i, x_j}(P, C) + d_{x_i, x_j} (C, Q)$$

Be definition, all of them can only be either $0$ or $1$. Therefore, the only case we need to worry about is when $d_{x_i, x_j} (P, Q) = 1$, $d_{x_i, x_j}(P, C) = 0$, and $d_{x_i, x_j} (C, Q) = 0$. And we are going to show that this case is impossible.

Observe that the implication of the above situation is

$$d_{x_i, x_j} (P, Q) = 1 \Rightarrow P(x_i) = P(x_j) \wedge Q(x_i) ≠ Q(x_j)$$

$$d_{x_i, x_j} (P, C) = 0 \Rightarrow P(x_i) = P(x_j) \wedge C(x_i) = C (x_j)$$

$$d_{x_i, x_j} (C, Q) = 0 \Rightarrow C(x_i) = C(x_j) \wedge Q(x_i) = Q(x_j)$$

And here, we have reached a contradiction between the first can the their implications. Therefore, this case will never happen, and therefore, the triangular inequality is proven.