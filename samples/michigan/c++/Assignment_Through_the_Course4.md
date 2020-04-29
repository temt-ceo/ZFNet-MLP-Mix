
#### Unordered Data Structures in C++ (Week1) submission task

**UnorderedMapExcercises.cpp**<br>
```
#include <iostream>

#include "UnorderedMapCommon.h"

StringIntMap makeWordCounts(const StringVec & words) {
  StringIntMap wordcount_map;
  
  // StringVec = std::vector<std::string>;
  // StringIntMap = std::unordered_map<std::string, int>;
  for (std::string str : words) {
    if ( wordcount_map[str] )
      wordcount_map[str]++;
    else
      wordcount_map[str] = 1;      
  }
  
  return wordcount_map;
}

int lookupWithFallback(const StringIntMap & wordcount_map, const std::string & key, int fallbackVal) {

  /*
  1. Given the input parameters shown, you need to figure out if the key exists in the wordcount_map or not.
  2. If the key exists, return the mapped value.
  3. If the key does not exists, then return the provided fallback value.
  People commonly use the [] operator with maps to conveniently do both assignments and lookups, but the []
  operator will insert a new key with a default value when the key is not found. Sometimes that is not desirable.
  (Also, if the map is marked "const", you will not be able to use the [] operator on it, because the map is read-only.)
  Instead, there is also the .at() function which can look up a key or throw an exception if not found; or there is the
  .find() function which can search for a key and return an iterator signifying the result. There is also
  tge strangely-named .count() function, which does not actually count beyond 1; it can only tell you if 
  the key is in the map or not.
  */
  if ( wordcount_map.count(key) && wordcount_ma.at(key) >= 1 )
    return wordcount_map.at(key);
  else
    return fallbackVal;
}

/* memoize: 実行速度を上げる為に処理の重い実行結果をキャッシュする事 */
int memoizedLongestPalindromeLength(LengthMemo & memo, const std::string & str, int leftLimit, intRightLimit, timeUnit startTime, double maxDuration) {
  if (leftLimit < 0 && leftLimit <= rightLimit) {
    throw std::runtime_error("leftLimit negative, but it's not the base case");
  }
  if (rightLimit < 0 && leftLimit <= rightLimit) {
    throw std::runtime_error("rightLimit negative, but it's not the base case");
  }
  
  if (false) {
    // Debugging spam messages
    int range = rightLimit - leftLimit + 1;
    if (range < 0) range = 0;
    std::cout << "Considering substring: " << str.surstr(leftLimit, range) << std::endl;
    std::cout << " because l/r limits are " << leftLimit << " " << rightLimit << std::endl;
  }
  
  // memoizeが失敗していないか確認
  const auto currentTime = getTimeNow();
  const auto timeRlapsed = getMilliDuration(startTime, currentTime);
  if (timeElapsed > maxDuration) {
    throw TooSlowException("taking too long"); // .hファイルで宣言がされている
  }
  
  /*
  Palindrome: 前から読んでも後ろから読んでも同じ単語（例:dad）。これを単語から探す処理がとても重いのでcachする。それがmemoization。
  
  LengthMemo = std::unordered_map<IntPair, int>;
  IntPair = std::pair<int, int>;
  The IntPair key is a pair of left and right index limits, and the mapped int is the longest palindrome length.
  So for example "xyzwDADxyzw", one entry in the map would be: Key=> the pair(0, 10), Mappedn value=> 3.
  */
  
  const IntPair pairKey = std::make_pair(leftLimit, rightLimit);
  
  if (memo.count(pairKey)) { // It returns 1 if found and otherwise 0. These values convert to true/false.
    // memo[pairKey]で検索するとentryがdefault valueで作成される。
    // ===============================================================
    // EXERCISE 3 - PART A - YOUR CODE HERE!
    
    return memo.at(pairKey);
    // ===============================================================
  }
  
  if (leftLimit > rightLimit) {
    memo[pairKey] = 0;
    return 0;
  }
  
  if (leftLimit == rightLimit && str.at(leftLimit) == str.at(rightLimit)) {
    memo[pairKey] = 1;
    return 1;
  }
  
  if (str.at(leftLimit) == str.at(rightLimit)) {
    int newLeft = leftLimit+1;
    int newRight = rightLimit-1;
    int middleSubproblemResult = memoizedLongestPalindromeLength(memo, str, newLeft, newRight, startTime, maxDuration);
    
    int middleMaxLength = newRight-newLeft+1;
    if (middleMaxLength < 0) middleMaxLength = 0;
    
    if (middleSubproblemResult == middleMaxLength) {
      int result = 2+middleSubproblemResult;
      
      memo[pairKey] = result;
      return result;
    }
  }

  int leftSubproblemResult = memoizedLongestPalindromeLength(memo, str, leftLimit, rightLimit-1, startTime, maxDuration);
  int rightSubproblemResult = memoizedLongestPalindromeLength(memo, str, leftLimit+1, rightLimit, startTime, maxDuration);
  
  // Return whichever result was greater. We can also store this result for memoization purpose.
  int greaterResult = std::max(leftSubproblemResult, rightSubproblemResult);
  
  // ===============================================================
  // EXERCISE 3 - PART B - YOUR CODE HERE!
  memo[pairKey] = greaterResult;
  return greaterResult;
  // ===============================================================
}
```

##### Overview of std::unordered_map
・The Standard Template Library in C++ offers a hash table implementation with std::unordered_map.
It has this name because a hash table is a more specific implementation of the abstract data type
generally called a “map,” which associates each unique lookup key with an assigned value. It is called
“unordered” in C++ to distinguish it from another map implementation, std::map. We will focus on
std::unordered_map below, but let’s briefly summarize how it’s different from std::map.

  The implementation of std::map uses some kind of balanced tree (the specifics may vary from system
to system). It maintains keys in a sorted order as a result, which is desirable sometimes, such as when
you want to iterate over the keys in the same order repeatedly. It offers O(log n) lookup times. In order
for classes to be compatible as key types, they need to have an equality operation defined, as well as a
“less than” operation defined for sorting.<br>
<br>
  By contrast, std::unordered_map is implemented as a hash table, and the keys are not arranged in
any predictable order, but instead are placed in hashing buckets as described in the lectures on hash
tables. It offers very fast O(1) lookup times on average (that is, constant-time). For a class type to be
compatible as a key, it must have an equality operation defined, and it also must have a hashing function
defined. We’ll talk more about that later in this document.<br>
<br>
  Although lookups on std::unordered_map are very efficient, after inserting many items, the table
will be automatically “re-hashed” and resized, and that can periodically lead to temporary slowdown. In
the future, if that is an issue you can do additional work to specify optimal sizes beforehand, but we will
trust in the system defaults to balance the load factor and maintain the table organization.<br>
<br>
  We may refer to objects of type std::unordered_map simply as “maps” sometimes for shorthand, so
in the future, please take note about what kind of map is being used. For this assignment, we will only
use std::unordered_map.<br>
<br>
**main.cpp**<br>
```
#include <iostream>
#include <string>

#include "UnorderedMapCommon.h"

void errorReaction(std::string msg) {
  std::cout << std::endl << "WARNING: " << msg << std::endl << std::endl;
}

/*
注. 各クラスはhファイルで以下のように宣言されている。
using StringVec = std::vector<std::string>;
using StringIntPair = std::pair<std::string, int>;
using StringIntMap = std::unordered_map<std::string, int>;
using StringIntPairVec = std::vector<StringIntPair>;
*/
int main() {
  
  constexpr int MIN_WORD_LENGTH = 5; //constexpr : const expression の変数は書き換える事ができなくなる
  StringVec boolstrings = loadBookStrings(MIN_WORD_LENGTH);
  
  std::cout << "Total strings loaded: " << bookstrings.size() << std::endl;
  
  /* Testing makeWordCounts */
  std::cout << "====== Testing makeWordCounts ======" << std::endl << std::endl;
  StringIntMap wordcount_map = makeWordCounts(bookstrings); // <- unordered_mapを生成します。
  std::cout << "Unique string occurrences: " << wordcount_map.size() << std::endl;
  
  StringIntPairVec sorted_wordcounts = sortWordCounts(wordcount_map);
  std::cout << "Sorted size: " << sorted_wordcounts.size() << std::endl << std::endl;
  
  StringIntPairVec top_wordcounts = getTopWordCounts(sorted_wordcounts, 20);
  std::cout << "20 most frequent words: " << std::endl;
  {
    int i = 1;
    for (const StringIntPair & wordcount_item : top_wordcounts) {
      auto word = wordcount_item.first;
      auto count  = wordcount_item.second;
      std::cout << i << ": " << word << " - " << " times" << std::endl;
      i++;
    }
  }
  std::cout << std::endl;
  
  // 生成されたstd::unordered_mapのプロパティを確認する
  if (wordcount_map.bucket_count() < 1) {
    errorReaction("No buckets in wordcount_map");
  }
  else {
    std::cout << "Report load factor: " << wordcount_map.load_factor() << std::endl;
    
    double load_factor = (double)wordcount_map.size() / wordcount_map.bucket_count();
    std::cout << "Calculated load factor: " << load_factor << std::endl << std::endl;
  }

  /* Checking lookupWithFallback */
  std::cout << "====== Checking lookupWithFallback without dependencies ======" << std::endl << std::endl;
  {
    StringIntMap wordcount_map;
    wordcount_map["bandersnatch"] = 3;
    auto wordcount_map_backup = wordcount_map;

    int bandersnatch_count = lookupWithFallback(wordcount_map, "bandersnatch", 0);
    std::cout << "Looking up \"bandersnatch\" with fallback of 0. Result (expecting 3): " << bandersnatch_count << std::endl;
    
    if (bandersnatch_count != 3) errorReaction("Should have found \"bandersnatch\" with a count of 3.");
    if (wordcount_map_backup != wordcount_map) errorReaction("The lookup operation changed wordcount_map somehow.");
  }


  // overwritten by the brute force run below
  double slow_time = 9000.0;
  int intended_pal_result = 0;
  bool have_intended_result = false;

  // Small example: Brute force is pretty fast, but memoization gives a noticeable speedup.
  const std::string str_small = "abbbcdeeeefgABCBAz";
  // Medium example: Brute force takes several seconds. Memoization is very fast.
  const std::string str_medium = "abbbcdeeeefgABCBAabcdefgh";
  // Large example: Brute force will probably time out. Memoization is still very fast.
  const std::string str_large = "abbbcdeeeefgABCBAabcdefghijkl";
  // This should be the longest palindrome hidden in each example.
  const std::string correct_palindrome_substring = "ABCBA";

  // Which string to use for testing
  const std::string str = str_small;

  // Larger examples may take an extremely long time for brute force.
  // A correct memoized algorithm can handle them all very quickly, though.
  // For large problem sizes, the memoized version can be thousands of times faster.

  {
    std::cout << "======= Finding the longest palindrome substring (brute force) =======" << std::endl << std::endl;
    std::cout << "Note: This may take a few seconds. The duration scales up very quickly\n as the problem size increases." << std::endl << std::endl;
    std::cout << "String: \"" << str << "\"" << std::endl;
    auto start_time = getTimeNow();
    auto stop_time = start_time;
    double max_duration = 10000.0; // 10 seconds
    int pal_result = 99999;
    try {
      pal_result = longestPalindromeLength(str, 0, str.length()-1, start_time, max_duration);
      stop_time = getTimeNow();
      intended_pal_result = pal_result;
      have_intended_result = true;
      std::cout << "Length of longest palindrome substring: " << pal_result << std::endl;
    }
    catch (TooSlowException& e) {
      stop_time = getTimeNow();
      std::cout << "(Timeout!) Brute force is taking too long on this example. Giving up." << std::endl;
    }
    slow_time = getMilliDuration(start_time, stop_time);
    std::cout << "Calculation time (ms): " << slow_time << std::endl;
    std::cout << std::endl;
  }

  // overwritten by the memoized test run
  double memoized_time = 9000.0;

  {
    std::cout << "======= Finding the longest palindrome substring (memoized) =======" << std::endl << std::endl;
    std::cout << "Note: If you haven't finished correctly implementing this function yet," << std::endl
      << " the provided starter code may take even longer than brute force and time out." << std::endl
      << " That could also happen if you have a bug or infinite recursion." << std::endl << std::endl;
    std::cout << "String: \"" << str << "\"" << std::endl;
    
    // Creating the memoization std::unordered_map
    LengthMemo memo;

    int pal_result = 99999;
    auto start_time = getTimeNow();
    bool timed_out = false;
    double max_duration = slow_time;
    try {
      pal_result = memoizedLongestPalindromeLength(memo, str, 0, str.length()-1, start_time, max_duration);
    }
    catch (TooSlowException& e) {
      timed_out = true;
    }
    auto stop_time = getTimeNow();
    memoized_time = getMilliDuration(start_time, stop_time);
    if (timed_out) {
      std::cout << "(Timeout!) Elapsed time (ms): " << memoized_time << std::endl;
      errorReaction("Terminated memoized algorithm due to timeout.");
    }
    else {
      std::cout << "Length of longest palindrome substring: " << pal_result << std::endl;
      if (have_intended_result) {
        if (pal_result != intended_pal_result) errorReaction("Your memoized palindrome function returned the wrong result.");
        else std::cout << " (Correct!)" << std::endl;
      }
      std::cout << "Calculation time (ms): " << memoized_time << std::endl;
      double speedup = slow_time/memoized_time;
      std::cout << "Speedup over brute force: " << speedup << " times faster" << std::endl;
      if (speedup < 2.0) errorReaction("Should have been at least 2 times faster.\n (A correct memoized solution will probably even be MUCH faster than that.)");
      else std::cout << " (Good!)" << std::endl;
      
      std::string found_palindrome = reconstructPalindrome(memo, str);
      std::cout << "Palindrome found (based on memoization): \"" << found_palindrome << "\"" << std::endl;
      if (found_palindrome != correct_palindrome_substring) errorReaction("The substring found is incorrect, so the memoization table\n is inaccurate or incomplete.");
      else std::cout << " (Correct!)" << std::endl;
    }

    std::cout << std::endl;
  }
  
  return 0;
}
```

**UnorderedMapCommon.h**<br>
```
#pragma once

include <string>
include <vector>
include <utility> // for std::pair
include <unordered_map> // for std::unordered_map
include <chrono> // for std::chrono::high_resolution_clock

// timer code to help catch mistakes(infinite loop)
using timeUnit = std::chrono::time_point<std::chrono::high_resolution_clock>;
static inline timeUnit getTimeNow() noexcept {
  return std::chrono::high_resolution_clock::now();
}
template <typename T>
static double getMilliDuration(T start_time, T stop_time) {
  std::chrono::duration<double, std::milli> dur_ms = stop_time - start_time;
  return dur_ms.count();
}

using StringVec = std::vector<std::string>;
using StringIntPair = std::pair<std::string, int>;
using StringIntMap = std::unordered_map<std::string, int>;
using StringIntPairVec = std::vector<std::StringIntPair>;

#include "IntPair.h"

using LengthMemo = std::unordered_map<IntPair, int>;
StringVec loadBookStrings(unsigned int min_word_length = 5);
bool wordCountComputer(const StringIntPair & x, const StringIntPair & y);
StringIntPairVec sortWordCounts(const StringIntMap & wordcount_map);
StringIntMap makeWordCounts(const StringVec & words);
int lookupWithFallback(const StringIntMap & wordcount_map, const std::string & key, int fallbackVal);
StringIntPairVec getBottomWordCounts(const StringIntPairVec & sorted_wordcounts, unsigned int max_words = 20);
StringIntPairVec getTopWordCounts(const StringIntPairVec & sorted_wordcounts, unsigned int max_words = 20);
int longestPalindromeLength(const std::string & str, int leftLimit, int rightLimit, timeUnit startTime, double maxDuration);
int memorizedLongestPalindromeLength(LengthMemo & memo, const std::string & str, int leftLimit, int rightLimit, timeUnit startTime, double maxDuration);
std::string reconstuctPalindrome(const LengthMemo & memo, const std::string & str);

// The unit tests handles this.
class TooSlowException : public std::runtime_error {
  public:
    using std::runtime_error;
}
```

**UnorderedMapCommon.cpp**<br>
```
#pragma once

#include <stdexcept> // for std::runtime_error
#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <cctype>
#include <algorithm>
#include <regex>

#include "UnorderedMapCommon.h"

StringVec loadBookStrings(unsigned int min_word_length) {
  static const std::string filenmae = "through_the_looking_glass.txt";
  static const std::string start_text = "CHAPTER I";
  static const std::string end_text = "End of the Project Gutenberg EBook";
  constexpr bool DEBUGGING = false;
  constexpr int DEBUGGING_MAX_WORDS = 30;
  
  std::ifstream instream(filename); // open the input file for reading with an ifstream(if=input file).
  if (!instream) {
    instream.close();
    throw std::runtime_error("Could not load book file: " + filename);
  }
  
  StringVec bookstrings; // prepare a vector of strings.
  
  // line変数をローカルにする為{ }で囲む。(この処理自体はメイン処理では無いから。)
  
  {
    std::string line;
    while (std::getline(instream, line)) {
      if (line.find(start_text) != std::string::npos) { // if not found at all, then this return the special value std::string::npos.
        break;
      }
    }
  }
  
  std::string line;
  bool stillReading = true;
  while (stillReading && std::getline(instream, line)) {
    if (line.find(end_text) != std::string::npos) {
      stillReading = false;
      break;
    }

    while (line.find("’") != std::string::npos) {
      line = std::regex_replace(line, std::regex("’"), "'"); // Replace right single quatation with plain apostrophe.
    }

    while (line.find("--") != std::string::npos) {
      line = std::regex_replace(line, std::regex("--"), " "); // Replace double dash with a space.
    }

    while (line.length() > 0) {
      // Keep only letters and convert to lower case.
      std::string trimmed_word = "";
      int consumedChars = 0;
      char prevChar = ' ';
      for (char c : line) {
        consumedChars++;
        if (std::isalpha(c)) {
          char thisChar = std::tolower(c);
          // 以下はword's や word-wordは１つの単語としてカウントする。
          if (prevChar == '\'') {
            trimmed_word += "'";
          }
          else if (prevChar == '-') {
            trimmed_word += '-';
          }
          trimmed_word += thisChar;
          prevChar = thisChar;
        }
        // word's や word-wordの形式でなければ'と-は無視する
        else if('\'' == c) {
          if (std::isalpha(prevChar)) {
            prevChar = '\'';
          }
          else {
            prevChar = ' ';
          }
        }
        else if('-' == c) {
          if (std::isalpha(prevChar)) {
            prevChar = '-';
          }
          else {
            prevChar = ' ';
          }
        }
        // 1つの単語が終了したら次の処理へ。
        else break;
      }
      
      line = line.substr(consumedChars);
      
      if (trimmed_word.length() >= min_word_length) {
        bookstrings.push_back(trimmed_word);
      }
      
      if (DEBUGGING && bookstrings.size() >= DEBUGGING_MAX_WORDS) {
        stillReading = false;
        break;
      }
    }
  }
  
  return bookstrings;
}

// For use with std::sort, it's VERY IMPORTANT to define a "<" and NOT "<=" otherwise std::sort may infinite loop.
bool wordCountComparator(const StringIntPair & x, const StringIntPair & y) {
  return x.second < y.second;
}

StringIntPairVec sortWordCounts(const StringIntMap & wordcount_map) {
  StringIntPairVec wordcount_vec;
  for (const StringIntPair & wc : wordcount_map) {
    wordcount_vec.push_back(wc); // copy all the entries from a map to a vector to sort with our comparator.
  }
  std::sort(wordcount_vec.begin(), wordcount_vec.end(), wordCountComparator);
  
  return wordcount_vec;
}

StringIntPairVec getBottomWordCounts(const StringIntPairVec & sorted_wordcounts, unsigned int max_words) {
  StringIntPairVec bottom_wordcounts;
  for (const auto & wordcount_item : sorted_wordcounts) {
    if (bottom_wordcounts.size() < max_words) {
      bottom_wordcounts.push_back(wordcount_item);
    }
    else break;
  }
  return bottom_wordcounts;
}

StringIntPairVec getTopWordCounts(const StringIntPairVec & sorted_wordcounts, unsigned int max_words) {
  StringIntPairVec top_wordcounts;
  for (auto rit = sorted_wordcounts.rbegin(); rit != sorted_wordcounts.rend(); rit++) {
    if (top_wordcounts.size() < max_words) {
      const auto& wordcount_item = *rit;
      top_wordcounts.push_back(wordcount_item);
    }
    else break;
  }
  return top_wordcounts;
}

int longestPalindromeLength(const std::string & str, int leftLimit, int rightLimit, timeUnit startTime, double maxDuration) {
  if (leftLimit > rightLimit) {
    return 0; // This could happen during recursive steps below.
  }
  
  if (leftLimit < 0) {
    throw std::runtime_error("leftLimit negative");
  }
  
  if (rightLimit < 0) {
    throw std::runtime_error("rightLimit negative, but it's not the base case"); // This could happen in the base case but it must be handled above already.
  }
  
  const auto currentTime = getTimeNow();
  const auto timeElapsed = getMilliDuration(startTime, currentTime);
  if (timeElapsed > maxDuration) {
    throw TooSlowException("taking too long");
  }
  
  if (leftLimit == rightLimit && str.at(leftLimit) == str.at(rightLimit)) {
    return 1;
  }
  
  if (str.at(leftLimit) == str.at(rightLimit)) { // If the first and last character match,
    int newLeft = leftLimit+1;   //move left limit to the right
    int newRight = rightLimit-1; //move right limit to the left
    int middleSubproblemResult = longestPalindromeLength(str, newLeft, newRight, startTime, maxDuration);
    int middleMaxLength = newRight-newLedt+1;
    
    if (middleMaxLength < 0) middleMaxLength = 0;
    
    if (middleSubproblemResult == middleMaxLength) {
      return 2+middleSubproblemResult;
    }
    // Otherwise don't return from function yet. We continue executing the code below
  }
  int leftSubproblemResult = longestPalingromeLength(str, leftLimit, rightLimit-1, startTime, maxDuration);
  int rightSubproblemResult = longestPalingromeLength(str, leftLimit+1, rightLimit, startTime, maxDuration);
  return std::max(leftSubproblemResult, rightSubproblemResult);
}

std::string reconstructPalindrome(const LengthMemo & memo, const std::string & str) {
  if (!str.length()) return "";
  int left = 0;
  int right = str.length()-1;
  
  LengthMemo edit_memo = memo;
  
  const int BEST_LEN = edit memo[std::make_pair(left, right)];
  
  bool loop_again = true;
  while (loop_again) {
    loop_again = false;
    
    int test_left, test_right;
    
    test_left = left+1;
    test_right = right;
    if (test_left <= test_right) {
      int test_len = edit_memo[std::make_pair(test_left, test_right)];
      if (test_len == BEST_LEN) {
        left = test_left;
        loop_again = true;
      }
    }
    
    test_left = left;
    test_right = right-1;
    if (test_left <= test_right) {
      int test_len = edit_memo[std::make_pair(test_left, test_right)];
      if (test_len == BEST_LEN) {
        right = test_right;
        loop_again = true;
      }
    }
    
    if (left <= right) {
      return str.substr(left, right-left+1);
    }
    else {
      return "";
    }
  }
}

```


**IntPair.h**<br>
```
#pragma once

#include <functional> // for std::hash
#include <utility> // for std::pair
#include <string> // for std::string


// We don't have to define an entire IntPair class ourselves since
// C++ provides a templated pair type already.
// We define IntPair as a type alias for convenience.
using IntPair = std::pair<int, int>;

// In the C++ standard library, classes like std::unordered_map and
// std::unordered_set use the default hashing object std::hash internally.
// However, std::hash doesn't natively support all possible types,
// just a few of the primitive C++ types like int and std::string.
// For more specific types, such as std::pair<int, int>, or for custom
// classes you make yourself, you need to specify a hashing function.
// Since std::hash is templated, we can add new cases for it using
// what is called "template specialization".

// In addition, custom class types made by users will also need to define
// their equivalence relation function, operator==, for this to work.
// Fortunately, std::pair<int,int> already gives us that automatically.

// ------------------------------------------------------------------

// Reference: https://en.cppreference.com/w/cpp/utility/hash

// We're adding more content to the standard namespace.
// (This isn't normally something you should do casually.)
namespace std {

  // A minor detail: std::hash is a struct, not a class.
  // In C++, a struct is very similar to a class, but it has public
  // members by default.

  // The "template <>" syntax indicates that we are specializing an existing
  // template for std::hash.

  template <>
  struct hash<IntPair> {

    // By overriding the () "operator", we can make an instance of a class
    // respond to similar syntax as if it were merely a function name.
    // The std::hash type is intended to work this way, as a "function object".
    // See also: https://en.cppreference.com/w/cpp/utility/functional

    // The () operator definition is where we will essentially define
    // our custom hashing function, and it returns the actual hash
    // as a std::size_t value (which is an integral type).
    std::size_t operator() (const IntPair& p) const {

      // We know that std::string has a well-defined hasher already,
      // so we'll turn our pair of ints into a unique string representation,
      // and then just hash that. We'll turn each integer into a string
      // and concatenate them with "##" in the middle, which should make
      // a unique string for any given pair of ints.
      std::string uniqueIntPairString = std::to_string(p.first) + "##" + std::to_string(p.second);

      // Get the default hashing function object for a std::string.
      std::hash<std::string> stringHasher;
      // Use the string hasher on our unique string.
      return stringHasher(uniqueIntPairString);
    }
  };

}
```

