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
 ・ Allocated(heap) memory ... The only way to create heap memory is with the new keyword. The new operator returns a **pointer** to the memory.メモリの下から積み上がる。<br>

**main.cpp**<br>
```
#include <iostream>

void foo() {
  int x = 42;
  std::cout << " x in foo(): " << x << std::endl;
  std::cout << "&x in foo(): " << &x << std::endl;
}

int main() {
  /* stack memoryの説明 */
  int num = 7;

  std::cout << " num in main(): " << num << std::endl;
  std::cout << "&num in main(): " << &num << std::endl;

  foo();

  int *p = &num;
  std::cout << " p: " << p << std::endl; // address, pointer変数の宣言時、typeに「 *」をつける。
  std::cout << "&p: " << &p << std::endl; // pointerのaddress(pより少し小さい, security researchの分野なので深追いは必要ない。)
  std::cout << "*p: " << *p << std::endl; // dereferencing(7)

  *p = 42;
  std::cout << "*p changed to 42" << p << std::endl;
  std::cout << " num:" << num << std::endl; // dereferencingを使い、置き換えられ7->42

  /* heap memoryの説明 */
  int *numPtr = new int; // heapにint情報のメモリを割当て、newを付けることでそのアドレスのみstackへ保存される
  std::cout << "*numPtr: " << *numPtr << std::endl;
  std::cout << " numPtr: " <<  numPtr << std::endl;
  std::cout << "&numPtr: " << &numPtr << std::endl;

  *numPtr = 42;
  std::cout << "*numPtr assigned 42." << std::endl;
  std::cout << "*numPtr: " << *numPtr << std::endl;
  std::cout << " numPtr: " <<  numPtr << std::endl;  // こっちはheapのアドレス
  std::cout << "&numPtr: " << &numPtr << std::endl;  // こっちはstackのアドレス(pointerのaddress)

  /* reference variable(alias)の説明 */
  int *x = new int;
  int &y = *x; // heapのint情報にyという名前を与える。「type &」はreference variableでエイリアスをつける。
  y = 4;

  std::cout << &x << std::endl;　// ポインタのstackアドレス
  std::cout <<  x << std::endl;　// x自体はheapのアドレス
  std::cout << *x << std::endl;  // 4(dereferencing)
  
  std::cout << &y << std::endl;　// heapのアドレス
  std::cout <<  y << std::endl;　// 4(y自体がheapを指す)
  //std::cout << *y << std::endl;  -> yはpointerでは無い為

  return 0;
}
```

**puzzle.cpp**<br>
```
#include <iostream>
#include "Cube.h"
using uiuc::Cube;

Cube *CreateCube() { // Cubeオブジェクトのpointerを返す関数なので、return typeに「 *」をつける。（返した形式と同じ形式にする必要がある。）
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

**heap.cpp**<br>
```
#include "Cube.h"
using uiuc::Cube;

int main() {
  /* heap memoryのサイクル */
  int *p = new int;
  Cube *c = new Cube; // heapにCubeのメモリを割当て、newを付けることでそのアドレスのみstackへ保存される

  *p = 42;
  (*c).setLength(4); // これは有効だがほとんど実際には見ない

  Cube *c2 = c; // heapのアドレスをstackのポインタに保存
  c->setLength(4); // stackからheapのオブジェクトにアクセスできる

  delete c; c = nullptr; // heapのCubeを削除、stackのcはこのままでは不安定の為、Null Pointer（0x0）をセットしてアクセスされた時は必ずエラーが発生するようにする。
  delete p; p = nullptr; // これでheapの積み上がったCubeとintが削除される
  delete c2; // これはcompile errorになる
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

puzzle: puzzle.cpp
  $(LD) $^ $(LDFLAGS) ./objs/Cube.o -o $@

heap: heap.cpp
	$(LD) $^ $(LDFLAGS) .objs/Cube.o -o $@

all: addressOf puzzle heap
CLEAN_RM += addressOf puzzle heap
```

**compile and execute**<br>
```
$ make
> g++ -std=c++14 -O0 -pedantic -Wall  -Wfatal-errors -Wextra  -MMD -MP -g -c  main.cpp -o .objs/main.o
> g++ -std=c++14 -O0 -pedantic -Wall  -Wfatal-errors -Wextra  -MMD -MP -g -c  Cube.cpp -o .objs/Cube.o
> g++ .objs/main.o .objs/Cube.o -std=c++14  -o main
> g++ puzzle.cpp -std=c++14  .objs/Cube.o -o puzzle
> puzzle.cpp:17:11: warning: address of stack memory associated with local variable 'cube' returned
>       [-Wreturn-stack-address]
>   return &cube;
>           ^~~~
> 1 warning generated.

$ ./main
>  num in main(): 7
> &num in main(): 0x7fff5571ca18
>  x in foo(): 42
> &x in foo(): 0x7fff5571c99c
>  p: 0x7fff5571ca18
> &p: 0x7fff5571ca10
> *p: 7
> *p changed to 42
>  num: 42
> *numPtr: 0
>  numPtr: 0x7fa038c03290
> &numPtr: 0x7fff5d03ba00
> *numPtr assigned 42.
> *numPtr: 42
>  numPtr: 0x7fa038c03290
> &numPtr: 0x7fff5d03ba00
> 0x7fff548c9a10
> 0x7fbb32c03290
> 4
> 0x7fbb32c03290
> 4

$ ./puzzle
> Surface Area: 0 <-- 本当なら6 * 15 * 15 (ローカル変数のメモリアドレスは返してはならない。)
> Volume: 0 <-- 本当なら15 * 15 * 15
```

**#include <...>**<br>
 This means compiler includes a standard library header from the system library.<br>
 The compiler will look for the ... header file in a system-wide library path that is located outside of current directory.<br>
<br>
**#include "--.h"**<br>
 In this time there is no need to write **#include "--.cpp"** . Because compiler generates objectfiles for each<br> "main.cpp" => "main.o" and "--.h" => "--.o". "--.cpp" files are linked by compiler, and linker combines files into executable "main" file.
<br>
<br>
**"--.o"**<br>
 This is an object file. Each cpp file is separately combined into object files. So Cube.cpp will be combined into Cube.o and main.cpp will be combined into main.o.<br>
 The main.o file will be linked against the compiled definitions in the Cube.o file. The linker program will also link system-wide object files, such as for iostream. After the compiler and linker programs finish processing the codes, then we can get an executable file as a result. In this case, that file name is simply named **main** .
<br>
<br>
**./main**<br>
 When we want to run a program that is in the current directory, we have to specify that by writing **./** in front of the name. Otherwise, Linux would look for system-wide command with that name instead.
<br>
<br>
**make clean**<br>
 This allow as clear out all the compiled object and executable files in order to ensure program get recompiled from scratch.
<br>
<br>
**Segfault (Segmentation fault)**<br>
case 1.
```
int* n = nullptr;
// This code might be compiled successfully, but this is called "undefined behavior". (単に未定義変数を参照したeasy miss.)
std::cout << *n << std::endl; // This dereference will almost certainly cause a segfault and crash immediately.
```
case 2.
```
int* x; // This pointer x contains a seemingly random memory address.
int* y2(nullptr); // built-in type int is not class types and there are no constructors. however we can still specify an initialization this way.
int* y3{nullptr}; // This is a new feature since C++11. This make it clear that we are performing an initialization not some kind of function call.
int h; // (built-in types are) not initialized
Box b; // (Class types are) ok.
```

