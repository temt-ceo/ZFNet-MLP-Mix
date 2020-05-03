
#### Unordered Data Structures in C++ (Week3) submission task

#### Graph Search Project
・ "Adjacency List" graph data structure(rather than using pointer, use value copies of simple ofject)<br>
・ GridGraph class: use a combination of std::unordered_map, std::unordered_set and std::pair.<br>
・ Implementing graoh internals and running a breath-first search(BFS) on a GridGraph.<br>

#### std::unordered_set
・ (requirement:)no duplicates; inserting the same item twice has no effect.<br>
・ In C++, the unordered set uses **hashing** internally, so the key type must support == operator as well as std::hash.<br>
・ Unlike the unordered map type, the keys for an unordered set do not have associated values.<br>

#### std::set
・ (ordered type) based on a binary tree implementation instead of **hashing**. Uses < and == operators to arrange items.<br>
・ appropriate when we need to maintain data in order.<br>

#### std::vector
・ This is also possible use as a general purpose ordered set.<br>
・ Other "set" data structures like the disjoint-sets have specific use cases, there are no STL classes.<br>

#### Initialization with type inference
・ In C++11 and newer, there are compiler features that can infer the correct types automatically.<br>

#### Graph search algorithms overview
・ Not ordered<br>
・ No obvious start<br>
・ breadth-first search(BFS): find the shortest paths from one vertex to other vetices in the graph.<br>



**main.cpp**<br>
```
#include <iostream>
#include <string>


#include "GraphSearchCommon.h"

void errorReaction(std::string msg);
std::string makeHeader1(std::string msg); // Header formatting for neat output in the terminal
std::string makeHeader2(std::string msg);

// Function prototypes(Definitions are below.)
void demoRunner();
void removeEdgeTester();
void removePointTester();
void countEdgesTester();
void graphBFS_Tester();
void graphBFS_Tester_NoPath();
void graphBFS_Tester_StartAtGoal();
void puzzleBFS_Tester();
void puzzleBFS_Tester_NoPath();
void puzzleBFS_Tester_StartAtGoal();
void puzzleBFS_Tester_Random();

int main() {
  srand(time(0)); // Initialize seed the randomness for subsequent rand() calls.
  
  constexpr bool showWelcome = true;
  constexpr bool showDemonstrations = false;
  constexpr bool doEx1Tests = false;
  constexpr bool doEx2Tests = false;
  constexpr bool doEx3Tests = false;
  constexpr bool doEx3SlowTests = true;
  
  GridGraph::allowPlotting =true;
  PuzzleState::allowPlotting =true;
  GridGraph::allowVerboseTextDescription =true;
  
  if (showWelcome) {
    std::cout << ... << std::endl;
  }
  else {
    std::cout << makeHeader2("Skipping welcome message.") << std::endl << std::endl;
  }
  
  if (showDemonstrations) {
    demoRunner();
  }
  
  if (doEx1Tests) {
    std::cout << makeHeader1("Excercise 1 tests") << std::endl << std::endl;
    countEdgesTester();
    removeEdgeTester();
    removePointTester();
  }
  else {
    std::cout << makeHeader2("Skipping tests for Excercise 1.") << std::endl << std::endl;
  }
  
  if (doEx2Tests) {
    std::cout << makeHeader1("Excercise 2 tests") << std::endl << std::endl;
    graphBFS_Tester_NoPath();
    graphBFS_Tester_StartAtGoal();
    graphBFS_Tester();
  }
  else {
    std::cout << makeHeader2("Skipping tests for Excercise 2.") << std::endl << std::endl;
  }
  
  if (doEx3Tests) {
    std::cout << makeHeader1("Excercise 3 tests") << std::endl << std::endl;
    if (doEx3SlowTests) {
      puzzleBFS_Tester_NoPath();
    }
    puzzleBFS_Tester_StartAtGoal();
    puzzleBFS_Tester();
    puzzleBFS_Tester_Random();
  }
  else {
    std::cout << makeHeader2("Skipping tests for Excercise 3.") << std::endl << std::endl;
  }
  
  return 0;
}

void demoRunner() {
  {
    std::unordered_set<int> favoriteNumbers;
    favoriteNumbers.insert(7);
    favoriteNumbers.insert(42);
    std::cout << "size() is now: " << favoriteNumbers.size() << std::endl;
    favoriteNumbers.insert(42);
    std::cout << "size() is now: " << favoriteNumbers.size() << std::endl;
    
    for (int x : favoriteNumbers) {
      std::cout << "Set contains: " << x << std::endl;    
    }
    std::cout << std::endl;
    
    std::cout << "Counting the key 7: " << favoriteNumbers.count(7) << std::endl;
    favoriteNumbers.erase(7);
    std::cout << "Counting the key 7: " << favoriteNumbers.count(7) << std::endl;
  }
  {
    IntPair pointA = {2,3};
    IntPair pointB = {1,3};
    IntPairPair AB = {pointA, pointB};
    IntPairPair BA = {pointB, pointA};

    std::cout << "Now is (AB == BA)? " << ((AB == BA) ? "true" : "false") << std::endl;

    std::hash<IntPairPair> ippHasher;
    std::cout << "Hashing AB: " << ippHasher(AB) << std::endl;
    std::cout << "Hashing BA: " << ippHasher(BA) << std::endl;
  }
  {
    IntPair point1 = IntPair(1,2);
    IntPair point2 = std::make_pair(1,2);
    auto point3 = IntPair(1,2);
    IntPair point4 = {1,2};
    
    std::cout << "Displaying print output for all objects:" << point1 << point2 << point3 << point4 << std::endl;
  }
}

void removeEdgeTester() {
  {
  
  }
}

```

**UnorderedMapCommon.h**<br>
```

```

**UnorderedMapCommon.cpp**<br>
```

```

