
#### Unordered Data Structures in C++ (Week3) submission task

#### Graph Search Project
・ "Adjacency List" graph data structure(rather than using pointer, use value copies of simple ofject)<br>
・ GridGraph class: use a combination of std::unordered_map, std::unordered_set and std::pair.<br>
・ Implementing graoh internals and running a breath-first search(BFS) on a GridGraph.<br>

#### std::unordered_set
・ (requirement:)no duplicates; inserting the same item twice has no effect.<br>
・ In C++, the unordered set uses **hashing** internally, so the key type must support == operator as well as std::hash.<br>
・ Unlike the unordered map type, the keys for an unordered set do not have associated values.<br>

#### std::set
・ (ordered type) based on a binary tree implementation instead of **hashing**. Uses < and == operators to arrange items.<br>
・ appropriate when we need to maintain data in order.<br>

#### std::vector
・ This is also possible use as a general purpose ordered set.<br>
・ Other "set" data structures like the disjoint-sets have specific use cases, there are no STL classes.<br>

#### Initialization with type inference
・ In C++11 and newer, there are compiler features that can infer the correct types automatically.<br>

#### Graph search algorithms overview
・ Not ordered<br>
・ No obvious start<br>
・ breadth-first search(BFS): find the shortest paths from one vertex to other vetices in the graph.<br>



**UnorderedMapExcercises.cpp**<br>
```
#include <iostream>

#include "UnorderedMapCommon.h"

```


<br>
**main.cpp**<br>
```
#include <iostream>
#include <string>

#include "UnorderedMapCommon.h"


/*
注. 各クラスはhファイルで以下のように宣言されている。
using StringVec = std::vector<std::string>;
using StringIntPair = std::pair<std::string, int>;
using StringIntMap = std::unordered_map<std::string, int>;
using StringIntPairVec = std::vector<StringIntPair>;
*/
int main() {
  
  
  return 0;
}
```

**UnorderedMapCommon.h**<br>
```

```

**UnorderedMapCommon.cpp**<br>
```

```

