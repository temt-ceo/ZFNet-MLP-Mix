#### widely-used C++ testing framework Catch

**week1_tests.cpp**<br>
```
#include <cstdlib>
#include <stdexcept>
#include <sstream>
#include <chrono>

#include "../LinkedList.h"
#include "../LinkedListExercises.h"

#include "../uiuc/catch/catch.hpp"

template<typename T >
void assertPtr<T* ptr> {
  if (!ptr) {
    throw std::runtime_error("Would have dereferenced a null pointer");
  }
}

template<typename T >
T& deref<T* ptr> {
  if (!ptr) {
    throw std::runtime_error("Would have dereferenced a null pointer");
  }
  else {
    return *ptr;
  }
}

TEST_CASE("Benchmark: Measuring slowdown for insertOrdered and merge", "[weight=0][.][bench]") {

  SECTION("Timing insertOrdered") {

    constexpr int NUM_TEST_RUNS = 10;
    constexpr int LIST_SIZE_SMALL = 10;
    constexpr int LIST_SIZE_MEDIUM = 200000;
    constexpr int LIST_SIZE_LARGE = LIST_SIZE_MEDIUM*10;
    
    LinkedList<int> list0;
    list0.push_back(1);
    list0.push_back(3);
    LinkedList<int> list0_correct;
    list0_correct.pushBack(1);
    list0_correct.pushBack(2);
    list0_correct.pushBack(3);

    LinkedList<int> list1;
    for (int i = 0; i < LIST_SIZE_MEDIUM; i++) {
      list1.pushBack(0);
    }
    list1.pushBack(100);

    LinkedList<int> list2;
    for (int i = 0; i < LIST_SIZE_LARGE; i++) {
      list2.pushBack(0);
    }
    list2.pushBack(100);

    std::cout << std::endl;

    {
      auto studentList0 = list0;
      studentList0.insertOrdered(2);
      // std::cout << studentList0 << " " << list0_correct << std::endl;
      if (studentList0 != list0_correct) {
        std::cout << "WARNING: It appears insertOrdered isn't correctly implemented yet.\n"
          << "  The running times below may not be meaningful." << std::endl;
      }
    }
    // Reference for chrono library usage: https://en.cppreference.com/w/cpp/chrono/duration/duration_cast
    {
      std::cout << "Timing insertOrdered:" << std::endl;
      auto studentList = list1;
      auto start_time = std::chrono::high_resolution_clock::now();
      for (int i=0; i<NUM_TEST_RUNS; i++) {
        studentList.insertOrdered(50);
      }
      auto stop_time = std::chrono::high_resolution_clock::now();
      std::chrono::duration<double, std::milli> dur_ms = stop_time - start_time;
      // Make sure the loop doesn't get optimized away.
      if (studentList.size()) std::cout << "Time elapsed" << dur_ms.count() << "ms" << std::endl;
    }
    {
      std::cout << "Again, after increasing list size 10x(10倍):" << std::endl;
      auto studentList = list2;
      auto start_time = std::chrono::high_resolution_clock::now();
      for (int i=0; i<NUM_TEST_RUNS; i++) {
        studentList.insertOrdered(50);
      }
      auto stop_time = std::chrono::high_resolution_clock::now();
      std::chrono::duration<double, std::milli> dur_ms = stop_time - start_time;
      if (studentList.size()) std::cout << "Time elapsed" << dur_ms.count() << "ms" << std::endl;
      std::cout << "The worst case should be 0(n) for insertOrdered,\n so the larger case might take nearly 10x longer here." << std::endl;
    }
  }

  SECTION("Timing merge") {
  
    constexpr int NUM_TEST_RUNS = 5;
    constexpr int LIST_SIZE_SMALL = 10;
    constexpr int LIST_SIZE_MEDIUM = 2000;
    constexpr int LIST_SIZE_LARGE = LIST_SIZE_MEDIUM*10;
    
    LinkedList<int> list0_l;
    list0_l.pushBack(1);
    list0_l.pushBack(3);
    LinkedList<int> list0_r;
    list0_r.pushBack(2);
    list0_r.pushBack(4);
    LinkedList<int> list0_correct;
    list0_correct.pushBack(1);
    list0_correct.pushBack(2);
    list0_correct.pushBack(3);
    list0_correct.pushBack(4);
    
    LinkedList<int> list1_l;
    LinkedList<int> list1_r;
    for (int i = 0; i < LIST_SIZE_MEDIUM; i++) {
      list1_l.pushBack(i);
      list1_r.pushBack(i+1);
      list1_r.pushBack(i+2);
      list1_l.pushBack(i+3);
    }
    
    LinkedList<int> list2_l;
    LinkedList<int> list2_r;
    for (int i = 0; i < LIST_SIZE_LARGE; i++) {
      list2_l.pushBack(i);
      list2_r.pushBack(i+1);
      list2_r.pushBack(i+2);
      list2_l.pushBack(i+3);
    }

    std::cout << std::endl;
    
    {
      auto studentList0 = list0_l.merge(list0_r);
      // std::cout << studentList0 << " " << list0_correct << std::endl;
      if (studentList0 != list0_correct) {
        std::cout << "WARNING: It appears merge isn't correctly implemented yet.\n"
          << " The runnng times below may not be meaningful." << std::endl;
      }
    }
    {
      std::cout << "Timing merge:" << std::endl;
      LinkedList<int> studentList;
      auto start_time = std::chrono::high_resolution_clock::now();
      for (int i=0; i<NUM_TEST_RUNS; i++) {
        studentList = list1_l.merge(list1_r);
      }
      auto stop_time = std::chrono::high_resolution_clock::now();
      std::chrono::duration<double, std::milli> dur_ms = stop_time - start_time;
      // Make sure the loop doesn't get optimized away.
      if (studentList.size()) std::cout << "Time elapsed" << dur_ms.count() << "ms" << std::endl;
    }
    {
      std::cout << "Again, after increasing list size 10x(10倍):" << std::endl;
      LinkedList<int> studentList;
      auto start_time = std::chrono::high_resolution_clock::now();
      for (int i=0; i<NUM_TEST_RUNS; i++) {
        studentList = list2_l.merge(list2_r);
      }
      auto stop_time = std::chrono::high_resolution_clock::now();
      std::chrono::duration<double, std::milli> dur_ms = stop_time - start_time;
      if (studentList.size()) std::cout << "Time elapsed" << dur_ms.count() << "ms" << std::endl;
      std::cout << "The worst case should be 0(n) for insertOrdered,\n so the larger case might take nearly 10x longer here." << std::endl;
    }
  }
}

TEST_CASE("Benchmark: Measuring slowdown for sorting algorithms", "[weight=0][.][bench]") {

  SECTION("Timing insertSort") {

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
      auto testSort = unsortedListSmall.insertionSort();
      if (testSort != sortedListSmall) {
        std::cout << "WARNING: It appears insertionSort or insertOrdered isn't correctly implemented yet.\n"
          << "  The running times below may not be meaningful." << std::endl;
      }
    }
    {
      std::cout << "Timing insertionSort:" << std::endl;
      LinkedList<int> sortedList;
      auto start_time = std::chrono::high_resolution_clock::now();
      for (int i=0; i<NUM_TEST_RUNS; i++) {
        sortedList = unsortedList1.insertionSort();
      }
      auto stop_time = std::chrono::high_resolution_clock::now();
      std::chrono::duration<double, std::milli> dur_ms = stop_time - start_time;
      // Make sure the loop doesn't get optimized away.
      if (sortedList.size()) std::cout << "Time elapsed" << dur_ms.count() << "ms" << std::endl;
    }
    {
      std::cout << "Again, after increasing list size 10x:" << std::endl;
      LinkedList<int> sortedList;
      auto start_time = std::chrono::high_resolution_clock::now();
      for (int i=0; i<NUM_TEST_RUNS; i++) {
        sortedList = unsortedList2.insertionSort();
      }
      auto stop_time = std::chrono::high_resolution_clock::now();
      std::chrono::duration<double, std::milli> dur_ms = stop_time - start_time;
      if (sortedList.size()) std::cout << "Time elapsed" << dur_ms.count() << "ms" << std::endl;
      std::cout << "The worst case should be 0(n^2) for insertionSort,\n so the larger case might take nearly 100x longer here." << std::endl;
    }
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

```

