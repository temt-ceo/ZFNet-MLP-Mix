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

```
