
## Graph

#### Graph ADT(abstruct data type)
```
Data:
- Vertices
- Edges
- Some data structure maintaining the structure between vertices and edges
Functions:
- insertVertex(K key);                      O(n)
- insertEdge(Vertex v1, Vertex v2, K key);  O(1)
- removeVertex(Vertex v);                   O(m)
- removeEdge(Vertex v1, Vertex v2);         O(1)
- incidentEdges(Vertex v1, Vertex v2);      O(n)
- areAdjacent(Vertex v1, Vertex v2);        O(1)
- origin(Edge e);
- destination(Edge e);


n: Vertixの数
m: Edgeの数
incident edgev: all of the edges on a node are its incident edges.
degree of the node: the count of how many incident edges that it has. 何個incident edgeを持っているか。
simple graph: a graph with no self loops. ( Max edges = n(n-1)/2 = ~O(n^2) )
```

Edge List<br>
- The Edge List performs worse in general than the Adjacency Matrix and the Adjacency List representations,<br>
but it is much simpler and easier to implement.<br>
It also takes less space than the alternatives, and can insert vertices and edges in constant time. <br>
The adjacency list can also insert vertices and edges in constant time, <br>
but if those are the only operations needed, <br>
then one need not waste space and additional code on building the adjacency list on top of the edge list.<br>

space: ◎<br>
insert: ◎<br>
remove: △<br>
incidentEdges: △<br>
areAdjacent: ×<br>

Adjacency Matrix<br>
- <br>

space: ×<br>
insert: △<br>
remove: △<br>
incidentEdges: △<br>
areAdjacent: ◎<br>

Adjacency List<br>
- <br>

space: ◎（ただしコードが複雑）<br>
insert: ◎<br>
remove: ◎<br>
incidentEdges: ◎<br>
areAdjacent: ×<br>

#### Graph search algorithms overview
**⚪︎traverse through the graph**<br>
・ Not ordered（<=> tree: ordered ）<br>
・ No obvious start（<=> tree: obvious start ）<br>
・ breadth-first search(BFS): find the shortest paths from one vertex to other vetices in the graph.<br>
・ queueを使用して探索時に１度だけノードを通るようにする<br>
・ Adjacency Edgesの内容を重複が無いようにqueueに順に（queueの先頭から見て、）Vertexを全て含める<br>
```
// Graph Search ADT (traverse through the graph)
BFS(G):
  Input: Graph, G
  Output: A labeling of the edges on G as discovery and cross edges.
  
  foreach (Vertex v : G.vertices()):
    setLabel(v, UNEXPLORED)
  foreach (Edge e : G.edges()):
    setLabel(e, UNEXPLORED)
  foreach (Vertex v : G.vertices()):
    if getLabel(v) == UNEXPLORED:
      BFS(G, v)

BFS(G, v):
  Queue q
  setLabel(v, VISITED)
  q.enqueue(v)
  
  while !q.empty:
    v = q.dequeue()
    foreach (Vertex w : G.adjacent(v)):
      if getLabel(w) == UNEXPLORED:
        setLabel(v, w, DISCOVERY)
        setLabel(w, VISITED)
        q.enqueue(w)
      elseif getLabel(v, w) == UNEXPLORED:
        setLabel(v, w, CROSS)
        

```

#### std::unordered_set
・ (requirement:)no duplicates; inserting the same item twice has no effect.<br>
・ In C++, the unordered set uses **hashing** internally, so the key type must support == operator as well as std::hash.<br>
・ Unlike the unordered map type, the keys for an unordered set do not have associated values.<br>

#### std::set
・ (ordered type) based on a binary tree implementation instead of **hashing**. Uses < and == operators to arrange items.<br>
・ appropriate when we need to maintain data in order.<br>

#### std::vector
・ This is also possible use as a general purpose ordered set.<br>
・ Other "set" data structures like the disjoint-sets have specific use cases, there are no STL classes.<br>

#### Initialization with type inference
・ In C++11 and newer, there are compiler features that can infer the correct types automatically.<br>

