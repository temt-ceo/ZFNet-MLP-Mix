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
    /* 以下はMapを使ってよく書くデバッグコード */
    import java.util.Map;
    import java.util.HashMap;
    Map<String, Integer> intMap = new HashMap<String, Integer>();
    for (Marker country : countryMarkers) {
       intMap.put( (String)country.getProperty("name"), 0 );
    }
    for (PointFeature quake : earthquakes) {
        String country = (String)quake.getProperty("country");
        if (country != null && intMap.get(country) != null ) {
            intMap.put(country, intMap.get(country) + 1);
        }
    }
    for (Map.Entry<String, Integer> entry : intMap.entrySet()) {
        System.out.println(entry.getKey() + " : " + entry.getValue());
    }    
}
```

## InheritanceとConstructor
 - ⑴ constructorがクラス内に無い時は自動でcompilerが用意する(但し、引数付constructorが存在する時、default(no-argument)constructorは自動で作成されない(c++と一緒))。extendsキーワードが無い時はextends Objectをcompilerが用意する。 <br><br>
 - ⑵ constructorの最初の行は this();またはsuper();でなければならない。<br>
     ※this();は他のConstructorを呼び出す。例えば引数なしのconstructorが呼ばれた時にデフォルトの引数をセットしたい時にthis('default value');のように呼び出す。super()はその先のconstructorにセットされる。<br><br>
 - ⑶ ⑵のthis();と super();どちらも無い時はsuper();をcompilerがConstructorの最初の行に用意する。(つまり最も最初に呼ばれるコードはObjectクラスのconstructorという事になる。) <br><br>
 - ⑷ ↑はメソッドでも応用可能で、method overriding でparent/childどちらも同じメソッドを有する時に敢えてparentのメソッドを呼びたい時はsuper.methodName();で呼ぶ事ができる。<br>

## Reference and Object type
 - Reference type => Compile time decisions. <br>
 - Object type => Run time decisions. <br><br>

## Abstract classes and Interfaces
 - Abstraction: <br>
 　　Abstractionは大事な内容はメソッドを中身まで記載するが、そうでない場合はメソッドの宣言だけで中身を隠している。 <br>
 　　↑の仕様を満たすため、共通コードなどは前もって定義しておき、動きの内容は実装者に任せるようなクラスに使われる。 <br>
 - Interface: <br>
 　　Interfaceはcodeの設計書を提供する。(c++の.hファイルのようなもの) <br>
 　　Interfaceはconstantsとabstractメソッドしか持てない。(何もキーワードをつけなければpublic static finalの定数となる。) <br>
 　　Interfaceはpublic static final(=constant)しか使えないので、Constantsに何もつけなくてもpublic static final変数(=constant)になる<br>
 　　↑の特徴により、純粋にAbstractなクラスを継承してオブジェクトの継承ツリーを構成したい時に利用する。 <br>
 　　c++の < や > のcompare関連のメソッドもComparable<E> というinterfaceで用意されている(メソッドはcompareTo)ので継承(Interfaceを継承する時はextendsではなくimplementsを使用する)して実装できる。 <br>
 - どちらもそのままではインスタンスを作れない。(Class内のいずれかのメソッドがabstractであればabstract classでなければならず、subclassでoverrideが必要) <br><br>
 - 用語説明:<br>
 　　final 変数: 変更できない変数（=constant）(値を設定しなかった時はConstructorの中で１度だけ設定できる) <br>
 　　static final 変数: static{}ブロックの中で１度だけ設定できる <br>
 　　final メソッド: overrideできないメソッド <br>
 　　final class: extendsできないclass <br>
 　　static 変数: staticメソッドからダイレクトに参照できるクラス変数（継承したサブクラスがアクセスできる変数)（動きが基本はインスタンス経由なのでprotectedなどのキーワードはつけない。） <br>
 　　private 変数: 継承したサブクラスが直接アクセスできない変数 <br><br>
 具体的な違いは..<br>
 - Abstract classはメソッドの中身も記載できるがInterfaceは出来ない。 <br>
 - Abstract classはpublic/private,static,finalキーワードの変数を自由に使用できるがInterfaceはpublic static finalしか使えない(=private変数が持てない。)。 <br>
 - Abstract classはabstract classとclassを継承できるがInterfaceはinterfaeしか継承出来ない。 <br>
 - Abstract classは１つしかclassを継承できないがInterfaceはinterfaeを複数継承出来る。 <br>
 - Abstract classのメソッドはabstractキーワードが必須(メソッドの中身を書いているものは書かない。)だがInterfaceはabstractキーワードはoptional。(全部abstractだから) <br><br>

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
  // 特にPolymorphismの中でややこしいのは以下:
  5. 上記コードでp[0].status()というPersonにしか無いメソッドを呼びその中でthis.isAsleep()というPersonにもStudentにもあるメソッドを呼んだ場合、StudentのクラスのisAsleepメソッドが呼ばれる。
*/
```

## User Events (Event-Driven code)
 - MouseListenerとKeyListenerはInterfaceなので、提供するjarなどのclassファイル内でデフォルトの動きが実装がされている。それをoverridingする事でイベントハンドリング処理が書ける。<br><br>

## Search
 - linear search: <br>
    　Airport ap1 = new Airport("Montreal", "Canada", "YMX"); <br>
    　Airport ap2 = new Airport("Lagos", "Nigeria", "LOS"); <br>
    　Airport ap3 = new Airport("Essen", "Germany", "ESS"); <br>
    　Airport ap4 = new Airport("Chicago", "USA", "ORD"); <br>
    　Airport ap5 = new Airport("Beijing", "China", "PEK"); <br>
    　Airport ap6 = new Airport("Sydney", "Australia", "SYD"); <br>
    　Airport ap7 = new Airport("Agra", "India", "AGR"); <br>
    　Airport[] airports = new ArrayList{ap1, ap2, ap3, ap4, ap5, ap6, ap7}; <br>
    　Loop to Find Beijing index 0 1 ->.. 4<br><br>
 - binary search: <br>
    　// ListではなくDictionaryを使用する。（keyによりTree構造でソートされるので。） <br>
    　2分割して目的地まで比較しながら探索する<br><br>
 - Why binary search is better than linear search: <br>
    　log2nだけBinary Searchが早い。（1Mのデータがある時、lineary searchが最大でn(=1M)処理が必要に対して、binary searchは最大でlog2n(=20)で処理が終わる）<br><br>

```
// toFind is a city name by linear search
public static String findAirportCode (String toFind, Airport[] airports)
{
  String airportCode = "";
  for (Airport apObj : airports) {
    /* 
      '=='は、String objectのメモリ上の参照先が同じかどうかを比較する。
      純粋に文字そのものが同一かどうかは'=='ではなく、str.equals(str)となる。
      Stringに限らず、オブジェクト同士を比較する時はメモリ参照先を比較する'=='ではなく.equals()を使用する。
    */
    if (toFind.equals(apObj.getCity()) {
      airportCode = apObj.getCode();
      break;
    }
  }
  if (airportCode == "") {
    return null;
  } else {
    return airportCode;
  }
}
```
```
// Binary Search Algorithm
public static String findAirportCode (String toFind, Airport[] airports)
{
  initialize low = 0; high = size of list -1; int mid;
  while (low <= high) {
    mid = (high+low)/2;
    // (String)str1.compareTo(str2)は辞書学的に(Unicodeの大小で)比較し、同じであれば0を返す。str1の最初の文字が辞書学的にstr2より大きければ正の数を、そうでなければ負の数を返す。
    int compare = toFind.compareTo(airports[mid].getCity());
    if (compare < 0){
      high = mid - 1
    } else if (compare > 0){
      low = mid + 1
    } else {
      return airports[mid].getCode();
    }
  }
  return null
}
```
## Sort
 - Selection Sort: 一番値が低いelementを探し一番先頭に持ってくる。これを繰り返す。(パフォーマンス最悪。ダメな例)<br>
    　For each position i from 0 to length-2 { swap the smallest element to i position; }<br>
 - Insertion Sort: 行ったり来たりするソート。（隣り合う２つに逆順が見つかるたびにindex=1まで戻る。パフォーマンス悪い。但しSelection Sortと違ってソートされた配列に対しては滅法早い。）<br>
      indexの0から順に隣り合う２つを比較し、swapが発生したら１つ前のindexも比較する。これをindex=1まで続ける。<br>
    　For each position position from 1 to length-1 { currInd = position; while (currInd>0 && val[currInd] < val[currInd-1]) {swap(); currInd = currInd-1;} }<br>
 - Collections.sort: Merge-sortを使っている。<br>
    　For each <br>
```
// Collection.sort<AirPort>のAirPortクラスオブジェクトでソートするには: genericを使う(c++との違いはimplementsキーワード)
public class AirPort implements Comparable<AirPort> {
  // 以下自分のオブジェクトがotherのオブジェクトより小さい時はnegativeの値を返す。
  public int compareTo(Airport other) {
    return (this.getCity()).compareTo(other.getCity()); // StringはComparableだからOK.
  }
  
  public static void main (String[] args) {
    List<Airport> airports = new ArrayList();
    ...
    airports.add(...);
    Collections.sort(airports);
  }

  // 実際にソートする時は元を壊さないように。また、以下はabstractのリスト(List<Marker>)からCompareToのあるEarthquakeMarkerにCASTしている。
  private void sortAndPrint(int numToPrint)
  {
    EarthquakeMarker[] markers = quakeMarkers.toArray(new EarthquakeMarker[quakeMarkers.size()]);
    Arrays.sort(markers, Collections.reverseOrder()); // Arrayをソートする時はArrays.sortを使う
    int i = 0;
    if (numToPrint > markers.length) {
      numToPrint = markers.length;
    }
    for (i=0; i < numToPrint; i++) {
      System.out.println(markers[i]);
    }		
  }
}
```

# Ⅱ. Data Structures and Performance
## 環境構築
```
● "JavaFX 11 is not part of the JDK anymore"な時の対処方法

1. https://gluonhq.com/products/javafx/
ここからOpen JavaFXをダウンロードし、解答、libフォルダに置く

2. .classpathに以下を追加
	<classpathentry kind="lib" path="lib/javafx-sdk-11.0.2/lib/javafx.base.jar"/>
	<classpathentry kind="lib" path="lib/javafx-sdk-11.0.2/lib/javafx.controls.jar"/>
	<classpathentry kind="lib" path="lib/javafx-sdk-11.0.2/lib/javafx.fxml.jar"/>
	<classpathentry kind="lib" path="lib/javafx-sdk-11.0.2/lib/javafx.graphics.jar"/>

3. VSCodeの虫（Debug Launch;）をクリックし、create launch.jsonをクリック

4. launch.jsonの中を以下のように修正(CodeLensというのはVSCodeがRun時に用意してくれるので試しにRunしてみるといい。)
        {
            "type": "java",
            "name": "CodeLens (Launch) - MainApp",
            "request": "launch",
            "cwd": "${workspaceFolder}",
            "vmArgs": "--module-path lib/javafx-sdk-11.0.2/lib --add-modules javafx.controls,javafx.fxml",
            "mainClass": "application.MainApp",
            "projectName": "MOOCTextEditor"
        },
※. JDK8からJDK9移行時に生ずるエラーの対処策: https://blog.codefx.org/java/five-command-line-options-hack-java-module-system/
                                      https://docs.oracle.com/javase/jp/9/migrate/toc.htm
   --add-exports:デフォルトでアクセスできないようになっている内部APIを使用する必要がある場合、これを使用してカプセル化を解除(--add-readsと共に使用)
   --add-opens: 非publicメンバーにアクセスするためにクラス・パスにあるコードにディープ・リフレクションの実行を許可する(Error: module javafx.graphics does not "opens javafx.scene.text" to unnamed moduleが出る対処)
   
● JDK8でdepricatedな、かつJDK9でremoveされたメソッドを使用している時の対処方法
install the Java 8 JDK and change the default build path and compiler compliance level in the project settings to use the older version of java instead, as well as changing the System Library that the project uses

※. ライセンス規約にも注意: https://ityarou.com/ithoudan0261/
Java SE: Java Platform, Standard Edition : デスクトップ（パソコン）やサーバーでJavaアプリケーションを開発する上で必要な機能が詰まったもの
　（２０１８年９月のバージョン（Java SE 11）から一部有償化※2。Java SE 10以前も無償サポート終了（更新すると一部有償化）。Androidの開発元のGoogleは、OpenJDKに移行済み。KotlinやGoなど企業が採用する言語は移る可能性がある。）
Java EE: Java Platform, Enterprise Edition(Java EE) : サーバー等の機能が詰まったもの（Web開発時はJava SEとセットで使う）
JDK： Java SE Development Kit : Javaを開発する上で必要(Java SE Dev Kit: OpenJDKを使えば無料で開発できるがサポート期間が極端に短い（半年）)
JRE: Java Runtime Environment(JRE) : 無料, Javaを動かす上で必要

※2. Oracle Technology Network License: 2019年4月16日、米OracleはOracle JDKのライセンスを変更 :
Oracle Binary Code License（BCL）はデスクトップ・サーバーともに商用・本番環境での無償利用が許されていたが、新しいライセンスOracle Technology Network Licenseでは個人利用・開発目的に限られる。
商用で利用したい場合は、Oracle Java SE Subscriptionsの購入が必要。(つまり今後これらのツールを更新して開発したり更新したものは無料で使えなくなった。)
```





