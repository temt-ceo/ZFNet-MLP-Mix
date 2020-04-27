### widely-used C++ testing framework Catch
#### Unordered Map

**week1_tests.cpp**<br>
```
#include <cstdlib>
#include <stdexcept>
#include <sstream>
#include <chrono>

#include "../uiuc/catch/catch.hpp"

#include "../UnorderedMapCommon.h"

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
}

// Tests: makeWordCounts

TEST_CASE("Testing makeWordCounts", "[weight=1]") {

  constexpr int MIN_WORD_LENGTH = 5;
  StringVec bookstrings = loadBookStrings(MIN_WORD_LENGTH);
  StringIntMap wordcount_map = makeWordCounts(bookstrings);
  
  SECTION("Checking that map contains the right number of keys") {
    const int number_of_keys = wordcount_map.size();
    const int number_of_keys_expected = 2181;
    REQUIRE(number_of_keys == number_of_keys_expected);
  }

  SECTION("Checking that \"bandersnatch\" was counted 3 items") {
    bool found_bandersnatch = wordcount_map.count("bandersnatch");
    if (found_bandersnatch) {
      const int bandersnatch_count = wordcount_map["bandersnatch"];
      const int bandersnatch_count_expected = 3;
      REQUIRE(bandersnatch_count == bandersnatch_count_expected);
    }
    else {
      REQUIRE(found_bandersnatch);
    }
  }

  SECTION("Checking that \"alice\" was recorded 434 times") {
    bool found_alice = wordcount_map.count("alice");
    if (found_alice) {
      const int alice_count = wordcount_map["alice"];
      const int alice_count_expected = 434;
      REQUIRE(alice_count == alice_count_expected);
    }
    else {
      REQUIRE(found_alice);
    }
  }

  SECTION("Checking that \"frabjous\" was recorded 1 times") {
    bool found_frabjous = wordcount_map.count("frabjous");
    if (found_frabjous) {
      const int frabjous_count = wordcount_map["frabjous"];
      const int frabjous_count_expected = 1;
      REQUIRE(frabjous_count == frabjous_count_expected);
    }
    else {
      REQUIRE(found_frabjous);
    }
  }
}

// Tests: lookupWithFallback (with no dependency on makeWordCounts)

TEST_CASE("Testing lookupWithFallback: When the key exists", "[weight=1]") {

  StringIntMap wordcount_map;
  wordcount_map["bandersnatch"] = 3;
  
  SECTION("Should return the found value") {
    const int bandersnatch_count = lookupWithFallback(wordcount_map, "bandersnatch", 0);
    const int bandersnatch_count_expected = 3;
    REQUIRE(bandersnatch_count == bandersnatch_count_expected);
  }
}
  
TEST_CASE("Testing traverseLevels", "[weight=2]") {
  // This is the tree from exampleTree2() in main.cpp
  std::string expected_traversal = "A B D J K C E I L F G M H ";
  GenericTree<std::string> tree2("A");
  auto A = tree2.getRootPtr();
  A->addChild("B")->addChild("C");
  auto D = A->addChild("D");
  auto E = D->addChild("E");
  E->addChild("F");
  E->addChild("G")->addChild("H");
  D->addChild("I");
  A->addChild("J");
  auto L = A->addChild("K")->addChild("L");
  L->addChild("M");
  std::vector<std::string> tree2_results = traverseLevels(tree2);
  // std::cout << "Your traverseLevels output:" << std::endl;
  std::stringstream outstream;
  for (auto result : tree2_results) {
    outstream << result << " ";
  }
  suto student_traversal = outstream.str();
  SECTION("Should do correct traversal on tree from exampleTree2()") {
    REQUIRE(expected_traversal == student_traversal);
  }
}
```
