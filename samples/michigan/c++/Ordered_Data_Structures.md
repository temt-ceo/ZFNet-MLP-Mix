## Ordered Data Structures in C++

### Tree Structures
#### Heap Sort(ArrayをBinary Tree構造のように扱える。Sort/Insert/RemoveがDictionaryのように扱える)
1. Build Heap by O(n).(Create a complete tree of the items in any order, then call heapifyDown on every non-leaf node from the bottom of the tree up to the root.)<br>
2. n x removeMin by O(log(n))<br>
3. Swap Elements To Order(ASC/DESC)<br>
(1.のO(n)はheight+1回のみ)

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

### Linear Structures

#### Arrays
・ Elements are all the same type.<br>
・ The size of the type of data is known.(指定のIndexまでのメモリ = Index x 固定のメモリ量)<br>
・ Arrays must store their data sequentially in memory<br>
　　(The capacity of an array is the maximum number of elements that can be stored.).<br>
　　(より大きなメモリを割り当て、そこに内容物を全てコピーすることで増やすことができる。)<br>
**main.cpp**<br>
```
#include <iostream>
#include <vector> /* 幅が足りなくなると倍々に用意し中身をコピーする。（倍にすることでO(n)となり処理がより早い） */

int main() {
  // Create a fixed sized array of 9 primes:
  int values[9] = { 2, 3, 5, 7, 11, 13, 17, 19, 23 };
  Cube cubes[3] = { Cube(11), Cube(42), Cube(400) };
  std::vector<int> values2{ 2, 3, 5, 7, 11, 13, 17, 19, 23 }
  
  // Outputs the 4th prime.
  std::cout << values[3] << std::endl;
  // Print the size of each type `int`:
  std::cout << sizeof(int) << std::endl; // -> 4
  std::cout << sizeof(Cube) << std::endl; // -> 8 (bytes)

  // standard vector = dinamically growing array libraly
  std::cout << "Capacity:" << values2.capacity() << std::endl; // -> 9
  values2.push_back(23));
  std::cout << "Size:" << values2.size() << std::endl; // -> 10(現在のサイズ)
  std::cout << "Capacity:" << values2.capacity() << std::endl; // -> 18

  // the offset from the beginning of the array to [2]:
  int offset = (long)&(values[2]) - (long)&(values[0]) # &によりアドレスを取得
  std::cout << offset << "  " << (long)&(values[2]) << std::endl; // -> (2 x 4 = )8  140734619896488
  
  return 0;
}
```

**Makefile**<br>
```
EXE = main
OBJS = main.o
CLEAN_RM =

include ../../_make/generic.mk
  ↓↓↓
# Compiler/linker config and object/depfile directory:
CXX = g++
LD  = g++
OBJS_DIR = .objs

# -MMD and -MP asks clang++ to generate a .d file listing the headers used in the source code for use in the Make process.
DEPFILE_FLAGS = -MMD -MP　#   -MMD: "Write a depfile containing user headers"、 -MP : "Create phony target for each dependency (other than main file)" (https://clang.llvm.org/docs/ClangCommandLineReference.html)

# Flags for compile:
CXXFLAGS += -std=c++14 -O0 $(WARNINGS) $(DEPFILE_FLAGS) -g -c $(ASANFLAGS)

# Flags for linking:
LDFLAGS += -std=c++14 $(ASANFLAGS)

all: $(EXE) # Rule for `all` (first/default rule):

$(EXE): $(patsubst %.o, $(OBJS_DIR)/%.o, $(OBJS)) # Rule for linking the final executable - $(EXE) depends on all object files in $(OBJS)
	$(LD) $^ $(LDFLAGS) -o $@

$(OBJS_DIR): # Ensure .objs/ exists:
	@mkdir -p $(OBJS_DIR)
	@mkdir -p $(OBJS_DIR)/uiuc

$(OBJS_DIR)/%.o: %.cpp | $(OBJS_DIR) # Rules for compiling source code.
	$(CXX) $(CXXFLAGS) $< -o $@

-include $(OBJS_DIR)/*.d # Additional dependencies for object files 
-include $(OBJS_DIR)/uiuc/*.d

# Standard C++ Makefile rules:
clean:
	rm -rf $(EXE) $(TEST) $(OBJS_DIR) $(CLEAN_RM) *.o *.d

tidy: clean
	rm -rf doc

.PHONY: all tidy clean
}
```

#### Arrays
**main.cpp**<br>
```
#include <iostream>

int main() {
  // Create a fixed sized array of 9 primes:
  int values[9] = { 2, 3, 5, 7, 11, 13, 17, 19, 23 };
  
  // Outputs the 4th prime.
  std::cout << values[3] << std::endl;
  return 0;
}
```

#### Linked Memory
・ Elements are all the same type.<br>
・ A list node refers to pair of both the data and the link.<br>
・ リストの中間へのInsertが可能になる（但しarraysに比べると遅い）<br>

**List.h**<br>
```
#pragma once

template <typename T>
  class List {
    public:
      const T & operator[](unsigned index);  <- l[0] (after the List<int> l;)
      void insertAtFront(const T & data);
      
    private:
      class ListNode {
        public:
	  const T & data;
	  ListNode *next;
	  ListNode(const T & data) : data(data), next(nullptr) { }
      }
      ListNode *head_; // head pointer
  }
```

**List.cpp**<br>
```
#include "List.h"
template <typename T>
const T & List<T>::operator[](unsigned index) {
  ListNode *thru = head_; // head_=0番目の前, Start a `thru` pointer to advance thru the list
  
  // Loop until the end of the list.
  whild (index > 0 && thru->next != nullptr) {
    thru = thru->next;
    index--; // これは目的のメモリまであといくつ辿っていくかを指す
  }
  
  return thru->data; // Return the data.
}

/* 先頭にpush */
template <typename T>
void List<t>::insertAtFront(const T & data) {
  ListNode *node = new ListNode(data); // Create a new ListNode on the heap.
  node->next = head_; // Set the new node's next pointer to the current head of the List.
  head_ = node;
}
```

#### ◉ Estimate Arrays and List Operations Speeds ◉
```
Access:
  Array: O(1)      O...Operation
  List: O(n)       ...([n]でアクセスする時に)n倍時間がかかる
Find Data:
  Array: O(n)      ...ソートされているなら、Binary Search(中央から探索する手段)が可能なのでより早い
  List: O(n)       ...ソートされていても、Binary Search出来ない（処理が効率化されない）
  BST Dictionary: average case: O(lg(n)), worst case: O(n) ... Dictionaryも早いがワーストケースではArrayに劣る
       BST = Binary Search Tree
 ↑↑Arrayが優れている↑↑
 ↓↓Listが優れている↓↓
Insert After(任意の後にinsert):
  Array: O(n)      ...遅い
  List: O(1)       ...早い
  BST Dictionary: average case: O(lg(n)), worst case: O(n) ... Dictionaryも早い
Delete After:
  Array: O(n)
  List: O(1)
  BST Dictionary: average case: O(lg(n)), worst case: O(n) ... Dictionaryも早い

 ↓↓どちらもok(ややListが速度で有利だが無視できるほど)↓↓
Queue Data Structure:
  Array: O(1)
  List: O(1)  
Stack Data Structure:
  Array: O(1)
  List: O(1)  
```

#### Queue
**main.cpp**<br>
```
#include <iostream>
#include <queue>

int main() {
  // Create a std::queue:
  std::queue<std::string> q;

  // Add
  q.push("Orange"); q.push("Blue"); q.push("Illinois");
  
  std::cout << "First pop():" << q.front() << std::endl; // q.front() -> "Orange"
  q.pop();
}
```

#### Stack
**main.cpp**<br>
```
#include <iostream>
#include <stack>

int main() {
  // Create a std::queue:
  std::stack<std::string> s;

  // Add
  s.push("Orange"); s.push("Blue"); s.push("Illinois");
  
  std::cout << "First pop():" << s.top() << std::endl; // q.top() -> "Illinois"
  s.pop();
}
```

### Tree Structures
#### Binary Tree
・ Perfect = A binary tree is perfect if and only if all interior nodes have two children and leaves are at the same level.<br>
・ Complete = A binary tree is complete if and only the tree is perfect up until the last level and all leaf nodes on the last level are pushed to the left. This means there can be only one node with one child, and that child will always be the left child.<br>
・ Is a full tree complete? No(Full = 全てのノードが0または2つの子ノードを持つ。つまり１つだけの子ノードを持っていない。).<br>
・ Is a complete tree full? No(Complete:左が子ノードまでの高さが高く右が左のノードより高く無いこと。1つのノードもあり得る).<br>
・ The height of a complete binary tree is floor(lg n).<br>
・ preOrder: ルートノードをSHOUT OUTしてから子ノードをSHOUT OUTする<br>
・ inOrder: 子ノードをSHOUT OUTしてからルートノードをSHOUT OUTする<br>
・ postOrder: ルートノードを後回しにする<br>
・ levelOrder: ルートノードから左から右に降りていく<br>
**BinaryTree.h**<br>
```
#pragma once

template <typename T>
class BinaryTree {
  public:
    // ...
  private:
    class TreeNode {
      public:
        T & data;
        TreeNode *left, *right;
	TreeNode(T & data) :
	  data(data), left(nullptr), right(nullptr) { }
    };
    TreeNode *root_;
};
```

#### Binary Search Tree(BST)
・ (BST-Based)Dictionary...左側をペアレントより小さく、右側を大きくして上から探索可能にする<br>
**Dictionary.h**<br>
```
...

template <typename K, typename D>
class Dictionary {
  public:
    Dictionary();
    const D & find(const K & key);
    void insert(const K & key, const D & data);
    const D & remove(const K & key);
    
  private:
    class TreeNode {
      public:
        const K & key;
        const D & data;
        TreeNode *left, *right;
	//
	// The ":" marks an initialization list for the constructor.
	// keyにkeyを、dataにdataを、leftとrightにnullptrを渡している。（ソースコードがなくとも）-> README.mdのShape.cpp
	//
	TreeNode(const K & key, const D & data) :
	  key(key), data(data), left(nullptr), right(nullptr) { }
    };
    TreeNode *head_;
};
```

**Dictionary.cpp**<br>
```
...

template <typename K, typename D>
const D& Dictionary<K, D>::find(const K & key) {
  //
  // "TreeNode*&" is a reference to a pointer to a TreeNode. This allows the tree's functions to rewire the tree based on the pointer connections.
  // You can have pointers to pointers and references to pointers in C++.
  //
  TreeNode *& node = _find(key, head_);
  if (node == nullptr) { throw std::runtime_error("key not found"); }
  return node->data;
}

template <typename K, typename D>
typename Dictionary<K, D>::TreeNode *& Dictionary<K, D>::_find(const K & key, TreeNode *& cur) const {
  if (cur == nullptr) { return cur; }
  else if (key == cur->key) { return cur; }
  else if (key < cur->key) { return _find(key, cur->left); }
  else                     { return _find(key, cur->right); }
}

template <typename K, typename D>
void Dictionary<K, D>::insert(const K & key, const D & data) {
  TreeNode *& node = _find(key, _head);
  //if (node) { throw std::runtime_error("error: insert() used on an existing key"); }
  node = new TreeNode(key, data); // pointer by reference(*&)のおかげでこの2行目のnode = でinsertされたことになる。
}

template <typename K, typename D>
const D & Dictionary<K, D>::remove(const K & key) {
  TreeNode *& node = _find(key, head_);
  return _remove(node);
}

template <typename K, typename D>
const D & Dictionary<K, D>::_remove(TreeNode *& node) {
  // Zero child remove(子ノードが無い（一番簡単）)
  if (node->left == nullptr && node->right ==nullptr) {
    const D & data = node->data;
    delete node;
    node = nullptr;
    return data;
  }
  // One child(left) remove
  else if (node->left != nullptr && node->right == nullptr) {
    const D & data = node->data;
    TreeNode *temp = node;
    node = node->left; // swapping
    delete temp;
    return data;
  }
  // One child(left) remove
  else if (node->left == nullptr && node->right != nullptr) {
    const D & data = node->data;
    TreeNode *temp = node;
    node = node->right; // swapping
    delete temp;
    return data;
  }
  // Two-child remove
  else {
    // 削除するノードの左側で一番右のリーフノード（削除するノードに最も値が近い）を削除するノードと置き換える
    TreeNode *& iop = _iop( node->left );
    _swap( node, iop );
    return _remove(node);
  }
}

```
Github repo: https://github.com/wadefagen/coursera/tree/master/bst<br>

**main.cpp**<br>
```
...
int main() {
  Dictionary<int, std::string> t;
  t.insert(34, "thirty four");
  t.insert(19, "ninteen");
  t.insert(51, "fifty one");
  t.insert(55, "fifty five");
  t.insert(4, "four");
  t.insert(11, "eleven");
  t.insert(20, "twenty");
  t.insert(2, "two");
  
  cout << "t.find(51): " << t.find(51) << endl;
  cout << "t.remove(11): " << t.remove(11) << endl;
  cout << "t.remove(51): " << t.remove(51) << endl;
  cout << "t.remove(19): " << t.remove(19) << endl;
  cout << "t.find(51): " << t.find(51) << endl;
}

> t.find(51): fifty one
> t.remove(11): eleven (zero child remove)
> t.remove(51): fifty one (one child remove)
> t.remove(19): nineteen (two child remove)
> t.find(51): 
> Caught exception with error message: error: key not found
```


#### BST Rotations (AVL Trees)
・ L...Left Rotation.<br>
・ R...Right Rotation.<br>
・ LR...Left-Right Rotation.<br>
・ RL...Right-Left Rotation.<br>
・ Rotation run in O(1) time.<br>
**AVL.cpp**<br>
```
template <typename K, typename D>
void AVL<K, D>::_ensureBalance(TreeNode *& cur) {
  // Calculate the balance factor
  int balance = height(cur->right) - height(cur->left);
  
  // Check if the node is current not in balance
  if ( balance == -2 ) {
    int l_balance = height(cur->left->right) - height(cur->left->left);
    if (l_balance == -1) { _rotateRight( cur ); }
    else                 { _rotateLeftRight( cur ); }
  } else if ( balance == 2 ) {
    int l_balance = height(cur->right->right) - height(cur->right->left);
    if (l_balance == 1) { _rotateLeft( cur ); }
    else                 { _rotateRightLeft( cur ); }  
  }
  
  _updateHeight(cur);
}

template <typename K, typename D>
void AVL<K, D>::_rotateLeft(TreeNode *& cur) {
  // x points to the subtree root.
  TreeNode *x = cur;
  // y points to the right child.
  TreeNode *y = cur->right;
  
  // Let node x's new right child to be the old left chld of y.
  x->right = y->left;
  // Node y's new left child is x. This puts node y on top.
  y->left =x;
  // Since cur is the original tree node pointer that points to the root of this subtree,
  // we need it to now point to the new root of the subtree, which is node y.
  cur = y;
  
  _updateHeight(x);
  _updateHeight(y);
}

template <typename K, typename D>
void AVL<K, D>::_updateHeight(TreeNode *& cur) {
if (!cur) return;
  cur->height = 1 + max(height(cur->left), height(cur->right));
}

/**
* Recursive IoP remove.(高さを均等化)
*/
template <typename K, typename D>
const D& AVL<K, D>::_iodRemove(TreeNode *& node, TreeNode *& iop) {
  if (iop->right != nullptr) {
    // General case: IoP not found yet, keep going deeper.
    const D& d = _iopRemove(node, iop->right);
    if (iop) { _ensureBalance(iop); }
    return d;
  } else {
    // Base case: Found IoP, swap the location:
    _swap( node, iop );
    std::swap( node, iop );
    // Remove the swapped node
    return _remove(iop):
  }

}
```


#### B Tree
・ All keys within a node are in sorted order.<br>
・ Each node contains no more than m-1 keys.<br>
・ Each internal node can have at most m children.<br>
・ Each internal node has exactly one more child than key.<br>
・ Aroot node can be a leaf or have ［2, m］ children.<br>
・ Each non-root, internal node has ［ceil(m/2), m］ children.<br>
・ The number of seeks is no more than logm(n).<br>
**BTree.h**<br>
```
@pragma once

template <typename K>
class BTree {
  public:
    // ,,,
  
  private:
    class BTreeNode {
      public:
        K* keys_;
	unsigned keys_ct_;
	bool _isLeaf;
	
	BTreeNode(): keys_(nullptr), kes_ct_(0), isLeaf(true) { }
	bool isLeaf() const;
    };
    BTreeNode *root_;
    
    BTreeNode & _fetchChild(unsigned index);
    
    bool _exists(BTreeNode & node, const K & key);
    // ...
};

#include "BTree.hpp"
```

**BTree.h**<br>
```
#include "BTree.h"

template <typename K>
bool BTree<K>::_exists(BTree<K>::BTreeNode & node, const K & key) {
  unsigned i;
  for (i = 0; i < node.keys_ct_ && key < node.keys_[i]; i++) { } // keys_[i]がヒットした時点で止まるのでその時のiを保持して次の行に進む
  
  if ( i < node.keys_ct_ && key == node.keys_[i] ) {
    return true;
  }
  
  if ( node.isLeaf() ) {
    return false;
  } else {
    BTreeNode nextChild = node._fetchChild(i);
    return _exists(nextChild, key);
  }
  
  template <typename K>
  typename BTree<K>::BTreeNode::isLeaf() const {
    // Stub implementation
    return true;
  }
  
  template <typename K>
  typename BTree<K>::BTreeNode & BTree<K>::_fetchChild(unsigned index) {
    // Stub implementation
    return *root_;
  }
}
```
