# Diffusion(拡散) on Networks
 - E[d] -- Expected Degreeの意味.(公式の"d"と置き換えられる)
 - nonzero steady state -- sustainableなinfectionがある状態（λ>0）
 - low density no condition (degreeが少ないと感染は少ない)
 - θ: あなたがランダムに会う人が感染している確率(0~1,0:感染無し)。これが一定値にあることをsteady state.
 - SIS Diffusion Model = シミュレーションをするための拡散モデル。
# Networks and Behavior
```
How does networs structure impact behavior?
 - diffusion .. simple infections, contagion
 - learning .. opinions, information
 - games on networks .. choices, decisions

Diffusion(拡散)
 - disease
 - ideas basic information
 - buy a product or not

S-Shape Spatial Pattern
 - 他の人が特定の作業をしていた人数が多いほど、その作業を行うまでの期間が短いなど（アドバイスなどで拡散した）
 - 農産物(交配種)など他の人が耕作している人が多いほど影響受ける。
x軸： 時間（特定の時間軸で実行する割合が増える。その早い遅いは環境の影響を受ける）
```
# Bass Model
```
Bass Model .. 予測によく使われる。 
 - A benchmark model with no explicit social structure
 - F(t): fraction of the population who have adopted action 1 at time t
 - p: rate of spontaneous  innovation/adoption .. 自発的に行う（最初のdF(t)/d(t)の傾き）
 - q: rate of imitation of adoption .. 真似をする(大きいとS-shapeに)
 式: dF(t)/dt = (p + q*F(t))(1-F(t))    1-F(t) .. まだadoptしていない(action=0)人達
 - "いつ"拡散(diffusion)し、"どれくらいになる"かはこの式では求められない
```
# Diffusion on Random Networks
```
Richer Model(より複雑なModel)を紹介
 - Idea, disease, computer virus spreads via connections in the network
 - Nodes are linked if one would "infect" the other (fluのように)
 - Work with Erdos-Renyi random network
 - ネットワークのコンポーネントが大きいほど感染は多い。
 - Size of the giant component when .. 1/n < p < log(n)/n (大きなコンポーネントができる条件。最低はcircleができ初める条件)
```
# Giant Component Poisson Case
```
Calculating the Size of the Giant Component(1度だけ感染する前提)
 - q: fraction of nodes in largest component
 - 1 - q: probability that a node is outside of the giant component
 - 1 - q = Σ(1-q)^d * P(d)
 - P(d): the chance that the node has d neighbors (d=degree)
 コンポーネントの拡大には生成されるdegreeが鍵を握っている。（最初は全てindividualな状態）
Solve for q..
 Solve 1 - q = Σ(1-q)^d * P(d)  .. E(d)により感染の大きさカーブが形成される。
```
# SIS Model
```
Susceptible(感受性が強い) Infective Susceptible (繰り返すことを意味している。)
 - get well randomely in any period at rate δ > 0
 - Let p be the percent infected
 - number of infected neighbors with rate v > 0, plus spontaneous ε
 Bass Modelに回復者をプラスしたような式
 - p = 1 - δ/v (但し、p>=0)(δ < v で感染者は増える。)
 - p(d): fraction of nodes of degree d infected = fraction of nodes that have d meetings

The fraction of the population with degree d that is infected in steady state is
ρ(d) = λθd/(λθd+1), where λ = υ/δ:
・ υ is the infection rate (at which uninfected node gets infected by an infected neighbor);
・ δ is the recovery rate (at which infected node recovers from infection);
・ θ is the fraction of randomly chosen neighbors who are infected.
```
# Solving the SIS Model
```
あなたが歩いて会う人の内感染している人の割合 θ = H(θ) = ΣP(d)λθd^2/[(λθd+1)*E[d]]
How H(θ) depends on P(d), E[d] etc.

nonzero steady stateをネットワークが持つためには -- υ/δ(=λ) > E[d]/E[d^2]
　上記を満たす為に通常のネットワークで必要なのは -- υ/δ(=λ) > 1/E[d] (=> E[d]==E[d^2])
　上記を満たす為にE-Rネットワークで必要なのは -- υ/δ(=λ) > 1/(1+E[d])
　上記を満たす為にpower-lowネットワークで必要なのは -- E[d^2] diverges(always have a nonzero steady state)(=> λ>0)

θ = Σ P(d)*λθ*d^2 / ((λθ*d + 1) * E[d])
P（d）: 予想されるdegreeの確率
```
# Fitting a Diffusion Model to Data
```
1. ネットワークに参加する人と参加しない人の拡散率をモデルで求める。

2. Application(適用): Financial Contagions  (欧州の国債持ち合いなど相互に負担し合う組み合わせ)
  {1,...,n}: Organizations (countries, firms, banks)
  pi:        price of investments of organization i.
  Cij:       cross holdings (iとj organizationによる)
  V:         Value of an Organization
  
  破産はどうなったら起こる？
  ->ネットワークが中程度に絡み合っている状態が１箇所の破産が他へも広まりやすい。（小程度では他と干渉しない可能性が高く、大程度では影響は希釈（分散）される為）
```
# EXERCISE
```
【Diffusion】
Regarding the "S-shaped adoption" pattern described in this video lecture from the data of Griliches(1957)
on hybrid corn diffusion, which of the following statements are correct?
a) The adoption rate kept accelerating until all farmers had adopted.
b) The adoption accelerated after some point of time in each of the states pictured in the graph.
c) This "S-shaped" adoption pattern is a very rare occurrence only observed in hybrid corn diffusion.
d) The adoption process started out slowly in each of the states pictured in the graph.
-> b, d (S-shapedの特徴)

【Bass Model】
The version of the Bass Model described here contains the following factors:
p: the rate of spontaneous adoption;
q: the rate of imitation;
F(t): the fraction who have adopted by time t.
Which of the following statements are correct regarding how the "S-shape" is generated by the model?
a) The acceleration part of the "S-shaped" adoption curve is generated by having p grow with time and
   q shrink with time.
b) The only way for this model to generate the acceleration part of the "S-shaped" adoption curve is to have a very tiny q.
c) The acceleration in adoption is generated by a q that is larger than p.
d) The adoption rate eventually slows down in this model because the population that hasn't adopted yet becomes small.

【Diffusion on Random Networks】
To study diffusion in the context of Erdos-Renyi random networks, we are exploring the size of the giant component
for values of p such that 1/n < p < log(n)/n. Since then there are some emerging components to analyze
and yet the network is unlikely to be connected.
Which of the following approximates this region of p for n = 50 nodes?
[Hint: log(50) is approximately 3.9.]
-> 1/50 < p < 3.9/50 => 0.02 < p < 0.078 -> p 非含有 (0.02, 0.078)

【Giant Component Poisson Case】
Recall that an approximation for the probability q that a node is in the giant component, as described in this lecture,
is the solution of the equation 1-q=Σ(1-q)^d * P(d)
Suppose the degree distribution is such that d = 0 has probability 1/3 and d=2 has probability 2/3,
so that P(0)=1/3, P(2)=2/3. Also, take (1-q)^0 = 1.
The (nonzero) probability q that a node is in the giant component is then a solution to which of the following equations?
-> P(0): 1-q = Σ(1-q)^d * P(d) = 1*1/3
   P(2): 1-q = Σ(1-q)^d * P(d) = (1-q)^2*2/3
   P(d): 1-q = 1/3 + 2/3 * (1-q)^2

【SIS Model】
Following a mean-field approximation of the SIS model, the fraction of the population
with degree d that is infected in steady state is ρ(d) = λθd/(λθd+1), where λ = υ/δ, and recall that:
・ υ is the infection rate (at which uninfected node gets infected by an infected neighbor);
・ δ is the recovery rate (at which infected node recovers from infection);
・ θ is the fraction of randomly chosen neighbors who are infected.
Which of the following statement(s) are correct in a case where θ and λ are positive?
[Hint: if your calculus(公式) is rusty, simply examine this expression for d at 0 and d very large.]
a) The steady state infection fraction ρ(d) increased in d.
b) The steady state infection fraction ρ(d) approaches 0 as d becomes very large.
c) The steady state infection fraction ρ(d) does not depend on the value of d.
d) The steady state infection fraction ρ(d) decreases in d.
-> a

【Solving the SIS Model】
According to the "Theorem: Conditions for Steady State of Mean-Field SIS Process" covered in this lecture:
If the parameters are such that υ=0.2, δ=0.2, E[d]=3 and E[d^2]=5,
does there exist a nonzero steady-state of θ?
a) Yes
b) No
-> a (Notice υ/δ(=λ) = 1 > E[d]/E[d^2] = 0.6)

【Solving the SIS Model】
Consider three degree distributions, each of them places equal liklihood on a three different degrees.
・ P1(d) is such that degrees are in {1,3,5}, each has probability 1/3;
・ P2(d) is such that degrees are in {2,3,4}, each has probability 1/3;
・ P3(d) is such that degrees are in {1,2,3}, each has probability 1/3;
Compare the corresponding (nonzero) steady-state rates of infected neighbors: θ1, θ2 and θ3:
According to our discussions on "Ordering Networks" in this lecture, which of the following comparisons are correct?
a) θ１ < θ3
b) θ2 < θ3
c) θ2 > θ3
d) θ１ > θ2
-> c, d (θ1>θ2 because P1 is a mean-preserving spread of P2(凹の上を直線で引っ張った状態). θ2>θ3 because P2 First-Order Stochastically Dominates P3.)

【Financial Contagions①】
From the example in the lecture, we see that A = C(hat)(I - C)^-1 = [[2/3 1/3][1/3 2/3]]. <= [[自分取り分, cross invest][cross invest 自分取り分]](上が１,下が2の会社)
What are the two firms' final market values based on the cross holdings and direct investments if there is a direct investment income of $9 to firm 2?
a) Firm1: $1/3, Firm2: $2/3
b) Firm1: $3,   Firm2: $6
c) Firm1: $1,   Firm2: $1
d) Firm1: $3,   Firm2: $9
-> b

【Financial Contagions②】
The simulation results concerning diversification and contagion (as described in this lecture) found patterns
regarding how the "percentage of organizations that fail" is affected different factors.
Which of the following statements are true?
a) The percentage of organizations that fail is (weakly) descreasing in d, the level of diversification.
b) The percentage of organizations that fail is (weakly) increasing in d, the level of diversification.
c) The percentage of organizations that fail is initially (weakly) increasing in d and then (weakly) decreasing.
-> c (The percentage of organizations that fail is higher as the threshold of failure θ becomes higher.
      The percentage is at its peak with medium diversification in terms of degree d.)
```

# EXERCISE 2
```
1. The picture plots three duffusion processes, with ...
   Among these three curves, which is the closest match in terms of its shape to the patterns found in Griliches(1957)'s analysis of the data on hybrid corn diffusion?
a) The blue curve. (S字状)
b) The red dashed curve.(逆凸字状)
c) The black dot-dashed curve.(逆S字状)
-> a
2. The version of the Bass Model described in lecture 5.2 is based on the following two key parameters:
  p: the rate of spontaneous adoption;
  q: the rate of imitation;
   The picture below plots three diffusion processes for the following three sets of parameters:
   p=0.01, q=0.9;
   p=0.2, q=0.9;
   p=0.2, q=0.1;
   Identify which parameters correspond to which curve.
-> p=0.01, q=0.9: Red curve (S字状)
   p=0.2, q=0.9: Blue dashed curve (早く100にたどり着く)
   p=0.2, q=0.1: Black dot-dashed curve (ゆっくり90ぐらいまでたどり着く)

3. The approximation for the probability, q, that a node is in the giant component, as described in lecture, 
   is the nonzero solution to the equation (1 - q) = Σd(1-q)^d*P(d).
   Suppose the degree distribution P(d) is such that d = 0 has probability 1/3 and d = 2 has probability 2/3.
   Also, take (1-q)^0 = 1.
   What is the nonzero probability q that a node is in the giant component?
   ->P(d=0): 1-q = 1 * 1/3
     P(d=2): 1-q = (1-q)^2 * 2/3
     P(d): 1-q = 1/3 + (1-q)^2 * 2/3
           3-3q = 1 + 2(1-q)^2
           2(1-q)^2 + 3q = 2
           2*q^2 - q = 0
           q = 0.5

4. Consider contagion with immunity and link failure, as described in lecture.
   In particular, consider contagion on a Erdos-Reny random network on n=51 nodes with p=0.15 being the probability of each link.
   Suppose a fraction π = 0.5 of the nodes are immune naturaly, and only a fraction f= 0.4 of the links result in contagion.
   What is the infected fraction of nodes, q(rounded to the nearest tenth)?
   [Hint: recall the infected fraction of nodes is the solution to some equation that you have seen in lecture 5.4]
   ->P(d=0): 1-q = 1 * 1/2
     P(d=1): 1-q = (1-q) * 4/10
     q:感染したノード数, 1-q:感染しなかったノード（これが5割）
     P(d=1): 5-5q=2 - 4q + 2q^2
             2q^2 + q - 3 = 0
             q^2 + 0.5q - 1.5 = 0
     p=0.15をかけてしも一桁で丸めると .2 <-   違うらしい。ここだけ分からなかった..

5. Consider the Mean-Field SIS Process described in lecture. Recall that the steady state infection fraction of people 
   that one meets is the solution to the following equation.
   θ = H(θ) = Σ P(d) * λ * θ * d^2 / ((λ*θ*d + 1) * E[d]).
   Consider a regular network with 50 nodes, each of which has exactly 10 neighbors. So P(10)=1, and P(d) = 0(d≠10).
   Further suppose the infection/recovery ratio is λ = ν/δ = 0.5.
   What is the (nonzero) steady state(θ) ?
   -> θ = 1 *0.5 * θ * 100 / (0.5 * θ * 10 + 1) * 10
      (50 * θ) / (50 * θ + 10) -　θ = 0
      θ = 0.2 -> 10 / 20 - 0.2 = 0.3 ≠ 0
      θ = 0.5 -> 25 / 35 - 0.5 = 0.1... ≠ 0
      θ = 0.8 -> 40 / 50 - 0.8 = 0
      よって0.8
      
6. Consider the Mean-Field SIS Process described in lecture. Recall that the steady state infection fraction of people that one meets..(上の問題と同じ説明)
   Consider a regular network setting in which each person has the same number of interactions. So P(d) = 1 for some d > 1 and P(d意外)=0).
   For which infection/recovery ratio is λ = ν/δ is there a positive steady state infection ratio.
   -> υ/δ(=λ) > E[d]/E[d^2]
      λ > 1/d

```
