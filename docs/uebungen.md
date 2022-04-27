# Übungen


##### Übung 1 (Wiederholung und Codereview)

??? "Übung 1 (12. + 14.4.)"

	- Übung bringt Frau Busjahn mit in die Übung

##### Übung 2 (String und algorithmisches Denken)

??? "Übung 2 (19. + 21.4.)"

	1. Erstellen Sie im Paket `uebungen.uebung2` eine Java-Klasse `Uebung2` mit `main()`-Methode. In diese Klasse implementieren wir statische Methoden. Öffnen Sie zum Lösen der Übung am besten die JavaDoc-Dokumentation der Klasse [String](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html). Überlegen Sie sich, bevor Sie jeweils anfangen zu implementieren, genau, wie Sie vorgehen möchten.

	2. Implementieren Sie eine Methode `static boolean isBinaryNumber(String s)`. Diese Methode überprüft, ob der `String s` einer Binärzahl entspricht, d.h. ob er nur `0` und `1` enthält. 

	3. Testen Sie die Methode `isBinaryNumber(String s)` z.B. mit den folgenden Aufrufen:
	```java
	System.out.println(isBinaryNumber("101101"));	// true
	System.out.println(isBinaryNumber("0"));		// true
	System.out.println(isBinaryNumber("101a01"));	// false
	System.out.println(isBinaryNumber("101201"));	// false
	```

	4. Implementieren Sie eine Methode `static int binaryToDecimal(String s)`. Diese Methode wandelt den `String s` in eine Dezimalzahl um, wenn `s` einer Binärzahl entspricht. Wenn `s` keiner Binärzahl entspricht, wird `-1` zurückgegeben. 

	5. Testen Sie die Methode `binaryToDecimal(String s)` z.B. mit den folgenden Aufrufen:
	```java
	System.out.println(binaryToDecimal("101101"));	// 45
	System.out.println(binaryToDecimal("0"));		// 0
	System.out.println(binaryToDecimal("000001"));	// 1
	System.out.println(binaryToDecimal("100000"));	// 32
	System.out.println(binaryToDecimal("101a01"));	// -1
	System.out.println(binaryToDecimal("101201"));	// -1
	```

	6. Implementieren Sie eine Methode `static String toLowerCase(String input)`. Diese Methode wandelt alle Großbuchstaben ('A'...'Z') in Kleinbuchstaben um (und nur diese - alle anderen Zeichen bleiben erhalten). Schauen Sie sich dazu auch nochmal die [ASCII-Tabelle](https://freiheit.f4.htw-berlin.de/prog1/variablen/#char) an.

	7. Testen Sie die Methode `toLowerCase(String input)` z.B. mit den folgenden Aufrufen:
	```java
	System.out.println(toLowerCase("abcdEFG"));		// abcdefg
	System.out.println(toLowerCase("abcd123EFG"));	// abcd123efg
	System.out.println(toLowerCase("ABC XYZ !%"));	// abc xyz !%
	```

	**Zusatz:**

	8. Implementieren Sie eine Methode `static boolean isPalindrome(String input)`. Diese Methode prüft, ob es sich bei `input` um ein Palindrom handelt (also von vorne nach hinten genauso gelesen werden kann, wie von hinten nach vorne). Groß- und Kleinschreibung wird nicht berücksichtigt! Die Methode [substring(int,int)](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html#substring(int,int)) aus `String` ist dabei wahrscheinlich nützlich!

	9. Testen Sie die Methode `isPalindrome(String input)` z.B. mit den folgenden Aufrufen:
	```java
	System.out.println(isPalindrome("Otto"));		// true
	System.out.println(isPalindrome("abc_CBA"));	// true
	System.out.println(isPalindrome("abc_-CBA"));	// false
	System.out.println(isPalindrome("-"));			// true
	System.out.println(isPalindrome("Dreh mal am Herd"));	// false
	```

	8. Angenommen, Sie sollen für einen gegebenen `String` angeben, ob er korrekt geklammerte Ausdrücke enthält (nur die Klammern betrachten). Wie würden Sie vorgehen? Nicht implementieren, nur nachdenken. Folgende Beispiele:
	```
	((()))()(()) 		// korrekt
	((())				// nicht korrekt
	(()))				// nicht korrekt
	())(				// nicht korrekt
	```


##### Übung 3 (Exceptions)

??? "Übung 3 (26. + 28.4.)"

	1. Schreiben Sie ein Programm zur Eingabe von zwei Zahlen mithilfe der Klasse `JOptionPane` aus dem und deren Division! Fangen Sie folgende Ausnahmen ab:
		- Falls die Eingabe keiner Zahl entspricht.
		- Falls die zweite Zahl eine 0 ist.

	2. **Scenario**:
		- Fenster zur Eingabe von Zahl 1 öffnet sich: <br/>
			![uebung2](./files/22_uebung2.png)
		- falsche Eingabe - keine Zahl:  <br/>
			![uebung2](./files/23_uebung2.png)
		- Fenster öffnet sich erneut (andere Nachricht!):  <br/>
			![uebung2](./files/24_uebung2.png)
		- Fenster zur Eingabe von Zahl 2 öffnet sich:  <br/>
			![uebung2](./files/25_uebung2.png)
		- die Division Zahl1/Zahl2 schlägt fehl (`ArithmeticException`), deshalb (andere Nachricht!):  <br/>
			![uebung2](./files/26_uebung2.png)
		- Ergebnis  <br/>
			![uebung2](./files/27_uebung2.png)

	3. Lagern Sie eine solche Eingabemöglichkeit in eine wiederverwendbare Methode aus, z.B. `public int inputInt(int min, int max)`, welche die eingegebene Zahl zurückgibt, wobei die eingegebene Zahl im Bereich `[min, max]` liegen muss.

	4. Lesen Sie eine Zahl ein und geben Sie die Zahl umgedreht (rückwärts gelesen) wieder aus (führende Nullen entfallen):
		```bash
		3456789 --> 9876543
		```

		```bash
		1000 --> 1
		```

	5. Lesen Sie eine Zahl ein und geben Sie die Quersumme der Zahl aus.

		```bash
		123456 --> 21
		```

		```bash
		1000 --> 1		
		```

	**Viel Spaß!**

##### Übung 4 (enum und zweidimensionale Arrays)

??? "Übung 4 (3. + 5.5.)"

	1. Gegeben ist die folgende Klasse `TicTacToe`:

		```java linenums="1"
		package uebungen.uebung2;

		public class TicTacToe 
		{
			??? hier soll ein enum definiert werden ???
			State[][] field;

			public TicTacToe()
			{
				field = new ???welcher Typ???[3][3];
				for(int i=0; i<field.length; i++)
				  for(int j=0; j<field[i].length; j++)
				  	field[i][j]=State.EMPTY;
			}

			public void makeMove(int i, int j, ???welcher Typ??? player)
			{
				if(field[i][j]==???was kommt hier hin??? && player!=???was kommt hier hin???)   
					field[i][j]=player;
			}
		}
		```
	
	2. Fügen Sie die `??? ... ???` korrekt aus.
	2. Fügen Sie alle notwendigen Klammern `{ }` ein, so dass die Anweisungsblöcke korrekt geklammert sind. 
	3. Erweitern Sie die Klasse `TicTacToe` um eine `print()`-Methode, die das Spielfeld auf die Konsole ausgibt (Setzen Sie z.B. für den Player `RED` ein `x` und für den Player `Black` ein `o` und für `EMPTY` ein Leerzeichen oder ein `-`). Die Ausgabe nach jeweils 2 Zügen von `RED` und `BLACK` könnte dann z.B. so aussehen: 

		```bash
		- o o 
		- x - 
		- - x 
		```

	4. Erweitern Sie die Klasse `TicTacToe` um eine `gewonnen()`-Methode (`true`, wenn ein Spieler drei Felder horizontal, diagonal oder vertikal belegt hat; ansonsten `false`).
	5. Erweitern Sie die Klasse `TicTacToe` um eine `unentschieden()`-Methode (`true`, wenn alle Felder besetzt sind, aber kein Spieler gewonnen hat; ansonsten `false`).
	6. Erstellen Sie eine Test-Klasse mit `main()`-Methode. Erstellen sie darin ein Objekt der Klasse `TicTacToe`. Führen Sie Züge aus (`makeMove()`) und prüfen Sie, ob gewonnen wurde oder unentschieden ist (mit entsprechenden Ausgaben). 
	7. Für 6. müssen Sie in der Testklasse Ihr `enum State` importieren. Warum ist das so? Was könnten Sie machen, damit das nicht notwendig ist?

	8. *Zusatz:* Sie können die Klasse `TicTacToe` beliebig erweitern, z.B.:
		- um Ausgaben, wenn gewonnen bzw. es unentschieden ist,
		- um Fehler in den Indizes `i` und `j` bei der `makeMove()`-Methode abzufangen,
		- eine Methode `spielen()` implementieren, die zufällig für die Spieler die Steine setzt usw.

	**Viel Spaß!**

##### Übung 5 (Listen und Mengen)

??? "Übung 5 (10. + 12.5.)"

	1. Erstellen Sie eine Klasse `Uebung5` mit `main()`-Methode.
	2. Definieren Sie in der `main()`-Methode eine Variable `words` vom Typ `String[]` und weisen Sie dieser Variablen folgende Werte zu:
		```java
		String[] words = {"Linux", "Apple", "Facebook", "Amazon", "IBM", "Lenovo", "Google", "IBM", "Microsoft", "Apple", "Google", "Twitter", "Skype", "Chrome", "Linux", "Firefox"};
		```

	**A. Listen (`List`)**

	1. Erstellen Sie eine Methode `public static List<String> createArrayList(String[] words)`. In dieser Methode soll eine `ArrayList` erstellt werden. Alle Elemente in dieser Liste sind vom Typ `String`. Befüllen Sie diese Liste mit allen Wörtern aus dem `words`-Array. Die Methode gibt die befüllte Liste (`List`) zurück. 
	2. Erstellen Sie eine Methode `public static void printList(List<String> list)`. Diese Methode gibt alle Elemente der Liste `list` auf der Konsole aus. Geben Sie auch die Anzahl der Elemente der Liste aus. 
	3. Erstellen Sie in der `main()`-Methode mithilfe der Methode `createArrayList(words)` eine Liste und speichern Sie diese Liste in einer Variablen vom Typ `List<String>`. Geben Sie alle Elemente dieser Liste mithilfe der Methode `printList()` auf der Konsole aus. 
	4. Studieren Sie alle Methoden für `List` unter [https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html).

		- Ermitteln Sie den Index in der Liste, in der `"Apple"` das **erste** Mal auftaucht. Erzeugen Sie folgende Ausgabe: 
			```bash
			Index des ersten Auftretens von Apple  : 1
			```

		- Ermitteln Sie den Index in der Liste, in der `"Apple"` das **letzte** Mal auftaucht. Erzeugen Sie folgende Ausgabe: 
			```bash
			Index des letzten Auftretens von Apple : 9
			```

		- Geben Sie den Wert des **ersten** Elementes der Liste aus. Erzeugen Sie folgende Ausgabe: 
			```bash
			erstes Element der Liste : Linux
			```	

		- Geben Sie den Wert des **letzten** Elementes der Liste aus. Erzeugen Sie folgende Ausgabe: 
			```bash
			letztes Element der Liste : Firefox
			```	

		- Löschen Sie die Werte `"Apple"`, `"Google"` und `"Facebook"`. Geben Sie die Liste erneut mithilfe der `printList(list)`-Methode aus.

	**B. Mengen (`Set`)**

	1. Erstellen Sie eine Methode `public static Set<String> createHashSet(String[] words)`. In dieser Methode soll eine `HashSet` erstellt werden. Alle Elemente in dieser Liste sind vom Typ `String`. Befüllen Sie diese Liste mit allen Wörtern aus dem `words`-Array. Die Methode gibt die befüllte Menge (`Set`) zurück. 
	2. Erstellen Sie eine Methode `public static void printSet(Set<String> set)`. Diese Methode gibt alle Elemente der Menge `set` auf der Konsole aus. Geben Sie auch die Anzahl der Elemente der Menge aus. 
	3. Erstellen Sie in der `main()`-Methode mithilfe der Methode `createHashSet(words)` eine Menge und speichern Sie diese Menge in einer Variablen vom Typ `Set<String>`. Geben Sie alle Elemente dieser Menge mithilfe der Methode `printSet()` auf der Konsole aus. Was beobachten Sie in Bezug auf die Anzahl der Elemente im Vergleich zur Anzahl der Elemente in der Liste? Warum ist das so?
	4. Erstellen Sie eine Methode `public static Set<String> createTreeSet(String[] words)`. In dieser Methode soll eine `TreeSet` erstellt werden. Alle Elemente in dieser Liste sind vom Typ `String`. Befüllen Sie diese Menge (`Set`) mit allen Wörtern aus dem `words`-Array. Die Methode gibt die befüllte Menge (`Set`) zurück. 
	5. Erstellen Sie in der `main()`-Methode mithilfe der Methode `createTreeSet(words)` eine Menge und speichern Sie diese Menge in einer Variablen. Geben Sie alle Elemente dieser Menge mithilfe der Methode `printSet()` auf der Konsole aus. Was beobachten Sie in Bezug auf die Sortierung der Elemente im Vergleich zur `HashSet`?

	**Zusatz**

	1. Erstellen Sie für die Liste eine Methode `public static List<String> findDoublets(List<String> list)`. Diese Methode erstellt eine Liste, in der alle Elemente enthalten sind, die in `list` doppelt vorkommen. Diese Elemente werden dann auch doppelt in die Resultat-Liste übernommen. Geben Sie diese Liste mithilfe der `printList()`-Methode in der `main()`-Methode aus.

	??? "Mögliche Ausgabe (je nach Reihenfolge des Aufrufs der Methoden)"

		```bash
		Liste mit 16 Elementen :
		--------------------------
		Linux
		Apple
		Facebook
		Amazon
		IBM
		Lenovo
		Google
		IBM
		Microsoft
		Apple
		Google
		Twitter
		Skype
		Chrome
		Linux
		Firefox
		Index des ersten Auftretens von Apple  : 1
		Index des letzten Auftretens von Apple : 9
		erstes Element in der Liste  : Linux
		letztes Element in der Liste : Firefox

		Liste mit 13 Elementen :
		--------------------------
		Linux
		Amazon
		IBM
		Lenovo
		IBM
		Microsoft
		Apple
		Google
		Twitter
		Skype
		Chrome
		Linux
		Firefox

		Doublets-
		Liste mit 4 Elementen :
		--------------------------
		Linux
		IBM
		IBM
		Linux

		ohne Doublets-
		Liste mit 9 Elementen :
		--------------------------
		Amazon
		Lenovo
		Microsoft
		Apple
		Google
		Twitter
		Skype
		Chrome
		Firefox

		Menge mit 12 Elementen :
		--------------------------
		Lenovo
		Google
		Apple
		Skype
		Linux
		IBM
		Twitter
		Chrome
		Microsoft
		Amazon
		Facebook
		Firefox

		Menge mit 12 Elementen :
		--------------------------
		Amazon
		Apple
		Chrome
		Facebook
		Firefox
		Google
		IBM
		Lenovo
		Linux
		Microsoft
		Skype
		Twitter
		```


