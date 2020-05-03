
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
  GridGraph graph1;
  graph1.insertEdge(IntPair(2,2), IntPair(1,2));
  graph1.insertEdge(IntPair(2,2), IntPair(2,1));
  graph1.plot(std::cout);
  std::cout << std::endl;
  graph1.printDetails(std::cout);
  std::cout << std::endl;

  GridGraph graph2 = graph1;
  graph2.removeEdge(IntPair(2,2), IntPair(2,1));
  graph1.plot(std::cout);
  std::cout << std::endl;
  graph1.printDetails(std::cout);
  std::cout << std::endl;

  GridGraph expectedGraph;
  expectedGraph.insertEdge(IntPair(2,2), IntPair(1,2))
  expectedGraph.insertPoint(IntPair(2,1));
  if (graph2 != expectedGraph) {
    throw std::runtime_error("The code did not remove the edge correctly.");
  }
}

void removePointTester() {
  GridGraph edgelessGraph; // Edge: Vertexを結ぶ線
  edgelessGraph.insertPoint(IntPair(1,2));
  edgelessGraph.insertPoint(IntPair(2,1));

  GridGraph graph;
  graph.insertEdge(IntPair(2,2), IntPair(1,2));
  graph.insertEdge(IntPair(2,2), IntPair(2,1));
  graph.removePoint(IntPair(2,2));
  if (graph.hasPoint(IntPair(2,2))) {
    errorReaction("The point (2,2) is still present.");
  }

  for (const auto & kv : graph.adjacencyMap) {
    IntPair k = kv.first;
    if (k == IntPair(2,2)) {
      errorReaction("Found (2,2) listed as an adjacent point somewhere.");
    }
  }
  if (graph != edgelessGraph) {
    errorReaction("The point removal should only remove the point and all the adjacency records that refer to it.");
  }

  GridGraph backupGraph = graph;

  graph.removePoint(IntPair(7,7));
  if (graph == backupGraph) {
    std::cout << "Good, the graph was unchanged by this operation." << std::endl << std::endl;
  }
}

void countEdgeTester() {
  GridGraph graph;
  graph1.insertEdge(IntPair(0,0), IntPair(0,1));
  graph1.insertEdge(IntPair(0,2), IntPair(0,1));
  graph1.insertEdge(IntPair(0,1), IntPair(1,1));
  graph1.insertEdge(IntPair(2,1), IntPair(1,1));
  graph1.insertEdge(IntPair(2,1), IntPair(3,1));
  graph1.insertEdge(IntPair(3,0), IntPair(3,1));
  graph1.insertEdge(IntPair(3,2), IntPair(3,1));
  graph1.plot(std::cout);
  std::cout << std::endl;
  graph1.printDetails(std::cout);
  std::cout << std::endl;

  int numEdges = graph.countEdges();
  
  if (numEdges != 7) {
    errorReaction("This isn't the correct number of edges.");
  }
}

void graphBFS_Tester() {
  GridGraph graph;
  
  std::unordered_set<IntPair> exclusionSet = {{6,0}, {0,5}};
  
  for (int row=0; row<=5; row++) {
    for (int col=0; col<=5; col++) {
      auto p1 = IntPair(row, col);
      auto p2 = IntPair(row+1, col);
      if (exclusionSet.count(p1) || exclusionSet.count(p2)) continue;
      graph.insertEdge(p1,p2);
    }    
  }

  for (int row=0; row<=6; row++) {
    for (int col=0; col<=4; col++) {
      auto p1 = IntPair(row, col);
      auto p2 = IntPair(row, col+1);
      if (exclusionSet.count(p1) || exclusionSet.count(p2)) continue;
      graph.insertEdge(p1,p2);
    }    
  }
  ... /* 省略(./tests/Catch5.md 参照。) */
}

... /* 省略(./tests/Catch5.md 参照。) */

void errorReaction(std::string msg) {
  std::cout << std::endl << "WARNING: " << msg << << std::endl << std::endl;
}
std::string makeHeader1(std::string msg) {
  ...
}
std::string makeHeader2(std::string msg) {
  ...
}
```

**GraphSearchCommon.h**<br>
```
#pragma once

#include <string>
#include <vector>
#include <list>
#include <queue>
#include <Utility>    // std::pair
#include <unordered_map>
#include <cstdlib>    // rand
#include <ctime>    // time

#include "IntPair2.h"
#include "GridGraph.h"
#include "PuzzleState.h"

std::list<IntPair> graphBFS(const IntPair & start, const IntPair & goal, const GridGraph & graph);
std::list<IntPair> puzzleBFS(const PuzzleState & start, const PuzzleState & goal);
```

**GraphSearchExcercises.cpp**<br>
```

```

**GridGraph.h**<br>
```

```

**aa.h**<br>
```

```

