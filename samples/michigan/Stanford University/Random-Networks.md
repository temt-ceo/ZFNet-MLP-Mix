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
 - Nodes born over time, foem links at random with existing nodes
  Form links with probability [proportional to number of links a node already has.]
  Probability of attaching to i is
   di(t)/2tm
   tm: links in total, 2tm: total degree
    - Newborn nodes form m links to existing nodes
初期からある古株のノードがたくさんDegreeを持つようになる。
 
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
 (aが１に近ければ逆U字、0に近ければfat-tail)
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

```
