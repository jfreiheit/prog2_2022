# JUnit-Tests

Testen von Programmen ist wichtig. Ohne Testen ist es kaum möglich, Fehler in Programmen zu entdecken. Bis jetzt haben wir unsere Programme immer durch reines Anwenden getestet, d.h. wir haben die implementierten Methoden aufgerufen und ihnen unterschiedliche Parameterwerte übergeben. Wir werden das jetzt ändern und nutzen dafür [JUnit](https://junit.org/junit5/).

## Allgemeines zum (Unit-)Testen

Der berühmte Informatiker [Edsger W. Dijkstra](https://de.wikipedia.org/wiki/Edsger_W._Dijkstra) hat über das Testen gesagt:

> Durch Testen kann man stets nur die Anwesenheit, nie aber die Abwesenheit von Fehlern beweisen

Das bedeutet, wir können durch das Testen Fehler finden. Wenn wir aber keine finden, dann wissen wir nicht, ob das Programm dann auch keine Fehler mehr enthält. Es ist leider nicht möglich, grundsätzlich die Fehlerfreiheit von Programmen zu prüfen. Aber das Testen stellt ein wichtiges Werkzeug dar, um Fehler zu entdecken. 

Es gibt verschiedene Arten von Tests:

![testen](./files/28_testen.png)

In der Abbildung erkennen wir, dass die Unit-tests, die wir hier kennenlernen wollen, am besten automatisierbar, am häufigsten und am einfachsten sind. Mit Unit-Tests können wir Methoden und Klassen testen, wobei wir die Tests implementieren. Die Idee ist, dass wir funktionale Einzelteile eines Programms separat und isoliert vom Rest auf ihre Korrektheit hin überprüfen. Wir versuchen extra, so wenig wie möglich die Effekte anderer Funktionalitäten bzw. Komponenten in die Tests einfließen zu lassen. Das erfolgt dann in den Komponenten- bzw. Integrationstests. Das hat zwei Vorteile: einerseits ist der zu prüfende Funktionsumfang überschaubar und andererseits können diese Unit-Tests leicht automatisiert ausgeführt werden. Es gibt, wie bereits eingangs erwähnt, keine Garantie von Fehlerfreiheit. Ein Nachteil der Unit-Tests besteht darin, dass sie schwierig für Methoden zu gestalten sind, in denen es Abhängigkeiten von der Laufzeitumgebung oder anderen Komponenten gibt.    

## JUnit

Für das Unit-Testen von Java-Programmen gibt es das Werkzeug [JUnit](https://junit.org/junit5/). Wir zeigen die Verwendung von JUnit an einem einführenden Beispiel. Angenommen, wir haben folgende kleine Javaklasse `Fakultaet`:

```java linenums="1"
public class Fakultaet {
	
	public long fakultaet(int number) throws IllegalArgumentException
	{
		if(number < 1) 
		{
			throw new IllegalArgumentException("Zahl muss groesser gleich 1 sein!");
		}
		long result = 1;
		for(int i = 2; i <= number; i++) 
		{
			long tmp = result;
			result *= i;
			if(tmp > result)
			{
				throw new IllegalArgumentException("Overflow!");
			}
		}
		return result;
	}
	
	public void print(int number)
	{
		System.out.printf("%3d! = %,d %n", number, fakultaet(number));
	}

}
```

Diese Klasse enthält eine Methode `fakultaet()`, welche ein `int` als Eingabeparameter erwartet und den Wert der Fakultätsberechnung als `long` zurückgibt. Ist der Eingabewert kleiner als `1` wird eine `IllegalArgumentException` geworfen. Tritt während der Berechnung der Fakultät ein Wertebereichsüberlauf auf (der größte Wert in `long` ist `9,223,372,036,854,775,807`), dann wird ebenfalls eine `IllegalArgumentException` geworfen. Die `print()`-Methode gibt die Berechnung geeignet aus. Angenommen, eine `main()`-Methode (oder eine andere) würde die Klasse in der folgenden Form verwenden:

```java linenums="1"
public static void main(String[] args) 
{
	Fakultaet f1 = new Fakultaet();
	int nr = 0;
	while(nr < 22)
	{
		try {
			f1.print(nr);
		}
		catch(IllegalArgumentException e) {
			System.out.println("number = " + nr + " : " + e.getMessage());
		}
		nr++;
	}
}
```

, dann wäre die Ausgabe: 

```bash
number = 0 : Zahl muss größer gleich 1 sein!
  1! = 1 
  2! = 2 
  3! = 6 
  4! = 24 
  5! = 120 
  6! = 720 
  7! = 5.040 
  8! = 40.320 
  9! = 362.880 
 10! = 3.628.800 
 11! = 39.916.800 
 12! = 479.001.600 
 13! = 6.227.020.800 
 14! = 87.178.291.200 
 15! = 1.307.674.368.000 
 16! = 20.922.789.888.000 
 17! = 355.687.428.096.000 
 18! = 6.402.373.705.728.000 
 19! = 121.645.100.408.832.000 
 20! = 2.432.902.008.176.640.000 
number = 21 : Overflow!
```

Wir wollen diese Klasse `Fakultaet` nun mithilfe von `JUnit` testen. Dazu wählen wir `File --> New --> JUnit Test Case`:

![junit](./files/104_junit.png)

Es erscheint folgender Dialog:

![junit](./files/105_junit.png)

Beachten Sie, dass unter `Class under test:` die Klasse `Fakultaet` eingetragen ist (oder Sie geben Sie dort ein). Der Name der Klasse wird mit `FakultaetTest` vorgeschlagen. Beachten Sie auch, dass bei den RadioButtons oben `New JUnit Jupiter test` ausgewählt ist. Klicken Sie auf `Finish`. 

Es entsteht folgende `FakultaetTest`-Klasse:

```java linenums="1"
import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.Test;

class FakultaetTest {

	@Test
	void test() {
		fail("Not yet implemented");
	}

}
```

Sollte es bei Ihnen Probleme bei der Erstellung der JUnit-Testklasse geben, dann beachten Sie folgende 2 Anforderungen:

1. Im `Java Build Path` muss `JUnit` enthalten sein:

	![junit](./files/106_junit.png)

	Hilfestellungen dazu finden Sie z.B. [hier](https://www.eclipse.org/community/eclipse_newsletter/2017/october/article5.php), [hier](https://www.eclipse.org/eclipse/news/4.7.1a/#junit-5-support) oder [hier](https://www.testingdocs.com/adding-junit5-library-to-a-project/). Beachten Sie, dass wir nicht `JUnit 4`, sondern `JUnit 5 (JUnit Jupiter)` verwenden! 

2. In der `module-info.java` muss `requires org.junit.jupiter.api;` eingetragen sein. 


Eine JUnit-Testklasse enthält keine `main()`-Methode, ist aber ausführbar. Sie enthält stattdessen Methoden, die mit `@Test` annotiert sind. Diese `@Test`-Methoden enthalten **Zusicherungen** (sogenannte **Assertions**). Mit diesen Assertions geben Sie das erwartete Ergebnis des Testfalls an. 

#### Assertions

Es gibt unterschiedliche Möglichkeiten, das *tatsächliche Ergebnis* der Ausführung mit dem *erwarteten Ergebnis* zu vergleichen:

- Gleichheit, Ungleichheit, kleiner, größer
- `null`, nicht `null`
- gleiche Objekte (`equals()`)

Diese Vergleiche werden mittels Assertions durchgeführt. Folgende Tabelle gibt einen Überblick über einige der am meisten verwendeten Assertions an. Alle Assertions, die es gibt, finden Sie [hier](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Assertions.html).

|Assertion |Beschreibung |
|----------|-------------|
|`fail(message)`|Lässt den Test scheitern (fail) mit Nachricht `message`. Wird genutzt, um zu überprüfen, ob Code unerreichbar ist oder bevor der Test implementiert ist. |
|`assertTrue(cond,m)`|Überprüft, ob Bedingung `cond` **wahr** ist oder scheitert mit Nachricht `m` |
|`assertFalse(cond,m)`|Überprüft, ob Bedingung `cond` **false** ist oder scheitert mit Nachricht `m`|
|`assertEquals(a,b,m)`|Überprüft, ob Parameter `a` und `b` **gleich** sind oder scheitert mit Nachricht `m`|
|`assertArrayEquals(a,b,m)` |Überprüft, ob Inhalte der Arrays `a` und `b` **gleich** sind oder scheitert mit Nachricht `m` |
|`assertNull(o,m)` |Überprüft, ob Object `o==null` ist oder scheitert mit Nachricht `m` |
|`assertNotNull(o,m)` |Überprüft, ob Object `o!=null` ist oder scheitert mit Nachricht `m` |
|`assertSame(o1,o2,m)` |Überprüft, ob Objektreferenz `o1==o2` ist oder scheitert mit Nachricht `m`|
|`assertNotSame(o1,o2,m)` |Überprüft, ob Objektreferenz `o1!=o2` ist oder scheitert mit Nachricht `m` |

Derzeit verwenden wir in unserer Testklasse nur die Assertion `fail()`. Diese steht aber nur dafür, dass wir diesen Test noch implementieren müssen. Das machen wir gleich.

### Annotationen

Neben den Assertions gibt es auch noch *Annotationen*, die beim Testen eine Rolle spielen. Eine Annotation haben wir bereits verwendet: `@Test`. Hier einen Überblick über die häufigsten Annotationen:

<table>
	<thead>
		<tr>
			<th>Annotation </th> 
			<th>Beschreibung </th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>
				@Test <br>
				public void method()
			</td>
			<td>
				Die Methode ist eine Testmethode
			</td>
		</tr>
				<tr>
			<td>
				@BeforeEach <br>
				public void method()
			</td>
			<td>
				Die Methode wird vor jedem Test ausgeführt
			</td>
		</tr>
		<tr>
			<td>
				@AfterEach <br>
				public void method()
			</td>
			<td>
				Die Methode wird nach jedem Test ausgeführt
			</td>
		</tr>
		<tr>
			<td>
				@BeforeAll <br>
				public static void method()
			</td>
			<td>
				Die Methode wird einmalig ausgeführt bevor die Tests starten (static!)
			</td>
		</tr>
		<tr>
			<td>
				@AfterAll <br>
				public static void method()
			</td>
			<td>
				Die Methode wird einmalig ausgeführt nachdem die Tests gelaufen sind
			</td>
		</tr>
	</tbody>
</table>

### Methode `fakultaet()` testen

Wir schreiben unsere erste Testmethode und wollen darin die Methode `fakultaet()` testen:

```java linenums="1"
import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.Test;

class FakultaetTest {

	@Test
	void testFakultaet6() 
	{
		// given
		Fakultaet f = new Fakultaet();
		
		// when
		long result = f.fakultaet(6);
		
		// then
		assertEquals(720, result, "6! should be 720");
	}

}
```

Wir haben dazu die Testmethode umbenant in `testFakultaet6()`, da wir für die Eingabe `6` die korrekte Ausführung testen wollen. Wir gehen nach dem `given-when-then`-Prinzip vor und haben es hier mithilfe von Zeilenkommentaren betont. Wir erzeugen zunächst ein `Fakultaet`-Objekt und wenden dann die `fakultaet()`-Methode für den Eingabeparameter `6` an. Das Ergebnis speichern wir in `result`. Nun kommt unsere erste *Assertion* zum Einsatz. Wir verwenden die *Assertion* `assertEquals`. Diese erwartet mindestens zwei Parameter. Der erste Parameter gibt das erwartete Resultat an (`720`) und der zweite Parameter ist der berechnete Wert (`result`). Der dritte Parameter ist optional und steht für die Zeichenkette, die ausgegeben wird, falls der berechnete Wert **nicht** dem erwarteteten Wert entspricht. 

Wir hätten hier auch die *Assertion* `assertTrue(720==result, "6! should be 720")` verwenden können. 

### Test ausführen

Unter `Run` wählen Sie nun `Coverage` (`Strg-Umschalt-F11`). Die Ansicht von Eclipse wechselt. Sie können unter `Run` auch `Coverage As --> JUnit Test` wählen. Das ist gleich. Auf der linken Seite in Eclipse erscheint ein grüner Balken als Zeichen dafür, dass alle Tests (derzeit nur einer) erfolgreich durchlaufen wurden. Darunter sind die Tests aufgeführt mit einem kleinen grünen Check am Icon. Der Test war erfolgreich. 

Wenn wir uns nun unsere `Fakultaet`-Klasse anschauen, dann sehen wir folgendes Bild:

![junit](./files/107_junit.png)

Hier wird die Testabdeckung angezeigt. Die grün markierten Zeilen wurden durch den Test ausgeführt. Die gelb markierten Zeilen sind Bedingungen, die `false` ergaben und für die es (noch) keinen Test gibt, der `true` für diese Bedingungen ergibt. Die roten Zeilen wurden in noch keinem Test ausgeführt. Wir erstellen deshalb weitere Tests, um eine vollständige Testabdeckung zu erzielen. 

### Exceptions testen

Um das Werfen einer Exception zu testen, bietet sich die *Assertion* `assertThrows()` an (siehe [hier](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/Assertions.html#assertThrows(java.lang.Class,org.junit.jupiter.api.function.Executable)). Diese *Assertion*  erwartet als ersten Parameter die erwartete `Exception`, in unserem Fall also `IllegalArgumentException`. Beachten Sie, dass der Typ `Class` (siehe [hier](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Class.html)) erwartet wird. Diesen erhalten wir, indem wir `.class` an den Klasennamen (also hier `IllegalArgumentException` anhängen). Der Unterschied zwischen `.class` und `.getClass()` ist der, dass bei `getClass()` bereits ein Objekt der Klasse existieren muss (wird auf ein Objekt der Klasse angewendet), während `.class` auf die Klasse angewendet werden kann, ohne dass ein Objekt der Klasse existieren muss. 

Der zweite Parameter der `assertThrows`-Methode erwartet ein `Executable` (siehe [hier](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/function/Executable.html)). Dies ist ein Interface mit genau einer Methode, nämlich `execute()`. Es ist üblich, dieses `Excutable` in der Schreibweise der funktionalen Programmierung (sie z.B. [hier](https://www.baeldung.com/java-functional-programming)) zu definieren. Die Testmethode, um Eingabeparameter kleiner `1` zu testen, könnte z.B. so aussehen:

```java linenums="22"
	@Test
	void testFakultaetSmallerThan1() 
	{
		// given
		Fakultaet f = new Fakultaet();
		
		// when
		Exception exception = assertThrows(IllegalArgumentException.class, () ->
        f.fakultaet(0));
		
		// then
		assertEquals("Zahl muss groesser gleich 1 sein!", exception.getMessage());
	}
```

Nach dem gleichen Prinzip können wir auch den Wertebereichsüberlauf testen:

```java linenums="37"
	@Test
	void testFakultaetOverflow() 
	{
		// given
		Fakultaet f = new Fakultaet();
		
		// when
		Exception exception = assertThrows(IllegalArgumentException.class, () ->
        f.fakultaet(22));
		
		// then
		assertEquals("Overflow!", exception.getMessage());
	}
```

Betrachten wir die Testabdeckung der Klasse `Fakultaet`, dann ist diese bereits sehr gut:

![junit](./files/108_junit.png)


### Konsolenausgaben testen

Es ist nicht üblich, Konsolenausgaben zu testen. Normalerweise werden die Ausgebestrings mit der `toString()`-Methode erzeugt und diese Methode wird dann auch getestet (z.B. mittels `assertEquals()`). Wir haben hier aber keine `toString()`-Methode und zeigen deshalb, wie die `print()`-Methode getestet werden könnte. Dazu wird die Standardausgabe "verbogen". Wir merken uns, den ursprünglichen `System.out`-Stream, der ein `PrintStream` aus dem Paket `java.io` ist. Dann erzeugen wir einen eigenen `ByteArrayOutputStream` (ebenfalls aus `java.io`). Nachdem wir unsere Ausgabe abgeschlossen haben, setzen wir `System.out` wieder auf das ursprüngliche Ausgabegerät, damit die Konsolenausgaben wieder erfolgen können. Unsere Testmethode für die `print()`-Methode könnte dann so aussehen:

```java linenums="71"
	@Test
	void testFakultaetPrint6() 
	{
		PrintStream originalOut = System.out;		// System.out merken (Standardausgabegeraet Konsole)
		ByteArrayOutputStream out = new ByteArrayOutputStream();
		System.setOut(new PrintStream(out));
		// given
		Fakultaet f = new Fakultaet();
		
		// when
		f.print(6);
		
		// then
		assertEquals("  6! = 720 \n", out.toString(), "should print 6! = 720 with linebreak");
		System.setOut(originalOut);					// System.out wieder auf das Standardausgabegeraet setzen
	}
```

Beachten Sie sowohl die Leerzeichen im Ausgabestring als auch den Zeilenumbruch. Nun haben wir eine vollständige Testabdeckung unserer `Fakultaet`-Klasse:

![junit](./files/109_junit.png)


!!! success
	Wir haben unsere erste Klasse mit `JUnit` getestet. Die wesentlichen Assertions, die verwendet werden, sind `assertEquals()`, `assertTrue()` und `assertThrows()`. Mit diesen Assertions können die allermeisten Methoden getestet werden. Beim testen von Arrays wird auch häufig `assertArrayEquals()` verwendet. Wählen Sie am besten eine beliebige Klasse aus den bisherigen Übungen oder Aufgaben aus, um selbständig dafür `JUnit`-Tests zu schreiben. 

## Test-Driven Development

Unit Tests können entweder nach Erstellung des Programmcodes geschrieben werden, um diesen nachträglich zu testen oder **vor** Erstellung des Programmcodes. Wenn wir die Tests vor der Erstellung des Programmcodes erstellen, dann beschreiben wir mit den Tests die Anforderungen an den zu erstellenden Code. Wir werden hier lernen, wie die Erstellung der Tests und des Programmcodes Hand-in_Hand erfolgen können. Diese Vorgehensweise nennt sich *Test-driven developement (TDD)*. 

Wir werden, wie üblich, TDD anhand eines Beispiels einführen. Unser Vorgehen lässt sich wie folgt beschreiben:

1. Wir schreiben einen Test, der die Anforderung für einen möglichst kleinen iterativen Schritt bei der Erstellung des Programmcodes beschreibt. 
2. Wir schreiben möglichst wenig Programmcode, so dass der Test genau erfüllt wird. 
3. Wir gehen wieder zu 1. und beschreiben den nächsten möglichst kleinen iterativen Schritt durch einen Test.
4. Wir wiederholen 2. für den neuen Test usw. 

Die folgende Abbildung visualisiert das Vorgehen. 

![testen](./files/35_testen.png)

Ausgangspunkt ist immer ein Test. Wir implementieren solange, bis dieser und alle vorher implementierten Tests erfolgreich durchlaufen werden. Dann schreiben wir einen weiteren Test und implementieren wieder so lange, bis dieser und alle vorher geschreibenen Tests erfolgreich durchlaufen werden. Dieses Vorgehen wird so lange wiederholt, so lange wir weitere Testfälle hinzufügen können, die jeweils neue Anforderungen beschreiben. Zur Implementierung des Codes gehört auch das Refactoring, d.h. wir verbessern bisher geschriebenen Code durch neue Tests. 

### TDD für einen Time-Zeit-Umrechner

Angenommen, wir wollen einen einfachen Konverter erstellen, der eine Uhrzeit im 12-Stunden-Zeitsystem (mit `am` und `pm`) in eine 24-Stundendarstellung umwandelt (von `String` nach `String`). Im Web findet man einige Beispiel, z.B. [hier](https://www.timeanddate.de/uhrzeit/am-pm-bedeutung). Dazu erstellen wir uns zunächst eine Klasse `UmrechnungTimeZeit.java`, die die Methode `convert()` enthält. Diese Methode erwartet einen `String` mit einer 12-Stunden-Zeit `time` und gibt einen `String` zurück, der die `time` im 24-Stundenformat darstellt.

=== "UmrechnungTimeZeit.java"
	```java
	public class UmrechnungTimeZeit {
		
		public String convert(String time)
		{
			return "";
		}

	}
	```

Das ist unsere Klasse in der Augangssituation. Bevor wir anfangen, zu implemntieren, wollen wir uns zunächst einen ersten einfachen Test schreiben, der uns eine erste Anforderung (einen ersten Testfall) beschreibt. Dazu wählen wir in Eclipse `File --> New --> JUnit Test Case`. Es erscheint folgendes Fenster: 

![testen](./files/29_testen.png)

Wir wählen `New JUnit Jupiter test` aus und benennen unsere Testklasse `TestUmrechnungTimeZeit` und geben an, dass die `Class under test` die Klasse `UmrechnungTimeZeit` ist (Auswahl durch `Browse...`). Wenn wir dann auf `Finish` klicken, erscheint:

![testen](./files/30_testen.png)

Das bestätigen wir mit `OK`. Es wird die `TestUmrechnungTimeZeit.java` erstellt, die so aussieht:

=== "TestUmrechnungTimeZeit.java"
	```java
	import static org.junit.jupiter.api.Assertions.*;
	import org.junit.jupiter.api.Test;

	class TestUmrechnungTimeZeit {

		@Test
		void test() {
			fail("Not yet implemented");
		}

	}
	```

Sollten Fehler beim Import der `junit.jupiter`-Pakete angezeigt werden, dann müssen Sie in Ihre `module-info.java` noch folgende Anweisung einfügen: 

=== "Einfügen in die module-info.java"
	```java
		requires org.junit.jupiter.api;
	```

Die Klasse `TestUmrechnungTimeZeit.java` ist eine JUnit-Testklasse. 

??? info "Video zur testgetriebenen Entwicklung"
	Die folgende testgetriebene Entwicklung wurde im folgenden Video gezeigt:
	<iframe src="https://mediathek.htw-berlin.de/media/embed?key=93a3784711126e3fa3712c112ea65251&width=720&height=450&autoplay=false&autolightsoff=false&loop=false&chapters=false&related=false&responsive=false&t=0" data-src="" class="iframeLoaded" width="720" height="450" frameborder="0" allowfullscreen="allowfullscreen" allowtransparency="true" scrolling="no"></iframe>

### Eine erste Testmethode für unser Beispiel

Zunächst einmal schauen wir uns an, wie wir unsere Testklasse ausführen. Dazu wählen wir in Eclipse unter `Run --> Run As ... --> JUnit Test`. Oder Sie wählen gleich den mittleren der gezeigten drei Buttons ![testen](./files/31_testen.png). Dann erhalten Sie folgendes Bild:

![testen](./files/32_testen.png)

Auf der linken Seite sehen Sie im `JUnit`-Reiter einen roten Querbalken. Dieser zeigt an, dass ein Test fehlgeschlagen ist. Im Editor-Fenster wird der fehlgeschlagene Test rot markiert. Es ist klar, dass der Test fehlschlägt, denn das bezweckt ja die Assertion `fail()`. Links unten sieht man den `Failure trace`. Dort ist in der ertsen Zeile die Fehlermeldung `Not yet implemented` zu sehen - das ist die Nachricht, die der `fail()`-Assertion übergeben wurde.

Wir implementieren nun die erste Testmethode. Dazu benennen wir die Methode `test()` um in `testConvert1amTo1()`. In unserem ersten Test wollen wir überprüfen, ob unsere Methode `convert(String time)` korrekt arbeitet, wenn ihr der `String` `1:00 am` übergeben wird. Die Idee ist nun die folgende: wir definieren unsere Testmethode so, dass wir angeben, welches Ergebnis wir erwarten, wenn der String `1:00 am` übergeben wird. Wir erwarten das Ergebnis `1:00`.

Generell sollte eine Testmethode in der folgenden Form aufgebaut sein:

- `given (preperation)`: gibt die Voraussetzungen des Tests an, z.B. die Erzeugung eines Objektes; bei uns: die Erzeugung eines `UmrechnungTimeZeit`-Objektes,
- `when (execution)`: beschreibt, was und wie ausgeführt werden soll; bei uns: die Ausführung der Methode `convert()` mit dem Parameterwert `"1:00 am"`,
- `then (verification)`: beschreibt, wie sich das Ergebnis der Ausführung in Bezug auf das erwartete Ergebnis verhalten soll; bei uns: `assertEquals(tatsaechlichesErgebnis, "1:00")`.

Nach Umbenennung der Methode `test()` in `TestUmrechnungTimeZeit.java` in `testConvert1amTo1(String time)` und Implementierung dieser Methode sieht die Klasse `TestUmrechnungTimeZeit.java` nun so aus:

=== "TestUmrechnungTimeZeit.java"
	```java linenums="1"
	import static org.junit.jupiter.api.Assertions.*;
	import org.junit.jupiter.api.Test;

	class TestUmrechnungTimeZeit {
		
		@Test
		void testConvert1amTo1() {
			// preperation --> given
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();
			
			// execution --> when
			String zeit = utz.convert("1:00 am");
					
			// verification --> then
			assertEquals(zeit, "1:00");				
		}

	}
	```

In der Testmethode erzeugen wir uns also zunächst ein Objekt von `UmrechnungTimeZeit`, damit wir die Objektmethode `convert()` überhaupt aufrufen können (Zeile `9`). Dann rufen wir die `convert()`-Methode auf und übergeben ihr den String `"1:00 am"`. Das Ergebnis der Methode wird in der Variablen `zeit` gespeichert (Zeile `12`). Dann vergleichen wir das tatsächliche Ergebnis (`zeit`) mit dem erwarteten Ergebnis (`"1:00"`) und wollen, dass beide gleich sind (`assertEquals()`).

Somit haben wir einen möglichst kleinen iterativen Schritt hin zur fertigen Implementierung als Test beschrieben. Unsere nächste Aufgabe ist nun, möglichst wenig Programmcode zu schreiben, so dass der Test genau erfüllt wird. Diese Aufgabe erledigen wir auf ganz simple Weise, indem unsere `convert()`-Methode einfach den String `"1:00"` zurückgibt. 

=== "UmrechnungTimeZeit.java"
	```java linenums="1" hl_lines="5"
	public class UmrechnungTimeZeit {
		
		public String convert(String time)
		{
			return "1:00";
		}

	}
	```

Das erscheint uns auf den ersten Blick völlig sinnlos - und das ist es natürlich irgendwie auch. Aber wir erinnern uns: wir wollen möglichst wenig Programmcode schreiben, so dass der Test genau erfüllt wird. Und das machen wir hier. Wenn wir nun unsere Testklasse ausführen, dann sehen wir:

![testen](./files/33_testen.png)

Auf der linken Seite ist nun ein grüner Balken zu sehen, d.h. alle unsere Tests (bis jetzt haben wir nur einen) sind korrekt. Unsere Methode `convert()` arbeitet in Bezug auf unsere Tests korrekt. 

Nun fügen wir einen zweiten Test hinzu. Dazu können wir die Methode `testConvert1amTo1()` einfach kopieren. Die neue Methode nennen wir `testConvert1amTo1()`, da wir nun testen wollen, ob unsere Methode auch den String `"2:00 am"` korrekt nach `"2:00"` umwandelt. Mit diesem Test beherzigen wir das Prinzip, den nächsten möglichst kleinen iterativen Schritt durch einen Test zu beschreiben. Dieser möglichst kleine Schritt ist für uns der Schritt von `"1:00 am"` nach `"2:00 am"`.

Die neue Testmethode sieht so aus:

=== "TestUmrechnungTimeZeit.java"
	```java linenums="1" hl_lines="18-28"
	import static org.junit.jupiter.api.Assertions.*;
	import org.junit.jupiter.api.Test;

	class TestUmrechnungTimeZeit {
		
		@Test
		void testConvert1amTo1() {
			// preperation --> given
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();
			
			// execution --> when
			String zeit = utz.convert("1:00 am");
					
			// verification --> then
			assertEquals(zeit, "1:00");				
		}
			
		@Test
		void testConvert2amTo2() {
			// preperation --> given
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();
			
			// execution --> when
			String zeit = utz.convert("2:00 am");
					
			// verification --> then
			assertEquals(zeit, "2:00");				
		}

	}
	```

Wenn wir die Testklasse nun ausführen, ohne die `convert()`-Methode zu ändern, erhalten wir folgendes Bild:

![testen](./files/34_testen.png)

Der Balken im `JUnit`-Fenster ist rot. Darunter wird uns angegeben, dass der Testfall `testConvert1amTo1()` immer noch korrekt ist (grüner Haken), aber der Testfall `testConvert2amTo2()` ist gescheitert (blaues Kreuz). Im Editorfenster ist rot unterlegt, welche Assertion gescheitert ist (`assertEquals(zeit, "2:00");`). 

Wir brauchen nun also eine Idee, wie wir die `convert()`-Methode anpassen können, so dass beide Testfälle korrekt ausgeführt werden. Wir versuchen es mit der Idee, einfach die ersten vier Zeichen des übergebenen Strings `time` zurückzugeben:

=== "UmrechnungTimeZeit.java"
	```java linenums="1" hl_lines="5"
	public class UmrechnungTimeZeit {
		
		public String convert(String time)
		{
			return time.substring(0,4);
		}

	}
	```

Wenn wir nun unsere Testklasse ausführen, dann sind beide Testfälle korrekt. Auf diese Art und Weise entwickeln wir nach und nach eine Implementierung der `convert()`-Methode. Dazu fügen wir nach und nach immer weitere Testfälle unserer Testklasse hinzu. Mindestens noch für folgende Fälle:

|`time` | erwartetes Ergebnis |
|-------|---------------------|
|`"1:15 am"` |`"1:15"` |
|`"11:00 am"` |`"11:00"` |
|`"11:15 am"` |`"11:15"` |
|`"1:00 pm"` |`"13:00"` |
|`"2:00 pm"` |`"14:00"` |
|`"1:15 pm"` |`"13:15"` |
|`"11:00 pm"` |`"23:00"` |
|`"11:15 pm"` |`"23:15"` |
|`"12:00 am"` |`"0:00"` |
|`"12:01 am"` |`"0:01"` |
|`"12:00 pm"` |`"12:00"` |
|`"12:01 pm"` |`"12:01"` |
|`"12:00 noon"` |`"12:00"` |
|`"12:00 midnight"` |`"0:00"` |

Das Auswahl der Testwerte ist ganz offensichtlich ein wichtiges Thema und bestimmt die Korrektheit der späteren Implementierung mit. Es ist wichtig, keinen Testfall zu vergessen. Leider gibt es dafür keine formalen Regeln, sondern nur intuitive Vorgaben. Es wird immer versucht, "Grenzwerte" zu ermitteln, um wirklich alle Testfälle abzudecken. 

### Quellcode aus dem Video

Im Video über JUnit wurde folgender Quellcode erzeugt:

=== "UmrechnungTimeZeit.java"
	```java linenums="1"
	package videos.video4;

	import static org.junit.jupiter.api.Assertions.assertEquals;

	import org.junit.jupiter.api.Test;

	public class UmrechnungTimeZeit {
		
		public String convert(String time)
		{
			final int LAST_THREE_CHARS = 3; // " pm" or " am"
			if(time.endsWith("am"))
			{
				return time.substring(0,(time.length()-LAST_THREE_CHARS));
			}
			else	// ends with pm
			{
				final int DIFFERENCE_BETWEEN_H_TO_HH = 12;
				int hourInt = this.getHoursInt(time);
				hourInt += DIFFERENCE_BETWEEN_H_TO_HH;
				String minutes = this.getMinutesStr(time);
				
				String zeit = hourInt + ":" + minutes;
				return zeit;
			}		
		}
		
		String getHoursStr(String time)
		{
			String[] allStr = time.split(":");
			return allStr[0];
		}
		
		String getMinutesStr(String time)
		{
			final int FIRST_TWO_CHARS = 2;
			String[] allStr = time.split(":");
			String afterDouble = allStr[1];
			String minutesStr = afterDouble.substring(0, FIRST_TWO_CHARS);
			return minutesStr;
		}
		
		int getHoursInt(String time)
		{
			String hoursStr = this.getHoursStr(time);
			int hoursInt = Integer.valueOf(hoursStr);
			return hoursInt;
		}

	}
	```

=== "UmrechnungTimeZeit.java"
	```java linenums="1"
	package videos.video4;

	import static org.junit.jupiter.api.Assertions.*;

	import org.junit.jupiter.api.Test;

	class UmrechnungTimeZeitTest {

		@Test
		void testConvert1amTo1() 
		{
			// given (preperation)
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();

			// when (execution)
			String zeit = utz.convert("1:00 am");

			// than (verification)
			assertEquals(zeit, "1:00", "1:00 am to 1:00 not working");
		}

		@Test
		void testConvert2amTo2() 
		{
			// given (preperation)
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();

			// when (execution)
			String zeit = utz.convert("2:00 am");

			// than (verification)
			assertEquals(zeit, "2:00", "2:00 am to 2:00 not working");
		}


		@Test
		void testConvert9amTo9() 
		{
			// given (preperation)
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();

			// when (execution)
			String zeit = utz.convert("9:00 am");

			// than (verification)
			assertEquals("9:00", zeit);
		}	

		@Test
		void testConvert10amTo10() 
		{
			// given (preperation)
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();

			// when (execution)
			String zeit = utz.convert("10:00 am");

			// than (verification)
			assertEquals("10:00", zeit);
		}

		@Test
		void testConvert1115amTo1115() 
		{
			// given (preperation)
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();

			// when (execution)
			String zeit = utz.convert("11:15 am");

			// than (verification)
			assertEquals("11:15", zeit);
		}

		@Test
		void testConvert1pmTo13() 
		{
			// given (preperation)
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();

			// when (execution)
			String zeit = utz.convert("1:00 pm");

			// than (verification)
			assertEquals("13:00", zeit);
		}


		@Test
		void testConvert3pmTo15() 
		{
			// given (preperation)
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();

			// when (execution)
			String zeit = utz.convert("3:00 pm");

			// than (verification)
			assertEquals("15:00", zeit);
		}
		

		@Test
		void testConvert545pmTo1745() 
		{
			// given (preperation)
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();

			// when (execution)
			String zeit = utz.convert("5:45 pm");

			// than (verification)
			assertEquals("17:45", zeit);
		}
		
		@Test
		void testConvert11pmTo23() 
		{
			// given (preperation)
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();

			// when (execution)
			String zeit = utz.convert("11:00 pm");

			// than (verification)
			assertEquals("23:00", zeit);
		}

		@Test
		void testGetHoursStr11pm()
		{
			// given (preperation)
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();

			// when (execution)
			String hour = utz.getHoursStr("11:00 pm");

			// than (verification)
			assertEquals("11", hour);
		}
		
		@Test
		void testGetHoursStr1pm()
		{
			// given (preperation)
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();

			// when (execution)
			String hour = utz.getHoursStr("1:00 pm");

			// than (verification)
			assertEquals("1", hour);
		}
		
		
		@Test
		void testGetHoursInt1pm()
		{
			// given (preperation)
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();

			// when (execution)
			int hours = utz.getHoursInt("1:00 pm");

			// than (verification)
			assertEquals(1, hours);
		}
		
		
		@Test
		void testGetHoursInt11pm()
		{
			// given (preperation)
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();

			// when (execution)
			int hours = utz.getHoursInt("11:00 pm");

			// than (verification)
			assertEquals(11, hours);
		}
		
		@Test
		void testGetMinutes1pm()
		{
			// given (preperation)
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();

			// when (execution)
			String minutes = utz.getMinutesStr("1:00 pm");

			// than (verification)
			assertEquals("00", minutes);
		}
			
		@Test
		void testGetMinutes11pm()
		{
			// given (preperation)
			UmrechnungTimeZeit utz = new UmrechnungTimeZeit();

			// when (execution)
			String minutes = utz.getMinutesStr("11:00 pm");

			// than (verification)
			assertEquals("00", minutes);
		}
	}
	```

=== "module-info.java"
	```java
	module SoSe2021 {
		requires java.desktop;
		requires org.junit.jupiter.api;
	}
	```

## Noch ein kleines Beispiel

Wir betrachten die Klasse `Power` mit 

```java linenums="1"
public class Power {
	private int base;
	private int exp;
	
	public Power(int base, int exp)
	{
		this.base = base;
		this.exp = exp;
	}
	
	public double value()
	{
		double value = 1.0;
		if(exp > 0)
		{
			for(int i=0; i<exp; i++)
			{
				value *= base;
			}
		}
		else
		{
			for(int i=0; i<-exp; i++)
			{
				value *= base;
			}
			value = 1.0 / value;
		}
		return value;
	}
	
	@Override
	public String toString()
	{
		return "(" + this.base + "^" + this.exp + ")";
	}
	
	public void print()
	{
		System.out.println(this.toString());
	}
	
	@Override
	public boolean equals(Object o)
	{
		if(o == null) return false;
		if(this == o) return true;
		if(this.getClass() != o.getClass()) return false;
		
		Power p = (Power)o;
		return (this.base==p.base && this.exp==p.exp);
	}
	
	@Override
	public int hashCode()
	{
		return 7*this.base + 11*this.exp; 
	}
}
```

Für diese Klasse erstellen wir eine Testklasse, die neben der Annotation `@Test` auch die Annotationen `@BeforeAll` und `@BeforeEach` exemplarisch verwendet. 

```java linenums="1"
import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

class PowerTest {
	static Power p1,p2,p3,p4;
	static int testnr = 1;

	@BeforeAll
	public static void setup()
	{
		p1 = new Power(2,3);
		p2 = new Power(2,3);
		p3 = new Power(-2,3);
		p4 = new Power(2,-3);
	}
	
	@BeforeEach
	public void printBeforeTests()
	{
		System.out.printf("%n %n --------------- Test %d ------------ %n", testnr);
		p1.print();
		p2.print();
		testnr++;
	}


	@Test
	void testToString() {
		String s = p1.toString();
		
		assertEquals("(2^3)", s, "Strings are not equal!");
	}

	@Test
	void testPower() {
		assertNotNull(p1, "no Power object");
	}

	@Test
	void testValue() {		
		double value = p1.value();
		
		assertEquals(value, 8.0, "2^3 should be 8.0");
	}
	
	@Test
	public void testEqualsObject() {
		assertTrue(p1.equals(p2), " 2^3 should be equal to 2^3!");
	}
}
```

Diese Testklasse deckt natürlich viel zu wenige Testfälle ab, aber es geht hier nur ums Prinzip. Führen Sie die Testklasse aus und beobachten Sie dabei auch die Konsole. Vor jeden Test (`@BeforeEach`) gibt es eine Ausgabe auf die Konsole. Bevor irgendein Test (`@Test`) ausgeführt wird (`@BeforeAll`) werden verschiedene Objekte der Klasse `Power` erzeugt. In den Testfällen werden aber nur `p1` und `p2` verwendet. Das müsste natürlich noch deutlich erweitert werden.

!!! success
	Wir haben JUnit-Testing kennengelernt. Unit-Tests sind eine gute Möglichkeit, einzelne Methoden automatisiert zu testen. Mithilfe von Unit-Tests können wir Code so entwicklen, dass alle formulierten Tests erfolgreich bestehen. Werden erst die Tests geschrieben und gegen die Tests implementiert, wird dieses Programmierverfahren Test-driven development genannt. Unit-Tests können aber auch verwendet werden, um existierenden Code zu testen. JUnit ist das Framework für Java-Unit-Tests. Ausführliche Informationen zu JUnit sind [hier](https://junit.org/junit5/) zu finden.   
