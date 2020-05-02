
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

