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

  /* reference variable(alias)の説明(reference variableはポインターと違ってアドレスが無い。エイリアス先そのものを指す。) */
  int *x = new int;
  int &y = *x; // heapのint情報にyという名前を与える。「type &」はreference variableでエイリアスをつける。
  y = 4;

  std::cout << &x << std::endl;　// pointer(stack memory)のaddress
  std::cout <<  x << std::endl;　// x自体はheapのアドレス
  std::cout << *x << std::endl;  // 4(dereferencing)
  
  std::cout << &y << std::endl;　// heapのアドレス
  std::cout <<  y << std::endl;　// 4(y自体がheapを指す)
  //std::cout << *y << std::endl;  -> yはpointerでは無い, これはcompile errorになる

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
<br>

#### Week3. Constructor/Destructor
**Key Concepts**<br>
 ・ Constructors and destructors<br>
 ・ Methods and operators<br>
 ・ Access through pointers and references<br>

**Cube.h**<br>
```
  ...
    public:
      /* Constructor */
      Cube(); // Custom default constructor(未定義でも自動で作成されるが、次行のように何らかの引数有りConstructorを定義すると作成されないので注意。)
      Cube(double length); // One argument constructor

      Cube(const Cube & obj); // Custom copy constructor

      /* Custom assignment operator */
      Cube & operator=(const Cube & obj); // Custom assignment operator(引数はconst reference of this class' typeと言う。１つしか入れられない。)

      /* Destructor */
      ~Cube(); // Custom Destructor(No argument, no return type)
  ...
```

**Cube.cpp**<br>
```
  ...
  /* Constructor */
  Cube::Cube() {
    length_ = 1;
    std::cout << "Default constructor invoked!" << std::endl;
  }

  Cube::Cube(double length) {
    length_ = length;
  }

  Cube::Cube(const Cube & obj) { // 実はこれも自動で作ってくれるので、作成は必要無い。
    length_ = obj.length_;
    std::cout << "Copy constructor invoked!" << std::endl;
  }

  /* Custom assignment operator */
  Cube & Cube::operator=(const Cube & obj) {
    length_ = obj.length_;
    std::cout << "Assignment operator invoked!" << std::endl;
    std::cout << "Transformed " << getVolume() << " -> " << obj.getVolume() << std::endl;
    return *this; // -> return the dereference of this
  }
  
  /* Destructor */
  Cube::~Cube() {
    std::cout << "Destroyed " << getVolume() << std::endl;
  }
  
  ...
```

**main.cpp**<br>
```
#include "../Cube.h"
using uiuc::Cube;

void foo(Cube c) { // When this function is called, the Object is copied and invoke Copy constructor.（Copy by value(メモリ消費)）
  // Nothing
}

void foo2() {
  Cube c; // -> This invoke Default Constructor
  return c;
}

void foo3(Cube & c) { // Copy by reference(メモリ非消費)
  // Nothing
}

void foo４(Cube * c) { // Copy by pointer(メモリ非消費)
  // Nothing
}

int main() {
  // Copy Constructor
  Cube c; // -> This invoke Default Constructor
  foo(c) // -> This invoke Copy Constructor
  Cube c2 = foo2(); // -> This invoke Copy Constructor twice.(右辺(関数間)で１回、左辺(main内)で一回)

  // Assignment Operator
  Cube cube1;
  Cube cube2;
  cube2 = cube1; // Assignment Operator(代入のみでConstructorを呼ばない. Assignment Operatorを呼ぶ)
  /* ↑ メモリ２つ消費(値渡し)。↓メモリ１つ消費(参照渡し)。 */
  Cube & cube3 = cube1;

  foo3(c)
  foo4(&c)
  
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
$ ./main
> Default constructor invoked!
> Copy constructor invoked!
```






**../_make/generic.mk**(for the reference; 参考までに)<br>
```
#
# This is a generic Makefile designed to compile a sample directory of code.
# This file depends on variables having been set before calling:
#   EXE: The name of the result file
#   OBJS: Array of objects files (.o) to be generated
#   CLEAN_RM: Optional list of additional files to delete on `make clean`
#
# @author ***, <**@*****.edu>
# @author ***
# @author *** (edits made for ***)
#


# Compiler/linker config and object/depfile directory:
CXX = g++
LD  = g++
OBJS_DIR = .objs

# -MMD and -MP asks clang++ to generate a .d file listing the headers used in the source code for use in the Make process.
#   -MMD: "Write a depfile containing user headers"
#   -MP : "Create phony target for each dependency (other than main file)"
#   (https://clang.llvm.org/docs/ClangCommandLineReference.html)
DEPFILE_FLAGS = -MMD -MP

# Provide lots of helpful warning/errors:
# (Switching from clang++ to g++ caused some trouble here. Not all flags are identically between the compilers.)
#WARNINGS_AS_ERRORS = -Werror # Un-commenting this line makes compilation much more strict.
GCC_EXCLUSIVE_WARNING_OPTIONS =  # -Wno-unused-but-set-variable
CLANG_EXCLUSIVE_WARNING_OPTIONS =  # -Wno-unused-parameter -Wno-unused-variable
ifeq ($(CXX),g++)
EXCLUSIVE_WARNING_OPTIONS = $(GCC_EXCLUSIVE_WARNING_OPTIONS)
else
EXCLUSIVE_WARNING_OPTIONS = $(CLANG_EXCLUSIVE_WARNING_OPTIONS)
endif
# ASANFLAGS = -fsanitize=address -fno-omit-frame-pointer # for debugging, if supported on the OS
WARNINGS = -pedantic -Wall $(WARNINGS_AS_ERRORS) -Wfatal-errors -Wextra $(EXCLUSIVE_WARNING_OPTIONS)

# Flags for compile:
CXXFLAGS += -std=c++14 -O0 $(WARNINGS) $(DEPFILE_FLAGS) -g -c $(ASANFLAGS)

# Flags for linking:
LDFLAGS += -std=c++14 $(ASANFLAGS)

# Rule for `all` (first/default rule):
all: $(EXE)

# Rule for linking the final executable:
# - $(EXE) depends on all object files in $(OBJS)
# - `patsubst` function adds the directory name $(OBJS_DIR) before every object file
$(EXE): $(patsubst %.o, $(OBJS_DIR)/%.o, $(OBJS))
	$(LD) $^ $(LDFLAGS) -o $@

# Ensure .objs/ exists:
$(OBJS_DIR):
	@mkdir -p $(OBJS_DIR)
	@mkdir -p $(OBJS_DIR)/uiuc

# Rules for compiling source code.
# - Every object file is required by $(EXE)
# - Generates the rule requiring the .cpp file of the same name
$(OBJS_DIR)/%.o: %.cpp | $(OBJS_DIR)
	$(CXX) $(CXXFLAGS) $< -o $@

# Additional dependencies for object files are included in the clang++
# generated .d files (from $(DEPFILE_FLAGS)):
-include $(OBJS_DIR)/*.d
-include $(OBJS_DIR)/uiuc/*.d


# Standard C++ Makefile rules:
clean:
	rm -rf $(EXE) $(TEST) $(OBJS_DIR) $(CLEAN_RM) *.o *.d

tidy: clean
	rm -rf doc

.PHONY: all tidy clean
