
#### Object-Oriented Data Structures in C++ (Week4) submission task

**ImageTransform.h**<br>
```
#pragma once

#include "uiuc/PNG.h"
using namespace uiuc;

PNG grayscale(PNG image);
PNG createSpotlight(PNG image, int centerX, int centerY);
PNG illinify(PNG image);
PNG watermark(PNG image, PNG secondImage);

```

**ImageTransform.cpp**<br>
```
#include <iostream>
#include <cmath>
#include <cstdlib>

#include "uiuc/PNG.h"
#include "uiuc/HSLAPixel.h"
#include "ImageTransform.h"

/* ******************


Write your name and email address in the comment space here:

Name: Takashi Tahara
Email: t****@outlook.com


******************** */

using uiuc::PNG;
using uiuc::HSLAPixel;

/**
 * Returns an image that has been transformed to grayscale.
 *
 * The saturation of every pixel is set to 0, removing any color.
 *
 * @return The grayscale image.
 */
PNG grayscale(PNG image) {
  for (unsigned x = 0; x < image.width(); x++) {
    for (unsigned y = 0; y < image.height(); y++) {
      HSLAPixel & pixel = image.getPixel(x, y);
      // Since `pixel` is a reference to the memory stored inside of the PNG `image`, no need to `set` the pixel.
      pixel.s = 0; // a pixel with a saturation set to 0%
will be a gray pixel 
    }
  }
  
  return image;
}

/**
 * Returns an image with a spotlight centered at (`centerX`, `centerY`).
 *
 * A spotlight adjusts the luminance of a pixel based on the distance the pixel is away from the center 
 * by decreasing the luminance by 0.5% per 1 pixel euclidean distance away the center.
 *
 * @param image A PNG object which holds the image to be modified
 * @param centerX The center x coordinate of the crosshair which is to be drawn.
 * @param centerY The center y coordinate of the crosshair which is to be drawn.
 *
 * @return The image with a spotlight.
 */
PNG createSpotlight(PNG image, int centerX, int centerY) {
  for (unsigned x = 0; x < image.width(); x++) {
    for (unsigned y = 0; y < image.height(); y++) {
      HSLAPixel & pixel = image.getPixel(x, y);
      int diff_x = (int)x - centerX;
      int diff_y = (int)y - centerY;
      double euclidean = sqrt(diff_x * diff_x + diff_y * diff_y);
      if (euclidean >= 160) {
        pixel.l = pixel.l * 0.2;
      } else {
        double darken_rate =  euclidean * 0.5;
        // luminance - (luminance * darken_rate(%))
        pixel.l = pixel.l - (pixel.l * darken_rate * 0.01);
      }
    }
  }
  
  return image;
}

/**
 * Returns an image transformed to Illini colors(イリノイ大学のシンボルカラー).
 *
 * The hue of every pixel is set to the a hue value of either orange or blue, 
 * based on if the pixel's hue value is closer to orange than blue.
 *
 * @param image A PNG object which holds the image data to be modified
 *
 * @return The illinify'd image.
 */
PNG illinify(PNG image) {
  double illini_orange = 11;
  double illini_blue = 216;
  for (unsigned x = 0; x < image.width(); x++) {
    for (unsigned y = 0; y < image.height(); y++) {
      HSLAPixel & pixel = image.getPixel(x, y);
      double target_hue = pixel.h;
      if (target_hue <= illini_orange) {
        pixel.h = illini_orange;
      } else if (target_hue <= illini_blue) {
        if ((target_hue - illini_orange) < (illini_blue - target_hue)) {
          pixel.h = illini_orange;
        } else {
          pixel.h = illini_blue;
        }
      } else {
        if ((target_hue - illini_blue) < (illini_orange + 360 - target_hue)) {
          pixel.h = illini_blue;
        } else {
          pixel.h = illini_orange;
        }
      }
    }
  }

  return image;
}

/**
 * Returns an image that has been watermarked by another image.
 *
 * The luminance of every pixel  of the second image is checked, if that pixel's luminance is 1 (100%),
 * then the pixel at the same location on the first image has its luminance increased by 0.2.
 *
 * @param firstImage The first of the two PNGs, which is the base image.
 * @param secondImage The second of the two PNGs, which acts as the stencil.
 *
 * @return The watermarked image.
 */
PNG watermark(PNG firstImage, PNG secondImage) {
  for (unsigned x = 0; x < firstImage.width(); x++) {
    if (x < secondImage.width()) {
      for (unsigned y = 0; y < firstImage.height(); y++) {
        if (y < secondImage.height()) {
          HSLAPixel & pixel_s = secondImage.getPixel(x, y);
          if (pixel_s.l == 1.0) {
            HSLAPixel & pixel_f = firstImage.getPixel(x, y);
            pixel_f.l = pixel_f.l > 0.8 ? 1.0 : pixel_f.l + 0.2;
          }
        }
      }
    }
  }

  return firstImage;
}

```

**uiuc/HSLAPixel.h**<br>
```
#pragma once

#include <iostream>
#include <sstream>

namespace uiuc {
  class HSLAPixel {
    public:
      double h; // the hue
      double s; // the saturation
      double l; // the luminance
      double a; // the alpha      
  }
}
```

**uiuc/HSLAPixel.cpp**<br>
```
#include <cmath>
#include <iostream>
#include "HSLAPixel.h"
using namespace std;

namespace uiuc {
}
```

**uiuc/PNG.h**<br>
```
#pragma once

#include <string>
#include <vector>
#include "HSLAPixcel.h"

using namespace std;

namespace uiuc {
  class PNG {
    public:
      PNG(); // -> creates an empty PNG image
      PNG(unsigned int width, unsigned int height); // -> Creates a PNG image of the specified dimensions.
      PNG(PNG const & other);
      ~PNG();
      bool operator== (PNG const & other) const; // -> Equality operator: checks if two images are the same.
      bool operator!= (PNG const & other) const; // -> Equality operator: checks if two images are different.
      bool readFromFile(string const & filename);
      bool writeToFile(string const & filename);

      HSLAPixcel & getPixel(unsigned int x, unsigned int y) const; // -> Gets a reference to the pixel at the given coordinates in the image.
      
      unsigned int width() const;
      unsigned int height() const;
      void resize(unsigned int newWidth, unsigned int newHeight);

      std::size_t computeHash() const; // -> Computes a hash of the image.

    private:
      unsigned int width_;
      unsigned int height_;
      HSLAPixel *imageData_;
      HSLAPixel defaultPixel_;
      void _copy(PNG const & other);
  };

  std::ostream & operator<<(std::ostream & out, PNG const & pixel);
  std::stringstream & operator<<(std::stringstream & out, PNG const & pixel);
}
```

**uiuc/PNG.cpp**<br>
```
#include <iostream>
#include <string>
#include <algorithm>
#include <functional>
#include <cassert>
#include "lodepng/lodepnh.h"
#include "HSLAPixel.h"
#include "PNG.h"
#include "RGB_HSL.h"

namespace uiuc {
  void PNG::_copy(PNG const & other) {
    delete[] imageData_; // Clear self
    width_ = other.width_;
    height_ = other.height_;
    imageData_ = new HSLAPixel(wisth_ * height_);
    for (unsigned i = 0; i < width_ * height_; i++) {
      imageData_[i] = other.imageData_[i];
    }
  }

  // creates an empty PNG image
  PNG() {
    width_ = 0;
    height_ = 0;
    imageData_ = new HSLAPixel[width * height];
  }
  
  // Creates a PNG image of the specified dimensions.
  PNG(unsigned int width, unsigned int height) {
    width_ = width;
    height_ = height;
    imageData_ = new HSLAPixel[width * height];
  }

  PNG(PNG const & other) {
    imageData_ = NULL;
    _copy(other);
  }

  ~PNG() {
    delete[] imageData_;
  }

  PNG const & PNG::operator=(PNG const & other) {
    if (this != &other) { _copy(other); }
    return *this
  }
  
  // Equality operator: checks if two images are the same.
  bool operator== (PNG const & other) const {
    if (width_ != other.width_) { return false; }
    if (height_ != other.height_) { return false; }

    for (unsigned i = 0; i < width_ * height_; i++) {
      HSLAPixel & p1 = imageData_[i];
      HSLAPixel & p2= other.imagData_[i]
      if (p1.h != p2.h || p1.s != p2.s || p1.l != p2.l || p1.a != p2.a) { return false; }
    }

    return true;
  }

  // Equality operator: checks if two images are different.
  bool operator!= (PNG const & other) const {
    return !(*this == other);
  }

  bool readFromFile(string const & filename) {
    vector<unsigned char>byteData;
    unsigned error = lodepng::decode(byteData, width_, height_, fileName);

    if (error) {
      cerr << "PNG decoder error " << error << ": " << lodepng_error_text(error) << endl;
      return false;
    }

    delete[] imageData_;
    imageData_ = new HSLAPixel[width_ * height_]l

    for (unsigned i = 0; i < byteData.size(); i += 4) {
      rgbaColor rgb;
      rgb.r = byteData[i];
      rgb.g = byteData[i + 1];
      rgb.b = byteData[i + 2];
      rgb.a = byteData[i + 3];

      hslaColor hsl = rgb2hs(rgb);
      HSLAPixel & pixel = imageData_[i/4];
      rgb.h = hsl.h;
      rgb.s = hsl.s;
      rgb.l = hsl.l;
      rgb.a = hsl.a;
    }

    return true;
  }
      
  bool writeToFile(string const & fileName) {
    unsigned char *byteData = new unsigned char[width_ * height_ * 4];

    for (unsigned i = 0; i < width_ * height_; i++) {
      hslaColor hsl;
      hsl.h = imageData_[i].h;
      hsl.s = imageData_[i].s;
      hsl.l = imageData_[i].l;
      hsl.a = imageData_[i].a;

      rgbaColor rgb = hsl2rgb(hsl);
      byteData[(i * 4)] = rgb.r;
      byteData[(i * 4) + 1] = rgb.g;
      byteData[(i * 4) + 2] = rgb.b;
      byteData[(i * 4) + 3] = rgb.a;
    }

    unsigned error = lodepng::encode(fileName, byteData, width_, height_);
    if (error) {
      cerr << "PNG encoding error " << error << ": " << lodepng_error_text(error) << endl;
    }

    delete[] byteData;
    return (error == 0);
  }

  // Gets a reference to the pixel at the given coordinates in the image.
  HSLAPixcel & getPixel(unsigned int x, unsigned int y) const {
    if (width_ == 0 || height_ == 0) {
      cerr << "ERROR: Call to uiuc::PNG::getPixel() made on an image width no pixels." << endl;
      assert(width_ > 0);
      assert(width_ > 0);
    }

    if (x >= width_) {
      cerr << "WARNING: Call to uiuc::PNG::getPixel(" << x << "," << y << ") tries to access x=" << x
          << ", which is outside of the image (image width: " << width_ << ")." << endl;
      cerr << "       : Truncating x to " << (width_ - 1) << endl;
      x = width_ - 1;
    }

    if (y >= height_) {
      cerr << "WARNING: Call to uiuc::PNG::getPixel(" << x << "," << y << ") tries to access y=" << y
          << ", which is outside of the image (image height: " << height_ << ")." << endl;
      cerr << "       : Truncating y to " << (height_ - 1) << endl;
      y = height_ - 1;
    }

    unsigned index = x + (y * width_);
    return imageData_[index];
  }
      
  unsigned int width() const {
    return width_;
  }

  unsigned int height() const {
    return height_;
  }

  void resize(unsigned int newWidth, unsigned int newHeight) {
    HSLAPixel * newImageData = new HSLAPixel[newWidth * newHeight];

    // Copy the current data to the new image data, using the existing pixel for coordinates within the bounds of the old image size.
    for (unsigned x = 0; x < newWidrh; x++) {
      for (unsigned y = 0; x < newHeight; y++) {
        if (x < width_ && y << height_) {
          HSLAPixel & oldPixel = this->getPixel(x, y);
          HSLAPixel & newPixel = newImageData[(x + (y * newWidth))];
        }
      }      
    }
    // Clear the existing image
    delete[] imageData_;
    // Update the image to reflect the new image size and data
    width_ = newWidth;
    height_ = newHeight;
    imageData_ = newImageData;
  }

  // Computes a hash of the image.
  std::size_t computeHash() const {
    std::hash<float> hashFunction;
    std::size_t hash = 0;
    
    for (unsigned x = 0; x < this->width(); x++) {
      for (unsigned y = 0; x < this->height(); y++) {
        HSLAPixel & pixel = this=>getPixel(x, y);
        hash = (hash << 1) + hash + hashFunction(pixel.h);
        hash = (hash << 1) + hash + hashFunction(pixel.s);
        hash = (hash << 1) + hash + hashFunction(pixel.l);
        hash = (hash << 1) + hash + hashFunction(pixel.a);
      }
    }
    
    return hash;
  }

  std::ostream & operator << ( std::ostream & os, PNG const & png) {
    os << "PNG(w=" << png.width() << ", h=" << png.height() << ", hash=" << std::hex << png.computeHash() << std::dec << ")";
    return os;
  }
}
```

**main.cpp**<br>
```
#include "ImageTransform.h"
#include "uiuc/PNG.h"

int main() {
  uiuc::PNG png, png2, result;
  
  png.readFromFile("alma.png");
  result = grayscale(png);
  result.writeToFile("out-grayscale.png");

  result = createSpotlight(png, 450, 150);
  result.writeToFile("out-spotlight.png");

  result = illinify(png);
  result.writeToFile("out-illinify.png");

  png2.readFromFile("overlay.png");
  result = watermark(png, png2);
  result.writeToFile("out-watermark.png");
}
```

**Makefile**<br>
```
# Excutable names:
EXE = ImageTransform
TEST = test

# Add all object files needed for compiling:
EXE_OBJ = main.o
OBJS = main.o ImageTransform.o

# Generated files
CLEAN_RM = out-*.png

# Include the master templated makefile:
include uiuc/make/uiuc.mk
```

**compile and execute**<br>
```
$ make
> g++  -std=c++14   -g -O0 -pedantic -Wall -Wfatal-errors -Wextra -Wno-unused-parameter -Wno-unused-variable -MMD -MP -msse2 -c main.cpp -o .objs/main.o
> g++  -std=c++14   -g -O0 -pedantic -Wall -Wfatal-errors -Wextra -Wno-unused-parameter -Wno-unused-variable -MMD -MP -msse2 -c ImageTransform.cpp -o .objs/ImageTransform.o
> g++  -std=c++14   -g -O0 -pedantic -Wall -Wfatal-errors -Wextra -Wno-unused-parameter -Wno-unused-variable -MMD -MP -msse2 -c uiuc/HSLAPixel.cpp -o .objs/uiuc/HSLAPixel.o
> g++  -std=c++14   -g -O0 -pedantic -Wall -Wfatal-errors -Wextra -Wno-unused-parameter -Wno-unused-variable -MMD -MP -msse2 -c uiuc/PNG.cpp -o .objs/uiuc/PNG.o
> In file included from uiuc/PNG.cpp:16:
> uiuc/PNG.h:121:15: warning: private field 'defaultPixel_' is not used
>       [-Wunused-private-field]
>     HSLAPixel defaultPixel_;        /*< Default pixel, returned in case...
>               ^
> 1 warning generated.
> g++  -std=c++14   -g -O0 -pedantic -Wall -Wfatal-errors -Wextra -Wno-unused-parameter -Wno-unused-variable -MMD -MP -msse2 -c uiuc/lodepng/lodepng.cpp -o .objs/uiuc/lodepng/lodepng.o
> g++ .objs/main.o .objs/ImageTransform.o .objs/uiuc/HSLAPixel.o .objs/uiuc/PNG.o .objs/uiuc/lodepng/lodepng.o  -std=c++14   -lpthread -o ImageTransform
> 
>  Built the main executable program file for the project:  ImageTransform
>  (Make sure you try "make test" too!)
> 
$ make clean
$ make test
> g++  -std=c++14   -g -O0 -pedantic -Wall -Wfatal-errors -Wextra -Wno-unused-parameter -Wno-unused-variable -MMD -MP -msse2 -c tests/part1.cpp -o .objs/tests/part1.o
> g++  -std=c++14   -g -O0 -pedantic -Wall -Wfatal-errors -Wextra -Wno-unused-parameter -Wno-unused-variable -MMD -MP -msse2 -c tests/part2.cpp -o .objs/tests/part2.o
> g++  -std=c++14   -g -O0 -pedantic -Wall -Wfatal-errors -Wextra -Wno-unused-parameter -Wno-unused-variable -MMD -MP -msse2 -c uiuc/catch/catchmain.cpp -o .objs/uiuc/catch/catchmain.o
> g++ .objs/ImageTransform.o .objs/uiuc/HSLAPixel.o .objs/uiuc/PNG.o .objs/uiuc/lodepng/lodepng.o .objs/tests/part1.o .objs/tests/part2.o .objs/uiuc/catch/catchmain.o  -std=c++14   -lpthread -o test
> 
>  Built the test suite program:  test
> 
$ ./ImageTransform
> 
$ ./test
  (pass)
$ make zip
> form.h ImageTransform.cpp
>   adding: uiuc/HSLAPixel.h (deflated 52%)
>   adding: uiuc/HSLAPixel.cpp (deflated 32%)
>   adding: ImageTransform.h (deflated 34%)
>   adding: ImageTransform.cpp (deflated 61%)
> Created zip file:  ImageTransform_submission.zip
```




**uiuc/make/uiuc.mk**(for the reference; 参考までに)<br>
```
# Created by Wade Fagen-Ulmschneider <waf@illinois.edu>
# A few tweaks for CS 400 by Eric Huber

ZIP_FILE = ImageTransform_submission.zip
COLLECTED_FILES = uiuc/HSLAPixel.h uiuc/HSLAPixel.cpp ImageTransform.h ImageTransform.cpp

# Add standard object files (HSLAPixel, PNG, and LodePNG)
OBJS += uiuc/HSLAPixel.o uiuc/PNG.o uiuc/lodepng/lodepng.o

# Use ./.objs to store all .o file (keeping the directory clean)
OBJS_DIR = .objs

# Use all .cpp files in /tests/
OBJS_TEST = $(filter-out $(EXE_OBJ), $(OBJS))
CPP_TEST = $(wildcard tests/*.cpp)
CPP_TEST += uiuc/catch/catchmain.cpp
OBJS_TEST += $(CPP_TEST:.cpp=.o)

# Config
CXX_CLANG = clang++
CXX_GNU = g++
CXX_WHICH = $(CXX_GNU)
CXX = $(CXX_WHICH)
LD = $(CXX_WHICH)
# STDVERSION = -std=c++1y # deprecated nomenclature
STDVERSION = -std=c++14 # proper but requires newer compiler versions (for better or worse)
STDLIBVERSION_CLANG = -stdlib=libc++ # Clang's version; not present on default AWS Cloud9 instance
STDLIBVERSION_GNU =   # blank on purpose; default GNU library
STDLIBVERSION = $(STDLIBVERSION_GNU)
WARNINGS = -pedantic -Wall -Wfatal-errors -Wextra -Wno-unused-parameter -Wno-unused-variable
CXXFLAGS = $(CS400) $(STDVERSION) $(STDLIBVERSION) -g -O0 $(WARNINGS) -MMD -MP -msse2 -c
LDFLAGS = $(CS400) $(STDVERSION) $(STDLIBVERSION) -lpthread
ASANFLAGS = -fsanitize=address -fno-omit-frame-pointer

#  Rules for first executable
$(EXE):
	$(LD) $^ $(LDFLAGS) -o $@
	@echo ""
	@echo " Built the main executable program file for the project: " $(EXE)
	@echo " (Make sure you try \"make test\" too!)"
	@echo ""

# Rule for `all`
all: $(EXE) $(TEST)

# Pattern rules for object files
$(OBJS_DIR):
	@mkdir -p $(OBJS_DIR)
	@mkdir -p $(OBJS_DIR)/uiuc
	@mkdir -p $(OBJS_DIR)/uiuc/catch
	@mkdir -p $(OBJS_DIR)/uiuc/lodepng
	@mkdir -p $(OBJS_DIR)/tests

$(OBJS_DIR)/%.o: %.cpp | $(OBJS_DIR)
	$(CXX) $(CXXFLAGS) $< -o $@

# Rules for executables
$(TEST):
	$(LD) $^ $(LDFLAGS) -o $@
	@echo ""
	@echo " Built the test suite program: " $(TEST)
	@echo ""

# Executable dependencies
$(EXE): $(patsubst %.o, $(OBJS_DIR)/%.o, $(OBJS))
$(TEST): $(patsubst %.o, $(OBJS_DIR)/%.o, $(OBJS_TEST))

# Include automatically generated dependencies
-include $(OBJS_DIR)/*.d
-include $(OBJS_DIR)/uiuc/*.d
-include $(OBJS_DIR)/uiuc/catch/*.d
-include $(OBJS_DIR)/uiuc/lodepng/*.d
-include $(OBJS_DIR)/tests/*.d

clean:
	rm -rf $(EXE) $(TEST) $(OBJS_DIR) $(CLEAN_RM) $(ZIP_FILE)

tidy: clean
	rm -rf doc

zip:
	@echo "!!! Preparing submission zip with student code..."
	@echo "!!! Make sure you have already tried compiling and testing your code"
	@echo "!!! thoroughly before submitting the zip on Coursera!"
	@echo ""
	@echo "Removing any previous version of zip file..."
	rm -rf $(ZIP_FILE)
	@echo "Creating new file..."
	zip $(ZIP_FILE) $(COLLECTED_FILES)
	@echo "Created zip file: " $(ZIP_FILE)

.PHONY: all tidy clean zip
```
