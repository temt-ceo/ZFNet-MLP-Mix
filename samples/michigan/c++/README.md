### Additional References for C++
#### ※Keep in mind in this document we are discussing C++14.
**◉searchable documentation**<br>
http://www.cplusplus.com/
<br><br>
**◉reference of standard library**<br>
https://en.cppreference.com/w/
<br><br>
**◉useful paperbook for the c++**<br>
https://stackoverflow.com/questions/388242/the-definitive-c-book-guide-and-list

## University of Illinois study memo
#### Week1. C++ Classes
**Cube.h**<br>
```
#pragma once

namespace uiuc {
  class Cube {
    public:
      double getVolume();
      double getSurfaceArea();
      void setLength(double length);

    private:
      double length_;
  };
}
```

**Cube.cpp**<br>
```
#include "Cube.h"

namespace uiuc {
  double Cube::getVolume() {
    return length_ * length_ * length_;
  }

  double Cube::getSurfaceArea() {
    return 6 *length_ * length_l
  }

  void Cube::setLength(double length) {
    length_ = length;
  }
}
```

**main.cpp**<br>
This also includes C++'s Standard Library(std) memo
```
#include <iostream>
#include "Cube.h"

using std::cout;
using std::endl;

int main() {
  uiuc::Cube c;

  c.setLength(3.48);
  double volume = c.getVolume();
  cout << "Volume: " << volume << endl; // cout means console out. endl means end of line.

  return 0;
}
```

**Makefile**<br>
```
EXE = main
OBJS = main.o Cube.o
CLEAN_RM =

include ../_make/generic.mk
```

**compile and execute**<br>
```
$ make
> g++ -std=c++14 -O0 -pedantic -Wall  -Wfatal-errors -Wextra  -MMD -MP -g -c  main.cpp -o .objs/main.o
> g++ -std=c++14 -O0 -pedantic -Wall  -Wfatal-errors -Wextra  -MMD -MP -g -c  Cube.cpp -o .objs/Cube.o
> g++ .objs/main.o .objs/Cube.o -std=c++14  -o main
$ ./main
> Volume: 42.1442
```
<br>

#### Week2. Stack/Heap Memory
**Key Concepts**<br>
 ・ Pointers and dereferencing<br>
 ・ Local(stack) memory<br>
 ・ Allocated(heap) memory<br>

**addressOf.cpp**<br>
```
#include <iostream>

int main() {
  int num = 7;

  std::cout << "Value: " << num << std::endl;
  std::cout << "Address: " << &num << std::endl;

  return 0;
}
```

**Makefile**<br>
```
EXE = main
OBJS = main.o Cube.o
CLEAN_RM =

include ../_make/generic.mk

addressOf: addressOf.cpp
  $(LD) $^ $(LDFLAGS) -o $@

foo: foo.cpp
  $(LD) $^ $(LDFLAGS) -o $@

puzzle: puzzle.cpp
  $(LD) $^ $(LDFLAGS) ./objs/Cube.o -o $@

all: addressOf foo puzzle
CLEAN_RM += addressOf foo puzzle
```

**compile and execute**<br>
```
$ make
> g++ -std=c++14 -O0 -pedantic -Wall  -Wfatal-errors -Wextra  -MMD -MP -g -c  main.cpp -o .objs/main.o
> g++ -std=c++14 -O0 -pedantic -Wall  -Wfatal-errors -Wextra  -MMD -MP -g -c  Cube.cpp -o .objs/Cube.o
> g++ .objs/main.o .objs/Cube.o -std=c++14  -o main
> g++ addressOf.cpp -std=c++14  -o addressOf
> g++ foo.cpp -std=c++14  -o foo
> g++ puzzle.cpp -std=c++14  .objs/Cube.o -o puzzle
> puzzle.cpp:17:11: warning: address of stack memory associated with local variable 'cube' returned
>       [-Wreturn-stack-address]
>   return &cube;
>           ^~~~
> 1 warning generated.

$ ./addressOf
> Value: 7
> Address: 0x7fff55b5da48
```
