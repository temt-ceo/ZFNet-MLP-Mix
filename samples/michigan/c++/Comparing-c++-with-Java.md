## Memory Models (Drawing Memory Models)
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


## member変数もlocal変数もheap memoryの情報のケース
 - List<T> = new ArrayList<T>() や Map = new HashMap()　の説明 (Java "interface" specifies behaviors, not implementation)<br><br>
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
      /* MarkerはJavaDocを見るとInterface キーワードが付いているのでabstract data typeであると分かる（c++の.hファイルだけみたいなもの）。
         また、SimplePointMarkerのJavaDocを見ると "All Implemented Interfaces: Marker"と書かれている。
         これは実装されるのはMarkerの機能を持ったものだけなので、new でオブジェクトを作成時に、左辺はMarkerとしても
         右辺は何かの種類のMarkerであれば何でもよく、そこはMarker自体は気にしない。（実際に.cppファイルの役割を果たすのがinstantiateする右辺のクラスということ。）
	 => UC SanDiegoでは以下のように習う。
	    Java "interface" specifies behaviors, not implementation. (左辺のReference Typeのこと)
	    Actual Java class implements List behavior.　(右辺のObject Typeのこと)
	 */
      Marker val = new SimplePointMarker(valLoc);
      map.addMarker(val);
      
      /* 以下も上記と同じ理由で。RandomAccessであるArrayList<T>はc++のvector<T>に近い。詳しくはc++の説明を見ること。
      　　またはJavaDoc(https://docs.oracle.com/javase/8/docs/api/)でArrayListの項目を調べると詳細が出てくる。
	   ※ ArrayListはjava.utilのpackageに入っているので、まずjava.utilをクリック、AbstractListやArrayList等が出てくるのでArrayListをクリックする。
	 同様にSet<T>とHashSet<T>, TreeSet<T>も同じ理由である。（但し、Mapだけ理由が少し異なる↓）（<T> means generic class.）*/
      //List<Marker> markers = new ArrayList<Marker>();
      
      /* 余談だが、Ordered ListであるArrayListではset an element at an indexで以下２通りの方法がある(ArrayListの強みは(c++のvectorのように)expand（拡張）が可能な事)
        countries[0] = 'a';
	countries.set(0, 'a'); // <-> countries.get(0);
	  ※ Lengthを取得する方法も２通りある
	  int len = countries.length; と int len = countries.size();
      */
    }
    
    private Map<String, Float> loadLifeExpectancyFromCSV(String fileName) {

         /* 以下のConstructor式では、左辺のMapがReference Type, 右辺(HashMapの事)がObject Typeと呼ばれる。
            Object Typeは機能や実装の詳細部分であり、Reference Typeはそのオブジェクトの動きを表すもの。
	    Map<T>, Set<T>, List<T>は全てInterfaceであり、このままではinstantiateできない。
	    JavaでInterfaceはあくまでClassのblueprintであり引数と返り値以外の中身を持たない（c++の.hファイルみたいなもの）。
	    -> https://stackoverflow.com/questions/20235118/difference-between-reference-type-and-object-type
	    */
         Map<String, Float> lifeExpMap = new HashMap<String, Float>();

         String[] rows = loadStrings(fileName); // -> String[] processing.core.PApplet.loadStrings(String filename)
         for (String row : rows) {
             String[] columns = row.split(",");
             if (columns.length == 6 && !columns[5].equals("..")) {
	         float value = Float.parseFloat(columns[5])
                 lifeExpMap.put(columns[4], value);
             }
         }

	return lifeExpMap;
    }
    
    /* 以下は実際にMapを参照しているところ */
    private void shadeCountries() {
        for (Marker marker : countryMarkers) {
            String countryId = marker.getId();
            if (lifeExpByCountry.containsKey(countryId)) {
                float lifeExp = lifeExpByCountry.get(countryId);
                int colorLevel = (int) map(lifeExp, 40, 90, 10, 255);
                marker.setColor(color(255-colorLevel, 100, colorLevel));
            }
            else {
                marker.setColor(color(150,150,150));
            }
	    /*  ↑Mapは以下のようにループさせることも可能(試しにremove試みている)
	      for (String key: lifeExpByCountry.keySet())
		  //lifeExpByCountry.remove(key)
		  //↑これはエラー(.keySet()メソッドは正しく更新されないので次のthreadがconsistency errorを吐く)になるのでこの場合は
		  // Map<String, Float> lifeExpMap = new HashMap<String, Float>();の部分を
		  // Map<String, Float> lifeExpMap = new ConcurrencyHashMap<String, Float>();としなければならない。
		 or 
	      for (Map.Entry<String, Float> entry: lifeExpByCountry.entrySet()) {
		  System.out.println(entry.getKey() + ": " + entry.getValue());
	      }
	         or
	      lifeExpByCountry.forEach((k,v) -> System.out.println(k,v)); // Java1.8
	      // Java1.9ではconstructorも簡単。
	      Map<String Float> lifeExpByCountry = Map.of("aa", 1.0f, "bb", 2.0f, "cc", 3.0f); // Java1.9
	    */
        }
    }
    
}
```



## Reference and Object type
 - Reference type => Compile time decisions. <br>
 - Object type => Run time decisions. <br><br>

**Case1.java**<br>
```
// in main
Person[] p = new Person[3]
p[0] = new Person();
p[1] = new Student(); // A Person array CAN store Student and Faculty objects.
p[2] = new Faculty();
```
**Case2.java**<br>
```
Public class Person {
  private String name;
  public String getName() {return name;}
}
Public class Student extends Person {
  private String id;
  public String getID() {return id;}
}
Public class Faculty extends Person {
  private String id;
  public String getID() {return id;}
}
Student s = new Student(); // この時 this が constructorに渡される。 (new自体はメモリ割当の働きのみ。その割当てたスペース自体がthisの事。つまりthisとは割り当てられたメモリスペースの事。)
Person p = new Person();
Person q = new Person();
Faculty f = new Faculty();
Object o = new Faculty();

p = s; //ok
int m = p.getID(); // 無理(今（一行上の行で）, Person p は Student s を指しているが、CompilerはPersonがgetIDを持っている事を知らないので、以下のように修正が必要)
                   // → int m = ((Student)p).getID(); (Runtime上ではp=sなので普通に取得が可能)
f = q; // 無理(こっちはRuntimeエラー。Compileは通る。)
o = s; // ok
```
