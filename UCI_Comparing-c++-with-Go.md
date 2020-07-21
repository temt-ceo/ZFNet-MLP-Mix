## Advantages of Go
 - C言語に近い効率 .. Code runs fast
 - PythonやJavaぐらい使いやすい .. Garbage Collection, Simpler objects
 - 並列処理 .. Concurrency is efficient

### C言語に近い効率
 - High-level languageからbinaryのMachine language(x86やAMDのこと)に変換がCompiledで一度だけされる。（CやC++のようにその後は変換する必要がない）
 - JavaですらMachine language(x86)にCompiledによる変換が一部しか(partially)されていなかった。残りはPythonのように実行時に変換されるInterpretationであった。<br>
   (JavaがCompileで生成しているのはMachine Codeではなくbyte code。これを実行時にMachine Languageに変換している。)

## Interpreter(PythonやJava)ぐらい使いやすい
Interpreterの特徴としてコードが書きやすい<br>
 - メモリ管理(de-allocate memory)を言語が（逐次解釈なので）自動で行う
 - 変数の推論が可能
 - Goはcompiled言語だが、Garbage Collection(de-allocate memory)というInterpreterの長所を取り入れた。

## WorkSpace
 - 
 - 

**mainを有するclass内**<br>
```
```
