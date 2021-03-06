# Visualization of Non-Numeric Data
- [Graphs and Networks](#graphs-and-networks)
- [Embedding Planar Graphs](#embedding-planar-graphs)
- [Graph Visualization](#graph-visualization)
- [Dimensionality Reduction](#dimensionality-reduction)

## Graphs and Networks
### Graphs
- Directed v. undirected
- Cyclic v. acyclic
- Tree: minimally connected, n nodes, n-1 edges
- Hierarchy: acyclic direected graph

### Face
_The region bounded by cycle of edges, starting and ending at the smae node_

### Isomorphic
_Two different embeddings of the same graph

### Node Degree
_= Number of edges_

- Directed graph nodes have an in-degree and an out-deggree
- Social networks
  - many low degree nodes and fewer high deggree nodes
  - logarithmic, power-law graphs
  <img src="images/social-networks.png" height="250px">
  
### Adjacency Matrix
_Matrix representation of a graph_

<img src="images/adjacency-matrix.png" height="200px">

- Square matrix: # of rows & columns = # of nodes
- [i, j] is set to 1 if edge connects i and j
- Symmetric, unless directed graph
- Diagonal represent edge between node and itself

## Embedding Planar Graphs
### Planar Embedding
_Edges connecting the nodes don't cross_

<img src="images/planar-graph.png" height="200px">

### Graph Embedding
- Create the graphc Laplacian Matrix: Adjacency matrix with elements <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;L_{ij}=\frac{1}{degree(i)}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\inline&space;L_{ij}=\frac{1}{degree(i)}" title="L_{ij}=\frac{1}{degree(i)}" /></a> for an edge between i and j

  <img src="images/laplacian-matrix.png" height="200px">

- Zero out the rows for nodes we have already positioned, subtract it from the identity matrix

  <img src="images/zero-out.png" height="200px">
- Create linear systems of equations
  - Solve <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;Ax=B_x" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\inline&space;Ax=B_x" title="Ax=B_x" /></a> for x coordinates
  - Solve <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;Ay=B_y" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\inline&space;Ay=B_y" title="Ay=B_y" /></a> for y coordinates

  <img src="images/linear-systems.png" height="200px">
  <img src="images/linear-systems-2.png" height="200px">

## Graph Visualization
### GEM Force Directed Layout
- Edges exert spring force on their nodes
  - Spring force diminished by node degree, making higher degree nodes "heavier"
- Nodes mutually repel each other
  - strength ~ 1/distance
- All nodes experience global forces
  - Gravitational force toward center

### Centralities
_A way of analyzing where a node should be positioned in a layout_
- Node degree: high degree appears in the center, low degree appears on the boundary
  - e.g. PageRank: sum of PageRanks of incoming links
- Isolation metric: total distance to all other nodes
  - Distance: the shorted number of edges to get from one node to another node
  - Closeness centrality = 1/ isolation metric
  - Graph centrality = 1/ distance to the farthest node
- Betweeness centrality
  - portion of all shortest paths between any two nodes that pass through the given node

### Edge Bundles
_Aggregate "similar" edges into "wire bundles" to simplify presentation_

<img src="images/edge-bundles.png" height="200px">

### Community Discovery
- Remove edges in order of decreasing Between Centrality
- As higher BC edges merge lower-level communities

## Dimensionality Reduction
_Better perceive the high dimensional data_

### Principal Component Analysis
_Find the dimension the data varies a lot_
- Compute a covariance matrix
- Eigenvalue: how ellipsoid is stretched and squashed
- Eigenvector: what direction the stretching and squashing occur in 
- Principal Component Analysis: keeps only the eigenvectors corresponding to largest magnitude eigenvalues

<img src="images/mandible.png" height="200px">

### Mutidimensional Scaling
_Preserve the distance between data when displaying them in lower dimension_

- Given distances <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;d_{ij}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\inline&space;d_{ij}" title="d_{ij}" /></a> between i and j
- Find positions <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;(x_i,&space;y_i)" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\inline&space;(x_i,&space;y_i)" title="(x_i, y_i)" /></a> 
- that minimize a stress function <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\sum_{j=1...N-1}^{}\sum_{i=i&plus;1...j}(\left&space;\|&space;(x_i,&space;y_i)-(x_j,&space;y_j)&space;\right&space;\|-d_{ij})^2" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\inline&space;\sum_{j=1...N-1}^{}\sum_{i=i&plus;1...j}(\left&space;\|&space;(x_i,&space;y_i)-(x_j,&space;y_j)&space;\right&space;\|-d_{ij})^2" title="\sum_{j=1...N-1}^{}\sum_{i=i+1...j}(\left \| (x_i, y_i)-(x_j, y_j) \right \|-d_{ij})^2" /></a>


