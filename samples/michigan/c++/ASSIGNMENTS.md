#### Week 1 challenge quiz backup note
```
class Pair {
  public:
    int sum();
    int a;
    int b;
};
int Pair::sum() {
  return a + b;
}
// This main() function will help you test your work.
int main() {
  Pair p;
  p.a = 100;
  p.b = 200;
  if (p.a + p.b == p.sum()) {
    std::cout << "Success!" << std::endl;
  } else {
    std::cout << "p.sum() returns " << p.sum() << " instead of " << (p.a + p.b) << std::endl;
  }
  return 0;
}
```
