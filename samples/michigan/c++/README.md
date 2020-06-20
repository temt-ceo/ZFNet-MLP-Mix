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
### Week1. C++ Classes
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

### Week2. Stack/Heap Memory
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

  delete c; c = nullptr; // heapのCubeを削除、stackのcはこのままでは不安定の為、Null Pointer（0x0）をセットしてアクセスされた時は必ずエラーが発生するようにする。(nullptrをdeleteしたとしても無視されるという利点もある。)
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

### Week3. Constructor/Destructor
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
    return *this; // -> return the dereference of this("this" is a pointer to the current object instance)
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

**for ( temporary variable declaration : container ) { loop body }**<br>
In the standard library, a std::vector is an array with automatic size.<br>
```
#include <iostream>
#include <vector>
int main() {
  std::vector<int> int_list;
  int_list.push_back(1);
  int_list.push_back(2);
  int_list.push_back(3);

  // これはメモリを一時的に割り当てる為、遅い。
  for (int x : int_list) {
    // This version of the loop makes a temporary copy of each list item by value.
    // Since x is a temporary copy, any changes to x do not modify the actual container.
    x = 99;
  }
  for (int x : int_list) {
    std::cout << x << std::endl;
  }
  std::cout << "If that worked correctly, you never saw 99." << std::endl;

  // これはメモリを一時的にも割り当て無い為、早い。
  for (int& x : int_list) {
    // This version of the loop will modify each item directly.
    x = 99;
  }
  for (int x : int_list) {
    std::cout << x << std::endl;
  }
  std::cout << "Everything was replaced with 99." << std::endl;

  // 大きなオブジェクトなど、早く処理したくてもitemの変更が必要無い場合は、以下にすると早く、変更の心配も無い。
  for (const int& x : int_list) {
    //x = 99; // This line could cause an error.
  }

  return 0;
}

```
<br>

### Week4. the Tower of Hanoi problem
**Key Concepts**<br>
 ・ Object-oriented design<br>
 ・ Templates(template type or <T>: this can take on different types)<br>
 ・ Class hierarchies and inheritance<br>

#### template type 1
**main.cpp**<br>
```
#include <vector>
#include <iostream>

int main() {
  // Template Type
  std::vector<int> v;
  v.push_back(2);
  v.push_back(3);
  v.push_back(5);

  std::cout << v[0] << std::endl;
  std::cout << v[1] << std::endl;
  std::cout << v[2] << std::endl;

  return 0;
}
```

**Makefile**<br>
```
EXE = main
OBJS = main.o
CLEAN_RM =

include ../_make/generic.mk

```

**compile and execute**<br>
```
$ ./main
> 2
> 3
> 5

```

#### template type 2
**main.cpp**<br>
```
#include <iostream>
using std::cout;
using std::endl;

#include "Cube.h"
using uiuc::Cube;

/* Use Template Type */
template <typename T>
T max(T a, T b) {
  if (a > b) { return a; }
  return b
}

int main() {
  cout << "max(3, 5): " << max(3, 5) << endl; // -> 5
  cout << "max('a', 'd'): " << max('a', 'd') << endl; // -> d
  cout << "max(\"Hello\", \"World\"): " << max("Hello", "World") << endl; // -> "World"
  cout << "max(Cube(3), Cube(6)): " << max(Cube(3), Cube(6)) << endl; // -> Need to define.

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
> main.cpp:17:9: fatal error: invalid operands to binary expression ('uiuc::Cube' and 'uiuc::Cube')
>   if (a > b) { return a; }
>       ~ ^ ~
> main.cpp:25:42: note: in instantiation of function template specialization 'max<uiuc::Cube>' requested here
>   cout << "max( Cube(3), Cube(6) ): " << max( Cube(3), Cube(6) ) << endl;
>                                          ^
> 1 error generated.
> make: *** [.objs/main.o] Error 1

```

#### template type 3(Inheritance) Shape -> Cube
**Shape.h**<br>
```
#pragma once

class Shape {
  public:
    Shape(); // Custom Default Constructor
    Shape(double width); // Single Argument Constructor
    double getWidth() const;

  private:
    double width_;
};
```

**Cube.h**<br>
```
#pragma once

#include "Shape.h"
#include "HSLAPixel.h"

namespace uiuc {
  /* Class Cube inherits class Shape.(99%のinheritsはpublic inherit) */
  class Cube : public Shape {
    public:
      Cube(double width, uiuc::HSLAPixel color); // Two arguments Constructor
      double getVolume() const;

    private:
      uiuc::HSLAPixel color_;
  };
}


```

**Cube.cpp**<br>
```
  ...
  /* Inheritしたclassはdefault constructorでbase classをinitializeしなければならない */
  Cube::Cube(double width, uiuc::HSLAPixel color) : Shape(width) { // -> c++にShapeをinitializeするように依頼している。
    color_ = color;
  }
  ...
```

**Shape.cpp**<br>
```
#include "Shape.h"

/* 1行も中身が無いが、single argument constructorに1を渡している */
Shape::Shape() : Shape(1) {
  // Nothing
}

/* 1行も中身が無いが、private variableにwidthを渡している */
Shape::Shape(double width) : width_(width) {
  // Nothing
}
...
```

#### ::
the scope resolution operator, ::, denotes the fully-qualified name of a function or variable that is a member of a class (or namespace). So, you will see things like class name::member name in writing and in code, in some contexts. Note how this is different from the member selection operator, ., which appears in contexts like class instance.member name. This is because :: shows a relationship to an entire class or to a namespace, whereas the . operator shows a relationship to a single instance of an object.

#### pass-by-reference
the class members utilize pass-by-reference as shown by the & operator; It means that a direct reference to the memory is being passed, which uses the same concept of indirection that pointers offer, but with a simple syntax—you just use the variable as you would normally, not by dereferencing a memory address explicitly with the * operator as pointers do, and yet you are still able to implicitly manipulate the data located at the original memory location. This convenience feature was one of the benefits of C++ over the older C language.


--------------------------------以下Javaの場合-------------------------------------------<br>
**SimpleLocation.java**<br>
```
public class SimpleLocation
{
  public double latitude;
  public double longitude;
  
  public SimpleLocation()
  {
    this.latitude = 32.9;
    this.longitude = -117.2;
  }
  public SimpleLocation(double lat, double lon)   // Overloading
  {
    this.latitude = lat;
    this.longitude = lon;
  }
  public double distance(SimpleLocation other)
  {
    return getDist(this.latitude, this.longitude,
                   other.latitude, other.longitude);
  } 
  public double distance(double otherLat, double otherLon)   // Overloading
  {
    return getDist(this.latitude, this.longitude, otherLat, otherLon);
  } 
}
```
**LocationTester.java**<br>
```
public class LocationTester
{
  public static void main(String[] args)
  {
    SimpleLocation ucsd = new SimpleLocation(32.9, -117.2);
    SimpleLocation lima = new SimpleLocation(-12.0, -77.0);
    
    System.out.println(ucsd.distance(lima));
  }
}
```
**実行**<br>
```
javac *.java
java LocationTester
-> 6567.659
```

