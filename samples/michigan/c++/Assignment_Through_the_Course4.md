
#### Unordered Data Structures in C++ (Week1) submission task

##### Unordered Map
・AAA.<br>

**UnorderedMapCommon.h**<br>
```
#pragma once

include <string>






include <iostream>

#include "GenericTree.h"

static void treeFactory(GenericTrr<int>& tree) {
  tree.clear();
  tree.createRoot(4);
  auto rt = tree.getRootPtr();
  auto fstChild = rt->addChild(8);
  rt->addChild(15);
  auto gsonFstChild = fstChild->addChild(16);
  fstChild->addChild(23);
  gsonFstChild->addChild(42);
}

// rootからlevel(heightの高さ)に沿って左ノードからvectorに格納する(後のcomplete treeでも使用する)
template <typename T>
std::vector<T> traverseLevels(GenericTree<T>& tree) {

  using TreeNode = typename GenericTree<T>::TreeNode; // For the convinience, define a type alias for the appropriate TreeNode dependent type. Now we can refer to a pointer to a TreeNode.
  
  std::vector<T> results;
  
  auto rootNodePtr = tree.getRootPtr();
  if (!rootNodePtr) return results;
  
  std::queue<const TreeNode*> nodesToExplore;
  const TreeNode* rootNode = rootNodePtr;
  nodesToExplore.push(rootNode);
  results.push_back(rootNode->data);

  while (!nodesToExplore.empty()) {
    const TreeNode* cur = nodesToExplore.front();
    nodesToExplore.pop();

    for (auto it = cur->childrenPtrs.begin(); it != cur->childrenPtrs.end(); it++) {
      const TreeNode* childPtr = *it;
      if (!childPtr) continue;

      nodesToExplore.push(childPtr);
      results.push_back(childPtr->data);
    }
  }  
  
  return results;
}
```

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

//
// 以下のPrintメソッドが実際に coutで出力する情報を書き出している。
//
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
      else {
        std::cerr << "[null]" << std::endl;
      }
    }
  
    delete curNode; // Delete the current node pointer
    curNode = nullptr;
  }
  
  if (targetingWholeTreeRoot) { // If we deleted the root node of this class instance, then reset the root pointer.
    rootNodePtr = nullptr;
  }
  
  return;
}

template <typename T>
void GenericTree<T>::compress() {
  if (!rootNodePtr) return;
  std::queue<TreeNode*> nodesToExplore;
  nodesToRxplore.push(rootNodePtr);
  while (!nodesToExplore.empty()) {
    TreeNode* frontNode = nodesToExplore.front();
    nodesToExplore.pop();
    if (!frontNode) {
      throw std::runtime_error("Error: Compression exploration queued a null pointer");
    }
    std::vector<TreeNode*> compressedChildrenPtrs;
    for (auto childPtr : frontNode->childrenPtrs) {
      if (childPtr) {
        compressedChildrenPtrs.push_back(childPtr);
        nodesToExplore.push(childPtr);
      }
    }
    // the std::vector::swap() function lets us replace the node's actual children pointer
    // vector with the new one, even though the compressed one is a local variable here. The
    // standard template library swaps the internals of the two structures so that the old
    // one expires here at local space, while the new one lives on with our node.
    frontNode->chldrenPtrs.swap(compressedChildrenPtrs);
  }

}

template <typename T>
std::ostream& GenericTree<T>::Print(std::ostream& os) const {
  const TreeNode* rootNodePtr = this->rootNodePtr;
  if (nullptr == rootNodePtr) {
    return os << "[empty tree]" << std::endl;
  }
  
  std::stack<const TreeNode*> nodesToExplore;
  nodesToExplore.push(rootNodePtr);
  
  std::stack<int> depthStack;
  depthStack.push(0);
  
  std::stack<std::vector<bool>> curMarginStack;
  curMarginStack.push( std::vector<bool>() ); // Each entry set to true means to display a vertical branch simbol; false meant to a blank space.
  
  std::stack<std::vector<bool>> trailingMarginStack;
  trailingMarginStack.push( std::vector<bool>() );
  
  While (!nodesToExplore.empty()) {
    const TreeNode* curNode = nodesToExplore.top();
    nodesToExplore.pop();
    
    int curDepth = depthStack.top();
    depthStack.pop();
    
    std::vector<bool> curMargin = curMarginStack.top();
    curMarginStack.pop();
    
    std::vector<bool> trailingMargin = trailingMarginStack.top();
    trailingMarginStack.pop();
    
    if (showDebugMessages) {
      os << "Depth: " << curDepth;
      std::cerr << " Data: ";
      if (curNode) {
        std::cerr << curNode->data << std::endl;
      }
       else {
        std::cerr << "[null]" << std::endl;       
       }
    }
    else {
      /* Print the tree as vertical art.
         Display two rows for each node: The first row adds vertical space
         for clarity (while continuing the trailing stems), and the second
         row displays the actual data item on a horizontal stem. */
         
      constexpr int LAST_ROW = 2;
      for (int row = 1; row <= LAST_ROW; row++) {
        // Iterate forward through the margin display flags to fill in the margin.
        for (auto stemIt = curMargin.begin(); stemIt != curMargin.end(); stemIt++) {
          bool showStem = *stemIt;
          std::string stemSymbol = "|";
          if (!showStem) {
            stemSymbol = " ";
          }
          
          bool isLastCol = false;
          if (stemIt + 1 == curMargin.end()) {
            isLastCol = true;
          }
          
          if (isLastCol) {
            if (LAST_ROW == row) {
              os << stemSymbol << "_ "; // The stem before the data item should be "|_ " in effect.
            }
            else if (showStem) {
              os << stemSymbol << std::endl;
            }
            else {
              os << std::endl;
            }
          }
          else {
            os << stemSymbol << " ";
          }
          
          // Bottom of loop for margin stems
        }
        
        // Bottom of loop for multi-row display
      }
      
      // At the end of the secong row, output the data.
      // The root node data is displayed alone on the first line correctly.
      if (curNode) {
        os << curNode->data << std::endl;
      }
      else {
        os << "[null]" << std::endl;
      }
    }
    
    // If the node has any children...
    if (curNode && curNode->childrenPtrs.size() > 0) {
      // iterate over childrenPtrs in reverse order.
      // When we do it++, we do iterate in the reverse direction correctly.
      for (auto it = curNode->childrenPtrs.rbegin(); it != curNode->childrenPtrs.rend(); it++) {
        const TreeNode* childPtr = *it;
        nodesToExplore.push(childPtr);
        
        depthStack.push(curDepth+1); // Record the depth.
        
        auto nextMargin = trailingMargin; // Prepare a working copy of the margin.
        
        // All nodes get an extra stem symbol next to their printout.
        nextMargin.push_back(true);
        curMarginStack.push(nextMargin);
        
        // But the trailing margin is an excepthion. Since it's displayed lowest it need to be blank.
        auto nextTrailingMargin = trailingMargin;
        if (curNode->childrenPtrs.rbegin() == it) {
          // This is the rightmost child, leave a blank trailing.
          nextTrailingMargin.push_back(false);
        }
        else {
          // Other children leave a vertical stem symbol trailing in the margin.
          nextTrailingMargin.push_back(true);
        }
        trailingMarginStack.push(nextTrailingMargin);
      }
    }
  }
  
  return os;
}

```
