#### Unordered Data Structures in C++ (Week1) study memo
AAA。

```
#include <iostream>
#include <iomanip>
#include <vector>
#include <algorithm>
#include <functional>

int insert(int value, std::vector<int> &table) {

}

int main() {
  int i, j, hit, max_hit = 0, max_value = -1;
  std::vector<int> value(500);
  
  int old_value = 0;
  for (i = 0; i<500; i++) {
    old_value += rand() % 100;
    value[i] = old_value;
  }

  std::vector<int> table(1000, -1); // <- Create hash table of size 1000 initialized with -1.
  
  for (i=0; i<500; i++) {
    hit = insert(value[i], table);
    if (hit > max_hit) {
      max_hit = hit;
      max_value = value[i];
    }
  }
  
    std::cout << std::setw(3) << l << ":";
  
  return 0;
}
```

#### Unordered Data Structures in C++ (Week2) study memo
--AAA。

```

```
