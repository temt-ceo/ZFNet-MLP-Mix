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

int computeHeight(Node *n) {
  // Implement computeHeight() here.
  static int counter = 0;
  if (!n) {
    return 0;
  }
  counter++;
  count(n->left);
  count(n->right);

  return counter;
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
