
#### Ordered Data Structures in C++ (Week3) submission task

**GenericTree.h**<br>
```
#pragma once

#include <stdexcept> // for std::runtime_error
#include <stack>
#include <queue>
#include <vector>
#include <iostream>
#include <ostream>

template <typename T>
class GenericTree {
  public:
    bool showDebugMessages;
    
    class TreeNode {
      public:
        TreeNode* parentPtr;
        std::vector< TreeNode* > childrenPtrs;
        T data; // The acctual node data.
        TreeNode* addChild(const T& childData);
        TreeNode() : parentPtr(nullptr) {} // Default constructor, Indicate that there is no parent.
        TreeNode(const T& dataArg) : parentPtr(nullptr), data(dataArg) {}
        TreeNode(const TreeNode& other) = delete; // Copy constructor, we will disable it.
        TreeNode& operator=(const TreeNode& other) = delete;
        ~TreeNode() {} // the members of the node class will have their own destructors and called automatically.        
    };
    
  private:
    TreeNode* rootNodePtr;
    
  public:
    TreeNode* createRoot(const T& rootData);
    TreeNode* getRootPtr() {
      return rootNodePtr;
    }
    void deleteSubtree(TreeNode* targetRoot); // Deallocate the entire subtree.
    void compress(); // If we used linked list for the children pointers, this wouldn't be necessary.
    GenericTree() : showDebugMessages(false), rootNodePtr(nullptr) {}
    GenericTree(const T& rootData) : GenericTree() { // Parameter constructor
      createRoot(rootData);
    }
    GenericTree(const GenericTree& other) = delete; // Copy constructor
    GenericTree& operator=(const GenericTree& other) = delete;
    
    void clear() {
      deleteSubtree(rootNodePtr);
      
      if (rootNodePtr) {
        throw std::runtime_error("clear() detected that deleteSubtree() had not reset rootNodePtr");
      }
    }
    
    ~GenericTree() {
      clear();
    }
    
    std::ostream& Print(std::ostream& os) const; // Print the tree to the output stream(e.g. cout)
};

template <typename T>
std::ostream& operator<<(std::ostream& os, const GenericTree<T>& tree) {
  return tree.Print(os);
}

// =================================================
//    Implementation section
// =================================================


```
