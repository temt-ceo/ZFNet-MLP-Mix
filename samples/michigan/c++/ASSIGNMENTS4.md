#### Unordered Data Structures in C++ (Week1) study memo
The main() procedure below will create 500 random values and call insert() on each one of them to insert them into the table. At the end, this procedure will report the length of the longest cluster encountered when inserting a value (as reported by your insert() function) and then print out the contents of the hash table so you can see how clusters form. Since the original hashed position will be the three least significant digits of the value stored there, it will be easy to see which values had to be relocated by linear probing, and how much probing was needed.<br>
<br>
hash functionを自作する。(arrayが埋まっている場合は１つ次を探索)
```
#include <iostream>
#include <iomanip>
#include <vector>
#include <algorithm>
#include <functional>

int insert(int value, std::vector<int> &table) {
  int counter = 0;
  int index = value % 1000;
  //std::cout << value << " -> " << index << " < " << table[index] << std::endl;
  if( table[index] == -1 ) {
    table[index] = value;
  }
  else {
    while ( true ) {
      counter++;
      index++;
      if (index > 999) {
        index = 0;
      }
      if( table[index] == -1) {
        table[index] = value;
        break;
      }
    }
  }
  return counter;
}

int main() {
  int i, j, hit, max_hit = 0, max_value = -1;
  std::vector<int> value(500);
  
  int old_value = 0;
  for (i = 0; i<500; i++) {
    old_value += rand() % 100; // ここは限りなく増え続ける（0〜９９ずつ）
    value[i] = old_value;
  }

  // Create hash table of size 1000 initialized with -1.
  std::vector<int> table(1000, -1);
  
  for (i=0; i<500; i++) {
    hit = insert(value[i], table);
    // std::cout << "Hit: " << hit << std::endl;
    if (hit > max_hit) {
      max_hit = hit;
      max_value = value[i];
    }
  }
  
  std::cout << "Hashing value " << max_value << " experienced " << max_hit << " collisions." << std::endl << std::endl;
  
  for (j = 0; j < 1000; j += 10) {
    std::cout << std::setw(3) << j < ":";
    for (i = 0l i < 10; i++) {
      if (table[j+i] == -1) // <-カッコがなくてもいいらしい
        std::cout << "      "; // 6ますの空白
      else
        srd::cout << std::setw(6) << table[j+i];
    }
    std::cout << std::endl;
  }
  
  return 0;
}
```

#### Unordered Data Structures in C++ (Week2) study memo
--AAA。

```

```
