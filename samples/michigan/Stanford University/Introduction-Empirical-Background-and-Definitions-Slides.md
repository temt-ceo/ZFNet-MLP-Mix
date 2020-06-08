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
Start with n nodes

each link is formed independently with some probability p

Serves as a benchmark : G(n,p)
```
