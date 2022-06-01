# Interfaces

*Interfaces* sind auch abstrakte Klassen. Interfaces enthalten ausschließlich abstrakte Methoden (keine Methode darf implementiert sein). Interfaces beschreiben **Schnittstellen**. Für Interfaces wird nicht das Schlüsselwort `class`, sondern `interface` verwendet. Klassen erben nicht von Interfaces, sondern **implementieren** sie. Deshalb wird auch nicht das Schlüsselwort `extends`, sondern das Schlüsselwort `implements` verwendet. Während in Java nur von genau einer Klasse geerbt werden kann (also auch nur von genau einer abstrakten Klasse), kann eine Klasse beliebig viele Interfaces implementieren. 

Interfaces sind automatisch `abstract`, d.h. das Schlüsselwort `abstract` muss nicht angegeben werden. Auch die Methoden in Interfaces müssen nicht als abstrakt gekennzeichnet werden. Interfaces können, wie abstrakte Klassen auch, als Typen verwendet werden. 

| Abtrakte Klasse | Interface |
|-----------------|-----------|
|können abstrakte und nicht-abstrakte (also implementierte) Methoden haben |können nur abstrakte Methoden beinhalten |
|es kann nur von einer (abstrakten) Klasse geerbt werden (Schlüsselwort `extends`) |es können beliebig viele Interfaces implementiert werden (Schlüsselwort `implements`), mehrere Interfaces durch Komma getrennt |
|abstrakte Klassen können selbst Interfaces implementieren |Interfaces können keine abstrakten Klassen implementieren (alle Methoden müssen ja abstrakt sein) |
|das Schlüsselwort `abstract` deklariert eine abstrakte Klasse (und eine abstrakte Methode) |das Schlüsselwort `interface` deklariert ein Interface |
|eine abstrakte Klasse kann von einer anderen abstrakten Klasse erben und mehrere Interfaces implementieren |ein Interface kann nur von *einem* anderen Interface erben |
|abtrakte Klassen können `final` Variablen (Konstanten), nicht-finale Variablen, statische und nicht-statische Variablen als Eigenschaften beinhalten |Interfaces können nur statische Konstanten (`static final`) als Eigenschaften beinhalten |
|die Eigenschaften einer abstrakten Klasse können `private`, `protected`, *default* und `public` sein |in Interfaces sind alle Eigenschaften `public` |
|Bsp.: `public abstract class Shape{ public abstract void draw(); }` |Bsp.: `public interface Drawable{ void draw(); }` | 


## Das Interface `Comparable`

Ehe wir uns ein eigenes Interface schreiben, schauen wir uns zunächst die Verwendung eines bereits existierenden Interfaces an. Es handelt sich um das Interface [Comparable](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Comparable.html) aus dem `java.lang`-Paket. Wenn Sie sich die Java-Dokumentation dieses Interfaces einmal anschauen, dann sehen Sie, dass es von sehr vielen Klassen implementiert wird. Dieses Interface enthält genau eine (natürlich abstrakte) Methode `compareTo()`. Diese Methode kennen wir auch schon, denn wir haben sie betrachtet, als wir [in Prog1 Strings](https://freiheit.f4.htw-berlin.de/prog1/hilfsklassen/#die-klasse-string) kennengelernt haben. 

Die Methode `this.compareTo(Object obj)` wird verwendet, um zu vergleichen, ob `this` größer, kleiner oder gleich `obj` ist. Das bedeutet, dass wir `compareTo()` in unserer Klasse implementieren sollten, wenn wir die Objekte unserer Klasse der Größe nach ordnen wollen, wenn wir also ermöglichen wollen, dass die Objekte der Klasse sortiert werden können. 

Die Methode `this.compareTo(Object obj)` gibt ein `int` zurück, für dessen Wert Folgendes gelten soll:

- ist der zurückgegebene `int`-Wert positiv (`> 0`), dann ist `this` **größer** als `obj`,
- ist der zurückgegebene `int`-Wert negativ (`< 0`), dann ist `this` **kleiner** als `obj`,
- ist der zurückgegebene `int`-Wert `0`, dann ist `this` **gleich** `obj`.

Angenommen, wir wollen für die folgende Klasse `Rectangle` (aus dem Abschnitt [Abstrakte Klassen](../abstrakt/#rectangle-erbt-von-shape)) festlegen, dass die Rechtecke der Größe nach geordnet werden können. Gegeben ist also zunächst folgende Klasse (wir verwenden hier auch `Shape` aus [Abstrakte Klassen](../abstrakt/#ein-beispiel-die-abstrakte-klasse-shape)):

```java linenums="1"
public class Rectangle extends Shape
{
	private int width, height;
	
	public Rectangle(int width, int height)
	{
		this.width = width;
		this.height = height;
	}
	
	@Override
	public double perimeter() 
	{	
		return (2.0 * (this.width + this.height));
	}

	@Override
	public double area() 
	{
		return (this.width * this.height);
	}
}
```

Die Klasse `Rectangle` erbt also von der abstrakten Klasse `Shape` und **muss** deshalb die Methoden `perimeter()` und `area()` implementieren. Nun geben wir an, dass `Rectangle` auch das Interface `Comparable` implementieren soll. Dazu ergänzen wir die erste Zeile um `implements Comparable`, d.h. die Klassendeklaration sieht jetzt so aus:

```java linenums="1"
public class Rectangle extends Shape implements Comparable
{
```

Wenn Sie das hinzufügen, stellen wir fest, dass ein **Fehler** erzeugt wird (die Klasse lässt sich nicht compilieren). Die Fehlerausgabe besagt: `The type Rectangle must implement the inherited abstract method Comparable.compareTo(Object)`. Es werden zwei `QuickFixes` angeboten, 

- entweder `Add unimplemented methods` 
- oder `Make type Rectangle abstract`. 

Letzteres wollen wir aber nicht (`Rectangle` soll nicht zu einer abstrakten Klasse gemacht werden). Also wählen wir `Add unimplemented methods`. Eclipse fügt uns die `compareTo()`-Methode in den Code ein:

```java linenums="1" hl_lines="23-27"
public class Rectangle extends Shape implements Comparable
{
	private int width, height;
	
	public Rectangle(int width, int height)
	{
		this.width = width;
		this.height = height;
	}
	
	@Override
	public double perimeter() 
	{	
		return (2.0 * (this.width + this.height));
	}

	@Override
	public double area() 
	{
		return (this.width * this.height);
	}

	@Override
	public int compareTo(Object o) {
		// TODO Auto-generated method stub
		return 0;
	}
}
```

Jetzt lässt sich der Code bereits compilieren, wir erhalten aber noch eine Warnung:

```bash
Comparable is a raw type. References to generic type Comparable<T> should be parameterized
``` 

Diese Warnung besagt, dass wir, wie wir das von Collections bereits kennen, auch das Interface `Comparable` **typisieren** sollen. Das wollen wir auch tun, denn wir implementieren dieses Interface hier für unsere Klasse `Rectangle`. Wir typisieren deshalb `Comparable` mit `Rectangle`: 


```java linenums="1" hl_lines="1"
public class Rectangle extends Shape implements Comparable<Rectangle>
{
	private int width, height;
	
	public Rectangle(int width, int height)
	{
		this.width = width;
		this.height = height;
	}
	
	@Override
	public double perimeter() 
	{	
		return (2.0 * (this.width + this.height));
	}

	@Override
	public double area() 
	{
		return (this.width * this.height);
	}

	@Override
	public int compareTo(Object o) {
		// TODO Auto-generated method stub
		return 0;
	}
}
```

Interssanterweise ist nun zwar unsere Warnung weg, aber dafür erhalten wir erneut einen Fehler:

```bash
The type Rectangle must implement the inherited abstract method Comparable<Rectangle>.compareTo(Rectangle)
```

Dadurch, dass wir `Comparable` mit `Rectangle` typisieren (was korrekt ist), wird nun verlangt, dass wir nicht mehr die Methode 

```java 
	@Override
	public int compareTo(Object o) {
		// TODO Auto-generated method stub
		return 0;
	}
```

implementieren, sondern die Methode 

```java 
	@Override
	public int compareTo(Rectangle o) {
		// TODO Auto-generated method stub
		return 0;
	}
```

Der Typ des Parameters hat sich durch unsere Typisierung also geändert. Das ist gut, denn dann müssen wir nicht mehr, wie z.B. bei `equals(Object o)`, prüfen, ob es sich bei dem übergebenen Objekt tatsächlich um ein `Rectangle` handelt. Wir ändern also den Parametertyp in `compareTo()`:

```java linenums="1" hl_lines="24"
public class Rectangle extends Shape implements Comparable<Rectangle>
{
	private int width, height;
	
	public Rectangle(int width, int height)
	{
		this.width = width;
		this.height = height;
	}
	
	@Override
	public double perimeter() 
	{	
		return (2.0 * (this.width + this.height));
	}

	@Override
	public double area() 
	{
		return (this.width * this.height);
	}

	@Override
	public int compareTo(Rectangle o) {
		// TODO Auto-generated method stub
		return 0;
	}
}
```

In Zukunft typisieren wir das `Comparable`-Interface noch, bevor wir `Add unimplemented methods` wählen. Wir typisieren es stets mit der Klasse, in der wir das Interface implementieren. 

Für die Implementierung müssen wir uns nun überlegen, wann ein `Rectangle`-Objekt größer (kleiner/gleich) sein soll, als ein anderes. Da `compareTo()` ein `int` zurückgibt, könnten wir z.B. die Summen von `height` und `width` verwenden:

```java linenums="23" 
	@Override
	public int compareTo(Rectangle o) {
		int diff = (this.height+this.width) - (o.height+o.width);
		return diff;
	}
```

Wenn die Summe von `height` und `width` von `this` größer ist, als von `o`, dann geben wir eine positive `int`-Zahl zurück, wenn sie kleiner ist, dann eine negative `int`-Zahl und wenn sie gleich sind, dann `0`. Damit entsprechen wir den Vorgaben von `compareTo()`. 


### Laufzeittypen eines `Rectangle`-Objektes

Ein `Rectangle`-Objekt ist nicht nur vom Laufzeittyp `Rectangle`, sondern auch

- von Laufzeittyp `Shape`, wegen `public class Rectangle extends Shape`, 
- vom Laufzeittyp `Comparable`, wegen `public class Rectangle implements Comparable` und
- vom Laufzeittyp `Object`, weil das **immer** so ist, weil jede Klasse implizit von `Object` erbt. 

Wir könnten nun also in jeder beliebigen Klasse eine Sortiermethode haben, z.B.: 

```java linenums="1"
	public static void sortieren(Comparable[] unsorted)
	{	
		for(int bubble=1; bubble<unsorted.length; bubble++)
		{
			for(int index=0; index<unsorted.length-bubble; index++)
			{
				if(unsorted[index].compareTo(unsorted[index+1]) > 0) 
				{
					Comparable tmp = unsorted[index];
					unsorted[index] = unsorted[index+1];
					unsorted[index+1] = tmp;			
				}
			}
		}
	}
```

Die Methode implementiert Bubble-Sort. In Zeile `7` verwenden wir die `compareTo()`-Methode. Das geht genau deshalb, weil klar ist, dass ein Objekt, das (auch) vom Typ `Comparable` ist, diese Methode auf jeden Fall als Eigenschaft besitzt. Wenn wir nun in der Klasse, in der die Methode `sortieren()` implementiert ist, folgende `main()`-Methode haben:

```java
public static void main(String[] args) {
		Rectangle[] rectArr = new Rectangle[6];
		rectArr[0] = new Rectangle(9, 13);
		rectArr[1] = new Rectangle(4, 17);
		rectArr[2] = new Rectangle(12, 5);
		rectArr[3] = new Rectangle(8, 9);
		rectArr[4] = new Rectangle(10, 11);
		rectArr[5] = new Rectangle(5, 15);
		System.out.printf("%n%n------------------------ unsortiert --------------------------%n%n");
		for(Rectangle r : rectArr)
		{
			System.out.println(r.toString());
		}
		System.out.printf("%n%n------------------------- sortiert ---------------------------%n%n");
		sortieren(rectArr);
		for(Rectangle r : rectArr)
		{
			System.out.println(r.toString());
		}
	}
``` 

dann erhalten wir folgende Ausgabe:

```bash
------------------------ unsortiert --------------------------

[  9 x 13 = 117,00 ] 
[  4 x 17 =  68,00 ] 
[ 12 x  5 =  60,00 ] 
[  8 x  9 =  72,00 ] 
[ 10 x 11 = 110,00 ] 
[  5 x 15 =  75,00 ] 


------------------------- sortiert ---------------------------

[ 12 x  5 =  60,00 ] 
[  8 x  9 =  72,00 ] 
[  5 x 15 =  75,00 ] 
[  4 x 17 =  68,00 ] 
[ 10 x 11 = 110,00 ] 
[  9 x 13 = 117,00 ] 
```

für den Fall, dass wir in unserer Klasse `Rectangle` auch die `toString()`-Methode wie folgt implementiert haben:

```java
	@Override
	public String toString()
	{	String s = String.format("[ %2d x %2d = %6.2f ] ", this.width, this.height, this.area()); 
		return s;
	}
```

!!! success
	Wir haben für unsere Klasse `Rectangle` das Interface `Comparable` implementiert. Das bedeutet, dass wir in `Rectangle` die Methode `compareTo()` so implementiert haben, dass `Rectangle`-Objekte der Größe nach sortiert werden können. Wir haben also eine *Ordnung* über `Rectangle`-Objekte definiert. Nach "außen" ist sichtbar, dass wir eine solche Ordnung implementiert haben, dass `Rectangle`-Objekte also *sortierbar* sind, weil sie (auch) vom Typ `Comparable` sind. Für alle Objekte, die in Java existieren, wissen wir, dass sie *sortierbar* sind, sobald sie auch vom Typ `Comparable` sind. `Comparable` stellt also eine *Schnittstelle* zur Sortierbarkeit dar. Wenn wir eine eigene Klasse schreiben und wir eine *Ordnung* über die Objekte dieser Klasse definieren können, sollten wir das Interface `Comparable` implementieren, denn dadurch geben wir nach "außen" an, dass sich die Objekte der Klasse *sortieren* (*ordnen*) lassen.

### Zwischenfazit

Wir haben nun schon mehrere Methoden kennengelernt, die wir für eigene Klassen implementieren sollten.

- Die `toString()`-Methode erben wir von `Objects`. Wir sollten `toString()` für "unsere" Klassen überschreiben, damit wir eine textuelle Repräsentation unserer Objekte haben. `toString()`wird implizit angewendet, sobald eine `String`-Repräsentation erforderlich ist, z.B. ist `System.out.println(refVariable);`das Gleiche wie `System.out.println(refVariable.toString());`. 
- Die `equals()`-Methode erben wir ebenfalls von `Objects`. Wir sollten `equals()` für "unsere" Klassen implementieren, um zu definieren, wann Objekte "unserer" Klasse *gleich* sind. Hierbei ist wichtig, zu beachten, dass `refVar1 == refVar2` ein reiner *Referenzvergleich* ist, der nichts darüber aussagt, ob die Objekte *gleich*  sind, sondern nur ein `true` ergibt, wenn beide Variablen auf dasselbe Objekt zeigen. Die *Gleichheit* von Objekten wird mittels `equals()`-Methode definiert. 
- Die `hashCode()`-Methode erben wir ebenfalls von `Objects`. Wir sollten `hashCode()`genau dann implementieren, wenn wir `equals()` implementieren. Wichtig ist, dass zwei Objekte den gleichen Hash-Code haben (`hashCode()` liefert den gleichen `int`-Wert zurück), wenn die beiden Objekte laut `equals()` gleich sind. Gut ist darüber hinaus (aber nicht Bedingung), dass zwei Objekte einen unterschiedlichen Hash-Code haben, wenn sie laut `equals()`-Methode nicht gleich sind (`equals()`liefert `false` zurück). Der Hash-Code wird bei Hash-basierten Datentypen, wie z.B. Collections verwendet, um diese einzusortieren. 
- Die Methode `compareTo()` muss implementiert werden, wenn wir das Interface `Comparable` implementieren. Mithilfe von `compareTo()` legen wir eine Ordnung über die Objekte der Klasse fest, d.h. wir geben an, wann ein Objekt größer/kleiner/gleich einem anderen Objekt der gleichen Klasse ist. Dadurch, dass wir das `Comparable`-Interface implementieren, zeigen wir nach "außen", dass die Objekte unserer Klasse *sortierbar*  sind. 


### Eine bessere Implementierung

Wir haben bereits bei der Implementierung der Klasse `Rectangle` gesehen, dass wir das Interface `Comparable` bei der Implementierung von `Rectangle` typisieren sollten. Das wäre für eine wirklich korrekte Implementierung der Methode `sortieren()` ebenfalls angebracht. Dann würden wir in dieser Methode `Comparable` mit `Rectangle` typisieren:

```java linenums="1" hl_lines="1 7 9"
	public static void sortieren(Comparable<Rectangle>[] unsorted)
	{	
		for(int bubble=1; bubble<unsorted.length; bubble++)
		{
			for(int index=0; index<unsorted.length-bubble; index++)
			{
				if(unsorted[index].compareTo((Rectangle) unsorted[index+1]) > 0) 
				{
					Comparable<Rectangle> tmp = unsorted[index];
					unsorted[index] = unsorted[index+1];
					unsorted[index+1] = tmp;			
				}
			}
		}
	}
```

Wenn wir also den Typ `Comparable` verwenden, dann ergänzen wir ihn um die Typisierung `<Rectangle>` (Zeilen `1` und `9`). Das führt allerdings dazu, dass wir dann auch in Zeile `7` den Typ von `unsorted[index+1]` nach `Rectangle` konvertieren müssen (`(Rectangle) unsorted[index+1]`). Damit verlieren wir aber unsere **allgemeine** Anwendbarkeit der Methode `sortieren()` für alle Klassen, die `Comparable` implementiert haben. Insbesondere würde die Methode dann nicht mehr für z.B. die Klasse `Circle` anwendbar sein: 

```java linenums="1"
public class Circle extends Shape implements Comparable<Circle>
{
	private double radius;
	
	public Circle(double radius)
	{
		this.radius = radius;
	}

	@Override
	public double perimeter() 
	{
		return Math.PI * 2.0 * this.radius;
	}

	@Override
	public double area() 
	{
		return Math.PI * this.radius * this.radius;
	}

	@Override
	public int compareTo(Circle o) {
		if(this.radius > o.radius) return 1;
		else if(this.radius < o.radius) return -1;
		else return 0; // this.radius == o.radius
	}
	
	
	@Override
	public String toString()
	{	String s = String.format("(radius: %.2f -> area: %.2f ] ", this.radius, this.area()); 
		return s;
	}

}
```

Wenn wir nun versuchen würden, die `sortieren()`-Methode auf ein `Circle[]` anzuwenden, ließe sich das Programm gar nicht compilieren:


```java linenums="1" hl_lines="16"
	public static void main(String[] args) 
	{
		Circle[] circArr = new Circle[6];
		circArr[0] = new Circle(5.0);
		circArr[1] = new Circle(5.5);
		circArr[2] = new Circle(4.0);
		circArr[3] = new Circle(2.5);
		circArr[4] = new Circle(7.0);
		circArr[5] = new Circle(1.0);
		System.out.printf("%n%n------------------------ unsortiert --------------------------%n%n");
		for(Circle c : circArr)
		{
			System.out.println(c.toString());
		}
		System.out.printf("%n%n------------------------- sortiert ---------------------------%n%n");
		// sortieren(circArr);		// Fehler
		for(Circle c : circArr)
		{
			System.out.println(c.toString());
		}
	}
```

Deshalb wäre es eine **bessere Implementierung**, wenn wir das Interface `Comparable` nicht in den konkreten Klassen `Rectangle` und `Circle` (und in jeder weiteren Klasse, die wir auf der Basis von `Shape` erstellen) implementieren, sondern gleich in der Abstrakten Klasse `Shape`:

```java
public abstract class Shape implements Comparable<Shape>
{
	
	public abstract double perimeter();
	public abstract double area();

}
```

Da `Shape` eine abstrakte Klasse ist, **muss** die Methode `compareTo()` **nicht** in `Shape` implementiert werden. Diese Methode würde nun `abstract` an alle Klassen vererbt, die von `Shape` erben:

=== "Rectangle.java"
	```java linenums="1" hl_lines="1 24 26"
	public class Rectangle extends Shape
	{
		private int width, height;
		
		public Rectangle(int width, int height)
		{
			this.width = width;
			this.height = height;
		}
		
		@Override
		public double perimeter() 
		{	
			return (2.0 * (this.width + this.height));
		}

		@Override
		public double area() 
		{
			return (this.width * this.height);
		}

		@Override
		public int compareTo(Shape o) 
		{
			Rectangle r = (Rectangle)o;
			int diff = (this.height+this.width) - (r.height+r.width);
			return diff;
		}
		
		@Override
		public String toString()
		{	String s = String.format("[ %2d x %2d = %6.2f ] ", this.width, this.height, this.area()); 
			return s;
		}

	}
	```

=== "Circle.java"
	```java linenums="1" hl_lines="1 23 25"
	public class Circle extends Shape
	{
		private double radius;
		
		public Circle(double radius)
		{
			this.radius = radius;
		}

		@Override
		public double perimeter() 
		{
			return Math.PI * 2.0 * this.radius;
		}

		@Override
		public double area() 
		{
			return Math.PI * this.radius * this.radius;
		}

		@Override
		public int compareTo(Shape o) 
		{
			Circle c = (Circle)o;
			if(this.radius > c.radius) return 1;
			else if(this.radius < c.radius) return -1;
			else return 0; 	// this.radius == c.radius
		}
		
		@Override
		public String toString()
		{	String s = String.format("(radius: %.2f -> area: %.2f ] ", this.radius, this.area()); 
			return s;
		}

	}
	```

Beachten Sie, dass die Klassen `Rectangle` und `Circle` jetzt **nur noch** von `Shape` erben, aber nicht mehr das Interface `Comparable` implementieren (jeweils Zeile `1`). Es darf nicht mehrmals von einer Klasse implementiert werden und `Shape` implementiert es ja bereits. 

Da `Shape` diese Interface aber *implementiert*, wird die Methode `compareTo()` als abstrakte Methode an die Klassen `Rectangle` und `Circle` vererbt. Die Methode muss also von diesen Klassen implementiert werden. Nun wird sie aber mit dem Parametertyp `Shape` vererbt (Zeile `24` in `Rectangle.java` bzw. `23` in `Circle.java`). Dieser Parameter muss deshalb zunächst innerhalb der Methode `compareTo()` konvertiert werden (Zeile `25` in `Circle.java` bzw. `26` in `Rectangle.java`). 

Die **allgemeine** Anwendung der Methode `sortieren()` in der Testklasse gelingt nun aber:

=== "TestklasseShape.java"
	```java linenums="1" hl_lines="4 10 12"
	public class TestklasseShape 
	{

		public static void sortieren(Comparable<Shape>[] unsorted)
		{	
			for(int bubble=1; bubble<unsorted.length; bubble++)
			{
				for(int index=0; index<unsorted.length-bubble; index++)
				{
					if(unsorted[index].compareTo((Shape) unsorted[index+1]) > 0) 
					{
						Comparable<Shape> tmp = unsorted[index];
						unsorted[index] = unsorted[index+1];
						unsorted[index+1] = tmp;			
					}
				}
			}
		}

		public static void main(String[] args) 
		{
			Rectangle[] rectArr = new Rectangle[6];
			rectArr[0] = new Rectangle(9, 13);
			rectArr[1] = new Rectangle(4, 17);
			rectArr[2] = new Rectangle(12, 5);
			rectArr[3] = new Rectangle(8, 9);
			rectArr[4] = new Rectangle(10, 11);
			rectArr[5] = new Rectangle(5, 15);
			System.out.printf("%n%n------------------------ unsortiert --------------------------%n%n");
			for(Rectangle r : rectArr)
			{
				System.out.println(r.toString());
			}
			System.out.printf("%n%n------------------------- sortiert ---------------------------%n%n");
			sortieren(rectArr);
			for(Rectangle r : rectArr)
			{
				System.out.println(r.toString());
			}
			
			Circle[] circArr = new Circle[6];
			circArr[0] = new Circle(5.0);
			circArr[1] = new Circle(5.5);
			circArr[2] = new Circle(4.0);
			circArr[3] = new Circle(2.5);
			circArr[4] = new Circle(7.0);
			circArr[5] = new Circle(1.0);
			System.out.printf("%n%n------------------------ unsortiert --------------------------%n%n");
			for(Circle c : circArr)
			{
				System.out.println(c.toString());
			}
			System.out.printf("%n%n------------------------- sortiert ---------------------------%n%n");
			sortieren(circArr);
			for(Circle c : circArr)
			{
				System.out.println(c.toString());
			}
		}
	}
	```

Wir können nun alle Objekte sortieren lassen, die auf der Klasse `Shape` basieren. 

```bash
------------------------ unsortiert --------------------------

[  9 x 13 = 117,00 ] 
[  4 x 17 =  68,00 ] 
[ 12 x  5 =  60,00 ] 
[  8 x  9 =  72,00 ] 
[ 10 x 11 = 110,00 ] 
[  5 x 15 =  75,00 ] 


------------------------- sortiert ---------------------------

[ 12 x  5 =  60,00 ] 
[  8 x  9 =  72,00 ] 
[  5 x 15 =  75,00 ] 
[  4 x 17 =  68,00 ] 
[ 10 x 11 = 110,00 ] 
[  9 x 13 = 117,00 ] 


------------------------ unsortiert --------------------------

(radius: 5,00 -> area:  78,54 ] 
(radius: 5,50 -> area:  95,03 ] 
(radius: 4,00 -> area:  50,27 ] 
(radius: 2,50 -> area:  19,63 ] 
(radius: 7,00 -> area: 153,94 ] 
(radius: 1,00 -> area:   3,14 ] 


------------------------- sortiert ---------------------------

(radius: 1,00 -> area:   3,14 ] 
(radius: 2,50 -> area:  19,63 ] 
(radius: 4,00 -> area:  50,27 ] 
(radius: 5,00 -> area:  78,54 ] 
(radius: 5,50 -> area:  95,03 ] 
(radius: 7,00 -> area: 153,94 ] 
```

### Eine noch bessere Implementierung

Obwohl wir nun in `Shape` das Interface `Comparable` implementieren, geben wir die Verantwortung der Implementierung der Methode `compareTo()` an die konkreten Klassen `Rectangle` und `Circle` weiter. Es stellt sich die Frage, ob sich die `compareTo()`-Methode nicht bereits in `Shape` implementieren ließe. Die Antwort auf diese Frage sollte **ja** lauten, denn ansonsten sollten wir das Interface gar nicht bereits durch die abstrakte Klasse `Shape` implementieren lassen. Wir haben in `Shape` genügend Informationen, um die `compareTo()`-Methode zu implementieren. Wir können dafür entweder `perimeter()` oder `area()` verwenden. Wir entscheiden uns für die Verwendung von `area()`:

=== "Shape.java"
	```java linenums="1" hl_lines="7-13"
	public abstract class Shape implements Comparable<Shape>
	{
		
		public abstract double perimeter();
		public abstract double area();
		
		@Override
		public int compareTo(Shape o) 
		{	
			return (this.area() - o.area());
		}

	}
	```

In abstrakten Klassen müssen nicht, im Gegensatz zu Interfaces, alle Methoden abstrakt sein. Es können auch Methoden bereits implementiert werden. Diese Methoden müssen dann nicht mehr in den Klassen implementiert werden, die von der abstrakten Klasse erben. Die Klassen `Rectangle` und `Circle` benötigen also keine eigene Implementierung der `compareTo()`-Methode mehr:

=== "Rectangle.java"
	```java linenums="1" hl_lines="1 24 26"
	public class Rectangle extends Shape
	{
		private int width, height;
		
		public Rectangle(int width, int height)
		{
			this.width = width;
			this.height = height;
		}
		
		@Override
		public double perimeter() 
		{	
			return (2.0 * (this.width + this.height));
		}

		@Override
		public double area() 
		{
			return (this.width * this.height);
		}

		@Override
		public String toString()
		{	String s = String.format("[ %2d x %2d = %6.2f ] ", this.width, this.height, this.area()); 
			return s;
		}

	}
	```

=== "Circle.java"
	```java linenums="1" hl_lines="1 23 25"
	public class Circle extends Shape
	{
		private double radius;
		
		public Circle(double radius)
		{
			this.radius = radius;
		}

		@Override
		public double perimeter() 
		{
			return Math.PI * 2.0 * this.radius;
		}

		@Override
		public double area() 
		{
			return Math.PI * this.radius * this.radius;
		}

		@Override
		public String toString()
		{	String s = String.format("(radius: %.2f -> area: %.2f ] ", this.radius, this.area()); 
			return s;
		}

	}
	```

Wir haben ausgenutzt, dass in der Klasse `Shape` bereits genügend Informationen vorliegen, um die Methode `compareTo()` korrekt für alle Klassen zu implementieren, die von `Shape` erben. Diese Methode muss dann von diesen konkreten Klassen nicht mehr implementiert werden. Wir vermeiden so doppelten Code. Die `testklasseShape` bleibt unverändert für alle abgeleiteten Klassen aus `Shape` anwendbar. 
