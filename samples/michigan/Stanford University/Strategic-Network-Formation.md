# Strategic Network Formation
 - δ -- Benefit(友人と結びつくことで得られる利益（友人の友人はδ^2, 友人の友人の友人はδ^3。δは１未満。）)
 - c -- Cost(結びつくのにかかるコスト（直接結びつく(path=1)友人が2人いれば2c。)
 - u -- δとcの合計(人が多いほど合計のcは大きくなるのでスター型の方がcomplete型（リンクが多数）よりuが高くなることもある。)<br>
 　u = Σij in g[1/di + 1/dj + 1/(di*dj)]  (u: utility)(F: frequency)
 
 - Pairwise Stable .. リンクを多くても１つ切ってそれ以上安定な状態にはなれない状態。（リンクを切ってマイナスがより大きくなるようならその前の状態がPairwise Stable）

 - Nash Stable .. リンクを幾つか切ってでも安定する状態（必ずすべてのノードが0以上のutility）
 
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
 - high cost: σ + ((n-2)σ^2)/2 < c                 empty network（結び付きの無い状態）が一番efficient
 
Pairwise Stability in the Connections Model
 - low cost: c < σ - σ^2      complete network(全てが全てと結びつく)がPairwise Stabile
 - medium low cost: σ - σ^2 < c < σ            star networkもそうだし、他のもPairwise Stabile
 - medium high cost: σ < c < σ + ((n-2)σ^2)/2  star networkはPairwise Stabileではない
 - high cost: σ + ((n-2)σ^2)/2 < c                 empty network（結び付きの無い状態）がPairwise Stabile
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
# SUGMS and Strategic Network Formation
```
Estimating Random Networks
 - pairwise stability : links form if and only if
    ε[ij] < U(Xi,Xj) and ε[ji] < U(Xj,Xi)
    U(Xi,Xj) - ε[ij] ... utility of a link between i, j
Strategic Formation Models(Modeling Stability and Dynamics)
 - Refining pairwise stability
   - Beyond Pairwise Stability
     - multiple links by individuals
     - coordinated deviations
   - Nash Stability : if and only if no player wants to delete some set of his or her links = Positive Network(NegativeがNetwork内に無い)
   　リンクを切ることでよりポジティブなネットワークができる時はNash Stabeでは無い
   

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

【Efficiency in the Connections Model】
Consider a symmetric version of the Connections Model in a society with n=7 people and with σ = 0.5 and c=1.
Which of the following describes the efficient network architectures?
a) The complete network
b) Star networks involving all nodes
c) The empty network
d) None of the above
-> d. In this case, σ-σ^2 = 0.25 and σ + (n-2)σ^2/2 = 1.125, and c=1 is in between of those two. Therefore the star networks are (uniquely) efficient.

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

【Nash Stabe ①】
Consider the networks with 3 nodes and the utilities as shown in the picture.
Which networks are Nash Stable?
a) 1(2)-2(1)-3(2)
b) 1(1)-2(1)-3(1) (トライアングル)
c) 1(0) 2(0) 3(0)
d) 1(1)-2(1) 3(0)
-> a,c,d (bはまだリンクが切れる余地がある。)

【Nash Stabe ②】
Consider the networks with 3 nodes as shown in the picture.
which networks are Pairwise Nash Stable?
a) 1(2)-2(1)-3(2)
b) 1(1.5)-2(1.5)-3(1.5) (トライアングル)
c) 1(3) 2(3) 3(3)
d) 1(1)-2(1) 3(2)
-> c (Network c is both Pairwise stable and Nash stable, so it is Pairwise Nash stable. Network b is only Pairwise stable but not Nash stable.)
  Pairwise Nash Stableとあるのでリンクを切れるだけ切った最もPositiveな状態を見つけ出す。
```
