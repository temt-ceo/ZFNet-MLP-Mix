#### Ordered Data Structures in C++ (Week3) study memo
stackとqueueを逆順にする。

```
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <string>

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

int main() {
  Node *n = new Node();
  n->left = new Node();
  n->right = new Node();
  n->right->left = new Node();
  n->right->right = new Node();
  n->right->right->right = new Node();

  // This should print a count of six nodes
  std::cout << count(n) << std::endl;

  // Deleting n is sufficient to delete the entire tree
  // because this will trigger the recursively-defined
  // destructor of the Node class.
  delete n;
  n = nullptr;

  return 0;
}
```

#### Ordered Data Structures in C++ (Week4) study memo

```
```
