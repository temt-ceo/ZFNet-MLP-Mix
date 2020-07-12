# Games on Networks
```
 人の動きを仮定する。
 - Each player chooses action x in {0,1}
 - Consider cases where i's payoff is
  ud(x(i), m(N))
  depends only on d(g) and m(N) - the number of neighbors of i choosing 1.(1を選んだ友達の数が影響する。degreeも影響する。)
  
  0を選ぶ: ud(0, m) = 0 (1を選ぶ閾値だけ設定したいので。)
  1を選ぶ: ud(1, m) = -t + m   (t:threshold, m:人数 tが3なら友達が3人以上1を選んだなら1を選ぶことになる。)
```
# Complements and Substitutes
```
strategic complements: positive relationships
 - for all d, m>m`
   u(1,m) - u(0,m) >= u(1,m`) - u(0,m`)

strategic substitues: negative relationships
 - for all d, m>m`
   u(1,m) - u(0,m) <= u(1,m`) - u(0,m`)

Equilibrium
 - Nash equilibrium: Every player's action is optimal for that player given the actions of others(他の選択をするきっかけになり収束した状態。)
```
# Properties of Equilibrium (イクイリブリウム)
```
Complete Lattice: 
 for every set of equilibria(平衡) X
  - there exists an equilibrium x` such that x`>=x for all x in X, and
  - there exists an equilibrium x`` such that x``<=x for all x in X.
  (３パターン大小関係を説明できればいい。)
```
# Multiple of Equilibria
```
Equilibrium relation to network structure
 - 凝集性(cohesive)
  degreeのうち、何割が凝集Sに繋がっているか。
  gateway(他の凝集性への)働きをするノードもある。
 
 - Both actions are played if and only if there is a group S that is at least q cohesive and such that its complement is at least 1-q cohesive.
  q-cohesiveがあるからその凝集が形成された。
```
# Application
```
Drop out decisions
 1. Value to being in the labor market depends on friends in labor force.
 2. Drop out if some number of friends drop out
 3. Some heterogenity in threshold(different costs, natural abilities)
 4. Homophily - segregation in network
 5. Different starting conditions: history ..
Strategic coplements
```
# Beyond 0-1 Choices
```
payoff: concave(凹状の) function
(0-1)と計算の方法は同じ: f(X) -cX => f`(X)-c = 0

Two types of pure equilibria
 - distributed: x* > xi > 0 for some i's
 - specialized: for each i either xi=0 or xi=x*

Specialized Equilibria
 - Maximal independent set - set S of nodes such that
   - no two nodes in S are linked, and
   - every node in N is either in S or linked to a node in S
 ※ Only stable equilibria are specialist equilibria such that every non-specialist has
   two specialists in his or her neighborhood.
```
# Linear Quadratic Model
```
g = w(weight)/b
x = α + gx (If α = 0, then x=gx, so unit eigenvector)
友達にフィードバックを返す（invertible）ことのできる式
特徴:
 - Natural feedback from complementarities, actions relate to the total
   feedback from various positions
 - Centrality: relative number of weighted influences going from one node to another
 - Captures complementarities.
```
# Repeated Games and Networks
```
Faver Exchange
 How does successful favor exchange depend on/influence network structure?
 v: value of favor
 c: cost of a favor (v>c>0)
 δ: discount factor (1>δ>0)
 p: prob. i needs a favor from j in a period
 value of perpetual(絶え間ない) relationship: p(v - c) / (1 - δ)

Favor exchange between two agents （*Favor Exchangeが絶え間なく起こる条件式）:
  c < δ * p(v - c) / (1 - δ)    左辺: cost, 右辺: value of future relationship

Ostracism(仲間はずれ) agent:
  c < 2 * δ * p(v - c) / (1 - δ)

周辺部のnodeもdegreeが２つあると持続的。
```
# Support and Clustering
```
If no pair of players could sustain favor exchange in isolation and a network is robust,
then all of its links are supported.

Support: With what frequently do a typical pair of connected nodes, A and B, have a common neighbor?
        (共通の友人の有無)

Clustering: With what frequently are a typical node A's neighbors, say B and C neighbors of each other? 
        (友人同士の繋がりの有無)
e.g.
・ Support=1, Clustering=.47 ... 三角形の鎖のような細長い関係。（友人同士が繋がっていない。）
　　- SupportはFavor Echangeによってとても高くなる(0.5~0.8)。`Social` Supportも高い(0.45~0.75)。
     (一般的には友達の関係)

・ Support=.46, Clustering=.88 ... 村（やサークル）のような構造になる。
　　- ClusteringはBusinessによってとても高くなる(ほぼ１に近くなる)。Marriageも高い(0.85)。
  　　(社交的な関係)
```
# Exercise
```
【Games on Networks①】
Suppose that the players' utility functions in a game played on a network are such that for player i:
u(0, m) = 0;
u(1, m) = m - 1.5;
where m is the number of neighbors of i who play action 1.
Therefore, a pure action on a given network is an equilibrium(平衡) if for any agent, he takes action 1 if and only if at least two neighbors do so.
Is the action profile depicted in the picture an equilibrium?
a) Yes.
b) No.
-> a

【Games on Networks②】
Consider the best shot public goods model discussed in lecture such that a player to take action 1 rather than 0 if and only if
none of his or her neighbors take action 1.
Is the action profile depicted in the picture an equilibrium?
a) Yes.
b) No.
-> a

【Complements and Substitutes】
Suppose the players' utility functions in a game where they choose either action 0 or 1 are such that for any player i:
u(0, m) = 0;
u(1, m) = m - 2.5;
where m is the number of neighbors of i choosing action 1.
Which of the following statement is correct?
a) The actions are strategic complements
b) The actions are strategic substitutes
c) Neither of the above because of the threshold 2.5
->a (Because u(1, m) - u(0, m) = m - 2.5 increases in m.)


【Equilibrium】
In graph theory, a maximal independent set is a set S such that every edge of the network has at most one endpoint in S and
every node not in S has at least one neighbor in S.
Consider the network on 4 nodes {a,b,c,d}, which of the following are maximal independent sets?
 b - a - d
     |
     c
a) {a,b,c,d}
b) {a,b}
c) {b,c,d}
d) {a}
-> c,d (independent set: a set S of nodes such that no two nodes in S are linked,
        Maximal: every node in N is either in S or linked to a node in S,
        Maximal independent set: 【Best shotでは】 each 1 has no 1's in its neighborhood, each 0 has at least one 1)

【Properties of Equilibrium】
According to this lecture, which of the following are NOT complete lattices?
(Suppose the partial order is defined based on the standard definition of greater than or equal to/less than or equal to between vectors.)
a) {(0,0,0), (0,1,0), (0,0,1), (0,1,1)}
b) {(0,0,0), (0,1,0), (0,0,1), (1,0,1)}
c) {(1,0,0), (0,1,0), (0,0,1), (1,1,1)}
d) {(0,0,0), (0,1,0), (1,0,1), (1,1,1)}
->b,c
  (aとbの違い:　(1,1,1)が一番大きい平衡状態、(0,0,0)が一番小さい平衡状態。aとdは４つ全てで<=による大小関係がある。bとcには無い。（dの(1,0,1)は(0,1,0)より大きい。）
   b and c are NOT complete lattices(格子):
   In b, there is no element (vector) that is weakly larger than all other elements.
   In c, there is no element (vector) that is weakly smaller than all other elements.)

【Multiple of Equilibria】
In the network depicted in the picture, let S(yellow) be the group of all yellow nodes, and S(blue) be the group of all blue nodes.
Which of the following statements are correct?
a) S(yellow) is 1/2-cohesive.
b) The cohesiveness of S(yellow) is 1/3.
c) S(yellow) is 1/3-cohesive.
d) The cohesiveness of S(yellow) is 1/2.
-> a,c,d (Notice the difference between "S is r-cohesive" and "S's cohesiveness is r".
          In this example, min[i非含有S(yellow)] {j非含有N and S(yellow)}/ degree = 0.5;
          Therefore S is r-cohesive for r<=0.5, and such S's cohesiveness is 0.5)

【Beyond 0-1 Choices - distributed】
Consider the local public good setting description in this lecture, so that player i's payoff is f(xi + ΣN*xi) - cxi.
Suppose that x* = 1, i.e. f`(1)=0
The picture depicts three action profiles a,b and c
with the action taken by the node inside the corresponding circle.

1: 1-0-1-0 (circle)
2: 1/3-1/3-1/3-1/3 (circle)
3: 1/2-1/4-1/2-1/4 (circle)
Which statements are correct?
a) 3 is not an equilibrium
b) 1 is a distributed equilibrium
c) 2 is a distributed equilibrium
d) 1 is a specialized equilibrium
->a,c,d (1 is a specialized equilibrium, 2 is a distributed equilibrium, 
         3 is not an equilibrium, as each of the players playing 1/4 has incentives to reduce her action to 0.)

【Beyond 0-1 Choices - specialized】
Consider the local public good setting covered in this lecture, so that player i's payoff is
f(x + Σx) - cx
Suppose x* = 1, i.e. f`(1) = c
The picture depicts three action profiles, a, b and c,
with the action taken by the node inside the circle.
1: 1-0-1-0 (circle)
2: 1/3-1/3-1/3-1/3 (circle)
3: 0-1-1-1 (circle)
Which statements are correct?
a) 3 is a stable equilibrium
b) 2 is an unstable equilibrium
c) 1 is a stable equilibrium
d) 2 is a stable equilibrium
->b, c (According to the definition, a is a stable equilibrium, b is an unstable equilibrium, c is not an equilibrium.)

【Linear Quadratic Model】
For a given undirected network G, consider the linear-quadratic model where player i's payoff is:
u(x(i),x(-i)) = x(i) - x(i)^2 + Σg*x(i)*x(j)
in which g =1 if i,j 非含有 G, and g = 0 otherwise.
In this setting, the best response of x(i) to x(-i) is a solution to which equation?
a) (1 + Σ[j≠i] x(j))/2 = x(i)
b) (1 + Σ[j非含有N] x(j))/2 = x(i)
c) (1 - Σ[j非含有N] x(j))/2 = x(i)
d) (1 + Σ[j≠i] x(j)) = x(i)
-> b (Notice that Σ[j非含有N] x(j) = Σgx(j))

【Favor Exchange】
In the favor exchange model with two agents, favor exchange can be sustained in perpetuity if and only if
c < δ * p(v - c) / (1 - δ)
Suppose δ = 0.5, v=3, c = 1, and p is the probability that one player needs a favor from the other in a period.
Which of the following p's are such that favor exchange can be sustained?
a) 0.8
b) 0.4
c) 0.6
d) 0.2
-> 1 < 0.5 * p (3-1)/(1-0.5)
   1 < 2p
   1/2 < p
   then, a,c

【Two ways to support Favor Exchange①】
Consider the favor exchange model with parameters δ=0.5, v=3, c=1, and p=0.4
δ * p(v - c) / (1 - δ) < c < 2 * δ * p(v - c) / (1 - δ)
Which of the following networks can be sustained in equilibrium?
a) (5 nodes)十字, d=4
b) (5 nodes)砂時計, d=4+2
c) (5 nodes)Circle+中央nodeを貫いた直線, d=4+2
d) (5 nodes)Circle+中央nodeを通る直角, d=4+2
->b,c,d (a is not because each of the periphery(周辺部) players has only one neighbor.)

【Two ways to support Favor Exchange②】
Consider the favor exchange model with parameters δ=0.5, v=3, c=1, and p=0.4, so that
δ * p(v - c) / (1 - δ) < c < 2 * δ * p(v - c) / (1 - δ)
Which out of the following networks has the property that all links are supported?
a) (5 nodes)十字, d=4
b) (5 nodes)砂時計, d=4+2
c) (5 nodes)Circle+中央nodeを貫いた直線, d=4+2
d) (5 nodes)Circle+中央nodeを通る直角, d=4+2
->b (Recall that for a network g, a link i,j非含有g is supported if there exists a node k ≠ i,j.
```

# Exercise 2
```
1. Consider the following two games played on a network.
   In both cases each player is a sellers of a good on a market. A seller's neighbors in the network are the other sellers
   whose actions affect the seller's profits (so the seller's `competitors`).
   
   a). Each player i chooses where to produce a high or low quantity (and then the market determines the price at which the player sells).
   
   in particular, a(i)=1 stands for a high quality q(i)=5,a(i)=0 stands for a low quantity q(i)=3.
   
   Player i's profit as a function of her quantity and the quantities of her neighbors in the network j非含有N is
   (100 - q(i) - Σ[j非含有N]q(j)/2)*q(i)
   
   b). Each player i chooses where to set a high or low price (and then the market will determine the quantity sold).
   
   In particular, a(i)=1 stands for changing a high price p(i)=5, and a(i)=0 stands for changing a low price p(i)=3.
   
   Player i's profit as a function of her price and the prices of her neighbors in the network j非含有N is
   (100 - p(i) + Σ[j非含有N]p(j)/2)*p(i)
   
   According to the definition of strategic compleents/substitutes, which statement is correct?
   
   1) a): actions are strategic complements
      b): actions are strategic complements
   2) a): actions are strategic complements
      b): actions are strategic substitutes
   3) a): actions are strategic substitutes
      b): actions are strategic complements
   4) a): actions are strategic substitutes
      b): actions are strategic substitutes
   ->2
   
2. Consider the following game of complements with threshold 2 played on the network depicted in the picture.
   In particular, the players' utility functions are such that for player i:
   u(0, m) = 0;
   u(1, m) = m - 1.6;
   In lecture we have already seen three equilibria, as depicted in the picture.
   A: 全部0, B: 一部1, C: sustainable平衡だが1は6つ（全部でノード数は11）
   There is one more pure-strategy equilibrium. How many nodes are choosing action 1 in that equilibrium?
   a)0
   b)3
   c)4
   d)7
   e)11
   ->c (全部でノード数は11なので11以上にはならない。B一部1の違うパターン)

3. Consider again a game of complements with threshold 2, now on a different network as depicted in the picture.
   Which of the following is the largest pure strategy quilibrium ("largest" in the sense of having most players choosing 1)?
   a) 全部0
   b) 一部1
   c) 平衡では最大
   d) 平衡では無い。
   e) いずれでも無い
   -> c

4. Consider the following best shot public game on a directed network g={12,23,31}.
   In particular, player i can borrow a textbook from j if she has a directed link (arrow) pointing at j, and j buys the textbook.
   ・ Each player strictly prefers to buy the textbook (a=1) if she cannotborrow a textbook from anyone;
   ・ Otherwise, she strictly prefers to not buy (a=0) the textbook if she can borrow a textbook from her neighbor.
   Which is an pure-strategy equilibrium of this game? The numbers in the circles are the actions of the nodes.
   a) 0-0-0 (circle)
   b) 0-1-0 (circle)
   c) 0-1-1 (circle)
   d) 1-1-1 (circle)
   e) none of them
   -> e
   
5. Questions 5 and 6 are based on the network depicted in the following picture.
   There are three groups, green, blue and yellow, in the network.
   ・ green: 3 nodes inner-degree=3 outer-degree=3
   ・ blue: 4 nodes inner-degree=5 outer-degree=5
   ・ yellow: 4 nodes inner-degree=6 outer-degree=2
   What is the cohesiveness of each of the three groups?
   a) Green group 0.6; Blue group 0.5; Yellow group 0.75.
   b) Green group 0.5; Blue group 0.5; Yellow group 0.75.
   c) Green group 0.6; Blue group 0.25; Yellow group 0.5.
   d) Green group 0.6; Blue group 0.5; Yellow group 0.67.
   ->cohesive = min[i非含有S(yellow)] {j非含有N and S(yellow)}/ degree = ターゲットのノード数/ターゲットのinner-degree
     Green= 3/3 Blue= 4/5, Yellow=4/6
   
```

