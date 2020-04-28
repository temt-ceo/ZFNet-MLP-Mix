
#### Unordered Data Structures in C++ (Week1) submission task

**UnorderedMapExcercises.cpp**<br>
```
#include <iostream>

#include "UnorderedMapCommon.h"

StringIntMap makeWordCounts(const StringVec & words) {
  StringIntMap wordcount_map;
  
  return wordcount_map;
}

int lookupWithFallback(const StringIntMap & wordcount_map, const std::string & key, int fallbackVal) {
  return -1337;
}

/* memoize: 実行速度を上げる為に処理の重い実行結果をキャッシュする事 */
int memoizedLongestPalindromeLength(LengthMemo & memo, const std::string & str, int leftLimit, intRightLimit, timeUnit startTime, double maxDuration) {
  if (leftLimit < 0 && leftLimit <= rightLimit) {
    throw std::runtime_error("leftLimit negative, but it's not the base case");
  }
  if (rightLimit < 0 && leftLimit <= rightLimit) {
    throw std::runtime_error("rightLimit negative, but it's not the base case");
  }
  
  const auto currentTime = getTimeNow();
  const auto timeRlapsed = getMilliDuration(startTime, currentTime);
  
  if (memo.count(pairKey)) {
    // ===============================================================
    // EXERCISE 3 - PART A - YOUR CODE HERE!
    return -1337;
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
  int greaterResult = std::max(leftSubproblemResult, rightSubproblemResult);
  
  // ===============================================================
  // EXERCISE 3 - PART A - YOUR CODE HERE!
  return -1337;

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
