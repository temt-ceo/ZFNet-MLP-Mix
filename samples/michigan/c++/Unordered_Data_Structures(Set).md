
## Disjoint Sets ("union-find" data structure)
・ A series of sets which are disjoint from one another.<br>
・ But every single element within a set is considered to be equivalent(同等/同量のもの) within that set.<br>

#### Disjoint Sets ADT(abstruct data type)
1. Maintain a collection S = {S0, S1, ... Sk} <br>
2. Each set has a representative member.(先頭のメンバがidであり一意でないといけない)<br>
3. API: void makeSet(const T & t); <br>
        void union(const T & k1, const T & k2); <br>
        T & find(const T & k); <br>

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
