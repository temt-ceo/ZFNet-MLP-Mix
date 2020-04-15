## Ordered Data Structures in C++
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
#include <vector>

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
  std::cout << "Capacity:" << values2.capacity() << std::endl; // -> 18(vectorのサイズをそのまま増やしている)

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

#### Estimate Arrays and List Operations Speeds
```
Access:
  Array: O(1)      O...Operation
  List: O(n)       ...([n]でアクセスする時に)n倍時間がかかる
Find Data:
  Array: O(n)      ...ソートされているなら、Binary Search(中央から探索する手段)が可能なのでより早い
  List: O(n)       ...ソートされていても、Binary Search出来ない（処理が効率化されない）
 ↑↑Arrayが優れている↑↑
 ↓↓Listが優れている↓↓
Insert After(任意の後にinsert):
  Array: O(n)      ...遅い
  List: O(1)       ...早い
Delete After:
  Array: O(n)
  List: O(1)


  
```
