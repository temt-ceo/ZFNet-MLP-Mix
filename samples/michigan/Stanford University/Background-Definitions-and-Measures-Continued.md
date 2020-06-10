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
・ Eigenvector Centrality(アイゲンベクター)
   式: Σ(j:friend of i) Cj .. Neighborのdegree centralityに比例する
     - Look for one with largest eigenvalue will be nonnegative(Perron-Frobenius Theorem)
     - Normalize entries to sum to one
     - Google Page Rank: score of a page is propotional to the sum of the scores of pages linked to it.
・ Bonacich Centrality .. base valueをshortest pathに掛けていく
   式: Cb(g) = ag1 + bgag1 + b^2g^2ag1 ... = a(g1 + bg^2*1 + b^2g^3*1 ...)
     - Normalize a to 1, need small b to be finite.
```
# Application Networks
```
What affects Diffusion(拡散)?
 上記のCentrality Measureを使って仮説を立て、その仮説によりDiffusionが行われていると(相関があると)実証する事
仮説例: In villages where first contacted people have "higher eigenvector centrality",
       there should be a better spread of information about micro-finance.
  (コンポーネントの中のリーダー(且つそのノードを中心に他のコンポーネントにも繋がっていく)のノードをどう見つけ出すかが主眼)
  (プロットしてHigher Participation(Diffusion)と良い相関係数を出す仮説が出るまでGrid Searchすれば良い。)

Which networks form?
- random graph models - "How"
  ・Useful Benchmark
   - component structure
   - diameter
   - degree distribution   
   - clustering
    ・ Properties of Networks
      1. Every network has some probability of forming
      2. How to make sense of that?
      3. Examine what happens for "large" networks
    ・ Specifying Properties
      - G(N) = all the undirected networks on the set of nodes N
  ・Tools and methods
   - properties and thresholds
- Economic/game theoretic models - "Why"

How does it depend on context?
```
# Diffusion Centrality
```
DCi(p,T) = Σt=1...T (pg)^t*1
 - i is initially informed, each informed node tells each of its neighbors with probability "p" in each period, run for T periods?
 - If T=1: propotional to degree.
 - If p<1/λ and T is large, becomes Katz-Bonacich
 - If p>=1/λ and T is large, becomes eigenvector 
```
# Examples of Properties
```
A(N)={g|Ni(g) nonempty for all i in N} -> property of no isolated nodes
A(N)={g|l(i,j) finite for all i,j in N} -> network is connected
A(N)={g|l(i,j)<log(n) for all i in N} -> diameter is less than log(n)
```
# Random Networks Thresholds and Phase Transitions
```
A phase transaction occurs at t(n)
t(n) is a threshold function for a monotone property A(N) if
 Pr[A(N) | p(n)] -> 1 if p(n)/t(n) -> infinity
 Pr[A(N) | p(n)] -> 0 if p(n)/t(n) -> 0
 Graphがpropertyのedgeを持つか(p=0.1)、cycleを持つか(p=0.2)など。
```
# Exercise
```
Which of the following is not evidence for Homophily?
-> There are usually an equal number of boys and girls in a preschool.

What is the (normalized) degree centrality of node1?
3/(13-1)=1/4

What is the decay centrality of node 1, with σ = 0.5
->(shortest path=1) * 3 * 0.5 + (shortest path=2) * 1 * 0.5^2 = 1.5 + 0.25 = 1.75

Compare nodes 3 and 4:which one has a larger betweenness centrality but a lower Bonacich centrality(b=1/3)?
->betweenness centralityの高いノード（消去法で行ける）

Consider all undirected networks with 3 nodes {1,2,3} - there are x^3=8 networks in total.
Which of the following is the property the every node has at least one lonk: A(N) = {g|N_i(b) is nonempty for all i in N}?
-> { {12,23},{13,23},{12,13},{12,23,13} }
There are 4 networks in A(N): 3 networks {12,23},{13,23} and {12,13}, each of which has 2 links; and {12,23,13}, which has 3 links.

The Small-World model in Watts and Strogatz(1999) shows that by randomly rewiring a small but nontrivial function of links from a highly structured lattice(格子):
-> Networks with high clustering and low diameter are generated.

```
