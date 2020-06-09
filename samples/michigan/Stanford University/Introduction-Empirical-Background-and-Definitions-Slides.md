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
# Exercise
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
# Exercise2
```
Given a network(N,g), define its complement to be the network(N,g'), such that (ij 被含有 g') if and only if (ij 非被含有 g).
Which statement is correct of networks with at least four nodes?
-> There is a network such that both itself and its complement are connected.
(Consider N={1,2,3,4} and g={12,23,34}, we have k'={13,24,14} is also connected.)

If g^2 is the matrix on right, which of the following network could be represented by g?
-> 最後に行き着く場所がどのノードかがMatrixの数字となる。g^2なので行って戻る動きができるので対角線が1以上になる。neighborの数がこの対角線の数になる

Among all possible connected networks with 5 nodes:
What is the largest possible diameter? What is the shortest possible diameter?
->4,1

On the Florentine Marriage Network(親戚), what is the diameter of the largest component?
->5

Consider the two networks of close friends in the following two schools:
school1: A university has 400 students; on average each student has also 8 close friends.
school2: A middle school has 150 students; on average each student has 8 close friends.
Which of the following is a reasonable approximation for the ratio Diameter(school1)/Diameter(school2)
For this question, you should presume that the networks are connected and use an approximation
from a network where links are formed uniformly at random (an E-R random graph).
(ここで、8 ≒ e^2, 150 ≒ e^5, 400 ≒ e^6)
->(e^6/e^2)/(e^5/e^2)...6/5=1.2    【AvgDist(n) / (log(n)/log(d(n))) => 1 (Diameterも同様に求められる)】

What is the overall clustering of the network? How about the average clustering?(When computing average
clustering and looking at the clustering for some node i, if it has 0 or 1 links, count its clustering as 0)
->1/3, 1/3  
(0/2 + 0/2 + 1/3 + 1 + 1/3) / 5 = 5/(3*5) ≒ 1/3   【1ペアのneighbor(つまり2つのneighbor)がlinkしていたら"1"】
Cl_1(g)=0,Cl_2(g)=0,Cl_3(g)=1/3,Cl_4(g)=1,Cl_5(g)=1/3
For overall clustering: There are 9 pairs of neighbors in total, and 3 out of which are linked, then cl(g)=3/9.
```
他の問題も、はっきり言って簡単。ミシガン大学の方がはるかに難しい。。

