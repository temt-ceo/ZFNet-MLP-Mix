# Growing Random Network
```
Degree Distribution
 - Start with m nodes fully connected
 - New node forms m links to existing nodes
 - An existing node has a probability m/t of getting new link each period
 - Expected degree for node i born at m<i<t is ↓ formed at birth.
  m + m/(i+1) + m/(i+2) + ... + m/t
 - t: time(回数),  i: node
```
# EXERCISE
```
Consider a Growing random network with links formed to existing nodes uniformly, 
with m=5, i.e. with 5 new links formed by each newborn node.
What is the expected degree for the node #7 (the 7th born node) at time t=10
(When the 10th node is born)?
-> 5 + 5/(8) + 5/(9) + 5/10
解説: The node #7
```
