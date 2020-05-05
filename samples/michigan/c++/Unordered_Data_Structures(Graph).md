
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

### Edge List
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

### Adjacency Matrix

space: ×<br>
insert: △<br>
remove: △<br>
incidentEdges: △<br>
areAdjacent: ◎<br>

### Adjacency List

space: ◎（ただしコードが複雑）<br>
insert: ◎<br>
remove: ◎<br>
incidentEdges: ◎<br>
areAdjacent: ×<br>

#### Graph search algorithms overview

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

**⚪︎traverse through the graph(spanning-tree)**<br>
・ Not ordered(Directedはある)（<=> tree: ordered ）<br>
・ No obvious start（<=> tree: obvious start ）<br>
・ breadth-first search(BFS): find the shortest paths from one vertex to other vetices in the graph.<br>
・ queueを使用して探索時に１度だけノードを通るようにする<br>
・ Adjacency Edgesの内容を重複が無いようにqueueに順に（queueの先頭から見て、）Vertexを全て含める<br>

### BFS
・ Breadth-first search<br>
・ 1番目のVertexから見て全てのVertexをqueueに入れ、queueから処理を進める。<br>
・ 全Vertexの先頭のVertexからの距離がわかる。（Adjacent Edgesとd(distance)とp(predecessor)を格納する事で）<br>
・ Undirectd（双方向が可能）では最も早い：O(m+n)。<br>
・ Cross Edgeが存在するという事はこれまでのdistance以下で来れる。（距離がより大きくはならない。）。<br>

### DFS
・ Depth-first search<br>
・ stackの利点を生かせる。cross edgeを見つけ他にtraverseする所がなければ、順に来た道を戻って他を探す。（全探索する）<br>
・ running timeはBFSと変わらない(O(n+m))<br>

### Kruskal's Algorithm
・ Undirected, weightedでminimum spanning tree(MST)を求める。<br>
・ weight(距離など)を考慮する<br>
・ MinHeap(weightの小さいものからHeapに詰め込む)<br>
・ Disjoint setとしてMinHeapの中身を取り出し、両端のVertexが同じsetかチェックする。<br>
・ weightの小さいものから徐々にset（Edge）をunionしていく(Weightの大きいEdgeはspanning-treeに入らない。)<br>
・ running time: O(m・lg(m))  <= Edge数 x log(Edge数)<br>

### Prim's Algorithm
・ Undirected, weightedでminimum spanning tree(MST)を求める。<br>
・ 先頭のVertexからweightの小さいEdgeをset unionしていく(あとはBFSと似ている)<br>
・ running time(Sparse Graph): O(m・lg(m))  <= Edge数 x log(Edge数)<br>
・ running time(Dense Graph): O(nの2乗・lg(n))<br>

### Dijkstra's Algorithm
・ DirectedまたはUndirected, weightedでShortest Path on the Graphを求める。<br>
・ Primとの違いは先頭から全てのedgeを経由してコストを求める（加算していく）。（GraphがDirectedで有効なことも違い）<br>
・ CiscoのSwitchと同じ計算法<br>
・ SSSP(先頭のVertexからの最短距離を求めるしかできない)<br>
・ running time: O(m + lg(n)) // The best running time in any Shortest Path Algorithm.<br>

#### どの局面で利用するのが一番適しているか(速度の観点で)(Landmark Path Problem)
・ weightがない場合: BFS<br>
・ Landmarkがあり、weightがない場合: BFS(X => 先頭からとLandmarkからの２回試みる, ⭕️ => Landmarkから１回だけでok.)<br>


```
////////////////
// Graph Search ADT (traverse through the graph): Graph Data Structureを探索する
//     (BFS Algorithm)
////////////////
BFS(G):
  Input: Graph, G
  Output: A labeling of the edges on G as discovery and cross edges.
  
  foreach (Vertex v : G.vertices()):
    setLabel(v, UNEXPLORED)
  foreach (Edge e : G.edges()):
    setLabel(e, UNEXPLORED)
  foreach (Vertex v : G.vertices()):
    if getLabel(v) == UNEXPLORED:
      BFS(G, v) // -> component++; を次行に追加する。

BFS(G, v):
  Queue q
  setLabel(v, VISITED)
  q.enqueue(v)
  
  while !q.empty: //======> O(n) : n=Vertex数
    v = q.dequeue()
    foreach (Vertex w : G.adjacent(v)): //======> O(2m)  or Σ:O(deg) : m=Edge数
      if getLabel(w) == UNEXPLORED:
        setLabel(v, w, DISCOVERY)
        setLabel(w, VISITED)
        q.enqueue(w)
      elseif getLabel(v, w) == UNEXPLORED:
        setLabel(v, w, CROSS) // cross edge: detect a cycle -> cycle=true; を次行に追加する。
  // 全体でO(n+m)のrunning time.

////////////////
// Graph Search ADT (traverse through the graph): Graph Data Structureを探索する
//     (DFS Algorithm)
////////////////
DFS(G):
  Input: Graph, G
  Output: A labeling of the edges on G as discovery and back edges. // cross->backになっただけ
  
  foreach (Vertex v : G.vertices()):
    setLabel(v, UNEXPLORED)
  foreach (Edge e : G.edges()):
    setLabel(e, UNEXPLORED)
  foreach (Vertex v : G.vertices()):
    if getLabel(v) == UNEXPLORED:
      DFS(G, v) // -> component++; を次行に追加する。

DFS(G, v):
  setLabel(v, VISITED)
  
    foreach (Vertex w : G.adjacent(v)): //======> O(2m)  or Σ:O(deg)
      if getLabel(w) == UNEXPLORED:
        setLabel(v, w, DISCOVERY)
        DFS(G, w)
      elseif getLabel(v, w) == UNEXPLORED:
        setLabel(v, w, BACK) // CROSS->BACKになっただけ
  // Labeling:
  //   Vertex: 2 x n=> O(n)
  //   Edge:   2 x m=> O(m)
  // Queries:
  //   Vertex: n=> O(n)
  //   Edge:   Σ(deg(v))=> 2m => O(m)

  // 全体でO(n+m)のrunning time.(BFSと同じ)

////////////////
// Minimum Spanning Tree ADT
//     (Kruskal's Algorithm)
////////////////
KruskalMST(G):
  DisjointSets forest
  foreach (Vertex v : G.vertices()):
    forest.makeSet(v)
  
  priorityQueue Q // min edge weight
  foreach (Edge e : G.edges()):
    Q.insert(e)
  
  Graph T 0 (V, {})

  while |T.edges()| < n-1:
    Edge (u,v) = Q.removeMin()
    if forest.find(u) != forest.find(v):
      T.addEdge(u, v)
      forest.union( forest.find(u), forest.find(v))
  
  return T


////////////////
// Shortest Path Algorithm ADT
//     (Dijkstra's Algorithm)
////////////////
DijkstraSSSP(G, s): // (Single Source Short Path)
  foreach (Vertex v : G.vertices()):
    d[v] = +inf // この辺りはPrimと似ている(Prim: vertexを進めるたびに距離がわかるとセットし直していく)
    p[v] = NULL
  d[s] = 0
  
  PriorityQueue Q // min distance, defined by d[v]
  Q.buildHeap(G.vertices())
  Graph T  // "labeled set"

  repeat in times:
    Vertex u = Q.removeMin()
    T.add(u)
    foreach (Vertex v : neighbors of u not in T):
      if cost(u, v) + d[u] < d[v]:
        d[v] = cost(u, v) + d[u]
        p[v] = m
  
  return T
```
