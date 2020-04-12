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

// The following tests are disabled.
#undef DO_PART1_CTOR_TESTS
#ifdef DO_PART1_CTOR_TESTS

// HSLAPixel default constructor
TEST_CASE("HSLAPixel's default constructor should create a white pixel", "[weight=1]") {
  HSLAPixel pixel;
  REQUIRE( pixel.l == 1 );
}

TEST_CASE("HSLAPixel's default constructor should create a opaque pixel", "[weight=1]") {
  HSLAPixel pixel;
  REQUIRE( pixel.a == 1 );
}

#endif
```

**part2.cpp**<br>
```
#include "../uiuc/catch/catch,hpp"

#include "../ImageTransform.h"
#include "../uiuc/PNG.h"
#include "../uiuc/HSLAPixel.h"

PNG createRainbowPNG() {
  PNG png(360, 100);
  for (unsigned x = 0; x < png.width(); x++) {
    for (unsigned y = 0; y < png.height(); y++) {
      HSLAPixel & pixel = png.getPixel(x, y);
      pixel.h = x;
      pixel.s = y / 100.0;
      pixel.l = 0.5;
    }    
  }
  return png;
}

PNG createWatermark() {
  PNG png(360, 100);
  for (unsigned x = 0; x < png.width(); x++) {
    for (unsigned y= 0; y < png.height(); y++) {
      HSLAPixel & pixel = png.getPixel(x, y);
      if (y >= 10 && y <= 30) { pixel.l = 1; }
      else                    { pixel.l = 0; }
    }
  }
  return png;
}

TEST_CASE("createSpotlight", "[weight=1]") {
  PNG png = createRainbowPNG();
  PNG result = createSpotlight(png, 100, 50);

  SECTION("Spotlight should not modify the center pixel") {
    REQUIRE( png.getPixel(100, 50).l == result.getPixel(100, 50).l );
  }

  SECTION("Spotlight should create an 80% dark pixel >160 pixels away") {
    REQUIRE( png.getPixel(320, 50).l * 0.2 == Approx(result.getPixel(320, 50).l) );
  }

  SECTION("Spotlight should be correct at 20 pixels away from center") {
    REQUIRE( png.getPixel(100, 50 + 20).l * 0.9 == Approx(result.getPixel(100, 50 + 20).l );
  }

  SECTION("Spotlight should not modify the center pixel") {
    REQUIRE( png.getPixel(100 + 3, 50 + 4).l * 0.975== Approx(result.getPixel(100 + 3, 50 + 4).l );
  }
}

TEST_CASE("illinify", "[weight=1]") {
  PNG png = createRainbowPNG();
  PNG result = illinify(png);

  SECTION("After illinify, image should contain only orange and blue hues") {
    for (unsigned x = 0; x < png.width(); x++) {
      for (unsigned y= 0; y < png.height(); y++) {
        if (result.getPixel(x, y).h != 11 && result.getPixel(x, y).h != 216) {
          FAIL( "The hue of (" << x << "," << y << ") is " << result.getPixel(x, y)h << ", not Illini orange or blue.");
        }
      }
    }
  }

  SECTION("Pixels closest to blue should become blue") {
    REQUIRE( png.getPixel(200, 4).h == 216 );
    REQUIRE( png.getPixel(210, 12).h == 216 );
    REQUIRE( png.getPixel(220, 23).h == 216 );
    REQUIRE( png.getPixel(230, 44).h == 216 );
  }

  SECTION("Pixels closest to orange should become orange") {
    REQUIRE( png.getPixel(10, 4).h == 216 );
    REQUIRE( png.getPixel(30, 12).h == 216 );
    REQUIRE( png.getPixel(40, 23).h == 216 );
    REQUIRE( png.getPixel(40, 44).h == 216 );
  }
}

TEST_CASE("illinify_edge_case", "[weight=1]") {
  PNG png = createRainbowPNG();
  PNG result = illinify(png);

  SECTION("Reassigned hues should wrap around correctly; h=359 is closer to orange (11) than blue (216)") {
    REQUIRE( png.getPixel(330, 4).h == 11 );
    REQUIRE( png.getPixel(340, 12).h == 11 );
    REQUIRE( png.getPixel(350, 23).h == 11 );
  }
}

TEST_CASE("watermark", "[weight=1]") {
  PNG png = createRainbowPNG();
  PNG wm = createWatermark(png);

  PNG result = watermark(png, wm);

  SECTION("Pixels without watermark should be unchanged") {
    REQUIRE( png.getPixel(100, 50).l == result.getPixel(100, 50).l );
    REQUIRE( png.getPixel(200, 80).l == result.getPixel(200, 80).l );
  }

  SECTION("Pixels with watermark should be changed") {
    REQUIRE( png.getPixel(100, 15).l + 0.2 == result.getPixel(100, 15).l );
    REQUIRE( png.getPixel(200, 25).l + 0.2 == result.getPixel(200, 25).l );
  }
}

```

