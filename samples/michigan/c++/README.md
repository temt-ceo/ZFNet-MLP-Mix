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
 ・ Pointers and dereferencing ... 変数のaddressを保存する変数。<br>
 ・ Local(stack) memory ... 関数の実行中のみ。実行後は解放。メモリがfffから始まるのは最高値から割当てるため。最高値から積み重なる。<br>
 ・ Allocated(heap) memory<br>

**addressOf.cpp**<br>
```
#include <iostream>

void foo() {
  int x = 42;
  std::cout << " x in foo(): " << x << std::endl;
  std::cout << "&x in foo(): " << &x << std::endl;
}

int main() {
  int num = 7;

  std::cout << " num in main(): " << num << std::endl;
  std::cout << "&num in main(): " << &num << std::endl;

  foo();
  
  return 0;
}
```

**puzzle.cpp**<br>
```
#include <iostream>
#include "Cube.h"
using uiuc::Cube;

Cube *CreateCube() { // Cubeオブジェクトのpointerを返す関数なので、返却値のtypeに「 *」をつける。（返した形式と同じ形式にする必要がある。）
  Cube cube;
  cube.setLength(15);
  return &cube; // addressを返す = pointerに保存される。この関数が終了するのでそのアドレスは解放される。
}

int main() {
  /* pointersの説明 */
  Cube *c = CreateCube(); // addressの変数をメモリに割当。実際はメモリの下の方にあるCube cubeを指す。
  someOtherFunction(); // ここの関数のメモリがCube cubeのメモリを使うため以降の処理は不可能になる。
  double a = c->getSurfaceArea();
  double v = c->getVolume();
  std::cout << "Surface Area: " << a << std::endl;
  std::cout << "Volume: " << v << std::endl;
  return 0;
}
```
上記は利用不可能なメモリをmain内で使用するのでコンパイル時に「warning: address of stack memory associated with local variable 'cube' returned [-Wreturn-stack-address]」と警告が出る。<br><br>
**Makefile**<br>
```
EXE = main
OBJS = main.o Cube.o
CLEAN_RM =

include ../_make/generic.mk

addressOf: addressOf.cpp
  $(LD) $^ $(LDFLAGS) -o $@

puzzle: puzzle.cpp
  $(LD) $^ $(LDFLAGS) ./objs/Cube.o -o $@

all: addressOf puzzle
CLEAN_RM += addressOf puzzle
```

**compile and execute**<br>
```
$ make
> g++ -std=c++14 -O0 -pedantic -Wall  -Wfatal-errors -Wextra  -MMD -MP -g -c  main.cpp -o .objs/main.o
> g++ -std=c++14 -O0 -pedantic -Wall  -Wfatal-errors -Wextra  -MMD -MP -g -c  Cube.cpp -o .objs/Cube.o
> g++ .objs/main.o .objs/Cube.o -std=c++14  -o main
> g++ addressOf.cpp -std=c++14  -o addressOf
> g++ puzzle.cpp -std=c++14  .objs/Cube.o -o puzzle
> puzzle.cpp:17:11: warning: address of stack memory associated with local variable 'cube' returned
>       [-Wreturn-stack-address]
>   return &cube;
>           ^~~~
> 1 warning generated.

$ ./addressOf
>  num in main(): 7
> &num in main(): 0x7fff52c0ba68
>  x in foo(): 42
> &x in foo(): 0x7fff52c0ba1c
$ ./puzzle
> Surface Area: 0 <-- 本当なら6 * 15 * 15
> Volume: 0 <-- 本当なら15 * 15 * 15
```
