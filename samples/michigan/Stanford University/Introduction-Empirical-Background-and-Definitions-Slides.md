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
# Clustering
```
Cl(g) = neighbor同士の結びつきの数 / degree(neighborの数)

For an Erdos-Renyi(E-R) random network with large n, its overall clustering CL(g) is approximately p( ≒ p ),
since p of all pairs of neighbors is linked.
```
# Excercise
```
How many networks on just 4 nodes -> 2^6

Is the walk from 1 to 6 represented by the dashed lines (1,2,3,4,5,3,6): (1) a path? (2) a cycle?
->No, No (同じノードを２回通るものはpathではない, 起点と最後のノードが同じでなければcycleではない)

In the network from the previous question, which of the following is the geodesic between 2 and 6?
->(2,3,6), (2,1,6) 最短距離でなければthe geodesicではない

This is a Cayley Tree d=3, after two steps. How many nodes in Total are there after one more step(3 steps)?
->3 * 2^2 =12       【kステップ後の増加するノード数: d * (d-1)^(k-1)   d:１回のステップで到達するノード数】の式を使う

A standard formula is that (1-x/n)^n = e^(-x) for large n provided x/n goes to 0. Given this, which of the
following is an approximation of (1-p)^n when n is large and p is ``small?
-> e^(-np)

When we say that some obserbed networks have "fat tails" compared to an Erdos-Renyi random networks, this refers to the observed network having which of the following compared to an E-R network with the same expected degree?
->too many nodes with very high degree
  too many nodes with very low degree
  (逆U字のカーブを予想するが、端の方は直線になることがあること)
  
What is the clustering of node Cl(g)
->1/3  【Cl(g) = neighbor同士の結びつきの数 / degree(neighborの数)】

For an Erdos-Renyi random network with n = 1000 and p = 0.1, which would be a good guess for its overall clustering, Cl(g)?
->0.1  【In large network, overall clustering CL(g) is approximately p( ≒ p )】

Which of the following represents row stochastic network?
-> degreeのweightを足すと1になる。【row stochastic: Σ g(ij) = 1】

```
他の問題は、はっきり言って簡単。ミシガン大学の方がはるかに難しい。。


