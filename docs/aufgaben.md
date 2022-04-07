# Aufgaben

##### Aufgabe 1 (Würfelspiel)

??? "Aufgabe 1"

	**Vorbereitung (siehe [Einstieg](../einstieg/#einstieg))**

	1. Informieren Sie sich über die Klasse `JOptionPane` aus dem Paket `javax.swing` (z.B. [hier](https://docs.oracle.com/javase/10/docs/api/javax/swing/JOptionPane.html) oder [hier](https://www.java-tutorial.org/joptionpane.html) oder [hier](https://de.wikibooks.org/wiki/Java_Standard:_Grafische_Oberfl%C3%A4chen_mit_Swing:_Top_Level_Container:_javax_swing_JOptionPane)) <br/>
	*Sollten Sie mit dem Java-Modulsystem arbeiten, d.h. sollten Sie in Ihrem Java-Projekt eine Datei `module-info.java` haben, dann müssen Sie in diese Datei (in den Anweisungsblock) die Anweisung `requires java.desktop;` einfügen - das ist das Modul, in dem sich das Paket `javax.swing` befindet.* 
	3. Erstellen Sie insbesondere folgenden Dialog (in den Buttons kann auch `Yes` und `No` stehen) und prüfen Sie, ob der `Nein`- oder der `Ja`-Button gedrückt wurde (im Beispiel steht `A` für den Namen eines Spielers – siehe Aufgabe unten):
		![aufgabe1](./files/19_aufgabe1.png)

	**Aufgabe**

	1. Implementieren Sie folgendes Würfelspiel:
		- An dem Spiel können beliebig viele Spieler teilnehmen.
		- Die Spieler sind nacheinander an der Reihe.
		- Wenn ein Spieler an der Reihe ist, dann befindet er sich in einem *Versuch*.
		- In einem *Versuch* kann der Spieler so lange würfeln, bis er entweder
			- eine 6 würfelt oder er
			- den Versuch freiwillig beendet.  
		- Hat der Spieler eine 6 gewürfelt, wird der gesamte *Versuch* mit `0` Punkten bewertet.
		- Hat der Spieler den Versuch freiwillig beendet, wird die in dem *Versuch* erzielte Summe aus sein Punktekonto addiert (gespeichert).
	2. Der Spieler, der zuerst eine bestimmte Punktzahl (z.B. `20`) erreicht hat, hat gewonnen. <br/>
		Beispiel mit zwei Spielern `A` und `B` bis Gesamtpunktzahl `20`:
		![aufgabe1](./files/20_aufgabe1.png)
	3. Laden Sie Ihr Lösung in Moodle hoch! Viel Spaß und viel Erfolg!


##### Aufgabe 2 (MyInteger)

??? "Aufgabe 2"

	**Vorbereitung (Selbstudium)**

	1. Eine *statische* Variable wird mit dem Schlüsselwort `static` deklariert, also z.B. `static int myVariable = 0;`. Der Zugriff auf eine solche statische Variable erfolgt nicht über eine Referenzvariable, sondern über den Klassennamen. Angenommen, die Variable `myVariable` wurde in der Klasse `MyClass` deklariert, dann erfolgt der Zugriff auf die Variable über `MyClass.myVariable`. Für Objektvariablen gilt, dass jedes Objekt seine eigene(n) Objektvariable(n) hat. Statische Variablen gibt es in der Klasse genau einmal. Alle Objekte "teilen" sich eine statische Variable. Eine statische Variable heißt deshalb auch *Klassenvariable*.
	2. Eine *statische* Methode wird ebenfalls mit dem Schlüsselwort `static` deklariert, also z.B. `public static void myMethod() {}`. Der Zugriff auf eine solche statische Methode erfolgt nicht über eine Referenzvariable, sondern über den Klassennamen. Angenommen, die Methode `myMethod()` wurde in der Klasse `MyClass` deklariert, dann erfolgt der Zugriff auf die Methode über `MyClass.myMethod()`. Wir kennen solche Methoden bereits, z.B. sind alle Methoden aus der Klasse `Math` statisch und wir können sie z.B. mit `Math.sqrt(value)` oder `Math.abs(value)` aufrufen. 
	3. siehe z.B. [hier](https://javabeginners.de/Grundlagen/Modifikatoren/static.php) oder [hier](https://www.programmierenlernenhq.de/statische-variablen-konstanten-und-methoden-in-java/) oder [hier](http://www.gailer-net.de/tutorials/java/Notes/chap25/ch25_12.html)

	**Info**: Die Klasse `MyInteger` ist eine sogenannte *Wrapper*-Klasse. Die Idee ist, dass `MyInteger` eine objektorientierte Hülle um den `int`-Typ darstellt. Implementieren Sie die Klasse `MyInteger`. Diese Klasse hat folgende Eigenschaften:

	1.	Statische Konstanten vom Typ `int` `MAX_VALUE` und `MIN_VALUE`, welche als Wert die größte bzw. kleinste `int`-Zahl enthalten.
	2.	Eine private Objektvariable `value` vom Typ `int`. (Dieser `value` hat jetzt eine "objektorientierte" Hülle: `MyInteger`).
	3.	Eine statische Methode `parseInt(String s)`, die den übergebenen `String s` als `int`-Zahl zurückgibt, wenn `s` einer Zahl entspricht. Wenn nicht, wirft die Methode eine `IllegalArgumentException`. Beachten Sie:
		a.	`s` kann mit `+` oder `–` beginnen,
		b.	`s` kann führende Nullen aufweisen,
		c.	die Länge von `s` kann mit `s.length()` ermittelt und jedes einzelne Zeichen aus `s` kann mit `s.charAt(index)` betrachtet werden. 
		d.	Ist `s` leer, wird eine `IllegalArgumentException` geworfen und wenn `s` keiner Zahl entspricht auch.
		e.	Die Exception wird nur weitergereicht, nicht hier behandelt.
	4.	Zwei Konstruktoren `MyInteger(int value)` und `MyInteger(String s)`, die jeweils die Objektvariable `value` initialisieren. Der zweite Konstruktor verwendet `parseInt(String)` und kann ebenfalls eine `IllegalArgumentException` werfen (reicht die Exception von `parseInt(String)` weiter). 
	5.	Eine Objektmethode `intValue()`, die den Wert von `value` zurückgibt.
	6.	Eine Objektmethode `doubleValue()`, die den Wert von `value` als `double` zurückgibt.
	7.	Eine statische Methode `valueOf(String s)`, die ein Objekt von `MyInteger` erzeugt und zurückgibt (und evtl. eine `IllegalArgumentException` wirft).
	8.	Eine statische Methode `valueOf(int value)`, die ein Objekt von `MyInteger` erzeugt und zurückgibt.
	9.	Überschreiben Sie außerdem die Methoden `equals()` und `toString()` (*Zusatz:*  auch `hashCode()` überschreiben).
	10. Testen Sie Ihre Klasse ausführlich in einer `Testklasse` mit `main()`-Methode.
	11. Laden Sie Ihr Lösung in Moodle hoch! Viel Spaß und viel Erfolg!




