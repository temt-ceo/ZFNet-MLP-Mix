
## Graph

#### Graph ADT(abstruct data type)
```
Data:
- Vertices
- Edges
- Some data structure maintaining the structure between vertices and edges
Functions:
- insertVertex(K key);
- insertEdge(Vertex v1, Vertex v2, K key);
- removeVertex(Vertex v);
- removeEdge(Vertex v1, Vertex v2);
- incidentEdges(Vertex v1, Vertex v2);
- areAdjacent(Vertex v1, Vertex v2);
- origin(Edge e);
- destination(Edge e);

```

**UpTree**<br>
```
int DisjointSets::find() {
  if (s[i] < 0) { return i; }
  else { return _find( s[i] ); }
}
```
UpTreeの場合、配列内のunionが効率化される(-1（ideal UpTree）のところをunion先のideal UpTreeにするだけなので)<br>
O(h) ( <= O(n) )。理想はO(1): ideal UpTreeに他が全て紐付けられている状態。<br>

**Smart Union**<br>
ideal UpTreeの-1を-(高さ+1)に変える。これにより以下の高さ(h)をなるべく増やさない工夫が可能となる。
・ Union by height: Keep the height of the tree as small as possible.<br>
・ Union by size: Minimize the number of nodes that increase in height.<br>
Both guarantee the height of the tree is log(n)<br>

**Path Compression**<br>
これを行えば、disjoint setはcomputer scienceの中で最も速度の早いdata structureとなる<br>


