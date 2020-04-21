#### Ordered Data Structures in C++ (Week1) study memo
stackとqueueを逆順にする。

```
#include <iostream>
#include <stack>
#include <queue>
#include <vector>

std::stack<int> reverse_stack(std::stack<int> s) {
  std::stack<int> reversed_s;
  
  int stack_size = s.size();
  for (int i = 0; i < stack_size; i++) {
    reversed_s.push(s.top());
    s.pop();
  }
  
  return reversed_s;
}

std::queue<int> reverse_queue(std::queue<int> q) {
  std::queue<int> reversed_q;
  
  std::vector<int> stores{};
  int queue_size = q.size();
  for (int i = 0; i < queue_size; i++) {
    stores.push_back(q.front());
    q.pop();
  }
  for (int i = queue_size; i > 0; i--) {
    reversed_q.push(stores[i - 1]);
  }
  
  return reversed_q;
}

void print_stack(std::string name, std::stack<int> s) {
  std::cout << "stack " << name << ": ";
  while (!s.empty()) {
    std::cout << s.top() << " ";
    s.pop();
  }
  std::cout << std::endl;
}

void print_queue(std::string name, std::queue<int> s) {
  std::cout << "queue " << name << ": ";
  while (!q.empty()) {
    std::cout << q.front() << " ";
    q.pop();
  }
  std::cout << std::endl;
}

int main() {
  std::stack<int> s, rs;
  std::queue<int> q, rq;
  
  s.push(1); s.push(2); s.push(3); s.push(4); s.push(5);
  
  print_stack("s", s);
  
  rs = reverse_stack(s);
  
  print_stack("reversed s", rs);
  
  q.push(1); q.push(2); q.push(3); q.push(4); q.push(5);
  
  print_queue("q", q);
  
  rq = reverse_queue(q);
  
  print_queue("reversed q", rq);
  
  return 0;
}
```

#### Ordered Data Structures in C++ (Week3) study memo
NodeとNode2紐づくノード数をカウントする。

```
int count(Node *n) {
  // Implement count() here.
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
