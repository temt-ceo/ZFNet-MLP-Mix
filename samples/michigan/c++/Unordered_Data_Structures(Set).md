
## Disjoint Sets ("union-find" data structure)
・ A series of sets which are disjoint from one another.<br>
・ But every single element within a set is considered to be equivalent(同等/同量のもの) within that set.<br>

#### Disjoint Sets ADT(abstruct data type)
1. Maintain a collection S = {S0, S1, ... Sk} <br>
2. Each set has a representative member.(先頭のメンバがidであり一意でないといけない)<br>
3. API: void makeSet(const T & t); <br>
　　 void union(const T & k1, const T & k2); <br>
　　 T & find(const T & k); <br>

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


