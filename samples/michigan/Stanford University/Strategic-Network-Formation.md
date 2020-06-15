# Strategic Network Formation
```
Random Network Modelsより個々を選択する向きがある（経済的な事等）
 - 0 <= σ <= 1 a benefit parameter for i from connection between i and j
 - 0 <= c(ij) cost to i of link to j
 - l(i,j) shortest path length between i,j
 benefitとcost、距離がネットワーク形成に影響するModel
 u(g) = Σσ^l(i,j) - Σ in N(g) * C(i,j)
  - σ=.5の時、距離が遠ざかるたびに、.5, .25, .125とbenefitは小さくなる
  - σ=.9の時、距離が遠ざかるたびに、.9, .81, .72とbenefitは小さくなる
  - 対しcost(C)は距離に関わらずneighbor数に比例(neighbor(N)が多いノードはコストが比例して下がる。)
  リレーションを維持するのにもコストがかかる。
  - u: utility function
Question:
 - Which (Random or Strategic) networks are best for society?
 - Which networks are formed by the agents?
```
# Pairwise Stability and Efficiency
```
Pairwise Stability
 - no agent gains from severing a link (must be beneficial to be maintained)
   => u(g) >= u(g-ij) for i and ij in g
 - no two agents both gain from adding a link (are pursued when available)
   => u(g+ij) > u(g) implies u(g+ij) < u(g) for ij not in g
 自分が直接結びついているノードに対してはBenefitが増していくが、またつなぎの場合はその数が多いほどBenefitが低下する
 また、ネットワークが大きいほど直接結びついているノードばかりでもBenefitはやはり少しずつ低下する
 ネットワークの全ノードが互いに結びついた状態をPairwise Stabilityという。

Efficiency
 「また、ネットワークが大きいほど直接結びついているノードばかりでもBenefitはやはり少しずつ低下する」
 ->そのため２つだけの繋がりの時が最もBenefitが高い = Efficient
 
 ↑よりBenefitを上げる構成が１つある。1-2-3-4の構成。2,3は2と３だけ結びついている時よりBenefitが少し上がる = Pareto Efficient

 1-2-3-4の構成は全体から見た時 Pareto Efficientではない（均等ではないから）が
 1-2 3-4の構成は全体から見た時 Pareto Efficientである（均等だから）。

→ Efficient(benefitが最も高い)を優先するかPairwise Stability（相互完全結びつき）を優先するか
```
# Connections Model
```
Strategic Network(損得のネットワーク)が以下にして結合していくかをモデリングする
 - low cost: c < σ - σ^2      complete network(全てが全てと結びつく)が一番efficient
 - medium cost: σ - σ^2 < c < σ + ((n-2)σ^2)/2     star networkが一番efficient
 - high cost: σ + ((n-2)σ^2)/2 < c                 empty networkが一番efficient
 
Pairwise Stability in the Connections Model
 - low cost: c < σ - σ^2      complete network(全てが全てと結びつく)がPairwise Stabile
 - medium low cost: σ - σ^2 < c < σ            star networkもそうだし、他のもPairwise Stabile
 - medium high cost: σ < c < σ + ((n-2)σ^2)/2  star networkはPairwise Stabileではない
 - high cost: σ + ((n-2)σ^2)/2 < c                 empty networkがPairwise Stabile
```
# Externalities and the Coauthor Model
```
Externalities
 - Positive u(g + ij) >= u(g)  ..但しijが計算している該当ノードでない場合
   ->Graphの該当ノードにリンクを追加した時にネットワーク内の他のノードのBenefitが下がる
 - Negative u(g + ij) <= u(g)  ..但しijが計算している該当ノードでない場合
   ->Graphの該当ノードにリンクを追加した時にネットワーク内の他のノードとBenefitが上がる

Coauthor Model
 - 親和性（synergy,collaboration）を計測する .. Benefitの大きさを求める式
 u = Σij in g[1/di + 1/dj + 1/(di*dj)]
   = 1 + Σij in g[1/dj + 1/(di*dj)]
  d: degree
  - 1-2の構成 -> 1/1 + 1/1 + 1/(1*1)
  - 1-2-3-4の構成の2 -> 1/1 + 1/2 + 1/(1*2) + 1/2 + 1/2 + 1/(2*2)
                      = 2 + 1 + 1/4 = 3.25 
  - 1-2-3-4の構成の1 -> 1/1 + 1/2 + 1/(1*2) = 2
```
# Network Formation and Transfers
```
Transfers
 - Outside intervention, taxing or subsidizing relationships
  (外部要因による繋がり)
 - stabilityとefficiencyのconflictを解消できるか？
  u(g) --> u(g) + t(g) に変える
 
Egalitarian Transfers
 - Every agent has societal incentives (どのノードも同じutilityを持つようになる)

Transfersは孤立したノードを除外するので完璧ではない。

Heterogeneity .. Enriching Strategic Models
 - Small worlds derived from costs/benefits のBasic Idea
   - low costs to local links .. high clustering
     distanceがあってもリンクを維持しようとする。
     - islands connections model
       The features of islands connections model include high clustering, 
       low diameter, and regular degree (which does not increase dramatically
       as the number of islands increase).
     小規模の所属するクラスターと他のそれぞれのクラスターとの繋がり。
   - high value to distant connections .. low diameter
   - high cost of distant connections .. few distant links

Strategic Formation
 - Efficient networks and stable Networks need not coincide
 - Need not coincide even when transfers are possible, and with complete information
 - Depends on 
   setting
   restrinctions on transfers, endogenous transfers..
   forward looking, errors ..
 - Can match and explain some observables
   
Strengths of an economic approach
 - Payoffs allow for a welfare analysis
 - Tie the nature of externalities to network formation..
 - Put network structures in context
 - Account for and explain some observables

Models that marry strategic with random are needed
 - Weaknesses of Random are Strengths of Economic approach, and vice versa.
 - Mixed models
   - allow for welfare/efficiency analysis
   - take model to data and fit observed networks
   - do so across applications
```
# EXERCISE
```
Consider a symmetric version of the Connections Model with the following utility function(↑の式)
and suppose σ = 0.5 and c = 0.4.
What is the utility of node 2, u2(g), in the pictured undirected network?
(※1,2,3がトライアングル、2-4-5がpath)
-> Benefit:0.5 * 3 + 0.25 * 1   Cost: 0.4 * 3 (1,3,4がNだから)   Then, 3*0.5 + 0.25 - 3 * 0.4
 
【Pairwise Stability】
Consider a symmetric version of Connections Model with σ=0.999 and c = 1.4 and a society of n = 3 people.
(ノードが３つのネットワーク)
In which row are network Pairwise Stable?
-> d (a,b,cはどれもマイナスが含まれる。dは完全バラバラだがマイナスがない(全0)。よってPairwise stable。)

【Pareto Efficient】
Consider a symmetric version of Connections Model with σ = 0.999 and c = 1.4 and a society of n=3 people.
(ノードが３つのネットワーク)
In which row are network Pareto Efficient?
-> b,d (３つのノードのネットワークなので、トライアングルは下がる。bは1-2-3の構成で1,3がプラス、dは1 2 3の構成。
        a,b,cいずれもマイナスがあるので全体から見た時はdがPareto Efficientとなる)

【Efficient】
Consider a symmetric version of Connections Model with σ = 0.999 and c = 1.4 and a society of n=3 people.
(ノードが３つのネットワーク)
In which row are network Efficient?
-> b(一番高いのがEfficient)

【Connections Model】
Consider a symmetric version of the Connections Model in a society with n = 5 people and σ = 0.5.
Which are the lower and upper bounds of the cost c, such that star networks are efficient?
a) 0.25, 0.5
b) 0.25, 0.875
c) 0.25, 1
d) 0.5, 1
-> σ - σ^2 = 0.5 - 0.25 = 0.25 (<-lower bound)
   σ + ((n-2)σ^2)/2 = 0.5 + (5 - 2)*0.25 / 2 = 0.5 + 0.75 / 2 = 0.875 (<- upper bound)
   Then, b

【Pairwise Stability in the Connections Model】
Consider a symmetric version of the Connections Model in a society with n = 7 people and σ = 0.5.
For which of the following costs c are star networks pairwise stable?
a) 0.8
b) 0.4
c) 0.6
d) 0.2
-> σ - σ^2 < c < σ ...  σ - σ^2 = 0.25. So.. 0.25 < c < 0.5. Then b.

【Externalities】
In the picture, consider adding a link 12 to the network g = {13, 24}.
So the left network is g, and the right network is g+12.
The number next to each node is the node's utility from that network.
Regarding the externality to node 3 brought by adding link 12, which if the following statements is correct?

1(3)-3(3) 2(3)-4(3)  =>  3(2)-1(3.25)-2(3.25)-4(2)
a) Adding link 12 brings positive externality on node 3.
b) Adding link 12 brings negative externality on node 3.
c) None of the above.
-> b (Since its value from the network is reduced from 3 to 2.)

【Coauthor Model】
Consider the undirected network in the picture using "Coauthor Model".
What is the utility of agent 1?
2-1-3
-> 4 (1/1 + 1/2 + 1/(1*2) が2つ = 2 * 2 = 4)

【Transfers】
Consider undirected networks on 3 nodes, with the utilities of nodes depicted in the picture.
Choose the option that correctly answers the following two questions:
(1) In which row are the etwork(s) Efficient?
(2) In which row are the etwork(s) Pairwise stable?
a) 3-3-3(トライアングル)
b) 4-5-4(スター)
c) 6-6 0(path)
d) 0 0 0
->(1):b, (2): c
理由) Transfersには前提となる２つの事があり、
 - completely isolated nodes that generate no value get 0
 - nodes that are completely interchangeable get same transfers
完全に孤立しているものはカウントしない。そして<合計した>uの最も大きいものがEfficient, 孤立したものを除き全て等しいuを持つものがPairwise stable。
孤立したものをカウントしないので見かけ上一番大きいもの(6)がEfficientになるのではなく、合計したものが一番大きいもの(13)をEfficientとする必要がある。

【Heterogeneity】
In the pairwise stable structure of network described in this video for Islands connections model, 
which of the following properties are correct?
(Islands connections model = low costs to local links)
a) Diameter increases proportionally to the total number of nodes, as the number of islands goes up
  (fixing the number of nodes in each island)
b) Relatively high clustering
c) Relatively low diameter
d) Average degree increases proportionally to the total number of nodes, as the number of islands goes up
  (fixing the number of nodes in each island)
-> b,c
  b: The features of islands connections model include high clustering, low diameter, and regular degree (which does not increase dramatically as the number of islands increase).

```