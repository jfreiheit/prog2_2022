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

	4. Lesen Sie eine Anzahl von Sekunden ein und schreiben Sie eine Umrechnung, so dass folgende Ausgabe entsteht (die Eingabe ist hier über die Konsole gezeigt) :
		```bash
		Gib eine Anzahl von Sekunden ein: 3456789
		3456789 Sekunden sind 40 Tage, 13 Minuten, 9 Sekunden.
		```

		```bash
		Gib eine Anzahl von Sekunden ein: 2345678
		2345678 Sekunden sind 27 Tage, 3 Stunden, 34 Minuten, 38 Sekunden.
		```

		```bash
		Gib eine Anzahl von Sekunden ein: 123456
		123456 Sekunden sind 1 Tag, 10 Stunden, 17 Minuten, 36 Sekunden.
		```

		```bash
		Gib eine Anzahl von Sekunden ein: 12345
		12345 Sekunden sind 3 Stunden, 25 Minuten, 45 Sekunden.		
		```

	5. Lesen Sie eine Zahl ein und geben Sie die Quersumme der Zahl aus.


	**Viel Spaß!**

