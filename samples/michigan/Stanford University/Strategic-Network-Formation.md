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
↓↓↓Efficiencyの説明。
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
 - Dynamic Strategic Network Formation
   - Natural dynamics: link is picked at random
     - added if it benefits both players(at least one)
     - deleted if it benefits either to delete it
   - Will find pairwise stable networks
 - Improving path: Natural dynamics(↑)の理論によりpathを追加する。（Negativeになるようにはpathは追加削除されない）
   -> この法則で行き着いた先がPaiwise Nash Stableとなる。
   ε: ネットワークのpathが変更れる確率(Negativeにも動く)。 1 - ε: ネットワークがそのまま変化なしの確率
 - Directed Network Formation
   - Nash Stable: (Two way flow)
     - low cost: c < σ - σ^2      two-way complete network are Nash Stabile
     - medium low cost: σ - σ^2 < c < σ            all star networkare Nash Stabile, plus others
     - medium high cost: σ < c < σ + ((n-2)σ^2)/2  peripherally sponsored star networks are Nash Stabileではない
     - efficient and stable can be empty(high costでは無い): σ - σ^2 < c < 2(σ - σ^2)   complete is efficient, not equilibrium
   - n-player "wheels" if c < n-1   otherwise empty.

Hybrid Network Models(テクニック)
 Types: i 非含有 {1,....,K}
 
 si : # of same type friends
 di : # of different type friends
 Ui = (si + γi*di)^α   utility to type i
   γ is the preference bias
   α < 1 captures diminishing returns.

モデルはパターンを求めるものであり、F値（やRSS）を求めることで、パターンの類似性具合などが求められる。
そのパターンを活用することでModelの効果は発揮できる。

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

【Dynamic Strategic Network】
Consider connetions model with
δ - δ^2 < c < δ (スター型が一番安定), and the following process described by A. Watts(2001):
・ a link is picked uniformly at random;
・ the link is added is that weakly benefits both player;
・ go back to the first bullet point and repeat;
Which statements are true? (This is to guide through the Proposition)
a) When the above process reaches a star network, it may still move forward to other networks.
b) Star networks involving all nodes are efficient.
c) Star networks involving all nodes are pairwise stable.
d) As the number of nodes n grows, the probability that the above process stops at a star network foes to 0.
-> b, c, d (= Once a process hits a star, it will stop there.)

【Improving path ①】
Consider all undirected networks with 3 nodes as depicted in the picture.
In which row(s) are the network Pairwise Nash stable?
a) 1(0) 2(0) 3(0)
b) 1(-1)-2(-1) 3(0), 1(0) 2(-1)-3(-1), 2(0) 3(-1)-1(-1)  <- 他に移行する余地あり
c) 1(1)-2(1)-3(1), 2(1)-1(1)-3(1), 1(1)-3(1)-2(1)
d) 1(2)-2(2)-3(2)
-> a, d

【Improving path ②】
Consider the discussion of improving paths with error ε = 0.05 on networks with 3 nodes (as described in this video, 
with utilities as a function of the network depicted in the picture).
Stariting from "the green network" in the middle of third row, what is the probability of staying at the same network after one step?
1(0) 2(0) 3(0)
1(-1)-2(-1) 3(0), 1(0) 2(-1)-3(-1), 2(0) 3(-1)-1(-1)  <- 他に移行する余地あり
2(1)-1(1)-3(1), 1(1)-2(1)-3(1)<==="the green network", 1(1)-3(1)-2(1)
1(2)-2(2)-3(2)
a) 0
b) 1
c) 0.15
d) 0.65
-> d (グリーンの状態は一番下の形態にしか変異できないので、there is a chance(1 - ε) / 3 to move to the nwtwork with 3 links and a chance ε/3 to move to
      either (left or right) networks with 2 links. Thus the probability of staying is 1 - (1 - ε)/3 - 2ε/3 = 0.65.)

【Directed Network Formation ①】
Consider a network formation game with directed networks on 2 nodes and with resulting utilities depicted in the picture as a function
of the directed links formed (with arrows pointing from the node that formed the link to the node at whivh it directed the link).
Which networks are the outcome of Nash equilibrium?
a) 1(0)   2(0)
b) 1(1)-->2(2)
c) 1(2)<--2(1)
d) 1(1)<->2(1)
-> b,c

【Directed Network Formation ②】
Consider the directed connections model with no decay (as described in this video)
What is the "payoff to each nodes" in the wheel network in the picture, with n=5 and c=3?
Wheels netowrk(1->2->3->4->5->1)
a) 3
b) 1
c) 2
d) 5
-> b (The total payoff is (n-1)-c = 1 )
```
# EXERCISE 2
```
Consider the four person society with the utility payoffs to agents as pictured below. The payoffs are listed as a function of
the network architecture and an agent's position in the network:
(agentの番号は無し)
[0] [0] [0] [0]    ---- (a)
[0] [0] [-1]-[-1]
[-1]-[-1] [-1]-[-1]
[1]-[-2]-[1] [0]                        <------(X)
[-2]-[-2]-[-2] [0] (左３つはトライアングル)
[3]-[0]-[0]-[3]    ---- (b)
[0]-[0]-[-3]-[3] (左３つはトライアングル)
[2]-[2]-[2]-[2] (輪)    ---- (d)
[1]-[0]-[1]-[0] (輪とpayoff=1同士でたすき)
[1]-[1]-[1]-[1] (complete network)    ---- (c)
Which out of the networks labeled a,b,c and d are Efficient (in terms of maximizing the total utility across all agents)?
-> d only (d is the only efficient network. Notice b is Pareto efficient, but not efficient in terms of maximizing the total utility across all agents.)
Which out of the networks labeled a,b,c and d are Pareto Efficient?
-> b and d
Which out of the networks labeled a,b,c and d are Pairwise stable?
-> a and c (bとdはどちらもリンクを切ってb->Xやd->bになることで個々のagentがより高いpayoff値を得られるから)

Consider a symmetric version of the Connections model with n = 10 and δ = 0.5.
For which of the costs listed below is a star network (involving all individuals) efficient but not pairwise stable? 
a) c = .2
b) c = .4
c) c = 1.4
d) c = 2.4
-> c  (c　非含有(0.5, 1.5) is the right condition.)

Consider a symmetric version of the Connections model with n = 6 and δ = 0.5 and c = 0.6.
Which of the following statements are correct for these parameter values?
1) Star networks involving all individuals are pairwise stable
2) Star networks involving all individuals are efficient
3) The empty network is pairwise stable
4) These exists a nonempty network that is pairwise stable
a) 2,3 and 4
b) 1,2,3 and 4
c) 1,3 and 4
d) 1 and 2
-> a (
Star networks are not pairwise stable, since c=0.6 is bigger than δ=0.5;
Star networks are uniquely efficient, since c=0.6 is between δ-δ^2 = 0.25 and δ+(n-2)δ^2/2 = 1;
Empty network is pairwise stable, since c=0.6 is bigger than δ= 0.5, and hence any pair of nodes have no incentive to add a link;
It can be verified that Ring networks are (the unique) pairwise stable nonempty networks, since (in this case with n=6) δ < c < (δ+δ^2+δ^3)(1-δ^2).
)

Consider the "islands" version of the connections model with K=3 islands and J=3 individuals on each island. Let δ=0.9, c=0.02 < 0.9-0.9^2, and C=4.
Which of the following statement are true for these parameter values?
1) The empty network is both efficient and pairwise stable.
2) The complete network is both efficient and pairwise stable.
3) In any efficient network, each island is completely connected internally.
4) In any pairwise stable network, each island is completely connected internally.

a) 1 and 2
b) 3 only
c) 1,2 and 4
d) 3 and 4
-> d (The efficient network is to have each island completely connected internally, and to have a star shape among islands.
      The pairwise stable network is to have each island completely connected internally, but no links between islands.)

```
