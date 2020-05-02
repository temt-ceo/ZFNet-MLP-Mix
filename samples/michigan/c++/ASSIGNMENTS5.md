### Unordered Data Structures in C++ (Week3) study memo
Disjoint Setsを使いGraphのProblemを解く<br>
すごい誘導問題なので、コメントは必要箇所のみ記載<br>
<br>
#### undirected graph(list of edges)を使いconnected要素をカウントし、各々がcircleを構成しているか判別する。
```
/* Problem:
Suppose you are given a undirected graph specified as a list of edges. In this challenge problem, we'll use a 
simplified disjoint sets data structure to count how many "connected components" the graph has, and whether
each one contains a cycle or not.
First, some background information: In an undirected graph, two vertices have connectivity if there is any path
leading from one to the other using any number of edges. So, a lone vertex by itself, with no edges, is not
"connected" to the other parts of the graph. A "connected component" is any subset of the graph vertices where
all the vertices have paths to each other, and where that set is maximal, meaning that no reachable vertices are
left out of the set. A connected component contains a "cycle" if there are two (or more) distinct paths connecting
any two vertices--that means there is a closed loop somewhere.
Example: An edge label like "(0,1)" means an edge between vertex 0 and vertex 1. Suppose we have vertices
numbered 0 through 8, and we have these edges:
(0,1), (1,2), (0,2), (3,4), (5,6), (6,7), (7,8)
Try drawing it on a sheet of paper. The three connected components are these sets of vertices:
{0, 1, 2}, {3, 4}, {5, 6, 7, 8}
You'll see the connected components are like islands of vertices. Here, {0, 1, 2} contains a cycle, and the other
two connected components do not contain cycles. Also, notice that for a set to be a connected component, it
must be maximal, meaning no vertices can be left out--and so {0, 1} is not called a connected component,
because the 2 is also reachable there. (Maximal does not mean "maximum". A single, lone vertex is a
connected component by itself, because the subset containing only that one vertex is maximal, considering
what can be reached from it. So, the sizes of the other connected components elsewhere do not matter.)
In graph theory, it's common to say "n" for the number of vertices and "m" for the number of edges in some
graph. For this problem, we'll say we have some undirected graph of some n vertices, which are arbitrarily
labeled with indices from 0 through n-1. (This is reasonable because we could otherwise relabel the vertices
using a hash table for lookups. Also, we won't assume that subsequent numbers are connected by edges,
although that may happen in our unit tests.) Then, we'll initialize a collection of disjoint sets as n singletons
(single element sets), one for each vertex; we have a DisjointSets class to represent this collection.
To create sets representing connected components, we can iterate over the graph edges: For each edge (A,B)
connecting vertex A to vertex B, we can union the sets that A and B belong to, so the disjoint sets data structure
now indicates now that A and B belong to the same set. Our member function for the union operation is called
"dsunion" to avoid conflicting with the C++ keyword "union".
At the end of the process of calling dsunion() on every pair of vertices in the edge list, the number of disjoint
sets should correspond to the number of connected components in the graph.
The disjoint sets data structure can also detect cycles. As the edges are being processed, if the edge currently
being processed connects vertex A and vertex B, and both vertex A and vertex B are already in the same
disjoint set, then the edge connecting vertex A and vertex B completes a cycle.
In the source code provided below, you should modify the definition of DisjointSets::dsunion (under TASK 1) and
the definition of DisjointSets::count_comps (under TASK 2) according to the hints in the code comments. We'll
detect cycles during the union procedure and we can count the number of components after all union
operations are completed.
The starter code main() also contains an example graph with expected output. When you're ready to submit,
we'll run your code through some randomized unit tests for grading. */

#include <iostream>

class DisjointSets {
  public:
    static constexpr int MAX_NODES = 256;
    int leader[MAX_NODES];
    bool has cycle[MAX_NODES];
    int num_components;

    DisjointSets() {
      for (int i = 0; i < MAX_NODES; i++) leader[i] = -1;
      for (int i = 0; i < MAX_NODES; i++) has_cycle[i] = false;
      num_components = 0;
    }
    
    int find_leader(int i) {
      if (leader[i] < 0) return i;
      else return find_leader(leader[i]);
    }
    
    bool query_cycle(int i) {
      int root_i = find_leader(i);
      return has_cycle[root_i];
    }
    
    void dsunion(int i, int j);
    void count_comps(int n);
};

// dsunion performs disjoint set union.
// Assuming it is only called once per pair of vertics i and j,
// it can detect when a set is including an edge that completes aa cycle.
// Modify the implementation of dsunion below to properly adjust the
// has_cycle array so that query_cycle(root_j) accurately reports whether
// the connected component of root_j contains a cycle.
void DisjointSets::dsunion(int i, int j) {
  bool i_had_cycle = query_cycle(i);
  bool j_had_cycle = query_cycle(j);
  int root_i = find_leader(i);
  int root_j = find_leader(j);
  
  if (root_i != root_j) {
    leader[root_i] = root_j;
    root_i = root_j;
  }
  else {
    // find_readerはleaderが-1の時、key(nodeのorigin)を返す
    // {0,1}は最初-1なのでleaderに[0]=1がセットされる
    // {1,2}は最初-1なのでleaderに[1]=2がセットされる
    // {3,4}は最初-1なのでleaderに[3]=4がセットされる
    //   :
    // {7,3}は[7]は-1だが[3]=>4,[4]=>5,[5]=>6,[6]=>7,[7]=>-1となり[3]のleaderが7という事になる。7を起点にcycleが完成。
    has_cycle[root_i] = true;
    has_cycle[root_j] = true;
  }

}

// count_comps should count how many connected components there are in the graph.
// The input n is the number of vertices in the graph.(numbering is 0 through n-1)
void DisjointSets::count_comps(int n) {
  // leaderはkeyがorigin,valueがdestination
  for (int i=0; i<n; i++) {
    if (leader[i] < 0) num_components++;
  }
}

int main() {
  constexpr int NUM_EDGES = 9;
  constexpr int NUM_VERTS = 8;
  
  int edges[NUM_EDGES][2] = {{0,1},{1,2},{3,4},{4,5},{5,6},{6,7},{7,3},{3,5},{4,6}};
  
  DisjointSets d;
  
  // Below should maintain information about whether leaders are part of connected components that contain cycles.
  for (int i = 0; i < NUM_EDGES; i++)
    d.dsunion(edges[i][0], edges[i][1]); // Edge List, Vertex List(0 to n-1)は既にあるのでをプロパティをセットすればok.(Edge List: setの左側がorigin,右側がdestination)
    
  // Below should count the number of components.
  d.count_comps(NUM_VERTS);

  std::cout << "For edge list: ";
  for (int i = 0; i < NUM_EDGES; i++) {
    std::cout << "(" << edges[i][0] << "," << edges[i][1] << ")"
         << ((i < NUM_EDGES-1) ? "," : "\n")
  }
  
  std::cout << "You counted num_components: " << d.num_components << std::endl; // This should be 2
  std::cout << "Cycle reported for these vertices (if any):" << std::endl;
  for (int i=0; i<NUM_VERTS; i++) {
    if (d.query_cycle(i))
      std::cout << i << " "; // this set of edges should be 3 4 5 6 7.
  }
  std::cout << std::endl;
  
  return 0;
}
```

#### Unordered Data Structures in C++ (Week4) study memo
BB

```
#include <iostream>


```
