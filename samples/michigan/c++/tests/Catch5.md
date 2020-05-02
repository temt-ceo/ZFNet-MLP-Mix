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
    REQUIRE(numEdges != 14);
  }

  SECTION("Should count 7 edges") {
    REQUIRE(numEdges == 7);
  }
}

```
