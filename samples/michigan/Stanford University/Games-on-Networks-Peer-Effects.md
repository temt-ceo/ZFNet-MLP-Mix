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


```
