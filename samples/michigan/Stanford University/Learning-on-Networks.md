# Networks and Behavior
```
Learning
 1. Each period get a payoff based on choice (A->必ず1のpayoff, B->pの確率で2, 1-pの確率で0のpayoff)
 2. Also observe neighbors' choices (to get maximized payoff)
 - Bayesian learning: repeated actions, observe each other
 - pの確率次第でagentsの動きは皆変わる（何人かはBを常に選ぶが。BがAより適しているとしても選ばない人もいる。Aが適している時は皆Aを選ぶ）
 （Learningには皆が同じpayoffである、同じイベントが何度も続くわけではない、Nwtworkが仕事していない、という限界がある。）

DeGroot Model: repeated communication, "naive" updating
 - Who has influence?
 - 皆が同じpayoff?、同じイベントが何度も続く?
 1. "T" weighted directed network, stochastic matrix
 2. Start with beliefs, attitude, etc. ... b(0) in [0,1]
 3. Updating: b(t) = Σ T * b(t-1)  左辺:i, 右辺:j
 (これも最終的には全員同じ選択をする。)

Convergence in DeGroot Model:
 - b(t) = T * b(t-1)
 - Convergenceするまで繰り返す計算を続ける。（tまで）
 
Influence:
 色々な所から影響を受ける（内向的な）Agentほど影響力が強い。 ... GoogleのPageRankと同じ考え方。
 - Let D = Σd
 - Claim s = d/D for each i
 - Recall s is unit eigenvector

Information Aggregation:
 - When is the consensus accurate?
 - Suppose true state is μ
   Agent i sees bi(0) = μ + εi(error)
   εi has 0 mean and finite variance, bounded below or abobe.
   
 - No Opinion Leaders
   s(i) = ΣTji*s(j)
    - If there is some i with Tji>a>0 for all j, then s(i) > a
   
```
# Exercise
```
【Bayesian learning】
Consider the model of observational Bayesian learning on a network that we have discussed in which action A pays 1 for sure and 
action B pays 2 with an initially unknown probability p, and 0 with probability 1-p. Suppose that the society is ina network that 
is connected and all agents start with the same beliefs over which possible values p could have, and think p to be either 1/4 or 3/4.
According the result we discussed, which of the following statement(s) are correct?
a) If p<0.5, then with probability 1 all agents will play action A from some time onwards.
b) If p>0.5, then with probability 1 all agents will play action B from some time onwards.
c) With probability 1, there is some time after which all agents will play the same action.
d) With probability 1, there is some time after which all agents will randomly choose between A and B on each turn.
-> a,c(Bの条件を全員が選ぶことはない。もし、Bの確率が悪ければいずれ全員Aを選ぶ。その為cも正しい。)

【DeGroot Model】
Consider the belief updating process from DeGroot Model.
Suppose that the network and initial beliefs are as depicted in the picture below. Notice that the network is discussed in this video, but
the initial beliefs are differ from those discussed in the video.
[[1/3 1/3 1/3], (initial belief=1) ... 1/3の確率で自分の動きを参考にしている
 [1/2 1/2  0 ], (initial belief=1) ... 1/2の確率で自分の動きを参考にしている
 [1/2  0  1/2]] (initial belief=0) ... 1/2の確率で自分の動きを参考にしている
After one step of updating, what are the three players' beliefs?
a) Player1: 1/3, Player2:1/2, Player3: 0
b) Player1: 2/3, Player2:1,   Player3: 0
c) Player1: 2/3, Player2:1,   Player3: 1/2
d) Player1: 2/3, Player2:1/2, Player3: 1
-> c

【Convergence in DeGroot Model①】
Consider the belief updating process from the DeGroot model.
Suppose that the network and initial beliefs are as depicted in the picture below, and represented by the matrix T. 
The belief vector at time t is b(t) = (3/4, 1/2, 0)`.
What is b(t+1)?
[[ 0  1/2 1/2], (initial belief=3/4)   T = [[ 0  1/2  1/2],
 [ 1   0   0 ], (initial belief=1/2)        [ 1   0    0 ],
 [ 0   1   0 ]] (initial belief=0)          [ 0   1    0 ]]
->(1/4  3/4  1/2)`
  (②が①の影響を受け取って①と同じ値になる。③が②の影響を受け取って②と同じ値になる。①は②と③の中間の値になる。矢印の先が影響を受ける元と考えると良い。）

【Convergence in DeGroot Model②】
Consider the belief updating process from the DeGroot model.
Suppose that the network and weights are as depicted below.
T = [[ 0  1/2  1/2],
     [ 0   0    1 ],
     [ 1   0    0 ]]
Do beliefs converge?
a) Yes.
b) No.
-> a

【Influence①】
Consider the belief updating process from the DeGroot model and a society with a strongly connected and aperiodic network,
so that beliefs will converge and each individual's influence on the final belief is represented by the s discussed in the lecture.
Which of the following describes the node who have the largest influence?
a) The node with the highest Closeness centrality
b) The node with the highest Eigenvector centrality
c) The node with the highest Betweenness centrality
-> b

【Influence②】
Consider the belief updating process from the DeGroot model.
As described in this lecture, suppose that all relationships are reciprocal(相互の) to that Tij > 0 if and only if Tji > 0,
and all people equally weight their connections so that Tij=1/di where d is person i's out degree.
Suppose that the network is also connected and that it is aperiodic so that the updating process converges.
Which of the following statements are correct?
a) The person with the hichest degree have the largest influence.
b) The person with the lowest degree have the largest influence.
c) Person i's influence is 1/2 for all i regardless of network size.
-> a (内向的ほど影響力が強い)

【Information Aggregation】
Consider the DeGroot model with a "regular" society in which every agent has exactly d > 1 friends (with the same d for all agents), and such that the 
network is connected. In particular, suppose that agent i puts weight Tij = a/d on each friend j, for some a>0 and weight Tii = 1-a>0 on him or herself.
In this case:
a) T will be wise and for large enough n each agent's limiting beliefs will be approximately correct with high probability since T is column stochastic (and so each person has influence 1/n).
b) T will not be wise and beliefs will fail to converge since each agent places a positive weight on him or herself and so will never update beliefs.
-> a
```
