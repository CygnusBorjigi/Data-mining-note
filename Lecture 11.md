# Lecture 11

# Hierarchical Clustering

#### Input: 

* A set of point $X = \{x_1, ..., x_n\} \subset \R$
* A distance function $d()$ where
	* $d(x_i, x_i) = 0$
	* $\forall x_i, x_j \in X$ where $x_i ≠ x_j$ we have $d(x_i, x_j) > 0$
	* $d(x_i, x_j) = d(x_j, x_i)$

#### Output: 

A set of nested clusters organized as a hierarchical tree.

/Screen Shot 2022-03-02 at 2.48.08 PM.png

Note:

* There are no assumptions on the number of clusters. I.e. any desired number of clusters can be obtained by "cutting" the diagram at the proper level.
* There are no defined function to measure or compare different clustering against each other.

#### Two main types of hierarchical clustering

1. Agglomerative:

	* Initialization: Start with the points in the input set, each as their own cluster
	* Iteration: For each iteration, merge the closest pair of clusters until only one cluster (or $k$ clusters, if specified in the input) left

2. Divisive:

	* Initialization: Start with one, all-inclusive cluster
	* Iteration: At each iteration, split the clusters until each cluster contains only one point (or when there are $k$ clusters, if specified in the input)

#### Computational Complexity

* Distance matrix is used for deciding which cluster to merge/split
* At least quadratic in the number of data points
* Not usable for large dataset

#### Agglomerative Clustering

This is one of the most popular hierarchical clustering algorithm

Steps:

1. Compute the distance matrix between all the input data points
2. Assign each data point to its own cluster
3. Merge the two closest cluster into one
4. Update the distance matrix
5. Repeat step 3 and 4 until there is only one cluster remaining OR there are $k$ clusters remaining id specified in the input.

Note:
* The key operation in this algorithm is the computation of the distance matrix between two clusters.
* Following the first point, different definition of the distance function can lead to different out come of the algorithm.

#### Define the distance between two clusters

##### Single-link distance

According to this distance function, let $C_i$ and $C_j$ be two clusters. Then the distance between those two clusters is the minimum distance between any object in $C_i$ and any object in $C_j$

$$d(C_i, C_j) = \text{ min } \{d\left(x(i), x(j)\right) | x(i) \in C_i, x(j) \in C_j\}$$

Notice the limitation of the single-link distance is:

1. It is sensitive to nice and outliers
2. It produces long, elongated clusters

##### Complete-link distance

According to this distance function, let $C_i$ and $C_j$ be two clusters. Then the distance between those two clusters is the maximum distance between any object in $C_i$ and any object in $C_j$.

$$D(C_i, C_j) = \text{ max } \{d(x(i), x(j)) | x(i) \in C_i , x(j) \in C_j\}$$

Notice the limitation of the complete-link distance is:

1. Tends to break large clusters
2. All clusters tend to have the same dimeter since the small clusters are merged into larger ones.

#### Group Average distance

According to this distance function, let $C_i$ and $C_j$ be two clusters. The the distance between those clusters is the average distance between any two objects in $C_i$ and any object in $C_j$

$$D(C_i, C_j) = \frac{1}{|C_i| \times |C_j|} \Sigma_{x(i) \in C_i, x(j) \in C_j} d(x(i), x(j))$$

Notice that the limitation in this case is:

* The distance function is biased towards globular clusters

#### Ward's distance

According to this distance function, let $C_i$ and $C_j$ be two clusters. The distance between those two clusters is the difference between the total within cluster sum of the squares for the two clusters separately, and the within clusters sum of the squares resulting from merging the two clusters in cluster $C_{ij}$

$$D(C_i, C_j) = \Sigma_{x \in C_i} (x - r_i)^2 + \Sigma_{x \in C_j} (x - r_j)^2 - \Sigma_{x \in C_{ij}} (x - r_{ij})^2$$

Where $r_i$ is the centroid of $C_i$, $r_j$ is the centroid of $C_j$ and $r_{ij}$ is the centroid of $C_{ij}$

/Screen Shot 2022-03-02 at 3.35.01 PM.png

#### Hierarchical Clustering: Time and Space complexity

Let $X$ be the dataset with $n$ data points. Then the space complexity is $O(n^2)$ and the time complexity is $O(n^3)$

For the times complexity, notice that there are $n$ steps and at each steps, the distance matrix, which has the size of $n^2$ must be updated and searched.

Also notice that the complexity can be reduced to $O(n^2 \log(n))$ for some of the approaches by using appropriate data strictures.

### Divisive hierarchical clustering

In this algorithm, we start with a single cluster containing all the data points. Then, at each iteration, we split the cluster into components until the we have separated each point into their own cluster OR when there are $k$ clusters if it is specified in the input.

This method is computational intensive, therefore is less common.

#### Clusterings of maximum spacing

Consider a set of points $X = \{x_1, ..., x_n\}$ and a distance function $d$ such that:

* $d(x_i, x_i) = 0$
* $d(x_i, x_j) > 0 \leftrightarrow x_i ≠ x_j$
* $d(x_i, x_j) = d(x_j, x_i)$

The goal here is to partition $X$ into $k$ groups so that the distance between any pair of points lying in different clusters is maximized

