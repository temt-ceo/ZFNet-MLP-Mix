
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


**テストユーティリティのためにバージョンを確認**<br>
```
g++ --version
> Configured with: --prefix=/Library/Developer/CommandLineTools/usr --with-gxx-include-dir=/usr/include/c++/4.2.1
> Apple LLVM version 8.0.0 (clang-800.0.42.1)
> Target: x86_64-apple-darwin15.6.0
> Thread model: posix
> InstalledDir: /Library/Developer/CommandLineTools/usr/bin

make --version
> GNU Make 3.81
> Copyright (C) 2006  Free Software Foundation, Inc.
> This is free software; see the source for copying conditions.
> There is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A
> PARTICULAR PURPOSE.
> 
> This program built for i386-apple-darwin11.3.0

```

**GraphSearchExcercises.cpp**<br>
```
#include "GraphSearchCommon.h"

// Excercise 1(countEdges & removePoint)
int GridGraph::countEdges() const {
  int numEdges = 0;
  
  // EXCERCISE code here.
  // adjacencyMapはunordered_mapで出来ている
  // neighborSetはunordered_setで出来ている
  //  using NeighborSet = std::unordered_set<IntPair>;
  //  std::unordered_map<IntPair, GridGraph::NeighborSet> adjacencyMap;
  // 求めるべきedgeの数：GridGraph::NeighborSetはadjacencyMapのvalue
  for (const auto & kv : adjacencyMap) {
    // (vertical,horizontal)のpointをstd::pairで維持し、紐付きのあるpoint(edge)をunprdered_setで管理している
    const auto & thePoint = kv.first;
    const auto & edgeSet = kv.second;
    int edgeCnt = edgeSet.size();
    numEdges += edgeCnt;
  }
  numEdges = numEdges / 2; // undirectedのため行きと帰りの双方がadjacencyに保存されている。edgeは合わせて１つなので2で割る

  return numEdges;
}

void GridGraph::removePoint(const IntPair & p1) {
  if (!hasPoint(p1)) return;
  
  const GridGraph::NeighborSet originalNeighbors = adjacencyMap.at(p1);
  
  // EXCERCISE code here.
  for (auto & neighborPoint : originalNeighbors) {
    removeEdge(p1, neighborPoint);
  }
  adjacencyMap.erase(p1);
}

// Excercise 2(implement BFS)
// finding the shortest path from a start point to a goal point.
std::list<IntPair> graphBFS(const IntPair & start, const IntPair & goal, const GridGraph & graph) {
  constexpr int maxDist = 100;
  std::queue<IntPair> exploreQ;           // BFSで使うキュー
  std::unordered_map<IntPair, IntPair> pred;// 先駆者vertexを格納=> discovery edgeを格納（最短経路を求めるときに使用）
  std::unordered_map<IntPair, int> dist;  // 各Vertexの最初のPointからの距離を格納（無限ループを防ぐ）
  std::unordered_set<IntPair> visitedSet; // ラベル"visited"の代わりにsetに詰め込む(std::unordered_setの方が自前でadjacenciesを持つより楽(labelの初期化も必要ない)だから)
  std::unordered_set<IntPair> dequeueSet; // （無くても良い。無限ループを防ぐデバッグ用）
  
  if (!graph.hasPoint(start)) throw std::runtime_error("Starting point doesn't exist in graph");
  if (!graph.hasPoint(goal)) throw std::runtime_error("Goal point doesn't exist in graph");
  
  pred[start] = start;
  dist[start] = 0;
  
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
    GridGraph::NeighborSet neighbors = graph.adjacencyMap.at(curPoint); // Change this...
      
    for (auto neighbor : neighbors) {
      bool neighborWasAlreadyVisited = (visitedSet.find(neighbor) != visitedSet.end()); // Change this...
      
      if (!neighborWasAlreadyVisited) {
      
        pred[neighbor] = curPoint;   // Record that the curPoint is the predecessor of the neighbor point

        
        visitedSet.insert(neighbor);  // Add neighbor to the visited set.

        
        exploreQ.push(neighbor);     // Push neighbor into the exploration queue.

        dist[neighbor] = dist[curPoint] + 1;
        if (dist[neighbor] > maxDist) {
          tooManySteps = true;
          break;
        }
        if (neighbor == goal) {
          foundGoal = true;
          break;
        }        
      } // ebd of handling the discovered neighbor 
    }
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

// Excercise 3(solving the pazzle)
std::list<PuzzleState> puzzleBFS(const PuzzleState & start, const PuzzleState & goal) {
  constexpr int maxDist = 35;
  
  std::queue<PuzzleState> exploreQ;
  std::unordered_map<PuzzleState, PuzzleState> pred;
  std::unordered_map<PuzzleState, int> dist;
  std::unordered_set<PuzzleState> visitedSet;
  std::unordered_set<PuzzleState> dequeuedSet;

  pred[start] = start;
  dist[start] = 0;
  visitedSet.insert(start);
  exploreQ.push(start);
  
  bool foundGoal = (start == goal);
  bool tooManySteps = false;
  
  while (!exploreQ.empty() && !foundGoal && !tooManySteps) {
    auto curState = exploreQ.front();
    exploreQ.pop();
    
    bool curPointWasPreviouslyDequeued = dequeuedSet.count(curState); // 既にadjacent調査済みpointか
    if (!curPointWasPreviouslyDequeued) {
      std::cout << "puzzleBFS ERROR: Dequeued a vertex that had already been dequeued before." << std::endl
        << " If you're using visitedSet correctly, then no vertex should ever be added" << std::endl
        << " to the explore queue more than once." << std::endl << std::endl;
      return std::list<PuzzleState>(); // return an empty path now.
    }
    else {
      dequeuedSet.insert(curState);
    }
    
    // EXCERCISE code here.
    auto neighbors = curState.getAdjacentStates(); // Change this! This line is totally wrong.
    // auto it = neighbors.begin();
    // neighbors.insert(it, start);             //while (!exploreQ.empty() && !foundGoal && !tooManySteps) {となっているので不要だった

    for (auto neighbor : neighbors) {
      //std::cout << "neighbor:" << neighbor << "            ,goal:" << goal << std::endl;
      // Check whether the neighbor has already been visited.
      bool neighborWasAlreadyVisited = (visitedSet.find(neighbor) != visitedSet.end()); // Change this...
      
      if (!neighborWasAlreadyVisited) {
        pred[neighbor] = curState;
        visitedSet.insert(neighbor);
        exploreQ.push(neighbor);

        dist[neighbor] = dist[curState] + 1;
        if (dist[neighbor] > maxDist) {
          tooManySteps = true;
          break;
        }
        if (neighbor == goal) {
          foundGoal = true;
          break;
        }
      } // ebd of handling the discovered neighbor
      
    } // end of for loop
    
  } // end of while loop
  
  if (tooManySteps) {
    std::cout << "pazzleBFS warning: Could not reach goal within the maximum allowed steps." << std::endl << std::endl;
      return std::list<IntPair>();
  }
  
  if (!foundGoal) {
    std::cout << "pazzleBFS warning: Could not reach goal.(This may be expected if no path exists.)" << std::endl << std::endl;
      return std::list<IntPair>();
  }
  
  // 最短経路を(リンクド)リストで返す
  std::list<PuzzleState> path;
  auto cur = goal;
  path.push_front(cur);
  while (pred.count(cur) && pred[cur] != cur) {
    path.push_front(pred[cur]);
    cur = pred[cur];
  }
  
  return path;
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
