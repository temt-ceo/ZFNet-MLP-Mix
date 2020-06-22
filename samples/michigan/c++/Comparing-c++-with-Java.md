# Memory Models (Drawing Memory Models)
 - Primitive types(boolean, byte, short, int, long, float, double, char) の6種類
 - Object types(Arrays and vlasses)
**memory**<br>
```
int var1 = 52; // stack memory
SimpleLocation ucsd; // stack memory
// c++と変わらない。
ucsd = new SimpleLocation(32.9, -117.2); //<- set a heap memory address of (SimpleLocation object)
// objectのメンバー変数もheap memory内にある。（constructorが呼ばれた時に前もって全部確保される。そのheap memoryのアドレスが返される）
SimpleLocation lima = new SimpleLocation(-12.0, -77.0);
lima.latitude = -12.04; // limaがアドレスを格納した変数なのでc++のarrow(->)でその参照先を参照するイメージ。

```
