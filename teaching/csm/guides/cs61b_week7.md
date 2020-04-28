# Graphs and Searches
 The next data structure you'll explore in 61B is the graph. Graphs are one of the most important data structures in computer science, since they model so many real world systems and problems. This guide assumes you know the basic vocabulary behind graphs (vertices, edges, adjacent vertices, cycles, weighted, directed, connected).

## Traversals
A _traversal_ is a specific order of visiting vertices in a graph. 

### Tree Traversals
There are 2 main types of traversals for trees:
1. Depth-First Traversal
2. Level Order Traversal

> Remember a tree is a graph that is acyclic and has $E = V - 1$ (the number of edges is one less than the number of vertices)

Let's start with level order traversal, since it's a bit easier to understand. To perform a level order traversal of a tree, simply visit the vertices in top-to-bottom, left-to-right order.

For depth first traversal, there are 3 types:
1. Pre-order traversal
2. In-order traversal
3. Post-order traversal

This the rough pseudocode for each, starting at the root of the tree:
```
// Pre-Order
def preorder(root):
	visit(root)
	preorder(root.left)
	preorder(root.right)

// In-Order
def inorder(root):
	inorder(root.left)
	visit(root)
	inorder(root.right)

// Post-Order
def postorder(root):
	postorder(root.left)
	postorder(root.right)
	visit(root)
```
**Common Mistake**: Remember that these traversals are recursive, so make sure to traverse the entire left subtree or right subtree before coming back to the root.

### Graph Traversals
There are also traversals for general graphs, not just trees! The 2 main traversals you'll see are DFS (depth-first search) and BFS (breadth-first search).

Both BFS and DFS are iterative methods that use a data structure (called the _fringe_) to store which vertices to visit in the next iteration. The difference between DFS and BFS is the data structure used for the fringe. For DFS, the fringe is a stack. For BFS, the fringe is a queue. Here is the rough pseudocode for both BFS and DFS:
```
initialize the fringe (Stack for DFS, Queue for BFS)

add the start vertex to the fringe

while the fringe is not empty:
	remove a vertex v from the fringe
	for each neighbor of v:
		add the neighbor to the fringe
	visit v
```

> Note: Technically, you also have to keep track of which vertices you've already visited so you don't visit a vertex twice.

## Shortest Paths
One of the classic graph problems is to find the shortest path between vertices in a graph. There are 2 algorithms that do this: Dijkstra's and A*.

### Dijkstra's
Dijkstra's algorithm, in addition to computing the shortest path from $S$ to $T$, also computes the shortest path from $S$ to all other vertices in the graph.

Let's look at the pseudocode for Dijkstra's:
```
def dijkstra(G):
	PQ = priority queue
	PQ.add(S, 0) // S is the start vertex
	while PQ is not empty:
		v = PQ.removeMin()
		for each neighbor of v:
			PQ.add(v, distance from S to v + distance from v to the neighbor)
```
> Technically, if the neighbor you are trying to add to the fringe is already on the fringe, you will just update its priority, not add it to the fringe again.

Notice that Dijkstra's is just the same as the pseudocode above, but the fringe is just a priority queue! Thus, we can think of Dijkstra's as BFS, where the fringe is a priority queue and the priority of elements in the priority queue is just the distance from $S$. 

### A*
A* finds the shortest path between a pair of vertices $S$ and $T$. A* is just Dijkstra's, but instead of the priority of elements being just the distance from $S$, the priority is the distance from $S$ plus the estimated distance to $T$, given by the _heuristic_. The worksheet solutions for this week have a good blurb on heuristics, so I suggest you take a look at that.

> Notice that A* with the heuristic equal to $0$ for all vertices is just Dijkstra's.

### Runtime
Dijkstra's uses 3 operations of a priority queue: `removeMin`, `add`, and `updatePriority`. This table summarizes the runtime analysis:
| Operation | Time per Operation  | # Operations |
|--|--| -- |
| `removeMin` | $O(log(V))$ | $V$ |
| `add` | $O(log(V))$ | $V$ |
| `updatePriority` | $O(log(V))$ | $O(E)$ |

> The $O(log(V))$ times come from the assumption that the priority queue is implemented using a heap.

Adding the times up, we get $O(V\ log \ V) + O(V \  log \ V) + O(E \ log \ V) = O((V + E) \ log \ V)$. Since $E$ is usually greater than $V$, the runtime of can be expressed as $O(E \ log \ V)$. 

**Common Mistake**: Many students think A* has a faster runtime than Dijkstra's because A* finds the shortest path from $S$ to $T$ faster than Dijkstra's in most cases. However, keep in mind that Dijkstra's and A* use the same pseudocode; thus, in the worst case A* is just as bad as Dijkstra's.


## Minimum Spanning Trees (MSTs)

If a graph with $V$ vertices and $E$ edges is connected, then we can find at least one set of $V - 1$ edges (a tree) that connects the graph. The tree that has the minimum sum of the weight of all the edges in it is called a _minimum spanning tree_ (MST).

There are 2 algorithms that compute an MST: Prim's and Kruskal's.

### Prim's
Prim's algorithm uses the following pseudocode:
```
def prim(G)
	T = {} // set of vertices in the MST
	T.add(s) // add the start vertex
	E = {} // set of edges in the MST
	while there are less than V - 1 elements in E:
		e = lightest edge where one endpoint is a vertex in T and the other point is a vertex not in T
		T.add(vertex at the endpoint of e that is not in T)
		E.add(e)
	return E
```
### Kruskal's
Kruskal's algorithm is described in the worksheet.

# Conclusion
Graphs are a really abstract thing to wrap your head around, so don't feel bad if you find these algorithms to be a little over your head right now. Even I'm still not completely comfortable with them. That being said, graphs are fundamental to a lot of topics in computer science, so it's worth it to put in the effort to learn them now.
