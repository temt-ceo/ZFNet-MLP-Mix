
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

```

