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

**Main Package**<br>
```
package main
import "fmt"
func main() {
  fmt.Printf("abc\n")
}
```

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

## Naming
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

**Pointerの復習(Object-Oriented Data Structures in C++より)**<br>
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
 - 
 - 
 - 
 - 
 - 
 - 
















