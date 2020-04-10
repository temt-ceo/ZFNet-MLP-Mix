
#### Object-Oriented Data Structures in C++ (Week4) submission task

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
```
