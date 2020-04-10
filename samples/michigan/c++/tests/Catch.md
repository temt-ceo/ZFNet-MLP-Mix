#### widely-used C++ testing framework Catch

**part1.cpp**<br>
```
#include <type_traits>

#include "../uiuc/catch/catch.hpp"

#include "../ImageTransform.h"
#include "../uiuc/PNG.h"
#include "../uiuc/HSLAPixel.h"

namespace uiuc_testing {
  template< class T >
  struct is_double : std::integral_constant<bool, std::is_same<double, T>::value> {};
}

TEST_CASE("HSLAPixel should have member h as double", "[weight=1]") {
  REQUIRE(uiuc_testing::is_double<decltype(HSLAPixel::h)>::value);
}

TEST_CASE("HSLAPixel should have member s as double", "[weight=1]") {
  REQUIRE(uiuc_testing::is_double<decltype(HSLAPixel::s)>::value);
}

TEST_CASE("HSLAPixel should have member l as double", "[weight=1]") {
  REQUIRE(uiuc_testing::is_double<decltype(HSLAPixel::l)>::value);
}

TEST_CASE("HSLAPixel should have member a as double", "[weight=1]") {
  REQUIRE(uiuc_testing::is_double<decltype(HSLAPixel::a)>::value);
}

```
