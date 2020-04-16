
#### Object-Oriented Data Structures in C++ (Week4) submission task

**LinkedList.h**<br>
```
#pragma once

#include <stdexcept> // for std::runtime_error
#include <iostream> // for std::cerr, std::cout
#include <ostream> // for std::ostream

// LinkedList class: Adoubly-linked list.
template <typename T>
class LinkedList {
  public:
      class Node {
          Node* next;
          Node* prev;
          T data;
          Node() : next(nullptr), prev(nullptr) {} // Default constructor
          Node(const T& dataArg) : next(nullptr), prev(nullptr), data(dataArg) {} // Argument constructor(the data should be copied.)
          Node(const Node& other) : next(nullptr), prev(nullptr), data(other.data) {} // Copy constructor
      
          // Copy assignment operator
          Node& operator=(const Node& other) {
            next = other.next;
            prev = other.prev;
            data = other.data;
            return *this;
          }
          ~Node() {}
      };
  private:
      Node* head_;
      Node* head_;
      int size_;
    
  public:
    static constexpr char LIST_GENERAL_BUG_MESSAGE[] = "[ERROR] Probable causes: wrong head_ or tail_ pointer, or some next or prev pointer not updated, or wrong size_";
    Node* getHeadPtr() { return head_; }
    Node* getTailPtr() { return tail_; }
    int size() const { return size_; }
    bool empty() const { return !head_; }
    
    // Return a reference to the actual front data.
    T& front() {
      if (!head_) {
        throw std::runtime_error("front() called on empty LinkedList");
      }
      else {
        return head_->data;
      }
    }

    // Instanceがconstの場合の為にoverloadしたfront()。
    const T& front() const {
      if (!head_) {
        throw std::runtime_error("front() called on empty LinkedList");
      }
      else {
        return head_->data;
      }      
    }

    T& back() {
      if (!tail_) {
        throw std::runtime_error("back() called on empty LinkedList");
      }
      else {
        return tail_->data;
      }
    }

    const T& back() const {
      if (!tail_) {
        throw std::runtime_error("back() called on empty LinkedList");
      }
      else {
        return tail_->data;
      }
    }
    
    void pushFront(const T& newData); // Push a copy of the new data item onto the front of the list.
    void pushBack(const T& newData);
    void popFront(); // Delete the front item.
    void popBack();
    void clear() { // Delete all items.
      while (head_) {
        popBack();
      }
    
      if (0 != size_) throw std::runtime_error(std::string("Error in clear: ") + LIST_GENERAL_BUG_MESSAGE);
    }
    
    bool equals(const LinkedList<T>& other) const;
    bool operator==(const LinkedList<T>& other) const {
      return equals(other);
    }
    bool operator!=(const LinkedList<T>& other) const {
      return !equals(other);
    }
    // This requires that the data type T supports stream output iteself. This is used by the operator<< overload defined in this file.
    std::ostream& print(std::ostream& os) const;

    // Assuming the list was previously sorted.
    void insertOrdered(const T& newData);
    bool isSorted() const;
    
    // The insertion sort algorythm thar relies on insertOrdered. This is not an efficient operation; insertion sort is O(n^2).
    LinkedList<T> insertionSort() const;
    LinkedList<LinkedList<T>> splitHalves() const;
    // [1,2,3] => [[1],[2],[3]]
    LinkedList<LinkedList<T>> explode() const;
    
    // Assuming this list instance is currently sorted, and the "other" list is also already sorted.
    LinkedList<T> merge(const LinkedList<T>& other) const;
    LinkedList<t> mergeSort() const; // wrapper function that call one of either mergeSortRecursive or mergeSortIterative.
    
    
}

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
