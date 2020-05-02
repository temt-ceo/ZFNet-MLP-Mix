### widely-used C++ testing framework Catch
#### Graph Search

**week3_tests.cpp**<br>
```
#include <cstdlib>
#include <stdexcept>
#include <sstream>
#include <chrono>

#include "../uiuc/catch/catch.hpp"

#include "../GraphSearchCommon.h"

template <typename T>
void assertPtr(T* ptr) {
  if (!ptr) {
    throw std::runtime_error("Would have dereferenced a null pointer");
  }
}

template <typename T>
T& deref(T* ptr) {
  if (!ptr) {
    throw std::runtime_error("Would have dereferenced a null pointer");
  }
  else {
    return *ptr;  
  }
}

// Tests: countEdges

TEST_CASE("Testing countEdges", "[weight=1][ex1]") {

  GridGraph graph;
  
  // top of the "I"
  graph.insertEdge(IntPair(0,0), IntPair(0,1));
  graph.insertEdge(IntPair(0,2), IntPair(0,1));
  
  // vertical bar of the "I"
  graph.insertEdge(IntPair(0,1), IntPair(1,1));
  graph.insertEdge(IntPair(2,1), IntPair(1,1));
  graph.insertEdge(IntPair(2,1), IntPair(3,1));
  
  // bottom of the "I"
  graph.insertEdge(IntPair(3,0), IntPair(3,1));
  graph.insertEdge(IntPair(3,2), IntPair(3,1));
  
  int numEdges = graph.countEdges();
  
  SECTION("Should not count each edge twice by mistake") {
    REQUIRE(numEdges != 14);
  }

  SECTION("Should count 7 edges") {
    REQUIRE(numEdges == 7);
  }
}

// Tests: removePoint

TEST_CASE("Testing removePoint:", "[weight=1][ex1]") {

  // Reference graph with only two points, for use later
  GridGraph edgelessGraph;
  edgelessGraph.insertPoint(IntPair(1,2));
  edgelessGraph.insertPoint(IntPair(2,1));
  
  GridGraph graph;
  
  graph.insertEdge(IntPair(2,2), IntPair(1,2));
  graph.insertEdge(IntPair(2,2), IntPair(2,1));

  const GridGraph backupGraph = graph;
  
  SECTION("Checking that point (2,2) was removed") {
  
    graph.removePoint(IntPair(2,2));
    
    REQUIRE(!graph.hasPoint(IntPair(2,2)));
    
    // looping over key-value pairs in the adjacency map
    for(const auto & kv : graph.adjacencyMap) {
      // the key type is a point(int pair)
      IntPair k = kv.first;
      REQUIRE(k != IntPair(2,2));
      if (k == IntPair(2,2)) {
        throw std::runtime_error("Found (2,2) key in the adjacencyMap");
      }
      else {
        // the value type is a set of neighboring points that are adjacent to this point in the graph
        const GridGraph::NeighborSet & adjacencies = kv.second;
        for (const IntPair & neighbor : adjacencies) {
          REQUIRE(neighbor != IntPair(2,2));
          if (neighbor == IntPair(2,2)) {
            throw std::runtime_error("Found (2,2) listed as an adjacent point somewhere");
          }
        }
      }
    }
  }

  SECTION("Checking that no other contents were added") {
    graph.removePoint(IntPair(2,2));
    REQUIRE(graph == edgelessGraph);
  }
}

```
