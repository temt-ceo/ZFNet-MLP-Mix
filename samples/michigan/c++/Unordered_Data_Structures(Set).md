
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
