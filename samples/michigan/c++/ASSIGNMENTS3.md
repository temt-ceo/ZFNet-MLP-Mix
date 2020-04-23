#### Ordered Data Structures in C++ (Week3) study memo
stackとqueueを逆順にする。

```
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <string>

class Node {
  public:
    int height;
    Node *left, *right;
    Node() {
      height = -1;
      left = right = nullptr;
    }
    ~Node() {
      delete left;
      left = nullptr;
      delete right;
      right = nullptr;
    }
};

#include <stack>
void computeHeight(Node *n) {

  // Implement computeHeight() here.
  static std::stack< Node* > s;

  if (n->left && n->left->height == -1) {
    // 左が未探索
    s.push( n );
    computeHeight(n->left);
  } else if (n->right && n->right->height == -1) {
    // 右が未探索
    s.push( n );
    computeHeight(n->right);
  } else if (!(n->left) && !(n->right) ) {
    // leaf nodeである
    n->height = 0;
    std::cout << "(" << n->height << ")" << std::endl;
    if ( !s.empty() ) {
      Node * parent = s.top();
      s.pop();
      computeHeight(parent);
    }
  } else {
    // 深部まで探索済み、且つ、leaf nodeではない
    int l_len = 0, r_len = 0;

    if (n->left) {
      l_len = n->left->height + 1;
    }
    if (n->right) {
      r_len = n->right->height + 1;
    }

    std::cout << "l_len: " << l_len << ", r_len" << r_len << std::endl;

    if ( l_len >= r_len ) {
      n->height = l_len;
    } else {
      n->height = r_len;
    }
    std::cout << "(" << n->height << ")" << std::endl;
    
    if ( !s.empty() ) {
      Node * parent = s.top();
      s.pop();
      computeHeight(parent);
    }
  }
  return;
}

// This function prints the tree in a nested linear format.
void printTree(const Node *n) {
  if (!n) return;
  std::cout << n->height << "(";
  printTree(n->left);
  std::cout << ")(";
  printTree(n->right);
  std::cout << ")";  
}

int main() {
  Node *n = new Node();
  n->left = new Node();
  n->right = new Node();
  n->right->left = new Node();
  n->right->right = new Node();
  n->right->right->right = new Node();

  computeHeight(n);

  printTree(n);
  std::cout << std::endl << std::endl;
  printTreeVertical(n);

  delete n;
  n = nullptr;

  return 0;
}
```

#### Ordered Data Structures in C++ (Week4) study memo

```
```
