# Growing Random Network
```
Degree Distribution
 - Start with m nodes fully connected
 - New node forms m links to existing nodes
 - An existing node has a probability m/t of getting new link each period
 ● t(time)経過後のDegreeを予測する
 - Expected degree for node i born at m<i<t is ↓ formed at birth.
  m + m/(i+1) + m/(i+2) + ... + m/t  (t: time(回数), m:links(数), i: node born at "i"th)
  or m(1 + log(t/i))
 birthdateが経過するとdegree（new link）の増えるスピードが鈍化する 
 ● 予測Degreeがある数dを下回る時のt(time)を求める
 - Nodes that have expected degree less than d at some time t are those such that
 m(1+log(t/i)) < d
 - Fraction(閾値後の断片)
 Ft(d) = (t - te^(d-m)/m)/t = 1 - e^(d(d-m)/m) 
```
# Mean Field Approximations
```
Continuous Time Approximation
 - starting condition .. di(i) = m
 - new links gained per unit time .. ddi(t)/dt = m/t
 Then, di(t) = m+ m * log(t/i)
 i: birth date
Power Law Explanations
 - Rich get richer .. growth of existing objects is proportional to size

Preferential Attachment
 - Newborn nodes form m links to existing nodes
 - tm links in total,
 - total degree is 2tm
 - Probability of attaching to i is d<i>(t)/2tm
 - Nodes born over time, form links at random with existing nodes
 初期からある古株のノードがたくさんDegreeを持つようになる。
 Expected degree: dd<i>(t)/dt = m(d<i>(t)/2tm) and d<i>(i)=m
                  => d<i>(t) = m(t/i)^0.5

Distribution of Expected Degrees
 Expected degree for node i born at m<i<t is
  di(t) = m(t/i)^0.5
 Nodes that have expected degree less than d at some time t are those such 
  m(t/i)^0.5 < d
 - Fraction(Distribution of Expected Degreesの断片)
 Ft(d) = 1 - (m/d)^2 and ft(d) = 2m^2/d^3
```
# Hybrid Models
```
Simple Hybrid
 - Simple version of Jackson-Rogers
 - Fraction a uniformly at random, 1-a　(fraction) via searching neighborhoods of friends
   (aはランダムに、1-aはその（ランダムにつながったノードの）neighborをサーチして繋がる事を想定したModel)
 - Fraction a uniformly at random, 1-a via preferential attachment
Meeting 'Friends of Friends'
 - Find new nodes via others: Network-based search
 - New node meets a*m nodes uniformly at random and directs links to them
 - Meets (1-a)m of their neighbors and attaches to them too.
 Relation to Preferential Attachment:
 - In a network with half degree k and half degree 2k individuals
 (Degreeが多いほど繋がりやすいという意味)
 - randomly select a link and then a node on one end of it
 - 2/3 chance that it has degree 2k,
 - 1/3 chance that it has degree k
 Chance of finding some node via the second part of this procedure is proportional to its degree.
 
 Nodes that have expected degree less than d at some time t are those i such that:
  (m + xam)(t/i)^(1/x) - xam < d  where x= 2/(1-a)
 a near 1 nearly exponential, a near 0 nearly preferential
 (a: どれぐらいPreferential AttachmentのModelを使用するかの割合)
 (aが１に近ければ逆U字(dgreeに対して指数的に増える)、0に近ければfat-tailになる（指数以外の要素がある）。)
 Small World .. a=0.56
 Prison Inmate Friendships .. a=1
 High School Romance .. a=1
 WWW ... a=0.36 (2/3がPreferential Attachment, 1/3がUnifomly Random)
```
# Fitting Hybrid Models
```
Friends of Friends/Hybrid Models:
 - Variety of degree distributions
 - Tie degree distributions to way in which links formed:
   - fat tails from network meeting process
   - more likely to meet well-connected nodes
 - Clustering from network meeting process -> friends of friends
   - connecting to friends of friends
 - Diameter naturally as small as E-R network ｰ>random
 - Assortativity in degree based on age (古いノードは古いノードとよりつながっている可能性がある) -> growth
Growth that's giving us assortativity,the randomness gives us small world diameter,
and the clustering part, the other part of small world is coming from friends of friends of the process.
       (avg. in-degree)  (r)  (Avg. clustering data) (Diameter)
WWW          4.6         0.57         0.11             11.3(avg.)
Citations    5.0         0.63         0.07             4
Coauthor     0.84        4.7          0.16             26
Ham Radio    3.5         5.0          0.47             5
Prison       2.7         無限          0.31             7
Rommance     0.83        無限           -               -
r = a/(1-a)
```
# Stochastic Block Models
```
表面上では分からない要素を繋がりの可能性の考慮を入れる
stochastic: 確率的な
Blue/Red
Pcross = 0.006
Pwithun = 0.089
Because likelihood of link depends on node attributes, also depends on whether nodes have friends in common.
```
# ERGMs, SERGMs, SUGMs
```
ERG Model:
 - Probability of a network depends on number of links
 - Probability of a network also depends on number of triangles.
 - likelihood of link depends on node attributes, also depends on whether nodes have friends in common.
 P = exp[ βlinks(g) + βtriangles(g) - c ]    c: constant
 β = log(p/(1-p)), hence p = e^β / (1 + e^β).
 トライアングルとスター（閉じていない）、アイソレイト(単独)のパラメータでProbabilityを求める  
 
Issues of ERGM:
 - MCMC estimation techniques are inaccurate (computing parameters)
 - Consistency of estimators of ERGMs (When are parameters accurate and how many nodes are needed?)
 - How to generate networks randomly?

SERGMs:
 - flexible way to introduce various local features and dependencies
 - Many networks lead to the same statistics, probabilities only depend on statistics
 - Networks with same statistics are "equivalent", collapse all equivalent networks.
 P = exp[ βS(g)] / Σ βS(g')
 - S can encode many things:
   - Links, cliques, k-stars, subgraphs, friends in common per link, multi-graphs, adapt for degree distributions
   - Can do preference based-models
   - Allow for node characteristics

Issues of SERGM:
 - Do they still provide enough information?
 
SUGMs:
 - Subgraph Generation Models
 リンクやトライアングルが徐々に結びつき合ってネットワークが構成されるというModel

```
# Estimating SUGMs
```
Sparseness
 - Incidental Triangles(Typical node involved in less than n^.5 links, n^.5 triangles)
  - 直感的な例) p(L) = o(n^-.5),  p(T) = o(n^-1.5)
  - 例（最初に42ノードと10個のトライアングルがある）) n=42, T=10 .. n choose 3 = 11480, p(T) = 0.00087  [10/11480 ≒ 0.00087]
  - 例（最初に42ノードと23個のリンクがある）) n=42, L=23 .. n choose 2 = 861 -(28 links in triangles) = 833,
    p(L) = 0.276  [23/833 ≒ 0.276]
  (ここのリンクは"トライアングルの中にないリンクのみ"を指している。)

Exercise:
  Consider the following graph: There are 25 nodes, 11 triangles and 31 links
  (out of which 24 are in some triangle and 7 are not in any triangles).
  [Note 25 choose 3 = 2300, 25 choose 2 = 300]
  
  According to this lecture, which of the following should be your estimation of the probability of triangles?
  a) p(T)=11/2300
  b) p(T)=7/276
  c) p(T)=24/2300
  d) p(T)=11/24
  -> a (T = 11, n choose 3 = 2300 Then p(T)=11/2300)

  According to this lecture, which of the following should be your estimation of the probability of links?
  a) p(L)=11/2300
  b) p(L)=7/276
  c) p(L)=24/2300
  d) p(L)=31/300
  ->b .. 7/(n choose 2 = 300 -(24 links in triangles) = 276)

Need for SERGMs / SUGMs:
 - Two nodes are either same or different(Block Modelはリンクが同じもの違うものの確率を出せる。Triangleは出せない。
 　 そこにSUGMを追加しトライアングルが同じもの違うものの確率を求める)
 - Step1: Estimate models
 - Step2: randomly generate links
 （Block Modelだけでは正しくネットワークを評価できないことが多い。SUGMを使いネットワークをモデリングすると似たネットワークを計算できる（上記理由により））

[Random Networks まとめ]
Strength Random Networks:
 - Generate large networks with well identified properties
 - Mimic real networks
 - Tie specific properties to specific processes

Weaknesses Random Network Models:
 - Missing the "Why" (Why this process?)
 - Missing implications(含有) of network structure (contextがないので効率的と言えない)
 - 実証に基づいた分析（small worldなど）の構造の説明が難しい
```
# EXERCISE
```
Consider a Growing random network with links formed to existing nodes uniformly, 
with m=5, i.e. with 5 new links formed by each newborn node.
What is the expected degree for the node #7 (the 7th born node) at time t=10
(When the 10th node is born)?
-> 5 + 5/(8) + 5/(9) + 5/10
解説: The node #7 born with 5 links, then 1 link is added with probability 5/8 when the 8th node is born,
then 1 more link with probability 5/9 when the 9th node is born,
and then 1 more with probability 5/10 when the 10th node is born.

What is the average degree of a growing random network with m, at date t? (m: mean)
-> 2m (Since d-m is exponentially distributed with mean m, so the average degree is m+m=2m, independent with t)

According to the lecture, which two of the following elements when combined can give a "Power Law" degree
distributions (which have "fat tails" similar to the obserbed data in practice)?
a. Nodes form links uniformly at random to other nodes
b. Growing networks, i.e. nodes are born dynamically, where older nodes have more links in expectation than the younger
c. "Rich get richer", i.e. links are formed with probability propotional to number of links a node already has
-> b and c

【Preferential Attachment】
Consider a Preferential attachment model described in the lecture, with m = 10 and t = 50.
What is the fraction of nodes with degree < 20 or F50(20)?
自力で回答:Ft(d) = (t - te^(d-m)/m)/t = 1 - e^(d(d-m)/m) = 1 - e^(20(20-50)/50) = 1 -e^(2*-3/5) = 1 - e^(-1.2) = 1 - 0.3 = 0.7
-> 0.75

【Hybrid Models】
Suppose in a network half degree k individuals and half degree 3k individuals,
you randomly select a link and then a node on one end of it.
What is the probability that the node found has degree 3k?
-> 0.75
(The chance of finding a node with degree 3k is 3k/k = 3 times of the chance of finding a node with degree k.
 Hence, the probability of finding a node with degree 3k is 3/(3+1)=0.75)

Consider a Hybrid model described in the lecture, with m = 10 and a = 0.5. 
What is approximately the threshold i such that nodes born after which have expected degree less than 20, at t = 50?
(i/t=[(m+xam)/(d+xam)]^x in which x = 2/(1-a))
->回答:16  .. x= 2/(1-a) = 4. Hence i/t = (3/4)^4 ≒ 0.32, and i = 16 is the threshold of interest.

【Fitting Hybrid Models】
Regarding the friends of friends/Hybrid models, which of the following statements are correct?
a. Only one specific "Power Law" degree distribution, in which log(f(d)) is linear in log(d), can be generated
b. High clustering results from "Preferential attachment"
c. Diameter is naturally as small as an Erdos-Renyi network
d. Assortativity in degree based on age only occures in "Friends of friends" models but other hybrid growing networks
->cのみ
a is wrong since fat tails n general not limited to the Power Law degree distribution are generated by hyrid models
b is wrong since high clustering is a result of the specific "frind of friends" searching procedure;
 Preferential attachment itself is not enough.
d is wrong since assortativity is a result of "growing" feature of the model; "Friends of friends" is not necessary.

【Block Models】
Consider the following example of a block model as discussed in the lecture.
Which of the following correctly calcurates Prob(Yelloe, Yellow) and Prob(Blue, Green)
Yellow .. 3 nodes, 3 inner path, two outer path to blue, one outer path to green
  -> 繋がる可能性は青2, 黄(3-1=)2, 緑1 .. yellow,green=1/(3つの黄のinner path * 4つの緑のinner path)=1/12
blue .. 4 nodes, 5 inner path, two outer path to yellow, two outer path to green
  -> 繋がる可能性は青(4-1=)3, 黄2, 緑1 .. blue,blue=5/6のprobability
green .. 4 nodes, 5 inner path, one outer path to yellow, two outer path to blue
  -> 繋がる可能性は青2, 黄1, 緑(4-1=)3
-> 1, 1/8 (1/8 = 2つの繋がり/(4つの青のinner path * 4つの緑のinner path))

【ERGMs】
If one fits an ERGM G(n,p) with just links, and finds a parameter β = 0.5. Which would be the corresponding
parameter p from the model?
-> e^0.5 / (1+e^0.5)
We have the relationship that β = log(p/(1-p)), hence p = e^β / (1 + e^β).

【SUGMs】
Which of the following lists the challenge of applying SUGMs to relatively dense(not parse) networks?
a) There tend to be many incidentally generated subgraphs and so the number that were directly
   generated needs to be estimated carefully.
b) There tend to be many subgraphs of one kind, which are impossible to calculate.
-> a (There can be too many incidentally generated subgraphs when the network is not sparse.)

【Random Networks まとめ】
In the example with Indian village data, relative to the standard link-based model(or block model),
the SUGM based on both link and traiangle counts:
a) Generates networks with better fit of observed data in terms of aspects other than links and triangles,
   such as the fraction in giant component, and patterns of diffution(captured by first eigenvalue)
   and homophily(captured by second eigenvalue);
b) Generates networks with better fit of obserbed data in terms of triangles.
-> Both.(a,b)

Consider the model of "Growing Random Networks" described in lecture 3.1(so the first model where links
are added to existing nodes uniformly at random), with m = 10.
At date t=40, what are:
(1)the expected degree for node 15 born at i=15, and
(2)the expected degree for node 30 born at i=30?
-> (1): 10 + 10/(16)≒0.67 + 10/(17) .. 10/40=0.25 => 10 + 0.46*25 = 21.5
   (2): 10 + 10/(31)≒0.33 + 10/(32) .. 10/40=0.25 => 10 + 0.29*10 = 12.9
(Growing Random Networks => birthdateが経過するとdegree（new link）の増えるスピードが鈍化する )

Consider the model of "Preferential Attachment Model" described in lecture 3.3, with m = 10.
At date t=40, what are:
(1)the expected degree for node 15 born at i=15, and
(2)the expected degree for node 30 born at i=30?
-> (1): m(t/i)^0.5 = 10(40/15)^0.5 = 10(8/3)^0.5 = 16.329
   (2): m(t/i)^0.5 = 10(40/30)^0.5 = 10(4/3)^0.5 = 11.547
(Preferential Attachment Model => 初期からある古株のノードがたくさんDegreeを持つようになる。)

Consider the model of "Hybrid Model" described in lecture 3.4, with m = 10.
Let us compare the cases of a = 0.8 and a = 0.2
At date t = 10,000, consider a node born at date i=20 and a node born at date i= 9999.
Recall that the approximation of the expected degree at time t of a node born at time i is
(m + 2am/(1-a))*(t/i)^((1-a)/2) - 2am/(1-a).
Which option makes the following statement correct?
At date t = 10,000,
a node born at date i=9999 has a ( 1 ) expected degree with a = 0.8 than with a = 0.2;
a node born at date i=20 has a ( 2 ) expected degree with a = 0.8 than with a = 0.2;
->a=0.2 .. 0.8がPreferential Attachment, 0.2がUnifomly Random
  and Preferential Attachment Model => 初期からある古株のノードがたくさんDegreeを持つ
  and Random Networks => birthdateが経過するとdegree（new link）の増えるスピードが鈍化する
  Then If a is lower, it depends Preferential Attachment = 初期からある古株のノードがたくさんDegreeを持つ
  and If a is higher, it depends Random Networks = birthdateが経過するとdegree（new link）の増えるスピードが鈍化する
  (1): higher, (2): lower
  
```
