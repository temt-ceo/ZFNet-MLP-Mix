# Diameter
```
Diameter = largest geodesic(最短距離のパス) (=largest shortest path)
#以下で使う
How close are nodes

How long does it take to reach average node?

How fast will information spread?

# 外れ値の影響を少なくしたい時は以下を使う。
Average path length

# リング形状の場合のDiameterの式
diameter = n/2 or (n-1)/2

# Tree形状の場合のDiameterの式
(treeが)K levels has n = 2^(K+1) - 1 nodes
so, K = 2log2(n+1) -1
diameter is 2K

```
# Random Graphs
```
# All have same degree - really are random
# some links may double back (ある末端のノードはまだ未達)

Start with n nodes

each link is formed independently with some probability p

Serves as a benchmark : G(n,p) # 現実世界とは異質である。

AvgDist(n) / (log(n)/log(d(n))) => 1 (Diameterも同様に求められる)

kステップ後の増加するノード数: d * (d-1)^(k-1)   d:１回のステップで到達するノード数
```
# Avg distance and diameter 
```
# Chernoff Boundsの式
Large d: log(3d) & log(d/3) tend to log(d)  ==> log(n)/log(d) ≒ 1
log(n)/log(3d) < 1 < log(n)/log(d/3)

# 最後まで到達しないノードの数
if k < log(n)/log(d) Then n - d^k (much bigger than d^k) are still unreached. 
log(d) ==> degree
log(n) ==> Nodeの数
```


