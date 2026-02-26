<!--
author:   Sebastian Zug
email:    your@email.de
version:  0.1.1
language: de
narrator: Deutsch Female
mode:     Textbook
comment:  Session 3: Vererbung und Klassenhierarchien – KommZuMint Schulversuch (BSZ Julius Weißbach / TU Bergakademie Freiberg)

import:   https://raw.githubusercontent.com/LiaScript/CodeRunner/master/README.md
          https://raw.githubusercontent.com/liascript-templates/plantUML/master/README.md
-->

# Session 3: Vererbung

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://raw.githubusercontent.com/LiaPlayground/Java_OOP_Course/refs/heads/main/Session_03_Vererbung.md)

> **Lernziele dieser Session:**
>
> - Das Konzept der Vererbung ("ist-ein"-Beziehung) verstehen
> - `extends` und `super` korrekt einsetzen
> - Methoden überschreiben mit `@Override`
> - Überladen von Methoden und Konstruktoren verstehen
> - Überschreiben vs. Überladen unterscheiden können
> - Vererbungshierarchien im UML-Diagramm darstellen

## Recap: Kapselung

**Was bedeutet `private` bei einem Attribut?**

- [( )] Es kann von überall gelesen werden
- [(x)] Es ist nur innerhalb der eigenen Klasse zugänglich
- [( )] Es kann nicht verändert werden
- [( )] Es ist nur im Konstruktor nutzbar

**Welche Methode erlaubt kontrollierten Lesezugriff auf ein privates Attribut?**

- [( )] Konstruktor
- [(x)] Getter
- [( )] Setter
- [( )] toString()

## Aufgabe: Drei Tierarten ohne Vererbung

Ergänzt die fehlenden Klassen `Katze` und `Vogel`. Beide sollen wie `Hund` die Attribute `name` und `alter` haben, einen Konstruktor, die Methode `vorstellen()` sowie jeweils eine eigene Methode:

- `Katze`: Methode `miauen()` → gibt `"<name> sagt: Miau!"` aus
- `Vogel`: Methode `fliegen()` → gibt `"<name> fliegt durch die Luft!"` aus

```java Main.java
class Hund {
    private String name;
    private int alter;

    public Hund(String name, int alter) {
        this.name = name;
        this.alter = alter;
    }

    public void vorstellen() {
        System.out.println("Ich bin " + name + ", " + alter + " Jahre alt.");
    }

    public void bellen() {
        System.out.println(name + " sagt: Wuff!");
    }
}

// TODO: Klasse Katze (gleiche Attribute, gleicher Konstruktor, gleiche vorstellen()-Methode, eigene miauen()-Methode)


// TODO: Klasse Vogel (gleiche Attribute, gleicher Konstruktor, gleiche vorstellen()-Methode, eigene fliegen()-Methode)


public class Main {
    public static void main(String[] args) {
        Hund h = new Hund("Bello", 5);
        // Katze k = new Katze("Minka", 3);
        // Vogel v = new Vogel("Tweety", 2);

        h.vorstellen();
        h.bellen();

        // k.vorstellen();
        // k.miauen();

        // v.vorstellen();
        // v.fliegen();
    }
}
```
@LIA.java(Main)

> **Habt ihr es gemerkt?** Wie oft habt ihr fast den gleichen Code geschrieben? Die Attribute `name` und `alter`, der Konstruktor und die Methode `vorstellen()` sind in **jeder** Klasse praktisch identisch. Ihr habt quasi dreimal Copy-Paste gemacht.

> Und jetzt wird es richtig nervig ... Stellt euch vor, der Kunde wünscht sich eine Änderung: Jedes Tier soll jetzt auch ein **Gewicht** haben. Die Methode `vorstellen()` soll außerdem das Gewicht mit ausgeben.

**Das ist das Problem!** Bei drei Klassen ist es noch machbar. Aber stellt euch vor, ihr habt **20 verschiedene Tierarten** – dann müsstet ihr die Änderung **20 Mal** an der gleichen Stelle machen. Dabei schleichen sich leicht Fehler ein, und wenn ihr eine Klasse vergesst, verhält sie sich anders als die anderen. Das nennt man **Code-Duplizierung** – und genau dafür gibt es eine Lösung: **Vererbung**.

## Lösung: Vererbung

Wir lagern die **gemeinsamen** Eigenschaften in eine **Oberklasse** (Elternklasse) aus. Die spezialisierten Klassen **erben** davon.

### UML-Diagramm

```text @plantUML
@startuml
class Tier {
    # name : String
    # alter : int
    + Tier(name: String, alter: int)
    + vorstellen() : void
}

class Hund {
    + Hund(name: String, alter: int)
    + bellen() : void
}

class Katze {
    + Katze(name: String, alter: int)
    + miauen() : void
}

class Vogel {
    - fluegelspannweite : double
    + Vogel(name: String, alter: int, fluegelspannweite: double)
    + fliegen() : void
}

Tier <|-- Hund
Tier <|-- Katze
Tier <|-- Vogel
@enduml
```

> **UML-Notation:** Der Pfeil mit **leerer Spitze** (△) zeigt von der Unterklasse zur Oberklasse und bedeutet "erbt von". Das `#` steht für `protected`.

### "ist-ein"-Beziehung

Vererbung modelliert eine **"ist-ein"-Beziehung**:

- Ein Hund **ist ein** Tier ✓
- Eine Katze **ist ein** Tier ✓
- Ein Vogel **ist ein** Tier ✓

**Welche Beziehung passt zu Vererbung?**

- [( )] Ein Auto **hat ein** Lenkrad
- [(x)] Ein PKW **ist ein** Fahrzeug
- [( )] Ein Schüler **benutzt** einen Stift
- [( )] Ein Buch **enthält** Seiten

## Vererbung in Java: `extends`

### Die Oberklasse Tier

```java Main.java
class Tier {
    protected String name;
    protected int alter;

    public Tier(String name, int alter) {
        this.name = name;
        this.alter = alter;
    }

    public void vorstellen() {
        System.out.println("Ich bin " + name + ", " + alter + " Jahre alt.");
    }
}

class Hund extends Tier {

    public Hund(String name, int alter) {
        super(name, alter);   // Konstruktor der Oberklasse aufrufen
    }

    public void bellen() {
        System.out.println(name + " sagt: Wuff!");
    }
}

class Katze extends Tier {

    public Katze(String name, int alter) {
        super(name, alter);
    }

    public void miauen() {
        System.out.println(name + " sagt: Miau!");
    }
}

public class Main {
    public static void main(String[] args) {
        Hund h = new Hund("Bello", 5);
        Katze k = new Katze("Minka", 3);

        h.vorstellen();   // Von Tier geerbt!
        h.bellen();       // Eigene Methode

        k.vorstellen();   // Von Tier geerbt!
        k.miauen();       // Eigene Methode
    }
}
```
@LIA.java(Main)

> **Erklärung:**
>
> - `extends Tier` – Hund erbt alle Attribute und Methoden von Tier
> - `super(name, alter)` – Ruft den Konstruktor der Oberklasse auf
> - `protected` – Das Attribut ist in der eigenen Klasse **und** in Unterklassen sichtbar

### Zugriffsmodifikator `protected`

| Modifikator | Eigene Klasse | Unterklasse | Andere Klassen |
|-------------|:---:|:---:|:---:|
| `private` | ✓ | ✗ | ✗ |
| `protected` | ✓ | ✓ | ✗ |
| `public` | ✓ | ✓ | ✓ |

### Was wird vererbt?

**Was erbt eine Unterklasse von der Oberklasse?**

- [[x]] Attribute (auch private, aber ohne direkten Zugriff)
- [[x]] Methoden (public und protected)
- [[x]] protected-Attribute mit direktem Zugriff

**Achtung:** Konstruktoren werden **nicht** vererbt! Jede Unterklasse muss ihren eigenen Konstruktor definieren und kann mit `super(...)` den Konstruktor der Oberklasse aufrufen.

## `super` – die Verbindung zur Oberklasse

Das Schlüsselwort `super` hat zwei Verwendungen:

**1. Konstruktor der Oberklasse aufrufen**

```java
public Hund(String name, int alter) {
    super(name, alter);  // MUSS die erste Zeile sein!
}
```

**2. Methode der Oberklasse aufrufen**

```java
public void vorstellen() {
    super.vorstellen();          // Methode der Oberklasse ausfuehren
    System.out.println("Wuff!"); // Eigenes Verhalten ergaenzen
}
```

## Methoden überschreiben: `@Override`

Eine Unterklasse kann geerbte Methoden **überschreiben**, um eigenes Verhalten zu definieren:

```java Main.java
class Tier {
    protected String name;
    protected int alter;

    public Tier(String name, int alter) {
        this.name = name;
        this.alter = alter;
    }

    public void gibLaut() {
        System.out.println(name + " macht ein Geraeusch.");
    }

    public void vorstellen() {
        System.out.println("Ich bin " + name + ", " + alter + " Jahre alt.");
    }
}

class Hund extends Tier {

    public Hund(String name, int alter) {
        super(name, alter);
    }

    @Override
    public void gibLaut() {
        System.out.println(name + " sagt: Wuff wuff!");
    }
}

class Katze extends Tier {

    public Katze(String name, int alter) {
        super(name, alter);
    }

    @Override
    public void gibLaut() {
        System.out.println(name + " sagt: Miau!");
    }
}

class Vogel extends Tier {
    private double fluegelspannweite;

    public Vogel(String name, int alter, double fluegelspannweite) {
        super(name, alter);
        this.fluegelspannweite = fluegelspannweite;
    }

    @Override
    public void gibLaut() {
        System.out.println(name + " sagt: Piep piep!");
    }

    @Override
    public void vorstellen() {
        super.vorstellen();   // Erst die Oberklassen-Version
        System.out.println("Fluegelspannweite: " + fluegelspannweite + " cm");
    }
}

public class Main {
    public static void main(String[] args) {
        Hund h = new Hund("Bello", 5);
        Katze k = new Katze("Minka", 3);
        Vogel v = new Vogel("Tweety", 2, 15.0);

        h.vorstellen();
        h.gibLaut();
        System.out.println();

        k.vorstellen();
        k.gibLaut();
        System.out.println();

        v.vorstellen();   // Ruft ueberschriebene Version auf!
        v.gibLaut();
    }
}
```
@LIA.java(Main)

> **`@Override`** ist eine **Annotation** – sie teilt dem Compiler mit, dass wir bewusst eine Methode der Oberklasse überschreiben. Wenn wir uns beim Methodennamen vertippen, gibt der Compiler eine Fehlermeldung. Das schützt vor Flüchtigkeitsfehlern.

## Aufgabe: Fußballspiel

Jetzt seid ihr dran! Modelliert die Akteure eines Fußballspiels mit Vererbung. Erstellt diese Aufgabe auf [OnlineGDB](https://www.onlinegdb.com/) (Sprache: **Java**).

### Aufgabenstellung

Auf dem Fußballplatz gibt es verschiedene Personen – aber alle haben einen Namen und ein Alter. Nutzt Vererbung, um die gemeinsamen Eigenschaften in einer Oberklasse zusammenzufassen.

```text @plantUML
@startuml
class Person {
    # name : String
    # alter : int
    + Person(name: String, alter: int)
    + vorstellen() : void
}

class Spieler {
    - trikotnummer : int
    - position : String
    + Spieler(name: String, alter: int, trikotnummer: int, position: String)
    + vorstellen() : void
}

class Trainer {
    - verein : String
    + Trainer(name: String, alter: int, verein: String)
    + vorstellen() : void
}

class Schiedsrichter {
    - anzahlSpiele : int
    + Schiedsrichter(name: String, alter: int, anzahlSpiele: int)
    + vorstellen() : void
}

Person <|-- Spieler
Person <|-- Trainer
Person <|-- Schiedsrichter
@enduml
```

1. Klasse `Person` mit `protected` Attributen `name` und `alter`, Konstruktor und Methode `vorstellen()` → gibt z.B. `"Max Mueller, 30 Jahre alt"` aus.
2. Klasse `Spieler extends Person` mit `trikotnummer` und `position`. Überschreibt `vorstellen()` mit `super.vorstellen()` und gibt zusätzlich `"  Trikot #9, Position: Stuermer"` aus.
3. Klasse `Trainer extends Person` mit `verein`. Überschreibt `vorstellen()` und gibt zusätzlich `"  Trainer bei <verein>"` aus.
4. Klasse `Schiedsrichter extends Person` mit `anzahlSpiele`. Überschreibt `vorstellen()` und gibt zusätzlich `"  Schiedsrichter (<n> Spiele geleitet)"` aus.
5. Erzeugt in `main` mindestens je ein Objekt und ruft `vorstellen()` auf.

<details>

<summary>**Tipp: Startercode**</summary>

```java
class Person {
    protected String name;
    protected int alter;

    public Person(String name, int alter) {
        // TODO
    }

    public void vorstellen() {
        // TODO: Name und Alter ausgeben
    }
}

class Spieler extends Person {
    private int trikotnummer;
    private String position;

    public Spieler(String name, int alter, int trikotnummer, String position) {
        // TODO: super(...) + eigene Attribute
    }

    @Override
    public void vorstellen() {
        // TODO: super.vorstellen() + Trikotnummer und Position
    }
}

// TODO: Klasse Trainer analog

// TODO: Klasse Schiedsrichter analog

public class Main {
    public static void main(String[] args) {
        // TODO: Objekte erzeugen und vorstellen
    }
}
```

</details>

<details>

<summary>**Musterloesung**</summary>

```java Main.java
class Person {
    protected String name;
    protected int alter;

    public Person(String name, int alter) {
        this.name = name;
        this.alter = alter;
    }

    public void vorstellen() {
        System.out.println(name + ", " + alter + " Jahre alt");
    }
}

class Spieler extends Person {
    private int trikotnummer;
    private String position;

    public Spieler(String name, int alter, int trikotnummer, String position) {
        super(name, alter);
        this.trikotnummer = trikotnummer;
        this.position = position;
    }

    @Override
    public void vorstellen() {
        super.vorstellen();
        System.out.println("  Trikot #" + trikotnummer + ", Position: " + position);
    }
}

class Trainer extends Person {
    private String verein;

    public Trainer(String name, int alter, String verein) {
        super(name, alter);
        this.verein = verein;
    }

    @Override
    public void vorstellen() {
        super.vorstellen();
        System.out.println("  Trainer bei " + verein);
    }
}

class Schiedsrichter extends Person {
    private int anzahlSpiele;

    public Schiedsrichter(String name, int alter, int anzahlSpiele) {
        super(name, alter);
        this.anzahlSpiele = anzahlSpiele;
    }

    @Override
    public void vorstellen() {
        super.vorstellen();
        System.out.println("  Schiedsrichter (" + anzahlSpiele + " Spiele geleitet)");
    }
}

public class Main {
    public static void main(String[] args) {
        Spieler s1 = new Spieler("Thomas Mueller", 35, 25, "Stuermer");
        Spieler s2 = new Spieler("Manuel Neuer", 38, 1, "Torwart");
        Trainer t = new Trainer("Vincent Kompany", 38, "FC Bayern");
        Schiedsrichter sr = new Schiedsrichter("Melanie Meyer", 43, 250);

        s1.vorstellen();
        System.out.println();
        s2.vorstellen();
        System.out.println();
        t.vorstellen();
        System.out.println();
        sr.vorstellen();
    }
}
```
@LIA.java(Main)

</details>

## Überladen: Gleicher Name, andere Parameter

Neben dem **Überschreiben** (Override) gibt es ein weiteres Konzept, das oft verwechselt wird: das **Überladen** (Overload). Beide haben denselben Methodennamen – aber die Idee dahinter ist eine ganz andere.

> **Überschreiben** = Eine Unterklasse ersetzt das Verhalten einer geerbten Methode (gleicher Name, **gleiche** Parameter).
>
> **Überladen** = In einer Klasse gibt es **mehrere Methoden mit demselben Namen**, aber **unterschiedlichen Parametern**.

### Beispiel: Mehrere Konstruktoren

Ein häufiger Anwendungsfall für Überladen sind **Konstruktoren**. Manchmal will man ein Objekt auf verschiedene Arten erzeugen können:

```java Main.java
class Tier {
    protected String name;
    protected int alter;

    // Konstruktor 1: mit Name und Alter
    public Tier(String name, int alter) {
        this.name = name;
        this.alter = alter;
    }

    // Konstruktor 2: nur mit Name (Alter unbekannt)
    public Tier(String name) {
        this.name = name;
        this.alter = 0;
    }

    public void vorstellen() {
        System.out.println("Ich bin " + name + ", " + alter + " Jahre alt.");
    }
}

public class Main {
    public static void main(String[] args) {
        Tier t1 = new Tier("Bello", 5);   // Konstruktor 1
        Tier t2 = new Tier("Minka");       // Konstruktor 2

        t1.vorstellen();
        t2.vorstellen();
    }
}
```
@LIA.java(Main)

> Java wählt automatisch den passenden Konstruktor anhand der **Anzahl und Typen der Argumente**. Das nennt man **Überladen** – der Name ist gleich, aber die **Signatur** (Parameterliste) unterscheidet sich.

### Beispiel: Überladene Methoden

Auch normale Methoden kann man überladen:

```java Main.java
class Rechner {

    public int addiere(int a, int b) {
        return a + b;
    }

    public int addiere(int a, int b, int c) {
        return a + b + c;
    }

    public double addiere(double a, double b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {
        Rechner r = new Rechner();

        System.out.println(r.addiere(3, 4));          // int, int -> 7
        System.out.println(r.addiere(1, 2, 3));       // int, int, int -> 6
        System.out.println(r.addiere(1.5, 2.5));      // double, double -> 4.0
    }
}
```
@LIA.java(Main)

> Drei Methoden mit dem Namen `addiere` – aber jede hat eine **andere Parameterliste**. Java erkennt anhand der Argumente, welche Version gemeint ist.

### Überschreiben vs. Überladen

<!-- data-type="none" -->
| | Überschreiben (Override) | Überladen (Overload) |
|---|---|---|
| **Wo?** | In der **Unterklasse** | In der **gleichen Klasse** |
| **Methodenname** | Gleich | Gleich |
| **Parameter** | Gleich | **Unterschiedlich** |
| **Rückgabetyp** | Gleich | Kann unterschiedlich sein |
| **Annotation** | `@Override` | – |
| **Zweck** | Verhalten **ersetzen/anpassen** | Methode für **verschiedene Eingaben** anbieten |

**Welches Konzept liegt hier vor?**

```java
class Tier {
    public void gibLaut() { ... }
}
class Hund extends Tier {
    @Override
    public void gibLaut() { ... }
}
```

- [(x)] Überschreiben (Override)
- [( )] Überladen (Overload)

**Und hier?**

```java
class Rechner {
    public int addiere(int a, int b) { ... }
    public int addiere(int a, int b, int c) { ... }
}
```

- [( )] Überschreiben (Override)
- [(x)] Überladen (Overload)

## Aufgabe: Getraenke-Bestellung

Erstelle diese Aufgabe auf [OnlineGDB](https://www.onlinegdb.com/) (Sprache: **Java**).

In dieser Aufgabe kommen **Überschreiben und Überladen** in einem Beispiel zusammen. Ihr modelliert ein Bestellsystem für Getraenke:

```text @plantUML
@startuml
class Getraenk {
    # name : String
    # preis : double
    + Getraenk(name: String, preis: double)
    + bestellen() : void
    + bestellen(anzahl: int) : void
}

class Heissgetraenk {
    - temperatur : int
    + Heissgetraenk(name: String, preis: double, temperatur: int)
    + bestellen() : void
}

Getraenk <|-- Heissgetraenk
@enduml
```

1. Klasse `Getraenk` mit `protected` Attributen `name` und `preis`, Konstruktor und **zwei** Methoden `bestellen()`:

   - `bestellen()` → gibt aus: `"1x Cola fuer 2.5 Euro"`
   - `bestellen(int anzahl)` → gibt aus: `"3x Cola fuer 7.5 Euro"` (**Überladen!**)

2. Klasse `Heissgetraenk extends Getraenk` mit zusätzlichem Attribut `temperatur`. **Überschreibt** `bestellen()` – ruft `super.bestellen()` auf und gibt zusätzlich die Temperatur aus.
3. Testet in `main` alle Varianten:

   - `cola.bestellen()` und `cola.bestellen(3)` – **Überladen** (verschiedene Parameter)
   - `kaffee.bestellen()` – **Überschreiben** (Heissgetraenk-Version statt Getraenk-Version)
   - `kaffee.bestellen(2)` – geerbte überladene Methode aus Getraenk

**Welches Konzept steckt hinter welchem Aufruf?**

- [[Überschreiben] [Überladen]]
- [( )            (x)         ] `cola.bestellen(3)` – anderer Parameter als `bestellen()`
- [(x)            ( )         ] `kaffee.bestellen()` – Heissgetraenk ersetzt Getraenk-Version
- [( )            (x)         ] `kaffee.bestellen(2)` – geerbte Methode mit int-Parameter

<details>

<summary>**Tipp: Startercode**</summary>

```java
class Getraenk {
    protected String name;
    protected double preis;

    public Getraenk(String name, double preis) {
        // TODO
    }

    public void bestellen() {
        // TODO: "1x <name> fuer <preis> Euro"
    }

    public void bestellen(int anzahl) {
        // TODO: "<anzahl>x <name> fuer <anzahl*preis> Euro"
    }
}

class Heissgetraenk extends Getraenk {
    private int temperatur;

    public Heissgetraenk(String name, double preis, int temperatur) {
        // TODO: super(...) + eigenes Attribut
    }

    @Override
    public void bestellen() {
        // TODO: super.bestellen() + Temperaturangabe
    }
}

public class Main {
    public static void main(String[] args) {
        // TODO: Getraenk und Heissgetraenk erzeugen, alle bestellen-Varianten testen
    }
}
```

</details>

<details>

<summary>**Musterloesung**</summary>

```java Main.java
class Getraenk {
    protected String name;
    protected double preis;

    public Getraenk(String name, double preis) {
        this.name = name;
        this.preis = preis;
    }

    public void bestellen() {
        System.out.println("1x " + name + " fuer " + preis + " Euro");
    }

    // Ueberladen: gleicher Name, anderer Parameter
    public void bestellen(int anzahl) {
        System.out.println(anzahl + "x " + name + " fuer " + (anzahl * preis) + " Euro");
    }
}

class Heissgetraenk extends Getraenk {
    private int temperatur;

    public Heissgetraenk(String name, double preis, int temperatur) {
        super(name, preis);
        this.temperatur = temperatur;
    }

    // Ueberschreiben: gleicher Name, gleiche Parameter wie in Oberklasse
    @Override
    public void bestellen() {
        super.bestellen();
        System.out.println("  (Serviertemperatur: " + temperatur + " Grad)");
    }
}

public class Main {
    public static void main(String[] args) {
        Getraenk cola = new Getraenk("Cola", 2.50);
        Heissgetraenk kaffee = new Heissgetraenk("Kaffee", 3.20, 65);

        System.out.println("--- Getraenk ---");
        cola.bestellen();        // bestellen()
        cola.bestellen(3);       // bestellen(int) -> Ueberladen

        System.out.println();
        System.out.println("--- Heissgetraenk ---");
        kaffee.bestellen();      // Override-Version mit Temperatur
        kaffee.bestellen(2);     // Geerbte ueberladene Methode
    }
}
```
@LIA.java(Main)

</details>

## Zusammenfassung

<!-- data-type="none" -->
| Konzept | Java-Syntax | Bedeutung |
|---------|-------------|-----------|
| Vererbung | `class Hund extends Tier` | Unterklasse erbt von Oberklasse |
| super (Konstruktor) | `super(name, alter)` | Konstruktor der Oberklasse aufrufen |
| super (Methode) | `super.vorstellen()` | Methode der Oberklasse aufrufen |
| protected | `protected String name` | Sichtbar in eigener Klasse + Unterklassen |
| Override | `@Override` | Geerbte Methode überschreiben |
| Überladen | `void m(int a)` / `void m(int a, int b)` | Gleicher Name, andere Parameter |
| "ist-ein" | Hund ist ein Tier | Semantik der Vererbung |

> **Vorschau Session 4:** Was passiert, wenn wir `Tier t = new Hund("Rex", 4)` schreiben? Die Variable ist vom Typ `Tier`, das Objekt aber ein `Hund`. Welche Version von `gibLaut()` wird aufgerufen? Das ist **Polymorphismus**!
