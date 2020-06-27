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

**Polymorphism.java**<br>
```
// in main
Person p[] = new Person[3];
p[0] = new Student("Cara", 1234);
System.out.print(p[0]); // これはPersonのtoStrint()ではなくStudentのtoString()が呼ばれる。(=>Polymorphism)

/*
  "Polymorphism" の要点
  1. Compilerはreference typeのみ参照する。（そのためreferenct typeに存在しないメソッドを呼ぼうとするとエラーになる）
  2. Runtime時にはobject typeの動きに追随する。
  3. (Subclass)でCastingする事で、compilerはCastingしている方のクラスのメソッドを確認するのでCompile Errorを防ぐ事ができる。
  4. Runtime Error(CastingしたクラスとObjectのクラスが一致しない時など)を防ぐには if (o instanceof ClassName ){} を使う。
*/
```

## InheritanceとConstructor
 - ⑴ constructorがクラス内に無い時は自動でcompilerが用意する(但し、引数付constructorが存在する時、default(no-argument)constructorは自動で作成されない(c++と一緒))。extendsキーワードが無い時はextends Objectをcompilerが用意する。 <br><br>
 - ⑵ constructorの最初の行は this();またはsuper();でなければならない。<br>
     ※this();は他のConstructorを呼び出す。例えば引数なしのconstructorが呼ばれた時にデフォルトの引数をセットしたい時にthis('default value');のように呼び出す。super()はその先のconstructorにセットされる。<br><br>
 - ⑶ ⑵のthis();と super();どちらも無い時はsuper();をcompilerがConstructorの最初の行に用意する。(つまり最も最初に呼ばれるコードはObjectクラスのconstructorという事になる。) <br><br>
 - ⑷ ↑はメソッドでも応用可能で、method overriding でparent/childどちらも同じメソッドを有する時に敢えてparentのメソッドを呼びたい時はsuper.methodName();で呼ぶ事ができる。<br>

## SAMPLE1 (直近の地震をアプレットで表示)
**EarthquakeCityMap.java**<br>
```
package module3;

//Java utilities libraries
import java.util.ArrayList;
//import java.util.Collections;
//import java.util.Comparator;
import java.util.List;
import processing.core.PApplet;
import de.fhpotsdam.unfolding.UnfoldingMap;
import de.fhpotsdam.unfolding.marker.Marker;
import de.fhpotsdam.unfolding.data.PointFeature;
import de.fhpotsdam.unfolding.marker.SimplePointMarker;
import de.fhpotsdam.unfolding.providers.Google;
import de.fhpotsdam.unfolding.utils.MapUtils;
import parsing.ParseFeed;

/** EarthquakeCityMap
 * An application with an interactive map displaying earthquake data.
 * Author: UC San Diego Intermediate Software Development MOOC team
 * @author Takashi Tahara
 * Date: June 27, 2020
 * */
public class EarthquakeCityMap extends PApplet {

	// You can ignore this.  It's to keep eclipse from generating a warning.
	private static final long serialVersionUID = 1L;
	// Less than this threshold is a light earthquake
	public static final float THRESHOLD_MODERATE = 5;
	// Less than this threshold is a minor earthquake
	public static final float THRESHOLD_LIGHT = 4;
	private UnfoldingMap map;
	//feed with magnitude 2.5+ Earthquakes
	private String earthquakesURL = "https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/2.5_week.atom";
	
	public void setup() {
		size(950, 600, OPENGL);
		map = new UnfoldingMap(this, 200, 50, 700, 500, new Google.GoogleMapProvider());		
	    map.zoomToLevel(2);
	    MapUtils.createDefaultEventDispatcher(this, map);	
	    List<Marker> markers = new ArrayList<Marker>();
	    List<PointFeature> earthquakes = ParseFeed.parseEarthquake(this, earthquakesURL);
		for (PointFeature point : earthquakes) {
			markers.add(createMarker(point));
		}
	    map.addMarkers(markers);
	}

	private SimplePointMarker createMarker(PointFeature feature)
	{  
		SimplePointMarker marker = new SimplePointMarker(feature.getLocation());
		Object magObj = feature.getProperty("magnitude");
		float mag = Float.parseFloat(magObj.toString());
	    int yellow = color(255, 255, 0);
	    int blue = color(0, 0, 255);
		int red = color(255, 0, 0);
		if (mag >= THRESHOLD_MODERATE) {
			marker.setColor(red);
			marker.setRadius(15.0f);
		} else if (mag >= THRESHOLD_LIGHT) {
			marker.setColor(yellow);
			marker.setRadius(10.0f);
		} else {
			marker.setColor(blue);
			marker.setRadius(5.0f);
		}
	    return marker;
	}
	
	public void draw() {
	    background(10);
	    map.draw();
	    addKey();
	}

	// helper method to draw key in GUI
	private void addKey() 
	{	
		// Remember you can use Processing's graphics methods here
		this.fill(255,255,255);
		this.rect(30, 50, 150, 250);
		this.textSize(12);
		this.fill(0, 0, 0);
		this.text("Earthquake Key", 50, 90);
		this.text("5.0+ Magnitude", 75, 140);
		this.text("4.0+ Magnitude", 75, 190);
		this.text("Below 4.0", 75, 240);
		this.fill(255, 0, 0);
		this.ellipse(50, 136, 15, 15);
		this.fill(255, 255, 0);
		this.ellipse(50, 186, 10, 10);
		this.fill(0, 0, 255);
		this.ellipse(50, 236, 5, 5);
	}

	//Add main method for running as application
	public static void main (String[] args) {
		PApplet.main(new String[] {"module3.EarthquakeCityMap"});
	}
}
```
**.classpath**<br>
```
<?xml version="1.0" encoding="UTF-8"?>
<classpath>
	<classpathentry kind="src" path="src"/>
	<classpathentry kind="src" path="data"/>
	<classpathentry kind="con" path="org.eclipse.jdt.launching.JRE_CONTAINER/org.eclipse.jdt.internal.debug.ui.launcher.StandardVMType/JavaSE-1.6"/>
	<classpathentry kind="lib" path="lib/core.jar"/> // processing.coreのjar file
	<classpathentry kind="lib" path="lib/gluegen-rt.jar"/>
	<classpathentry kind="lib" path="lib/jogl-all.jar">
		<attributes>
			<attribute name="org.eclipse.jdt.launching.CLASSPATH_ATTR_LIBRARY_PATH_ENTRY" value="unfolding-app-template/libNative"/>
		</attributes>
	</classpathentry>
	<classpathentry kind="lib" path="lib/log4j-1.2.15.jar"/>
	<classpathentry kind="lib" path="lib/sqlite-jdbc-3.7.2.jar"/>
	<classpathentry kind="lib" path="lib/json4processing.jar"/>
	<classpathentry kind="lib" path="lib/libTUIO.jar"/>
	<classpathentry kind="lib" path="lib/unfolding.0.9.7-uscd.jar"/>
	<classpathentry kind="output" path="build"/>
</classpath>
```
