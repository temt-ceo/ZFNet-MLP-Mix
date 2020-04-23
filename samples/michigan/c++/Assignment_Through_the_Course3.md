
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

template <typename T>
typename GenericTree<T>::TreeNode* GenericTree<T>::createRoot(const T& rootData) {
  if (nullptr != rootNodePtr) {
    constexpr char ERROR_MESSAGE[] = "Tried to createRoot when root already exists";
    std::cerr << ERROR_MESSAGE << std::endl;
    throw std::runtime_error(ERROR_MESSAGE);
  }
  rootNodePtr = new TreeNode(rootData);
  return rootNodePtr;
}

template <typename T>
typename GenericTree<T>::TreeNode* GenericTree<T>::TreeNode::addChild(const T& childData) {
  TreeNode* newChildPtr = new TreeNode(childData);
  newChildPtr->parentPtr = this;
  childrenPtrs.push_back(newChildPtr);
  return newChildPtr;
}

template <typename T>
void GenericTree<T>::deleteSubtree(TreeNode* targetRoot) {
  if (nullptr == targetRoot) {
    return;
  }
  // Check that the specified node to delete is in the same tree.
  {
    TreeNode* walkBack = targetRoot;
    while (walkBack->parentPtr) {
      walkBack = walkBack->parentPtr;
    }
    if (walkBack != rootNodePtr) {
      throw std::runtime_error("Tried to delete a node from a different tree");
    }
  }
  
  bool targetingWholeTreeRoot = (rootNodePtr == targetRoot);
  if (targetRoot->parentPtr) {
    bool targetWasFound = false;
    for (auto& currentChildPtr : targetRoot->parentPtr->childrenPtrs) {
      if (currentChildPtr == targetRoot) {
        currentChildPtr = nullPtr;
        targetWasFound = true;
        break;
      }
    }

    if (!targetWasFound) {
      constexpr char ERROR_MESSAGE[] = "Target node to delete was not listed as a child of its parent";
      std::cerr << ERROR_MESSAGE << std::endl;
      throw std::runtime_error(ERROR_MESSAGE);
    }
  }
  
  // Now, we need to make sure all the descendents get deleted.
  std::stack<TreeNode*> nodesToExplore;
  std::stack<TreeNode*> nodesToDelete;
  nodesToExplore.push(targetRoot);
  
  while (!nodesToExplore.empty()) {
    TreeNode* curNode = nodesToExplore.top();
    
    nodesToExplore.pop();
    
    if (showDebugMessages) {
      std::cerr << "Exploring node: ";
      if (curNode) {
        std::cerr << curNode->data << std::endl;
      }
      else {
        std::cerr << "[null]" << std::endl;      
      }
    }
  
    if (!curNode) { // If nullptr...
      continue;
    }

    nodesToDelete.push(curNode);

    for (auto childPtr : curNode->childrenPtrs) {
      nodesToExplore.push(childPtr);
    }
  }
  
  while (!nodeToDelete.empty()) {
    TreeNode * curNode = nodesToDelete.top();
    nodesToDelete.pop();
    
    if (showDebugMessages) {
      std::cerr << "Deleting node: ";
      if (curNode) {
        std::cerr << curNode->data << std::endl;
      }
    }
  }
}


```
