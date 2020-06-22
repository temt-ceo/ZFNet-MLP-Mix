# Memory Models (Drawing Memory Models)
 - Primitive types(boolean, byte, short, int, long, float, double, char) の6種類
 - Object types(Arrays and vlasses)
**mainを有するclass内**<br>
```
int var1 = 52; // stack memory
SimpleLocation ucsd; // stack memory
// c++と変わらない。
ucsd = new SimpleLocation(32.9, -117.2); //<- set a heap memory address of (SimpleLocation object)
// objectのメンバー変数もheap memory内にある。（constructorが呼ばれた時に前もって全部確保される。そのheap memoryのアドレスが返される）
// (class内のconstructorなどメソッドの引数のvariableはstack memory.)
SimpleLocation lima = new SimpleLocation(-12.0, -77.0);
lima.latitude = -12.04; // limaがheap memoryのアドレスを格納した変数なのでc++のarrow(->)でその参照先を参照するイメージ。
lima = ucsd; // ←はアドレスが書き換わるのでlimaは正にucsdと同じ(heap memory上の)オブジェクトを参照する
```
**SimpleLocation.java**<br>
```
public classs SimpleLocation // class名は必ずファイル名と同じにするというjavaのルールがある
{
  public double latitude;
  public double longitude;
  public SimpleLocation(double latIn, double lonIn)
  {
      this.latitude = latIn; // c++と同じようにlatitude = latIn;と書いても大丈夫。javaは最初stack memoryのローカル変数を確認し
      this.longitude = lonIn;// その後、heap memoryのメンバー変数を確認する。
  }
}
```