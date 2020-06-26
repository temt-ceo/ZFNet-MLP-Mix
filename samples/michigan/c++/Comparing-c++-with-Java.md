# Memory Models (Drawing Memory Models)
 - Primitive types(boolean, byte, short, int, long, float, double, char) の6種類
 - Object types(Arrays and vlasses)

**mainを有するclass内**<br>
```
int var1 = 52; // stack memory
SimpleLocation ucsd; // stack memory
// c++と変わらない。
ucsd = new SimpleLocation(32.9, -117.2); //<- set a heap memory address of (SimpleLocation object)
// objectのメンバー変数もheap memory内にある。（constructorが呼ばれた時に前もって全部確保される。そのheap memoryのアドレスが返される。)(class内のconstructorなどメソッドの引数のvariableはstack memory.)
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
      this.longitude = lonIn;// その後、heap memoryのメンバー変数を確認する。(this.が必ず必要なわけではない。)
  }
}
```
#### 次にmember変数もlocal変数もheap memoryの情報のケース
**ArrayLocation.java**<br>
```
public class ArrayLocation {
    private double[] coords;
    
    public ArrayLocation(double[] coords) {
        this.coords = coords;
    }
    
    public static void main(String[] args) {
        double[] coords = {5.0, 0.0};  // この時点で既にheap memory
        ArrayLocation accra = new ArrayLocation(coords); // coordsにはアドレスが入っているだけ
        coords[0] = 32.9; // heap memory内の値を変更
        coords[1] = -117.2;
        System.out.println(accra.coords[0]); //->32.9(同じheap memory内の値を変更されたから。)
    }
    
    public int[] sunColorSec(float seconds)
    {
       int[] rgb = new int[3];
       float diffFrom30 = Math.abs(30-seconds);
       float ratio = diffFrom30/30;
       rgb[0] = (int)(255*ratio); // convert to int from float
       rgb[1] = (int)(255*ratio);
       rgb[2] = 0;

       //System.out.println("R" + rgb[0] + " G" + rgb[1] + " B" + rgb[2]);
       return rgb;
    }
    
    public void setup() {
      // ...
      Location valLoc = new Location(-38.14f, -73.03f);
      /* MarkerはJavaDocを見るとInterface キーワードが付いているのでabstract data typeであると分かる。
         また、SimplePointMarkerのJavaDocを見ると "All Implemented Interfaces: Marker"と書かれている。
         これは実装されるのはMarkerの機能を持ったものだけなので、new でオブジェクトを作成時に、左辺はMarkerとしても
         右辺は何かの種類のMarkerであれば何でもよく、そこはMarker自体は気にしない。*/
      Marker val = new SimplePointMarker(valLoc);
      map.addMarker(val);
      
      /* 以下も上記と同じ理由で、ArrayListをparent classであるListでabstract的に受け取っている。 
         そのため右辺はArrayListでもLinkedListでもAttributeListでも
      　　何でも良いという柔軟性が生まれる。RandomAccessなArrayListはc++のvectorArrayに近い。詳しくはc++の説明を見ること。 */
      //List<Marker> markers = new ArrayList<Marker>();
    }
    
    	private Map<String, Float> loadLifeExpectancyFromCSV(String fileName) {

         /* 以下のConstructor式では、左辺のMapがReference Type, 右辺(HashMapの事)がObject Typeと呼ばれる。
            Object Typeは機能や実装の詳細部分であり、Reference Typeはそのオブジェクトの動きを表すもの。*/
         Map<String, Float> lifeExpMap = new HashMap<String, Float>();

         String[] rows = loadStrings(fileName);
         for (String row : rows) {
             // Reads country name and population density value from CSV row
             String[] columns = row.split(",");
             if (columns.length == 6 && !columns[5].equals("..")) {
                 lifeExpMap.put(columns[4], Float.parseFloat(columns[5]));
             }
     }

		return lifeExpMap;
	}
}
```
