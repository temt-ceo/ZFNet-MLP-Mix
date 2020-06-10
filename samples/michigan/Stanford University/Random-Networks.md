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


```
