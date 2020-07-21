# Advantages of Go
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

# Work Space
 - Install .. golang.org (Download Go => stable versionのみ)
 - Workspaces(Javaと変わらない。強制でもない。):
   - src   .. source code files
   - pkg   .. libraly packages(linkするパッケージ)
   - bin   .. executables
 - GOPATH .. 環境変数（インストール時にセットしてくれるので気しなくてよし: win=>C:\Users\yourname\go = Goのインストール先)
 - Packages .. `package billpkg` でdefineしていたら、 `import ("billpkg")` でlinkできる(`,`や`;`が要らない)<br>
   (Packageの一つは`main`という名前でこのPackageをbuildする。当然だが...`main`にはmain()メソッドが必要だし、そこを処理起点とする。)<br>

**mainを有するPackage `main`**<br>
```
package main
import "fmt"
func main() {
  fmt.Printf("abc\n")
}
```
