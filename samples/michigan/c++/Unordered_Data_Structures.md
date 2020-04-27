## Unordered Data Structures in C++

### Hash Tables
・Decompose a real-world problem, such as cache memory, into the appropriate implementation of a hash table,<br>

#### Hashing
1. a hash function: Map our input space into an array index.その代わりO(1)だけで行うので著しく早い。<br>
2. an array: Stores the actual data.(this is going to be indexed in by hash function.)<br>
3. collision handling strategy: Handle the collisions that occur when our hash function maps different values to the same point in the array.<br>
SUHA: The simple uniform hashing assumption.(値が同じにならない事。hash functionの要件③。要件①はO(1)である事。要件②は毎回同じ値になる事。)<br>
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

**Heap.hpp**<br>
```
#include "Heap.h"
#include <algorithm>
#include <iostream>

template <typename T>
Heap<T>::Heap() {
  size_ = 0;
  capacity_ = 2;
  item_ = new T[capacity_ + 1];;
}

template <typename T>
void Heap<T>::insert(const T key) {
  // Check to ensure there's space to insert an element
  // ...if not, grow the array
  if ( size_ == capacity_ ) { _growArray(); }

  size_++;
  item_[size_] = key;
  
  _heapifyUp(size_);
  
  std::cout << "After Heap<T>::insert(key = " << key << "): ";
  for (unsigned i = 1; i <=size_; i++) {
    std::cout << item_[i] << " ";
  }
  std::cout << std::endl;
}

template <class T>
T Heap<T>::removeMin(){
  T minValue = item_[1];
  std::swap( item_[1], item_[size_--] ); // Swap first item(value) with last item(value).
  
  _heapifyDown();
  
  return minValue; // Return the minimum value
}

template <class T>
void Heap<T>::_heapifyUp( unsigned index ) {
  if ( !_isRoot(index) ) {
    if ( item_[index] < item_[_parent(index)] ) {
      std::swap( item_[index], item_[_parent(index)] );
      _heapifyUp( _parent(index) );
    }
  }
}

template <class T>
void Heap<T>::_heapifyDown() {
  _heapifyDown(1);
}

template <class T>
void Heap<T>::_heapifyDown(unsigned index) {
  if ( !_isLeaf(index) ) {
    T minChildIndex = _minChild(index);
    if ( item_[index] > item_[minChildIndex] ) {
      std::swap( item_[index], item_[minChildIndex] );
      _heapifyDown( minChildIndex );
    }
  }
}

template <class T>
void Heap<T>::_growArray() {
  T* newItem = new T[(2 * capacity_) + 1];
  for (unsigned i = 1; i <= capacity_; i++) { newItem[i] = item_[i]; }
  
  capacity_ = 2 * capacity_;
  delete[] item_;
  item_ = newItem;
  std:cerr << "Heap<T>::_growArray() increased array to capacity_ == " << capacity_ << std::endl;
}

template <class T>
bool Heap<T>::_isRoot(unsigned index) const {
  return (index == 1):
}

template <class T>
bool Heap<T>::_isLeaf(unsigned index) const {
  return (index * 2 > size_);
}

template <class T>
int Heap<T>::_parent( unsigned index ) const {
  return index / 2;
}

template <class T>
int Heap<T>::_minChild( unsigned index ) const {
  unsigned left = index * 2;
  unsigned right = (index * 2) + 1;
  
  if (right > size_) {
    // No right child exists
    return left;
  } else if (item_[left] <= item_[right] ) {
    return left;
  } else {
    return right;
  }
}

```
