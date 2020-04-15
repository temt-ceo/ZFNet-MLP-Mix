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
    list0_correct.push_back(1);
    list0_correct.push_back(2);
    list0_correct.push_back(3);

    LinkedList<int> list1;
    for (int i = 0; i < LIST_SIZE_MEDIUM; i++) {
      list1.push_back(0);
    }
    list1.push_back(100);

    LinkedList<int> list2;
    for (int i = 0; i < LIST_SIZE_LARGE; i++) {
      list2.push_back(0);
    }
    list2.push_back(100);

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
      studentList0.insertOrdered(2);
      if (studentList0 != list0_correct) {
        std::cout << "WARNING: It appears insertOrdered isn't correctly implemented yet.\n"
          << "  The running times below may not be meaningful." << std::endl;
      }
    }

  }

  SECTION("Pixels with watermark should be changed") {
    REQUIRE( png.getPixel(100, 15).l + 0.2 == result.getPixel(100, 15).l );
    REQUIRE( png.getPixel(200, 25).l + 0.2 == result.getPixel(200, 25).l );
  }
}

```

