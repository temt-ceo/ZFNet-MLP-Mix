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

```
