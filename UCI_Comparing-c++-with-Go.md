# 1. Advantages of Go
 - C言語に近い効率 .. Code runs fast
 - PythonやJavaぐらい使いやすい .. Garbage Collection, Simpler objects
 - 並列処理 .. Concurrency is efficient

## C言語に近い効率
 - High-level languageからbinaryのMachine language(x86やAMDのこと)に変換がCompiledで一度だけされる。（CやC++のようにその後は変換する必要がない）
 - JavaですらMachine language(x86)にCompiledによる変換が一部しか(partially)されていなかった。残りはPythonのように実行時に変換されるInterpretationであった。<br>
   (JavaがCompileで生成しているのはMachine Codeではなくbyte code。これを実行時にMachine Languageに変換している。)

## Interpreter(PythonやJavaの一部)ぐらい使いやすい
Interpreterの特徴としてコードが書きやすい<br>
 - メモリ管理(de-allocate memory)を言語が（逐次解釈なので）自動で行う
 - 変数の推論が可能
 - Goはcompiled言語だが、Garbage Collection(de-allocate memory)というInterpreterの長所を取り入れた。

## Simpler objects と Concurrency
 - c++やJavaと違ってclassという用語を使わない。structs(データ)と付属のメソッドを使う。<br>
   (classの実装を簡単にしたもの。inheritanceもconstructorもgenericsもない。)
 - Concurrency is the management of multiple tasks at the same time.<br>
   (タスク間でコミュニケーションをとる機能は大規模システムほど必要。)<br>
   専用の予約語:<br>
   - GoRoutines .. concurrent tasks
   - Channels   .. communicate between tasks
   - Select     .. enables task synchronization

# 2. Work Space
 - Install .. golang.org (Download Go => stable versionのみ)
 - Workspaces(Javaと変わらない。強制でもない。):
   - src   .. source code files
   - pkg   .. libraly packages(linkするパッケージ)
   - bin   .. executables
 - GOPATH .. 環境変数（インストール時にセットしてくれるので気しなくてよし: win=>C:\Users\yourname\go = Goのインストール先)
 - Packages .. `package billpkg` でdefineしていたら、 `import ("billpkg")` でlinkできる(`,`や`;`が要らない)<br>
   (Packageの一つは`main`という名前でこのPackageをbuildする。当然だが...`main`にはmain()メソッドが必要だし、そこが処理起点)<br>

## Go Tool
 - import は 多くのpackageライブラリとGo Standard Libraryどちらにも使える。
 - Searches directories specified by GOROOT and GOPATH.
 - Several Commands:
   - `go build` .. compiles the program (Arguments can be a list of packages or a list of .go files). 実行ファイルを作る
   - `go run`   .. compiles .go files and runs the executable. 
   - `go test`  .. runs tests using files ending in "_test.go"
   - `go doc`   .. prints documentation for a package
   - `go fmt`   .. formats source code files. インデントをしてくれる。(GoはPythonみたいにインデントを強制していないが、このコマンドは全部のsourceを整えてくれる。) 
   - `go get`   .. downloads packages and installs them. 新しいライブラリをインストールしたい時
   - `go list`  .. lists all installed packages 

# 3. Basic Data Types
 - Case Sensitive - 大文字を識別する
 - Must have type - `var x int`, `var x, y int` (varは変数を宣言するというkeyword。typeはnameの後に来る)
 - 整数、少数、String - それぞれGoがCompile時に専用のMachine Codeに変換する(少数はOSによって違うHardwareを使うかもしれないし、Stringはbyteの一続きを変換する)
 - Type Declarations - aliasの定義
   - `type Celsius float64` (温度(C))  .. `var temp Celsius`
   - `type IDnum int`                 .. `var pid IDnum`
 - `var x = 100` - 変数の推論ができる(c++などと同様途中で少数にはならない)
 - Uninitialized variables have a zero value. - `var x int // x = 0`, `var x string // x = ""`
 - 省略型がある。 - `x := 100` (宣言、推論、代入を行う。 functionの中でのみ可。)

## その他
 - Pointers (c++同様)
   - `&` returns the address of a variable/function
   - `*` returns data at an address(dereferencing)
 - new() function creates a variable and returns a pointer to the variable (当然.. heapメモリを使うのだから.. ここもc++と変わらず)
   - e.g. `ptr := new(int)` `*ptr = 3`
 - Blocks - A sequence of declarations and statements within {}. (ここもc++と変わらず)
 - Scope - Go is lexically scoped using blocks.(外側のblockで宣言されていたらアクセスできる)
 - Deallocation
   - compiled言語ではheap変数は手動でdeallocateするのがほとんど (例:C言語ではheapの変数は手動でallocate,deallocateが必要。`x = malloc(32);`, `free(x);`)
   - Garbage Collection
     - InterpreterではGarbage Collectionを行ってくれる。(JavaはJava Virtual Machineで行っている)
     - エラーが起こりにくくプログラマーに優しい。
     - 遅い（理由はInterpreterが必要だから）
     - Compiled(ここではGo)のGarbage Collectionではpointerを追跡していつ開放して良いか決めている。
     - Compiledでは本当はGarbage Collectionがない方が（例えばc++）早いが、役に立つのでGoでは採用している。
 - Conversion characters(%s,etc)
   - fmt.Printf("Hi %s", x)
 - Integers
   - int8, int16, int32, int64, uint8, uint16, uint32, uint64 これは`var x int`とすればcompilerが判別してくれるのでわざわざ示さなくても良い。
 - Floating Point
   - float32 (6 digits of precision), float64 (15 digits of precision)
   - `var x float64 = 123.45`   (decimals)
   - `var x float64 = 1.2345e2` (scientific notation)
 - 虚数もある(complex)
 - UTF-8(ASCIIを最初の8bitで含んでいるという利点がある) .. Goのデフォルト
 - Type Conversions - T()
   - `var x int32 = 1` `var x int16 = 2` `x = y` ... Fail, `x = int32(y)` ... OK

## Pointerの復習(Object-Oriented Data Structures in C++より)
```
  int *x = new int;
  int &y = *x; // heapのint情報にyという名前を与える。「type &」はreference variableでheap変数のエイリアスをつくる。
  y = 4;

  std::cout << &x << std::endl;　// pointer(stack memory)のaddress。「&」はアドレスを取得する。
  std::cout <<  x << std::endl;　// x自体はheapのアドレス
  std::cout << *x << std::endl;  // 4(dereferencing)
  
  std::cout << &y << std::endl;　// heapのアドレス。「&」はアドレスを取得する。
  std::cout <<  y << std::endl;　// 4(y自体がheapを指す)
  //std::cout << *y << std::endl;  -> yはpointerでは無い, これはcompile errorになる
```

## String Package 
 - Unicode Package
   - IsDigit
   - IsSpace
   - IsLetter
   - IsLower
   - IsPunct
   - ToUpper
   - ToLower
 - String Package(Functions to manipulate UTF-8 encoded strings)
   - Compare(a, b) .. 0 if a==b
   - Contains(s, substr)
   - HasPrefix(s, prefix)
   - Index(s, substr) .. returns the index of the first instance of substr in s.
   - Replace(s, old, new, n) .. returns a copy of the string s with the first n instances of old replaced by new.
   - HasPrefix(s, prefix)
   - ToUpper(s)
   - ToLower(s)
   - TrimSpace(s)
 - Strconv Package
   - Atoi(s) .. converts string to int
   - Itoa(s) .. converts int to string
   - FormatFloat(f, fmt, prec, bitSize) .. converts float to string
   - ParseFloat(s, bitSize) .. converts string to float

## Constants (constants .. Expression whose value is known at compile time.)
 - Type is inferred from righthand side.(e.g. `const x = 1.3`, `const (y = 4 \r z = "Hi")`) (setを作るのに便利)
 - `iota` .. Generate a `set` of related but distinct constants.(setを作る。 個々のvalueはconstantだが値自体はなんでも良い時に使う。)
   - Often represents a property which has several distinct possible values (個々の値は別々だが関連のあるconstantのsetを表す)
     - Days of the week
     - Months of the year
```
  type Grades int
  const (
    A Grades = iota // Each constant is assigned to a unique integer
    B               // Starts at 1 and increments
    C
    D
    F
  )
```
## 課題 1-1
 - Write a program which prompts the user to enter a floating point number and prints the integer which is a truncated version of the floating point number that was entered. Truncation is the process of removing the digits to the right of the decimal place. Submit your source code for the program, “trunc.go”.
```
package main

import (
	"fmt"
)

func main() {
	var ans float64
	fmt.Printf("Please enter a 'Floating Point Number'\n")
	fmt.Scan(&ans)
	fmt.Printf("Your Imput is Truncated and Printed Like This: %d\n", int(ans))
}
```
## 課題 1-2
 - Write a program which prompts the user to enter a string. The program searches through the entered string for the characters ‘i’, ‘a’, and ‘n’. The program should print “Found!” if the entered string starts with the character ‘i’, ends with the character ‘n’, and contains the character ‘a’. The program should print “Not Found!” otherwise. The program should not be case-sensitive, so it does not matter if the characters are upper-case or lower-case. Submit your source code for the program, “findian.go”.
```
import (
	"fmt"
	"strings"
)

func main() {
	var str string
	fmt.Printf("Please enter a string\n")
	fmt.Scan(&str)
	str = strings.ToLower(str)
	ans1 := strings.Index(str, "i")
	ans2 := strings.Index(str, "a")
	ans3 := strings.Index(str, "n")
	if ans1 == 0 && ans3 == len(str)-1 && ans2 > -1 {
		fmt.Printf("Found!\n")
	} else {
		fmt.Printf("Not Found!\n")
	}
}
```

## Control Flow (for, switch, scan)
 - whileが無い
```
for i:=0; i<10; i++ {
  fmt.Printf("hi ")
}
// while loop
i := 0
for i<10 {
  fmt.Printf("hi ")
}
// infinite for loop
for {
  fmt.Printf("hi ")
}
```
 - switch/caseにbreakが要らない
```
switch x {
case 1:
  fmt.Printf("case1") // breakが無いけどcase1しか実行されない。
case 2:
  fmt.Printf("case2")
default:
  fmt.Printf("nocase")
}
// for-loopのbreak and continue自体は存在する
```
 - Tagless Switch (タグが無い時はbooleanを式は見る)
```
switch {
case x > 1: // x > 1の時に実行される。
  fmt.Printf("case1")
case x < -1:
  fmt.Printf("case2")
default:
  fmt.Printf("nocase")
}
```
 - Scan (a: pointerを引数に取る。, b: ユーザーの入力をscanする。, c: スキャンされた数をreturnする。)
```
var appleNum int
fmt.Printf("Number of apples?")
num, err := fmt.Scan(&appleNum) // ユーザー入力を待つ (5と入力しEnterが押されたら、引数に代入する)
fmt.Printf(appleNum)            // "5"と出力
```

# 4. Composite Data Types
 - Arrays
   - 指定したTypeでFixed-LengthのArrayが作れる
   - Elementsは0(intなら0, stringなら"")で初期化される
 - Array Literal
   - 値を最初から(全部)セットしておいたArray
   - [5]の部分は[...]とすればArrayの大きさを推測してくれる
 - Hash Tables
   - Maps
     - Use `make` to create a map.
     - map[`key type`] `value type`
 - Structs
   - 他のオブジェクトの集合体
   - 例: Person Struct {Name, Address, phone}
     - 方法: Make a single struct which contains all 3 vars
 - Slices (sliceとは: A "window" on an underlying array。 pythonにもある。)
   - 3つのPropertiesを持つ
     - Pointer: indicates the start of the slice
     - Length
     - Capacity: the maximum number of elements
   - Arrayと非常によく似た式でSliceを初期化できる`...`が無いだけ (内部で何が行われているかというと元となるArrayが作られた後、全部をsliceしたSliceが作られている。)
 - Make a Slices
   - Create a slice(and array) using `make()`
   - 引数はtypeとlength/capacityの２つ
   - Append
     - sliceのSizeを増やしたい時、末尾に追加する
     - 但し、sliceだけではなくarrayにもinsertされる。(そのためarrayのサイズを増やす必要がある時は増やされる)
```
// Arrays
var x [5]int
x[0] = 2
fmt.Printf(x[1]) // 0

// Array Literal
var x [5]int = [5]{1,2,3,4,5} // 下の式と同一
x := [...]int{1,2,3,4,5}
for i, v range x {            // Iterating (pythonと同じくrangeが使える。indexも拾える。)
  fmt.Printf("ind %d, val %d", i, v)
}

// Hash Tables(Maps)
var idMap map[string]int
idMap = make(map[string]int)         // Use make() to create a map
idMap := map[string]int {"joe": 123} // May define a map literal({:}の事)
id, p := idMap["joe"]      // idはvalue, pはboolean(存在するかどうか)
fmt.Println(idMap["joe"])  // 見つからなかったらzero(""など)が返る
idMap["jane"] = 456
delete(idMap, "joe")
fmt.Println(len(idMap))    // いくつキーがあるか。
for key, val := range idMap { // Iterating through a Map
  fmt.Println(key, val)
}

// Structs
type struct Person {
  name string   // <- field
  addr string
  phone string
}
var p1 Person
p1.name = "joe"
x = p1.addr
p1 := new(Person) // 初期化方法(=> 全てのfieldはzero(""など)で初期化される)
p1 := Person(name: "joe", addr: "a st.", phone: "123") // struct literal((:)の事)でも初期化できる

// Slices
arr := [..]string{"a", "b", "c", "d", "e", "f", "g"}
s1 := arr[1:3]
s2 := arr[2:5] // s1とオーバーラップしても問題なし
fmt.Printf(len(s1), cap(s1)) // -> " 2 6 " (The capacities are the difference between the length of the underlying array and the starting index of the slice.)
s := []int{1,2,3,4,5} // これはSlice。Sliceを初期化した場合は(Length = Capacityとなる。Pointerは0の位置。)

// Make a Slices
sli = make([]int, 10) // 0で初期化したい時でlength=capacityとしたい時
   - 引数はtypeとlength/capacityの２つ
sli = make([]int, 10, 15) // lengthとcapacityを別々に指定する時
sli = append(sli, 100)
```
## 課題 2
 - Write a program which prompts the user to enter integers and stores the integers in a sorted slice. The program should be written as a loop. Before entering the loop, the program should create an empty integer slice of size (length) 3. During each pass through the loop, the program prompts the user to enter an integer to be added to the slice. The program adds the integer to the slice, sorts the slice, and prints the contents of the slice in sorted order. The slice must grow in size to accommodate any number of integers which the user decides to enter. The program should only quit (exiting the loop) when the user enters the character ‘X’ instead of an integer.
```
package main

import (
  "fmt"
  "sort"
  "strconv"
)

func main() {
  sli := make([]int, 3)
  i := 0
  for {
    fmt.Printf("Please enter a integer or 'X' if you want to exit.\n")
    var num string
    fmt.Scan(&num)
    if num == "X" {
      break
    } else {
      val, _ := strconv.Atoi(num)
      if i < 3 {
        sli[0] = val
      } else {
        sli = append(sli, val)
      }
      sort.Ints(sli)
      fmt.Println(sli)
    }
    i++
  }
}
```
# 5. Protocols and Formats
 - 




