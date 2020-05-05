
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
#include "GraphSearchCommon.h"
int GridGraph::countEdges() const {
  int numEdges = 0;
  
  // EXCERCISE code here.

  return numEdges;
}

void GridGraph::removePoint(const IntPair & p1) {
  if (!hasPoint(p1)) return;
  
  const GridGraph::NeighborSet originalNeighbors = adjacencyMap.at(p1);
  
  // EXCERCISE code here.
}

std::list<IntPair> graphBFS(const IntPair & start, const IntPair & goal, const GridGraph & graph) {
  constexpr int maxDist = 100;
  std::queue<IntPair> exploreQ;
  std::unordered_map<IntPair, IntPair> pred;
  std::unordered_map<IntPair, int> dist;
  std::unordered_set<IntPair> visitedSet;
  std::unordered_set<IntPair> dequeueSet;
  
  if (!graph.hasPoint(start)) throw std::runtime_error("Starting point doesn't exist in graph");
  if (!graph.hasPoint(goal)) throw std::runtime_error("Goal point doesn't exist in graph");
  
  pred[start] = start;
  dist[start] = start;
  
  visitedSet.insert(start);
  exploreQ.push(start);
  
  bool foundGoal = (start == goal);
  bool tooManySteps = false;
  
  while (!exploreQ.empty() && !foundGoal && !tooManySteps) {
    
    auto curPoint = exploreQ.front();
    exploreQ.pop();
    bool curPointWasPreviouslyDequeued = dequeuedSet.count(curPoint);
    if (curPointWasPreviouslyDequeued) {
      std::cout << "graphBFS ERROR" << std::endl << std::endl;
      return std::list<IntPair>();
    }
    else {
      dequeuedSet.insert(curPoint);
    }
    
    // EXCERCISE code here.
    
  }
  
  if (tooManySteps) {
    std::cout << "graphBFS warning: Could not reach goal within the maximum allowed steps." << std::endl << std::endl;
      return std::list<IntPair>();
  }
  
  if (!foundGoal) {
    std::cout << "graphBFS warning: Could not reach goal.(This may be expected if no path exists.)" << std::endl << std::endl;
      return std::list<IntPair>();
  }
  
  std::list<IntPair> path;
  auto cur = goal;
  path.push_front(cur);
  while (pred.count(cur) && pred[cur] != cur) {
    path.push_front(pred[cur]);
    cur = pred[cur];
  }
  
  return path;
}
```

**GridGraph.h**<br>
```
#pragma once
#include <iostream>
#include <stdexcept>
#include <unordered_map>
#include <unordered_set>
#include <algorithm> // std::sort
#include <vector>
#include <string>
#include <sstream>

#include "IntPair2.h"

class GridGraph {
  using NeighborSet = sed::unordered_set<IntPair>;
  
  std::unordered_map<IntPair, GridGraph::NeighborSet> adjacentMap;
  
  static bool allowPlotting;
  static bool allowVerboseTextDescription;
  
  bool checkUnitDistance(const IntPair & p1, const IntPair & p2) const {
    int dist_x = p1.first - p2.first;
    int dist_y = p1.second - p2.second;
    int dist2 = dist_x*dist_x + dist_y*dist_y;
    return (dist2 == 1);
  }
  
  void insertPoint(const IntPair & p) {
    // Just by referencing to given key, it is inserted if not already present.
    adjacencyMap[p];
  }
  
  void insertEdge(const IntPair & p1, const IntPair & p2) {
    if (!checkUnitDistance(p1,p2)) {
      std::cerr < "Error: Can't add edge from " << -1 << " to " << p2 << std::endl;
      throw std::runtime_error("Requested an invalid edge insertion");
    }
    
    adjacencyMap[p1].insert(p2);
    adjacencyMap[p2].insert(p1);
  }
  
  void removeEdge(const IntPair & p1, const IntPair & p2) {
    if (hasEdge(p1,p2)) {
      adjacencyMap[p1].erase(p2);
      adjacencyMap[p2].erase(p1);
    }
  }
  
  void removePoint(const IntPair & p1); // defigned in GraphSearchExcercises.cpp
  
  bool hasPoint(const IntPair & p) const {
    return adjacencyMap.count(p);
  }
  
  bool hasEdge(const IntPair & p1, const IntPair & p2) const {
    int directions = 0;
    if (adjacencyMap.count(p1) && adjacencyMap.at(p1).count(p2)) {
      directions++; // edgeの１方向が存在した。
    }
    if (adjacencyMap.count(p2) && adjacencyMap.at(p2).count(p1)) {
      directions++; // 往復のedgeも存在した
    }
    if (directions == 0) {
      return false;
    }
    else if (directions == 2) {
      return true;
    }
    else {
      throw std::runtime_error("hasEdge: edge found, but only in one direction");
    }
  }
  
  int countVertices() const {
    return adjacencyMap.size();
  }
  
  int countEdges() const; // excercise
  
  bool operator==(const GridGraph & other) const {
    return (adjacencyMap == other.adjacencyMap);
  }
  
  bool operator!=(const GridGraph & other) const {
    return !(*this == other); // check for inequality by delegating to the implementation of operator==.
  }
  
  std::ostream & plot(std::ostream & os) const;
  std::ostream & printDetails(std::ostream & os) const;
}

// Adds "<<" support
static inline std::ostream & operator<<(std::ostream & os, const GridGraph & graph) {
  if (GridGraph::allowPlotting) {
    return graph.plot(os);
  }
  else {
    return graph.printDetails(os);
  }
}
```

**GridGraph.cpp**<br>
```
#include "GridGraph.h"

bool GridGraph::allowPlotting = true; // Initialization of static variables.
bool GridGraph::allowVerboseTextDescription = true;

std::ostream & GridGraph::plot(std::ostream & os) const {
  if (!GridGraph::allowPlotting) return os;
  
  if (adjacencyMap.empty()) {
    return os << "Empty graph plot" << std::endl;
  }
  
  int minRow = 0;
  int minCol = 0;
  int maxRow = 0;
  int maxCol = 0;
  if (adjacencyMap.size() > 0) {
    auto firstPointKey = adjacencyMap.begin()->first; // (*iterator).first
    maxRow = minRow = firstPointKey.first;
    maxCol = minCol = firstPointKey.second;
    for (const auto & kv : adjancencyMap) {
      const auto & pointKey = kv.first;
      auto pointRow = pointKey.first;
      auto pointRow = pointKey.second;
      minRow = std::min(minRow, pointRow);
      minCol = std::min(minCol, pointCol);
      maxRow = std::max(maxRow, pointRow);
      maxCol = std::max(maxCol, pointCol);
    }
  }
  
  for (int row = minRow; row <= maxRow; row++) {
    std::stringstream understream;
    for (int col = minCol; col <= maxCol; col++) {
      IntPair pos = {row, col};
      if (hasPoint(pos)) {
        os << pos;
      }
      else {
        os << "       ";
      }
      IntPair posRight = {row, col+1};
      if (hasEdge(pos, posRight)) {
        os << "----";
      }
      else if (col < maxCol) {
        os << "    ";
      }
      if (row < maxRow) {
        IntPair posDown = {row+1, col};
      }
      if (hasEdge(pos, posDown)) {
        understream << "   |   ";
      }
      
    }
  }
}

```

