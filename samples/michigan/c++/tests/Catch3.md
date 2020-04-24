#### widely-used C++ testing framework Catch

**week3_tests.cpp**<br>
```
#include <cstdlib>
#include <sstream>

#include "../uiuc/catch/catch.hpp"

#include "../GenericTree.h"
#include "../GenericTreeExercises.h"


TEST_CASE("Displaying manual test output", "[weight=0]") {
  treeFactoryTest();
  traversalTest();
}

TEST_CASE("Testing treeFactory preliminaries", "[weight=1]") {
  GenericTree<int> tree(9999);
  treeFactory(tree);
  auto root = tree.getRootPtr();
  SECTION("Root should not be null") {
    REQUIRE(nullptr != root);
  }
  SECTION("Root data should remove the previous setting") {
    REQUIRE(root);
    REQUIRE(9999 != root->data);
  }
  SECTION("Root data should be 4") {
    REQUIRE(root);
    REQUIRE(4 == root->data);
  }
  SECTION("Deepest data should be 42") {
    REQUIRE(root);
    REQUIRE(root->childrenPtrs.at(0));
    REQUIRE(root->childrenPtrs.at(0)->childrenPtrs.at(0));
    REQUIRE(root->childrenPtrs.at(0)->childrenPtrs.at(0)->childrenPtrs.at(0));
    REQUIRE(42 == root->childrenPtrs.at(0)->childrenPtrs.at(0)->childrenPtrs.at(0)->data);
  }
}

TEST_CASE("Testing treeFactory further", "[weight=1]") {

  std::string exemplarTreeStr = "4\n";
  exemplarTreeStr += "|\n";
  exemplarTreeStr += "|_ 8\n";
  exemplarTreeStr += "|  |\n";
  exemplarTreeStr += "|  |_ 16\n";
  exemplarTreeStr += "|  |  |\n";
  exemplarTreeStr += "|  |  |_ 42\n";
  exemplarTreeStr += "|  |\n";
  exemplarTreeStr += "|  |_ 23\n";
  exemplarTreeStr += "|\n";
  exemplarTreeStr += "|_ 15\n";
  GeenricTree<int> tree(9999);
  treeFactory(tree);
  std::stringstream output;
  output << tree;
  SECTION("Trees should match") {
    
  }

  SECTION("Timing mergeSortRecursive") {
  
    constexpr int NUM_TEST_RUNS = 3;
    constexpr int LIST_SIZE_SMALL = 10;
    constexpr int LIST_SIZE_MEDIUM = 700;
    constexpr int LIST_SIZE_LARGE = LIST_SIZE_MEDIUM*10;
    
    LinkedList<int> unsortedList1;
    for (int i = LIST_SIZE_MEDIUM; i > 0; i--) {
      unsortedList1.pushFront(i);
      unsortedList1.pushBack(i);
    }

    LinkedList<int> unsortedList2;
    for (int i = LIST_SIZE_LARGE; i > 0; i--) {
      unsortedList2.pushFront(i);
      unsortedList2.pushBack(i);
    }

    LinkedList<int> unsortedListSmall;
    LinkedList<int> sortedListSmall;
    for (int i = LIST_SIZE_SMALL; i > 0; i--) {
      unsortedListSmall.pushFront(i);
      unsortedListSmall.pushBack(i);
      sortedListSmall.pushFront(i);
      sortedListSmall.pushBack(i);
    }

    std::cout << std::endl;
    
    {
      auto testSort = unsortedListSmall.mergeSortRecursive();
      if (testSort != sortedListSmall) {
        std::cout << "WARNING: It appears mergeSortRecursive or merge isn't correctly implemented yet.\n"
          << "  The running times below may not be meaningful." << std::endl;
      }
    }
    {
      std::cout << "Timing mergeSortRecursive:" << std::endl;
      LinkedList<int> sortedList;
      auto start_time = std::chrono::high_resolution_clock::now();
      for (int i=0; i<NUM_TEST_RUNS; i++) {
        sortedList = unsortedList1.mergeSortRecursive();
      }
      auto stop_time = std::chrono::high_resolution_clock::now();
      std::chrono::duration<double, std::milli> dur_ms = stop_time - start_time;
      // Make sure the loop doesn't get optimized away.
      if (sortedList.size() != unsortedList.size()) std::cout << "WARNING: List size didn't match!" << std::endl;
      if (sortedList.size()) std::cout << "Time elapsed" << dur_ms.count() << "ms" << std::endl;
    }
    {
      std::cout << "Again, after increasing list size 10x:" << std::endl;
      LinkedList<int> sortedList;
      auto start_time = std::chrono::high_resolution_clock::now();
      for (int i=0; i<NUM_TEST_RUNS; i++) {
        sortedList = unsortedList2.mergeSortRecursive();
      }
      auto stop_time = std::chrono::high_resolution_clock::now();
      std::chrono::duration<double, std::milli> dur_ms = stop_time - start_time;
      if (sortedList.size() != unsortedList.size()) std::cout << "WARNING: List size didn't match!" << std::endl;
      if (sortedList.size()) std::cout << "Time elapsed" << dur_ms.count() << "ms" << std::endl;
      std::cout << "A correct implementation is O(n log n), so the larger sort should take\n about 11x-13x longer in this case." << std::endl;
    }
  }
  
  SECTION("Timing mergeSortIterative") {
  
    std::cout << std::endl;

    std::cout << "mergeSortIterative is being tested on much larger lists than the others"
      << std::endl << " because this implementation exhibits extra overhead for small list sizes."
      << std::endl << " The change in running time becomes more clearly O(n log n) with inputs this large." << std::endl;

    constexpr int NUM_TEST_RUNS = 1;
    constexpr int LIST_SIZE_SMALL = 10;
    constexpr int LIST_SIZE_MEDIUM = 50000;
    constexpr int LIST_SIZE_LARGE = LIST_SIZE_MEDIUM*10;
    
    LinkedList<int> unsortedList1;
    for (int i = LIST_SIZE_MEDIUM; i > 0; i--) {
      unsortedList1.pushFront(i);
      unsortedList1.pushBack(i);
    }

    LinkedList<int> unsortedList2;
    for (int i = LIST_SIZE_LARGE; i > 0; i--) {
      unsortedList2.pushFront(i);
      unsortedList2.pushBack(i);
    }

    LinkedList<int> unsortedListSmall;
    LinkedList<int> sortedListSmall;
    for (int i = LIST_SIZE_SMALL; i > 0; i--) {
      unsortedListSmall.pushFront(i);
      unsortedListSmall.pushBack(i);
      sortedListSmall.pushFront(i);
      sortedListSmall.pushBack(i);
    }
    
    {
      auto testSort = unsortedListSmall.mergeSortIterative();
      if (testSort != sortedListSmall) {
        std::cout << "WARNING: It appears mergeSortIterative or merge isn't correctly implemented yet.\n"
          << "  The running times below may not be meaningful." << std::endl;
      }
    }
    {
      std::cout << "Timing mergeSortIterative:" << std::endl;
      LinkedList<int> sortedList;
      auto start_time = std::chrono::high_resolution_clock::now();
      for (int i=0; i<NUM_TEST_RUNS; i++) {
        sortedList = unsortedList1;
        sortedList = sortedList.mergeSortIterative();
        if (!sortedList.isSorted()) std::cout << "WARNING: mergeSortIterative result not sorted." << std::endl;
      }
      auto stop_time = std::chrono::high_resolution_clock::now();
      std::chrono::duration<double, std::milli> dur_ms = stop_time - start_time;
      // Make sure the loop doesn't get optimized away.
      if (sortedList.size() != unsortedList.size()) std::cout << "WARNING: List size didn't match!" << std::endl;
      if (sortedList.size()) std::cout << "Time elapsed" << dur_ms.count() << "ms" << std::endl;
    }
    {
      std::cout << "Again, after increasing list size 10x:" << std::endl;
      LinkedList<int> sortedList;
      auto start_time = std::chrono::high_resolution_clock::now();
      for (int i=0; i<NUM_TEST_RUNS; i++) {
        sortedList = unsortedList2;
        sortedList = sortedList.mergeSortIterative();
        if (!sortedList.isSorted()) std::cout << "WARNING: mergeSortIterative result not sorted." << std::endl;
      }
      auto stop_time = std::chrono::high_resolution_clock::now();
      std::chrono::duration<double, std::milli> dur_ms = stop_time - start_time;
      if (sortedList.size() != unsortedList.size()) std::cout << "WARNING: List size didn't match!" << std::endl;
      if (sortedList.size()) std::cout << "Time elapsed" << dur_ms.count() << "ms" << std::endl;
      std::cout << "A correct implementation is O(n log n), so the larger sort should take\n about 11x-13x longer in this case." << std::endl;
    }

    std::cout << "Note: Compiler optimizations or overhead might cause some of these to run"
      << std::endl << " faster or slower than expected, even if your implementation is correct."
      << std::endl << " The algorithms aren't all being tested on the same list size, but tather on lists"
      << std::endl << " larger enough to show the asymptotic complexity. (Compare the ratio of running times"
      << std::endl << " for a single algorithm before and after the increase in input size.)" << std::endl;

  }
}
// ====================================================
// Tests: insertOrdered
// ====================================================

TEST_CASE("Testing insertOrdered: Insert at front", "[weight=1]") {
  LinkedList<int> l;
  l.pushBack(1);
  l.pushBack(2);
  l.pushBack(3);
  l.pushBack(7);
  l.pushBack(49);
  
  auto expectedList = l;
  expectedList.pushFront(-100);
  auto studentResultList = l;
  auto* expectedAddress = studentResultList.getTailPtr();
  studentResultList.insertOrdered(-100);
  
  SECTION("Checking that values are correct") {
    REQUIRE(studentResultList == expectedList);
  }
  
  SECTION("Checking that the list prev links and tail pointer are being set correctly") {
    REQUIRE(studentResultList.assertPrevLinks());
  }
  
  SECTION("Checking that the list size is being tracked correctly") {
    REQUIRE(studentResultList.assertCorrectSize());
  }
  
  SECTION("Checking that the existing node addresses didn't change") {
    auto* studentAddress = studentResultList.getTailPtr();
    REQUIRE(studentAddress == expectedAddress);
  } 
}

TEST_CASE("Testing insertOrdered: Insert at end", "[weight=1]") {
  LinkedList<int> l;
  l.pushBack(1);
  l.pushBack(2);
  l.pushBack(3);
  l.pushBack(7);
  l.pushBack(49);
  
  auto expectedList = l;
  expectedList.pushBack(100);
  auto studentResultList = l;
  auto* expectedAddress = studentResultList.getHeadPtr();
  studentResultList.insertOrdered(100);
  
  SECTION("Checking that values are correct") {
    REQUIRE(studentResultList == expectedList);
  }
  
  SECTION("Checking that the list prev links and tail pointer are being set correctly") {
    REQUIRE(studentResultList.assertPrevLinks());
  }
  
  SECTION("Checking that the list size is being tracked correctly") {
    REQUIRE(studentResultList.assertCorrectSize());
  }
  
  SECTION("Checking that the existing node addresses didn't change") {
    auto* studentAddress = studentResultList.getHeadPtr();
    REQUIRE(studentAddress == expectedAddress);
  } 
}

TEST_CASE("Testing insertOrdered: Insert to empty list", "[weight=1]") {
  LinkedList<int> l;
  auto expectedList = l;
  expectedList.pushBack(100);
  auto studentResultList = l;
  studentResultList.insertOrdered(100);
  
  SECTION("Checking that values are correct") {
    REQUIRE(studentResultList == expectedList);
  }
  
  SECTION("Checking that the list prev links and tail pointer are being set correctly") {
    REQUIRE(studentResultList.assertPrevLinks());
  }
  
  SECTION("Checking that the list size is being tracked correctly") {
    REQUIRE(studentResultList.assertCorrectSize());
  }
}

TEST_CASE("Testing insertOrdered: Insert in middle", "[weight=1]") {
  LinkedList<int> l;
  l.pushBack(1);
  l.pushBack(2);
  l.pushBack(3);
  l.pushBack(7);
  l.pushBack(49);
  
  auto studentResultList = l;
  auto* expectedAddress = studentResultList.getHeadPtr();
  studentResultList.insertOrdered(5);
  
  SECTION("Checking that values are correct") {
    REQUIRE(studentResultList == expectedList);
  }
  
  SECTION("Checking that the list prev links and tail pointer are being set correctly") {
    REQUIRE(studentResultList.assertPrevLinks());
  }
  
  SECTION("Checking that the list size is being tracked correctly") {
    REQUIRE(studentResultList.assertCorrectSize());
  }
  
  SECTION("Checking that the existing node addresses didn't change") {
    auto* studentAddress = studentResultList.getHeadPtr();
    REQUIRE(studentAddress == expectedAddress);
  } 
}

TEST_CASE("Testing merge: Left list empty; right list non-empty", "[weight=1]") {
  LinkedList<int> left;
  LinkedList<int> right;
  right.pushBack(1);
  right.pushBack(2);
  right.pushBack(3);
  auto expectedList = right;

  auto studentResultList = left.merge(right);
  
  SECTION("Checking that values are correct") {
    REQUIRE(studentResultList == expectedList);
  }
  
  SECTION("Checking that the list prev links and tail pointer are being set correctly") {
    REQUIRE(studentResultList.assertPrevLinks());
  }
  
  SECTION("Checking that the list size is being tracked correctly") {
    REQUIRE(studentResultList.assertCorrectSize());
  }
}

TEST_CASE("Testing merge: Left list non-empty; right list empty", "[weight=1]") {
  LinkedList<int> left;
  left.pushBack(1);
  left.pushBack(2);
  left.pushBack(3);
  LinkedList<int> right;
  auto expectedList = left;

  auto studentResultList = left.merge(right);
  
  SECTION("Checking that values are correct") {
    REQUIRE(studentResultList == expectedList);
  }
  
  SECTION("Checking that the list prev links and tail pointer are being set correctly") {
    REQUIRE(studentResultList.assertPrevLinks());
  }
  
  SECTION("Checking that the list size is being tracked correctly") {
    REQUIRE(studentResultList.assertCorrectSize());
  }
}

TEST_CASE("Testing merge: Left and right lists non-empty; same size", "[weight=1]") {
  LinkedList<int> left;
  left.pushBack(1);
  left.pushBack(5);
  left.pushBack(10);
  left.pushBack(20);
  LinkedList<int> right;
  right.pushBack(2);
  right.pushBack(4);
  right.pushBack(11);
  right.pushBack(19);
  LinkedList<int> expectedList = left;
  expectedList.pushBack(1);
  expectedList.pushBack(2);
  expectedList.pushBack(4);
  expectedList.pushBack(5);
  expectedList.pushBack(10);
  expectedList.pushBack(11);
  expectedList.pushBack(19);
  expectedList.pushBack(20);

  auto studentResultList = left.merge(right);
  
  SECTION("Checking that values are correct") {
    REQUIRE(studentResultList == expectedList);
  }
  
  SECTION("Checking that the list prev links and tail pointer are being set correctly") {
    REQUIRE(studentResultList.assertPrevLinks());
  }
  
  SECTION("Checking that the list size is being tracked correctly") {
    REQUIRE(studentResultList.assertCorrectSize());
  }
}

TEST_CASE("Testing merge: Left and right lists non-empty; left list is longer", "[weight=1]") {
  LinkedList<int> left;
  left.pushBack(1);
  left.pushBack(5);
  left.pushBack(10);
  left.pushBack(20);
  LinkedList<int> right;
  right.pushBack(2);
  right.pushBack(4);
  LinkedList<int> expectedList = left;
  expectedList.pushBack(1);
  expectedList.pushBack(2);
  expectedList.pushBack(4);
  expectedList.pushBack(5);
  expectedList.pushBack(10);
  expectedList.pushBack(20);

  auto studentResultList = left.merge(right);
  
  SECTION("Checking that values are correct") {
    REQUIRE(studentResultList == expectedList);
  }
  
  SECTION("Checking that the list prev links and tail pointer are being set correctly") {
    REQUIRE(studentResultList.assertPrevLinks());
  }
  
  SECTION("Checking that the list size is being tracked correctly") {
    REQUIRE(studentResultList.assertCorrectSize());
  }
}

TEST_CASE("Testing merge: Left and right lists non-empty; right list is longer", "[weight=1]") {
  LinkedList<int> left;
  left.pushBack(1);
  left.pushBack(20);
  LinkedList<int> right;
  right.pushBack(2);
  right.pushBack(4);
  right.pushBack(11);
  right.pushBack(19);
  LinkedList<int> expectedList = left;
  expectedList.pushBack(1);
  expectedList.pushBack(2);
  expectedList.pushBack(4);
  expectedList.pushBack(11);
  expectedList.pushBack(19);
  expectedList.pushBack(20);

  auto studentResultList = left.merge(right);
  
  SECTION("Checking that values are correct") {
    REQUIRE(studentResultList == expectedList);
  }
  
  SECTION("Checking that the list prev links and tail pointer are being set correctly") {
    REQUIRE(studentResultList.assertPrevLinks());
  }
  
  SECTION("Checking that the list size is being tracked correctly") {
    REQUIRE(studentResultList.assertCorrectSize());
  }
}

```
