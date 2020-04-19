
#### Object-Oriented Data Structures in C++ (Week4) submission task

**LinkedListExcercises.h**<br>
```
#pragma once // Prevent the header from being included more than once per cpp file.

include <iostream>
include <string>

#include "LinkedList.h"

template <typename T>
void LinkedList<T>::insertOrdered(const T& newData) {

}

template <typename T>
LinkedList<T> LinkedList<T>::merge(const LinkedList<T>& other) const {
  LinkedList<T> left = *this;
  LinkedList<T> right = other;
  
  LinkedList<T> merged;
  
  return merged;
}

```

```
/*  When functions do not modify
an argument, the argument is marked const accordingly; entire functions are also tagged const when they do
not modify the current class object itself. */

// Notes about "const":

// Marking member functions as "const" at the end of their prototype means
// they can not change the class instance. This is useful to keep yourself
// from making a mistake, but also required when a class instance is passed
// as a reference to a const type in a function. In that case, the class
// instance is basically marked "read only", and you can only use its member
// functions that are const.

// When using a pointer to iterate over const objects, you might use a syntax
// like this:
//
// const Node* thisCur = head_;
//
// The type here is pointer to const Node, so the pointer may change to
// point to a different Node, but the actual Nodes that it points to may
// not be edited through it.

// It's important for you to think about what can or should be marked "const"
// and do so as soon as possible as you design. This is called "const
// correctness". https://isocpp.org/wiki/faq/const-correctness
```

**LinkedList.h**<br>
```
#pragma once

#include <stdexcept> // for std::runtime_error
#include <iostream> // for std::cerr, std::cout
#include <ostream> // for std::ostream

// LinkedList class: Adoubly-linked list.
template <typename T>
class LinkedList {
  public:
      class Node {
          Node* next;
          Node* prev;
          T data;
          Node() : next(nullptr), prev(nullptr) {} // Default constructor
          Node(const T& dataArg) : next(nullptr), prev(nullptr), data(dataArg) {} // Argument constructor(the data should be copied.)
          Node(const Node& other) : next(nullptr), prev(nullptr), data(other.data) {} // Copy constructor
      
          // Copy assignment operator
          Node& operator=(const Node& other) {
            next = other.next;
            prev = other.prev;
            data = other.data;
            return *this;
          }
          ~Node() {}
      };
  private:
      Node* head_;
      Node* head_;
      int size_;
    
  public:
    static constexpr char LIST_GENERAL_BUG_MESSAGE[] = "[ERROR] Probable causes: wrong head_ or tail_ pointer, or some next or prev pointer not updated, or wrong size_";
    Node* getHeadPtr() { return head_; }
    Node* getTailPtr() { return tail_; }
    int size() const { return size_; }
    bool empty() const { return !head_; }
    
    // Return a reference to the actual front data.
    T& front() {
      if (!head_) {
        throw std::runtime_error("front() called on empty LinkedList");
      }
      else {
        return head_->data;
      }
    }

    // Instanceがconstの場合の為にoverloadしたfront()。
    const T& front() const {
      if (!head_) {
        throw std::runtime_error("front() called on empty LinkedList");
      }
      else {
        return head_->data;
      }      
    }

    T& back() {
      if (!tail_) {
        throw std::runtime_error("back() called on empty LinkedList");
      }
      else {
        return tail_->data;
      }
    }

    const T& back() const {
      if (!tail_) {
        throw std::runtime_error("back() called on empty LinkedList");
      }
      else {
        return tail_->data;
      }
    }
    
    void pushFront(const T& newData); // Push a copy of the new data item onto the front of the list.
    void pushBack(const T& newData);
    void popFront(); // Delete the front item.
    void popBack();
    void clear() { // Delete all items.
      while (head_) {
        popBack();
      }
    
      if (0 != size_) throw std::runtime_error(std::string("Error in clear: ") + LIST_GENERAL_BUG_MESSAGE);
    }
    
    bool equals(const LinkedList<T>& other) const;
    bool operator==(const LinkedList<T>& other) const {
      return equals(other);
    }
    bool operator!=(const LinkedList<T>& other) const {
      return !equals(other);
    }
    // This requires that the data type T supports stream output iteself. This is used by the operator<< overload defined in this file.
    std::ostream& print(std::ostream& os) const;

    // Assuming the list was previously sorted.
    void insertOrdered(const T& newData);
    bool isSorted() const;
    
    // The insertion sort algorythm thar relies on insertOrdered. This is not an efficient operation; insertion sort is O(n^2).
    LinkedList<T> insertionSort() const;
    LinkedList<LinkedList<T>> splitHalves() const;
    // [1,2,3] => [[1],[2],[3]]
    LinkedList<LinkedList<T>> explode() const;
    
    // Assuming this list instance is currently sorted, and the "other" list is also already sorted.
    LinkedList<T> merge(const LinkedList<T>& other) const;
    LinkedList<t> mergeSort() const; // wrapper function that call one of either mergeSortRecursive or mergeSortIterative.
    LinkedList<T> mergeSortRecursive() const; // return a new list containing the sorted elements of the current list in O(n log n) time.
    LinkedList<T> mergeSortIterative() const; // return a new list containing the sorted elements of the current list in O(n log n) time.
    
    LinkedList() : head_(nullptr), tail_(nullptr), size_(0) {} // Default constructor
    LinkedList<T>& operator=(const LinkedList<T>& other) { // Copy assignment operator
      // clear the current list.
      clear();
      
      // walk along the other list and push copies of its data.
      const Node* cur = other.head_;
      while (cur) {
        pushBack(cur->data);
        cur = cur->next;
      }
      
      return *this;
    }
    LinkedList(const LinkedList<T>& other) : LinkedList() { // Copy constructor(`*this = other` does copy assignment(look at abobe).)
      *this = other;
    }
    ~LinkedList() {
      clear();
    }
    
    bool assertCorrectSize() const;
    bool assertPrevLinks() const;        
}

// =======================================================
// Implementation section
// =======================================================

// Operator overload stream output like std::cout
template <typename T>
std::ostream& operator<<(std::ostream& os, const LinkedList<T>& list) {
  return list.print(os)
}

// In some versions of C++ we have to redeclare a constant static member at global scope like this to ensure linker doesn't give an error.
template <typename T>
constexpr char LinkedList<T>::LIST_GENERAL_BUG_MESSAGE[];

// Push a copy of new data item onto the front.
template <typename T>
void LinkedList<t>::pushFront(const T& newData) {
  Node* newNode = new Node(newData);
  if (!head_) {
    head_ = newNode;
    tail_ = newNode;
  }
  else {
    Node* oldHead = head_;
    oldHead->prev = newNode;
    newNode->next = oldHead;
    head_ = newNode;
  }
  size_++;
}

template <typename T>
void LinkedList<T>::pushBack(const T& newData) {
  Node* newNode = new Node(newData);
  
  if (!head_) {
    head_ = newNode;
    tail_ = newNode;
  }
  else {
    Node* oldTail = tail_;
    oldTail->next = newNode;
    newNode->prev = oldTail;
    tail_ = newNode;
  }
  size_++;
}

template <typename T>
void LinkedList<T>::popFront() {
  if (!head_) return;
  
  // 最後の１つの場合
  if (!head_->next) {
    delete head_; // deallocate the last one.
    head_ = nullptr;
    tail_ = nullptr;
    size_--;
    if (0 != size_) throw std::runtime_error(std::string("Error in popFront: ") + LIST_GENERAL_BUG_MESSAGE);
    return;
  }
  
  Node* oldHead = head_;
  head_ = head_->next;
  head->prev = nullptr;
  delete oldHead; // deallocate the old head_ item
  oldHead = nullptr; // for safety.
  size_--;
}

template <typename T>
void LinkedList<T>::popBack() {
  if (!head_) return;
  // 最後の１つの場合
  if (!tail_->prev) {
    delete tail_;
    head_ = nullptr;
    tail_ = nullptr;
    size_--;
    if (0 != size_) throw std::runtime_error(std::string("Error in popBack: ") + LIST_GENERAL_BUG_MESSAGE);
    return;
  }
  
  Node* oldTail = tail_;
  tail_ = tail_->prev;
  tail_->next = nullptr;
  delete oldTail;
  ildTail = nullptr;
  size_--;
}

template <typename T>
bool LinkedList<T>::isSorted() const {
  if (size_ < 2) return true; / size 0 or 1 are sorted.
  if (!head_) throw std::runtime_error(std::string("Error in isSorted: ") + LIST_GENERAL_BUG_MESSAGE);
  
  const Node* prev = nullptr;
  const Node* cur = head_;
  while (cur->next) {
    prev = cur;
    cur = cur->next;
    if (!(prev->data <= cur->data)) {
      return false;
    }
  }
  
  return true;
}

template <typename T>
bool LinkedList<T>::equals(const LinkedList<T>& other) const { // This check runs in O(n) time.
  if (size_ != other.size_) {
    return false;
  }
  
  const Node* thisCur = head_;
  const Node* otherCur = other.head_;
  
  while (thisCur) {
    if (!otherCur) {
      throw std::runtime_error(std::string("Error in equals: ") + "otherCur missing a node or wrong item count");
    }
    
    if (thisCur->data != otherCur->data) {
      return false;
    }
    thisCur = thisCur->next;
    otherCur = otherCur->next;
  }
  
  return true;
}

// This is not an efficient operation: O(n^2)
template <typename T>
LinkedList<T> LinkedList<T>::insertionSort() const {
  LinkedList<T> result;
  
  const Node* cur = head_;
  while (cur) {
    result.insertOrdered(cur->data);
    cur = cur->next;
  }
  
  return result;
}

/*
// A different implementation of insertionSort that doesn't use pointers directly
template <typename T>
LinkedList<T> LinkedList<T>::insertionSort() const {
  // Make result list
  LinkedList<T> result;

  // Temporary working copy of original list
  LinkedList<T> temp = *this;

  // Consume the temporary copy and insert items into the result in order
  while (!temp.empty()) {
    result.insertOrdered(temp.front());
    temp.popFront();
  }

  return result;
}
*/

// This is used by the operator<< overload
template <typename T>
std::ostream& LinkedList<T>::print(std::ostream) const {
  // List format will be 【(1)(2)(3)】, etc.
  os << "【";
  Node* cur = head_;
  while (cur) {
    os << "(" << cur->data << ")";
    cur = cur->next;
  }
  os << "】";
  return os;
}

template <typename T>
LinkedList<LinkedList<T>> LinkedList<T>::splitHalves() const {
  LinkedList<LinkedList<T>> halves;
  LinkedList<T> leftHalf = *this;
  LinkedList<T> rightHalf;
  
  if (size_ < 2) {
    halves.pushBack(leftHalf);
    halves.pushBack(rightHalf);
    return halves;
  }
  
  int rightHalfLength = size_ / 2;
  for (int i=0; i<rightHalfLength; i++) {
    T dataToCopy = leftHalf.back();   // Copy a data element from right end of the left half
    rightHalf.pushFront(dataToCopy);  // to the left end of the right half.
    leftHalf.popBack();
  }
  
  halves.pushBack(leftHalf);
  halves.pushBack(rightHalf);
  
  return halves;
}

template <typename T>
LinkedList<LinkedList<T>> LinkedList<T>::explode() const {
  LinkedList<T> workingCopy = *this;
  LinkedList< LinkedList<T> > lists;
  while (!workingCopy.empty()) {
    LinkedList<T> singletonList;
    singletonList.pushBack(workingCopy.front());
    workingCopy.popFront();
    lists.pushBack(singletonList);
  }
  
  return lists;
}

template <typename T>
LinkedList<T> LinkedList<T>::mergeSortRecursive() const {
  if (size_ < 2) {
    return *this; // Return a copy of the current list.
  }
  
  // split this list into a list of two lists.
  LinkedList<LinkedList<T>> halves = splitHalves();
  LinkedList<T>& left = halves.front();
  LinkedList<T>& right = halves.back();
  // recurse on each of the two halves.
  left = left.mergeSortRecursive();
  right = right.mergeSortRecursive();
  return left.merge(right);
}

template <typename T>
LinkedList<T> LinkedList<T>::mergeSortIterative() const {
  if (size_ < 2) {
    return *this;
  }
  LinkedList<LinkedList<T>> workQueue = explode();
  
  while(workQueue.size() > 1) {
    LinkedList<T> left = workQueue.front():
    workQueue.popFront();
    LinkedList<T> right = workQueue.front():
    workQueue.popFront();
    LinkedList<T> merged = left.merge(right);
    workQueue.pushBack(merged);
  }
  return workQueue.front();
}

// This is a wrapper function that calls one of either mergeSortRecursive or mergeSortIterative.
template <typename T>
LinkedList<T> LinkedList<T>::mergeSort() const {
  return mergeSortRecursive();
}

template <typename T>
bool LinkedList<T>::assertCorrectSize() const {
  int itemCount = 0;
  const Node* cur = head_;
  while (cur) {
    itemCount++;
    cur = cur->next;
  }
  if (itemCount != size_) throw std::runtime_error(std::string("Error in assertCorrectSize: ") + LIST_GENERAL_BUG_MESSAGE);
  else return true;
}

template <typename T>
bool LinkedList<T>::assertPrevLinks() const {
  LinkedList<const Node*> forwardPtrList;
  LinkedList<const Node*> reversePtrList;
  {
    const Node* cur = head_;
    while (cur) {
      forwardPtrList.pushBack(cur);
      cur = cur->next;
    }
  }
  {
    const Node* cur = tail_;
    while (cur) {
      reversePtrList.pushFront(cur);
      cur = cur->prev;
    }
  }
  if (forwardPtrList == reversePtrList) return true;
  else throw std::runtime_error(std::string("Error in assertPrevLinks: ") + LIST_GENERAL_BUG_MESSAGE);
}
#include "LinkedListExcercise.h"

```

To compile and run the main program:<br>
```
make clean && make && ./main
```
To compile and run the test suite:<br>
```
make clean && make test && ./test
```
To compile and run the benchmarks:<br>
```
make clean && make test && ./test [bench]
```
