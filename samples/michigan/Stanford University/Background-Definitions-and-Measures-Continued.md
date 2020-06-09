# Homophily(似たものに惹かれる傾向)
```
Spring algorithm: お互いが引き合う大きさをspring（バネ）で表現する

```
# Centrality Measures
```
Global patterns of networks
 - degree distributions, path lengths..

Segregation Patterns
 - node types and homophily

Local Patterns
 - Clustering, Transitivity, Support...

Positions in networks
 - Neighborhoods, Centrality, Influence...

・ Degree Centrality .. そのノードのdegree / (n-1) 
・ Closeness Centrality .. (n-1) / そのノードのΣl(i,j)  l: shortest path
・ Decay Centrality .. Σi≠jσl(i,j) : Closeness Centralityに似ているが違う公式(離れるたびにexponentialに係数が小さくなる)
   (σは0~1の値をとる。0に近いほどDegree Centralityに、1に近いほどcomponent size centralityに近くなる)
   (Normalizeする時は(n-1)σで割る (n-1)σ = 最小のdecay centralityの値)
・ Betweenness Centrality (Freeman) .. そのノードを通過するiとj間の最小パスの数の合計 / iとj間の最小パスの数の合計（但しi,jにそのノードを含まない）
　　式: Σi,j≠k [Pk(i,j)/P(i,j)] / [(n-1)(n-2)/2]
```
# Exercise
```
Which of the following is not evidence for Homophily?
-> There are usually an equal number of boys and girls in a preschool.

What is the (normalized) degree centrality of node1?
3/(13-1)=1/4

What is the decay centrality of node 1, with σ = 0.5
->(shortest path=1) * 3 * 0.5 + (shortest path=2) * 1 * 0.5^2 = 1.5 + 0.25 = 1.75


```
