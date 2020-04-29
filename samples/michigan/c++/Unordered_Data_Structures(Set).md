
## Disjoint Sets ("union-find" data structure)
・ A series of sets which are disjoint from one another.<br>
・ But every single element within a set is considered to be equivalent(同等/同量のもの) within that set.<br>

#### Hashing
1. a hash function: Map our input space into an array index.その代わりO(1)だけで行うので著しく早い。<br>
2. an array: Stores the actual data.(this is going to be indexed in by hash function.)<br>
3. collision handling strategy: Handle the collisions that occur when our hash function maps different values to the same 

**Heap.h**<br>
```
#pragma once

template <typename T>
class Heap {
  public:
    Heap();
    void insert(const T key);
    T removeMin();
  private:
    unsigned size_;
    unsigned capacity_;
    T *item_;
    
    int _parent(unsigned index) const;
    int _minChild(unsigned index) const;

    void _heapifyUp(unsigned index);
    void _heapifyDown();
    void _heapifyDown(unsigned index);
    void _growArray();

    bool _isRoot(unsigned index) const;
    bool _isLeaf(unsigned index) const;
}

#include "Heap.hpp"
```
