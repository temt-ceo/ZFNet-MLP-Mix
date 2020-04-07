#### Object-Oriented Data Structures in C++ (Week1) study memo
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

#### Object-Oriented Data Structures in C++ (Week2) study memo
```
#include <iostream>

class Pair {
  public:
    int first, second;
    void check() {
      first = 5;
      std::cout << "Congratulations! The check() method of the Pair class \n has executed. (But, this isn't enough to guarantee \n that your code is correct.)" << std::endl;
    }
};

Pair *pairFactory() {
  Pair *p =  new Pair;
  return p;
}

int main() {
  Pair *p;
  p = pairFactory();
  
  // This function call should work without crashing:
  p->check();
  
  // Deallocating the heap memory. (Assuming it was made on the heap!)
  delete p;

  std::cout << "If you can see this text, the system hasn't crashed yet!" << std::endl;  

  return 0;
}
```

#### Object-Oriented Data Structures in C++ (Week2) submission task
```
// You need to include some header(s) here!
#include <iostream>

// You need to define your main() function here!
int main() {
    std::cout << "Hello, world!" << std::endl;
    std::cout << "Greetings from Illinois!" << std::endl;
    return 0;
}

// Notes:

// If you get a compiler error saying "undefined reference to main",
// it means you haven't defined your main function correctly.

// Standard output is the system stream for normal text output on the terminal.
// One way to write to it, that you have learned, is with std::cout.

// Your main function should write the following messages to standard output:
// Hello, world!
// Greetings from Illinois!

// Also note, the autograder does not care about letter case, punctuation,
// or spacing! But it does care about spelling and the order of words!
```


#### Object-Oriented Data Structures in C++ (Week3) study memo
```
/* Class Pair has already been declared
 * as shown in the following comments:
 *
 * class Pair {
 * public:
 *   int *pa,*pb;
 *   Pair(int, int);
 *   Pair(const Pair &);
 *  ~Pair();
 * };
 *
 * Implement its member functions below.
 */
/* (Assignment1)Constructor */
Pair::Pair(int a, int b) {
  pa = new int(a);
  pb = new int(b);
}

/* (Assignment2)Copy Constructor( It should set up the newly constructed Pair as a "deep copy".) */
Pair::Pair(const Pair & other) {
  pa = new int(* other.pa);
  pb = new int(* other.pb);
}

/* (Assignment3)Destructor */
Pair::~Pair() {
  delete pa; pa = nullptr;
  delete pb; pb = nullptr;
}

int main() {
  Pair p(15,16);
  Pair q(p);
  Pair *hp = new Pair(23,42);
  delete hp;
  
  std::cout << "If this message is printed,"
    << " at least the program hasn't crashed yet!\n"
    << "But you may want to print other diagnostic messages too." << std::endl;
  return 0;
}
```
