## Graphs

- Multiple predecessors and successors
- Directed and bi-directed graphs
  - If the direction of predecessor/successor does not matter, bi-directed graph can be used
- Terminology
  - V is a set of nodes/vertices (vertex)
  - E is a set of edges in directed graph (from node to node)
  - Node can have n neighbours
  - Node can be a neighbour to itself
  - Path is a line of nodes in a directed graph
  - Ring is a path which returns to the first node
- Both nodes and edges can contain useful information
- Nodes and edges can be weighted (e.g. price, kilometres etc.)
- If needed, markings of handled nodes and edges can be used (usually referenced as colors)
- Graph is made of sets of nodes and edges
  - Modified set operations are used for handling them
  - Nodes are clearly a set
  - Edges are actually a relation

### Depth first search

- Start from a node and color it (means already visited)
- Move to neigbours, if they're not colored yet
- If something was not visited yet, pick a new starting node
- Classifying edges
  - Tree edge: leads to unvisited node
  - Back edge: leads back to predecessor
  - Forward edge: leads from node to successor
  - Crossed edge: do not lead to predecessor or successor
- Tree edges form a spanning forest, i.e. set of directed trees, which contain all of the nodes in the graph
  - If a spanning forest contains only one tree, it is called spanning tree
  - There can be multiple different spanning trees depending on the root node
