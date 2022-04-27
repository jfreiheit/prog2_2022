# Aufzählungstypen (enum)

## Motivation

Angenommen, Sie wollen mithilfe einer Variablen eine festgelegte Menge an Zuständen beschreiben, z.B.

```java
String tag = "MONTAG" 	// kann auch Werte "Dienstag" usw. annehmen
int tag = 0; 			// Magic Number für "Montag"
```

Das Problem: 
- die Variablen können auch beliebige andere Werte (aus dem jeweiligen Wertebereich) annehmen, z.B. `Tag = "hallo"` oder `Tag=4711`,
- Magic Numbers sollen vermieden werden --> meistens schlechte Lesbarkeit

=== "Beispiel TicTacToe"
	```java
	public class TicTacToe 
	{
		int[][] field;

		TicTacToe()
		{
			field = new int[3][3];
			for(int i=0; i<field.length; i++)
				for(int j=0; j<field[i].length; j++)
					field[i][j]=0;
		}

		void makeMove(int i, int j, int player)
		{
			if(field[i][j]==0 && player==1 || player==2) 	
				field[i][j]=player;
		}
	}
	```

- Zustände `EMPTY` (`0`), `RED` (`1`), `BLACK` (`2`) verschlüsselt --> magic numbers
- `field[i][j]` könnte auch beliebige andere `int`-Werte annehmen
- Code nahezu unlesbar

### Erster Verbesserungsversuch: Konstanten

=== "Beispiel TicTacToe mit Konstanten"
	```java
	public class TicTacToe 
	{
		int[][] field;
		static final int EMPTY = 0;		// Feld ist leer
		static final int RED = 1;		// auf das Feld hat rot gesetzt
		static final int BLACK = 2;		// auf das Feld hat schwarz gesetzt

		TicTacToe()
		{
			field = new int[3][3];
			for(int i=0; i<field.length; i++)
				for(int j=0; j<field[i].length; j++)
					field[i][j]=EMPTY;
		}

		void makeMove(int i, int j, int player)
		{
			if(field[i][j]==EMPTY && player==RED || player==BLACK) 	
				field[i][j]=player;		// hier wird auf das Feld rot oder schwarz gesetzt
		}
	}
	```

- etwas besser, aber immer noch beliebige Werte für `field[i][j]` möglich

## Der Aufzählungstyp `enum`

Anforderungen:

- eigener Datentyp
- endliche Anzahl an Zuständen bzw. Werten
- leserliche Bezeichnung der Werte

Lösung:
- *Enumerations* (sog. Aufzählungstypen)
- Schlüsselwort `enum`


Syntax:
```java
	enum TypName {WERT1, WERT2, WERT3};
```


Verwendung:
- `TypName` nun als Datentyp verwendbar, z.B. `TypName[][]`
- Zugriff auf Werte über statische Punktschreibweise, z.B. `TypName.WERT1`


=== "Beispiel TicTacToe mit enum"
	```java linenums="1" hl_lines="3 4 8 11 17"
	public class TicTacToe 
	{
		enum State {EMPTY, RED, BLACK};
		State[][] field;

		TicTacToe()
		{
			field = new State[3][3];
			for(int i=0; i<field.length; i++)
				for(int j=0; j<field[i].length; j++)
					field[i][j]=State.EMPTY;
		}

		void makeMove(int i, int j, State player)
		{
			if(field[i][j]==State.EMPTY && player!=State.EMPTY) 	
				field[i][j]=player;
		}
	}
	```

- typsicher
- rot und schwarz über `State.RED` und `State.BLACK` erreichbar
- andere Zustände nicht möglich

#### Details:

- alle `enum` erben implizit von `java.lang.Enum`
- `enum` sind Referenztypen
- die Konstanten (Werte) in `enum` sind automatisch `static` und `final`
- `==` kann verwendet werden (auch `switch()`); `equals()` gibt es aber auch


=== "Beispiel enum"
	```java linenums="1"
	State s = State.EMPTY; 	// s = 0 oder s = "rot" oder so geht nicht 
							//-> typsicher
	switch(s)
	{
		case EMPTY: 	System.out.println("leeres Feld"); break;
		case RED: 	System.out.println("roter Stein"); break;
		case BLACK: 	System.out.println("schwarzer Stein Feld"); break;
	}
	```

#### Weiteres: 

- auch Definition von Methoden möglich
- `toString()`, `equals()` usw. aus `Object` können überschrieben werden
- Konstanten können mit Attributen versehen werden (dann noch privater Konstruktor notwendig)
- Zugriff auf das Array von Konstanten mithilfe von `values()`

=== "Beispiel für Werte mit Attributen"
	```java
	enum Farben {
		KREUZ(12), PIK(11), HERZ(10), KARO(9);
		private int farbwert;
		
		private Farben(int wert)
		{
			this.farbwert=wert;
		}	
		
		@Override
		public String toString()
		{
			char c = ' ';
			switch(this)
			{
				case KREUZ 	: c ='\u2663'; 	break;
				case PIK 	: c ='\u2664'; 	break;
				case HERZ 	: c ='\u2665'; 	break;
				case KARO 	: c ='\u2666'; 	break;
			}
			return String.valueOf(c);
		}
	}
	```

=== "noch ein Beispiel für Werte mit Attributen"
	```java
	enum Karten {
		AS(11), ZEHN(10), NEUN(0), ACHT(0), SIEBEN(0), K(4), D(3), B(2);
		private int kartenwert;
		
		private Karten(int wert)
		{
			this.kartenwert=wert;
		}
		
		@Override
		public String toString()
		{
			String s = "";
			switch(this)
			{
				case AS 	: s ="A"; 	break;
				case ZEHN 	: s ="10"; 	break;
				case NEUN 	: s ="9"; 	break;
				case ACHT 	: s ="8"; 	break;
				case SIEBEN : s ="7"; 	break;
				case K 		: s ="K"; 	break;
				case D 		: s ="D"; 	break;
				case B 		: s ="B"; 	break;
			}
			return s;
		}
	}
	```


??? "Ausführliches Beispiel - Skat.java"
	```java linenums="1"
	package vorbereitungen.enums;

	import java.util.Arrays;
	import java.util.Random;

	public class Skat {
		Karte[] p1;
		Karte[] p2;
		Karte[] p3;
		Karte[] skat;
		
		enum Karten {
			AS(11), ZEHN(10), NEUN(0), ACHT(0), SIEBEN(0), K(4), D(3), B(2);
			private int kartenwert;
			
			private Karten(int wert)
			{
				this.kartenwert=wert;
			}
			
			@Override
			public String toString()
			{
				String s = "";
				switch(this)
				{
					case AS 	: s ="A"; 	break;
					case ZEHN 	: s ="10"; 	break;
					case NEUN 	: s ="9"; 	break;
					case ACHT 	: s ="8"; 	break;
					case SIEBEN : s ="7"; 	break;
					case K 		: s ="K"; 	break;
					case D 		: s ="D"; 	break;
					case B 		: s ="B"; 	break;
				}
				return s;
			}
		}
		
		enum Farben {
			KREUZ(12), PIK(11), HERZ(10), KARO(9);
			private int farbwert;
			
			private Farben(int wert)
			{
				this.farbwert=wert;
			}	
			
			@Override
			public String toString()
			{
				char c = ' ';
				switch(this)
				{
					case KREUZ 	: c ='\u2663'; 	break;
					case PIK 	: c ='\u2664'; 	break;
					case HERZ 	: c ='\u2665'; 	break;
					case KARO 	: c ='\u2666'; 	break;
				}
				return String.valueOf(c);
			}
		}
		
		class Karte {
			Karten k;
			Farben f;
			
			Karte(Karten k, Farben f)
			{
				this.k=k;
				this.f=f;
			}
			
			@Override
			public Karte clone()
			{
				return new Karte(this.k,this.f);
			}
			
			@Override
			public String toString()
			{
				return f.toString()+k.toString()+" ";
			}
			
		}
		
		class Deck {
			Karte[] deck;
			
			Deck()
			{
				deck = new Karte[32];
				int index = 0;
				for(Farben f: Farben.values())
				{
					for(Karten k:Karten.values())
					{
						deck[index++] = new Karte(k,f);
					}
				}
			}
			
			@Override
			public String toString()
			{
				String s = "";
				for(int i=0; i<deck.length; i++)
				{
					s += deck[i].f.toString() + deck[i].k.toString() +" ";
					if(i==7 || i==15 || i==23 || i==31) s+="\n";
				}
				return s;
			}
			
			public void print()
			{
				System.out.println(this.toString());
			}

		}
		
		Skat()
		{
			p1 = new Karte[8];
			p2 = new Karte[8];
			p3 = new Karte[8];
			skat = new Karte[2];	
		}
		
		boolean existsFalse(boolean[] b)
		{
			for(int i=0; i<b.length; i++)
			{
				if(!b[i]) return true;
			}
			return false;
		}
		
		public void geben()
		{
			Deck d = new Deck();
			Random r = new Random();	
			boolean[] b = new boolean[32]; 
			Arrays.fill(b, false);
			int indexP1 = 0, indexP2 = 0, indexP3 =0, indexSkat = 0;
			int zz = r.nextInt(32);
			while(existsFalse(b))
			{
				while(b[zz])
				{
					zz = r.nextInt(32);
				}
				b[zz] = true;
				if(indexP1<8)
				{
					p1[indexP1++] = d.deck[zz].clone();
				}
				else if(indexP2<8)
				{
					p2[indexP2++] = d.deck[zz].clone();
				}
				else if(indexP3<8)
				{
					p3[indexP3++] = d.deck[zz].clone();
				}
				else if(indexSkat<2)
				{
					skat[indexSkat++] = d.deck[zz].clone();
				}
			}
		}
		
		public void sortieren()
		{
			
		}
		
		public void print()
		{
			System.out.print("Spieler 1 : ");
			for(Karte k : p1) System.out.print(k.toString()+" ");
			System.out.println();
			System.out.print("Spieler 2 : ");
			for(Karte k : p2) System.out.print(k.toString()+" ");
			System.out.println();
			System.out.print("Spieler 3 : ");
			for(Karte k : p3) System.out.print(k.toString()+" ");
			System.out.println();
			System.out.print("Skat      : ");
			for(Karte k : skat) System.out.print(k.toString()+" ");
			System.out.println();
		}

		public static void main(String[] args) {
			Skat s = new Skat();
			s.geben();
			s.print();

		}

	}
	```

### Nützliche Links für enums

- [Oracle Docs](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html)
- [W3Schools](https://www.w3schools.com/java/java_enums.asp)
- [Java enums - so geht's](https://www.heise.de/tipps-tricks/Java-enums-so-geht-s-4055338.html)
- [Enums](https://www.torsten-horn.de/techdocs/java-enums.htm)
- [Java Tutorial - Enums (youtube)](https://www.youtube.com/watch?v=NIUGbgLU5Uk)


