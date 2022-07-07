# Mausereignisse

Bei den Mausereignissen unterscheiden wir zwei Arten von Ereignissen:

- Ereignisse der Maus, die einmalig von der Maus ausgelöst werden. Dazu gehören die Ereignisse, wenn die linke Maustaste gedrückt wird, wenn die gedrückte Taste losgelassen wird, wenn sich die Maus auf eine Komponente bewegt, die sich an den Mauslistener angemeldet hat oder wenn der Mauszeiger diese Komponente wieder verlässt. 
- Ereignisse der Maus, die durch Bewegung der Maus permanent ausgelöst werden. Dazu gehört die Mausbewegung selbst oder auch die Bewegung der Maus bei gedrückter Taste.  

Die erste Art von Mausereignissen können wir behandeln, indem wir

- den `MouseListener` implementieren und
- eine Komponente (z.B. ein `JPanel`) an den `MouseListener` anmelden. 


Die zweite Art von Maus(-Bewegungs-)ereignissen können wir behandeln, indem wir

- den `MouseMotionListener` implementieren und
- eine Komponente (z.B. ein `JPanel`) an den `MouseMotionListener` anmelden. 

Wir betrachten dieses Semester aber nur den `MouseListener`. 

### Der `MouseListener`

Wir betrachten ein einfaches Beispiel mit `JPanels`, die in einem `size x size`-Gitter angeordnet sind. Diese `JPanels` haben alle eine zufällig erzeugte Hintergrundfarbe (siehe `randomColor()`. In dieser Klasse implementieren wir zunächst den `MouseListener`, d.h. wir fügen in den Klassenkopf `implements MouseListener` ein und lassen durch Eclipse die zu implementierenden Methoden einfügen:


```java linenums="1"
import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Font;
import java.awt.GridLayout;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.util.Random;

import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;

public class Mausereignisse extends JFrame implements MouseListener
{
	private int size;
	private JPanel[][] panels;
	private JPanel panel;
	
	public Mausereignisse(int size)
	{
		super();
		this.size = size;
        this.setTitle("Mausereignisse");
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        
        JPanel mainPanel = initContent(size);

        this.getContentPane().add(mainPanel, BorderLayout.CENTER);

        this.setSize(400,400);
        this.setVisible(true);
	}
	
	private Color randomColor() 
	{
		Random r = new Random();
		int red = r.nextInt(256);
		int blue = r.nextInt(256);
		int green = r.nextInt(256);
		
		Color c = new Color(red, blue, green);
		return c;
	}
	
	private JPanel initContent(int size)
    {
        this.panel = new JPanel();
        this.panel.setLayout(new GridLayout(size, size));
        this.panel.addMouseListener(this);
        
        this.panels = new JPanel[size][size];
        for (int row = 0; row < panels.length; row++) 
        {
            for (int col = 0; col < panels[row].length; col++) 
            {
    			this.panels[row][col] = new JPanel();
    			this.panels[row][col].setLayout(new BorderLayout());
    			JLabel label = new JLabel(row + " " + col);
    			label.setHorizontalAlignment(JLabel.CENTER);
    			label.setFont(new Font("Verdana", Font.BOLD, 24));
    			label.setForeground(Color.WHITE);
    			this.panels[row][col].add(label);
    			this.panels[row][col].setBackground(randomColor());
    			panel.add(this.panels[row][col]);
    		}
		}
        return this.panel;
    }
	
	public static void main(String[] args) 
	{
		new Mausereignisse(4);
	}

	@Override
	public void mouseClicked(MouseEvent e) {
		System.out.println("mouse clicked");	
	}

	@Override
	public void mousePressed(MouseEvent e) {
		System.out.println("mouse pressed");	
	}

	@Override
	public void mouseReleased(MouseEvent e) {
		System.out.println("mouse released");		
	}

	@Override
	public void mouseEntered(MouseEvent e) {
		System.out.println("mouse entered");
	}

	@Override
	public void mouseExited(MouseEvent e) {
		System.out.println("mouse exited");
		
	}
}
```

Wie wir sehen, sind es nun nicht mehr nur eine Methode, wie beim `ActionListener`, sondern gleich fünf Methoden, die wir implementieren können. Unsere zunächst einfachste Implementierung (aber zumindest am Anfang stets empfohlen!) ist die einfache Ausgabe auf die Konsole. Wir dürfen auf keinen Fall vergessen, uns an den `MouseListener` **anzumelden**! In diesem Beispiel melden wir das `panel`, das eine Objektvariable ist, an den `MouseListener` an (siehe Zeile `49`). 

Wir könnten auch das gesamte Fenster an den `MouseListener` anmelden. Dazu würden wir in den Konstruktor von `Mausereignisse` ein `this.addMouseListener(this);` einfügen. Damit würde `this` (also das Objekt von `Mausereignisse`, welches ja selbst ein `JFrame` ist) an den `MouseListener` angemeldet. Die Methode `addMouseListener()` erwartet ein Objekt der Implementierung von `MouseListener`. Das wäre ebenfalls `this`, da der `MouseListener` in der Klasse `Mausereignisse` implementiert ist. 

Weil jedoch später der Umgang mit den Mausereignissen etwas leichter ist, melden wir das `panel`-Objekt an den `MouseListener` an. Wir hätten auch jedes einzelne `JPanel` anmelden können. Das zeigen wir später.

### Die Methoden des `MouseListener`

Nun ist der `MouseListener` implementiert und das `panel`-Objekt ist an den `MouseListener` angemeldet. Wir können nun die einzelnen Methoden des `MouseListener` ausprobieren:

- `mousePressed()` wird aufgerufen, wenn die (linke) Maustaste heruntergedrückt wird. Drücken Sie die Maustaste und halten Sie sie gedrückt. In der Konsole erscheint `mousePressed`.
- `mouseReleased()` wird aufgerufen, wenn die (linke) Maustaste nach dem Drücken wieder losgelassen wird. Lassen Sie die Maustaste, nachdem auf der Konsole `mousePressed` erschienen ist, wieder los. In der Konsole erscheint `mouseReleased`.
- `mouseClicked()` wird aufgerufen, wenn erst `mousePressed()` und dann `mouseReleased()` aufgerufen wurde. `mouseClicked()` beschreibt also die Kombination aus Maustaste gedrückt und Maustaste losgelassen - einen Maus-Klick also.
- `mouseEntered()` wird aufgerufen, wenn Sie den Mauszeiger auf die Komponente bewegen, die an den `MouseListener` angemeldet ist. Wenn Sie in unserem Fall also die Maus in die Zeichenfläche `canvas` bewegen, dann wird die Methode `mouseEntered()` einmalig ausgeführt. 
- `mouseExited()` wird aufgerufen, wenn Sie den Mauszeiger von der Komponente wieder wegbewegen, die an den `MouseListener` angemeldet ist. Wenn Sie in unserem Fall also die Maus von der Zeichenfläche in die Titelleiste des Fensters oder ganz aus dem Fenster (der `ContentPane`) bewegen, dann wird die Methode `mouseExited()` einmalig ausgeführt. 


??? Übung
	Probieren Sie intensiv aus, wann welche Methoden aufgerufen werden! <br/>
	1. Schaffen Sie es, dass zwar `mouse pressed` und danach `mouse released` auf der Konsole erscheint, nicht aber `mouse clicked`? Wann ist das der Fall? <br/>
	2. Erzeugen Sie folgende Ausgabereihenfolge auf der Konsole: `mouse pressed`, `mouse exited`, `mouse released`, `mouse entered`!


### Objektmethoden der Klasse `MouseEvent`

All diesen Methoden wird ein `MouseEvent` als Parameter übergeben. Dieses Objekt besitzt einige nützliche Objektmethoden. Die wichtigsten dabei sind sicherlich die Auskünfte darüber, **wo** das Mausereignis ausgelöst wurde. Wir betrachten einige Objektmethoden des `MouseEvent`-Objekts am Beispiel der `mouseClicked(MouseEvent e)`-Methode. 

- die Methode `getX()` gibt den `x`-Wert der Koordinate zurück, bei dem das `MouseEvent` stattgefunden hat. Die Koordinate bezieht sich auf die Komponente, die an den `MouseListener` (bzw. `MouseMotionListener`) angemeldet ist. Das ist auch der Grund, warum wir nicht das gesamte Fenster an den `MouseListener`  (und `MouseMotionListener`) angemeldet haben. Der Punkt `[x=0, y=0]` befindet sich bei dem `Canvas`-Objekt in der linken oberen Ecke der `ContentPane`. Der Punkt mit den Koordinaten `[x=0, y=0]` beim Fenster ist der linke obere Punkt des Fensters, d.h. der linke obere Punkt der Titelleiste. 
- die Methode `getY()` gibt den `y`-Wert der Koordinate zurück, bei dem das `MouseEvent` stattgefunden hat (Koordinate der Komponente).
- die Methode `getPoint()` gibt ein `Point`-Objekt (von `java.awt`) zurück. Ein `Point`-Objekt besitzt die sichtbaren Objektvariablen `x` und `y` für die Koordinaten.

Wir probieren beide Möglichkeiten aus, die Koordinaten zu erfragen:

```java linenums="49"
	@Override
	public void mouseClicked(MouseEvent e) {
		Point p = e.getPoint();
		int x = e.getX();
		int y = e.getY();
		System.out.println("mouseClicked bei [x=" + p.x +" ,y=" + p.y + "]");	
		System.out.println("mouseClicked bei [x=" + x +" ,y=" + y + "]");
	}
```  

Beide Ausgaben zeigen (natürlich) die gleichen Koordinaten an, z.B. 

```bash
mouseClicked bei [x=388 ,y=215]
mouseClicked bei [x=388 ,y=215]
```

Beachten Sie, dass Sie in allen Methoden, in denen das `MouseEvent` übergeben wird, die Koordinaten des Ereignisses auslesen können. 

Weiterhin kann mithilfe des `MouseEvent`-Objektes überprüft werden, ob während des auslösenden Mausereignisses eine besondere Taste auf der Tastatur gedrückt wurde, z.B. die `Alt`-Taste (linke `option`-Taste beim Mac), die `AltGraph`-Taste (rechte `option`-Taste beim Mac), die `Ctrl`-Taste, oder die `Meta`-Taste (`Windows`-Taste bzw. `Apple`-Taste):


```java linenums="75" hl_lines="9-13"
	@Override
	public void mouseClicked(MouseEvent e) {
		Point p = e.getPoint();
		int x = e.getX();
		int y = e.getY();
		System.out.println("mouseClicked bei [x=" + p.x +" ,y=" + p.y + "]");	
		System.out.println("mouseClicked bei [x=" + x +" ,y=" + y + "]");

		if(e.isAltDown()) System.out.println("Alt-Taste gedrueckt");
		if(e.isAltGraphDown()) System.out.println("AltGraph-Taste gedrueckt");
		if(e.isControlDown()) System.out.println("Ctrl-Taste gedrueckt");
		if(e.isMetaDown()) System.out.println("Meta-Taste gedrueckt");
		if(e.isShiftDown()) System.out.println("Shift-Taste gedrueckt");
	}
```  

Probieren Sie die Maus-Klicks bei unterschiedlich gedrückter Taste auf der Tastatur aus!

Mithilfe der Objektmethode `getClickCount()` lässt sich die Anzahl die Klicks abfragen:



```java linenums="75" hl_lines="15"
	@Override
	public void mouseClicked(MouseEvent e) {
		Point p = e.getPoint();
		int x = e.getX();
		int y = e.getY();
		System.out.println("mouseClicked bei [x=" + p.x +" ,y=" + p.y + "]");	
		System.out.println("mouseClicked bei [x=" + x +" ,y=" + y + "]");

		if(e.isAltDown()) System.out.println("Alt-Taste gedrueckt");
		if(e.isAltGraphDown()) System.out.println("AltGraph-Taste gedrueckt");
		if(e.isControlDown()) System.out.println("Ctrl-Taste gedrueckt");
		if(e.isMetaDown()) System.out.println("Meta-Taste gedrueckt");
		if(e.isShiftDown()) System.out.println("Shift-Taste gedrueckt");

		if(e.getClickCount() == 2) System.out.println("Doppelklick");
	}
``` 

Nochmal, weil es wichtig ist: alle diese Objektmethoden stehen allen Methoden zur Verfügung, denen ein `MouseEvent` übergeben wird. Wir können sie also in allen Methoden aus `MouseListener` (und `MouseMotionListener`) anwenden. 

## Farben ändern mit Mausklick

Wir implementieren nun die `mouseClicked()`-Methode, um die Farbe des JPanels zu ändern, in das wir geklickt haben. Um das `JPanel` zu ermitteln, in das wir geklickt haben, benötigen wir folgende Daten:

- den `x`- und den `y`-Wert der Koordinate, in die wir die Maus innerhalb des `panel`-Objektes geklickt haben,
- die Breite und die Höhe jedes einzelnen JPanels,
- um die Breite und die Höhe jedes einzelnen JPanels zu berechnen, benötigen wir die Höhe und die Breite des `panel`-Objektes.


```java linenums="75" 
	@Override
	public void mouseClicked(MouseEvent e) {
		int x = e.getX();
		int y = e.getY();
		int width = this.panel.getWidth();
		int height = this.panel.getHeight();
		int panelWidth = width / this.size;
		int panelHeight = height / this.size;
		
		int row = y / panelHeight;
		int col = x / panelWidth;
		
		System.out.println("mouse clicked [x=" + x + ", y=" + y + "]");
		System.out.println("width : " + width + ", height : " + height);
		System.out.println("row : " + row + " , col : " + col);
		this.panels[row][col].setBackground(randomColor());
	}
``` 

- In den Zeilen `77` und `78` fragen wir die Koordinaten des Mausklicks ab.
- In den Zeilen `79` und `80` ermizteln wir die Breite und Höhe des `panel`-Objektes.
- In den Zeilen `81` und `82` berechnen wir die Breite und Höhe jedes einzelnen `JPanels`.
- In den Zeilen `84` und `85` berechnen wir mit den obigen Informationen den Zeilen- und Spalten-Index des angeklickten `JPanels` im `panels`-Array.
- Nach Ausgabe aller Informationen auf die Konsole (Zeilen `87-89`) wird für dieses angeklickte `JPanel` die Hintergrundfarbe neu gesetzt (Zeile `90`).

## Jedes JPanel einzeln anmelden

Wir haben den `MouseListener` an das `panel`-Objekt angemeldet, das alle `JPanels` enthält. Wir ändern dies nun und melden stattdessen jedes einzelne `JPanel` aus dem `panels`-Array an den `MouseListener` an. Dies erledigen wir mithilfe einer [anonymen Klasse](../ereignisse/#ereignisbehandlung-mit-anonymer-klasse), die den `MouseListener` implementiert. 


```java linenums="1"
import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Font;
import java.awt.GridLayout;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.util.Random;

import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;

public class Mausereignisse extends JFrame 
{
	private int size;
	private JPanel[][] panels;
	private JPanel panel;
	
	public Mausereignisse(int size)
	{
		super();
		this.size = size;
        this.setTitle("Mausereignisse");
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        
        JPanel mainPanel = initContent(size);

        this.getContentPane().add(mainPanel, BorderLayout.CENTER);

        this.setSize(400,400);
        this.setVisible(true);
	}
	
	private Color randomColor() 
	{
		Random r = new Random();
		int red = r.nextInt(256);
		int blue = r.nextInt(256);
		int green = r.nextInt(256);
		
		Color c = new Color(red, blue, green);
		return c;
	}
	
	private JPanel initContent(int size)
    {
        this.panel = new JPanel();
        this.panel.setLayout(new GridLayout(size, size));
        
        this.panels = new JPanel[size][size];
        for (int row = 0; row < panels.length; row++) 
        {
            for (int col = 0; col < panels[row].length; col++) 
            {
    			this.panels[row][col] = new JPanel();
    			this.panels[row][col].setLayout(new BorderLayout());
    			JLabel label = new JLabel(row + " " + col);
    			label.setHorizontalAlignment(JLabel.CENTER);
    			label.setFont(new Font("Verdana", Font.BOLD, 24));
    			label.setForeground(Color.WHITE);
    			this.panels[row][col].add(label);
    			this.panels[row][col].setBackground(randomColor());
    			this.panels[row][col].addMouseListener(new MouseListener() {

					@Override
					public void mouseClicked(MouseEvent e) {
						System.out.println("einzelnes JPanel clicked");
						Object src = e.getSource();
						if(src instanceof JPanel) {
							JPanel here = (JPanel)src;
							here.setBackground(Mausereignisse.this.randomColor()); 
						}
						
					}

					@Override
					public void mousePressed(MouseEvent e) {
						// TODO Auto-generated method stub
						
					}

					@Override
					public void mouseReleased(MouseEvent e) {
						// TODO Auto-generated method stub
						
					}

					@Override
					public void mouseEntered(MouseEvent e) {
						// TODO Auto-generated method stub
						
					}

					@Override
					public void mouseExited(MouseEvent e) {
						// TODO Auto-generated method stub
						
					}
    				
    			});
    			panel.add(this.panels[row][col]);
    		}
		}
        return this.panel;
    }
	
	public static void main(String[] args) 
	{
		new Mausereignisse(4);

	}
}

```

Beachten Sie, dass nun nicht mehr die `Mausereignisse`-Klasse den `MouseListener` implementiert, sondern die Implementierung innerhalb der anonymen Klasse erfolgt (siehe Zeilen `63-100`). Die Anmeldung an den `MouseListener` erfolgt nun für jedes einzelne `JPanel` aus dem `panels`-Array. Jedes dieser `JPanel` besitzt nun also sein eigenes `MouseListener`-Objekt. 


??? Übung
	Lassen Sie sich erneut die Koordinaten der Mausklicks auf die Konsole ausgeben.  


