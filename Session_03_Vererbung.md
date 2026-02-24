<!--
author:   Ihr Name
email:    your@email.de
version:  0.1.0
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

## Das Problem: Code-Duplizierung

Stellt euch vor, wir wollen verschiedene Tierarten modellieren:

```java
class Hund {
    private String name;
    private int alter;
    // Konstruktor, Getter, Setter ...

    public void vorstellen() {
        System.out.println("Ich bin " + name + ", " + alter + " Jahre alt.");
    }
}

class Katze {
    private String name;     // Gleich wie bei Hund!
    private int alter;       // Gleich wie bei Hund!
    // Konstruktor, Getter, Setter ... (gleich wie bei Hund!)

    public void vorstellen() {   // Gleich wie bei Hund!
        System.out.println("Ich bin " + name + ", " + alter + " Jahre alt.");
    }
}

class Vogel {
    private String name;     // Schon wieder dasselbe!
    private int alter;
    // ...
}
```

> **Problem:** Die Attribute `name` und `alter` sowie die Methode `vorstellen()` sind in **jeder** Klasse identisch. Das ist fehleranfällig und schwer wartbar.

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
- [[ ]] Konstruktoren
- [[x]] protected-Attribute mit direktem Zugriff
*********

**Achtung:** Konstruktoren werden **nicht** vererbt! Jede Unterklasse muss ihren eigenen Konstruktor definieren und kann mit `super(...)` den Konstruktor der Oberklasse aufrufen.

*********

## `super` – die Verbindung zur Oberklasse

Das Schlüsselwort `super` hat zwei Verwendungen:

### 1. Konstruktor der Oberklasse aufrufen

```java
public Hund(String name, int alter) {
    super(name, alter);  // MUSS die erste Zeile sein!
}
```

### 2. Methode der Oberklasse aufrufen

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

**Was gibt folgender Code aus?**

```java
Tier t = new Tier("Unbekannt", 1);
Hund h = new Hund("Rex", 4);
t.gibLaut();
h.gibLaut();
```

- [( )] Zweimal "macht ein Geraeusch"
- [( )] Zweimal "Wuff wuff"
- [(x)] "Unbekannt macht ein Geraeusch" und "Rex sagt: Wuff wuff!"
- [( )] Compilerfehler

## Übung: Fahrzeug-Hierarchie

Erstelle diese Aufgabe auf [OnlineGDB](https://www.onlinegdb.com/) (Sprache: **Java**).

### Aufgabenstellung

Implementiere folgende Klassenhierarchie:

```text @plantUML
@startuml
class Fahrzeug {
    # marke : String
    # baujahr : int
    + Fahrzeug(marke: String, baujahr: int)
    + anzeigen() : void
}

class PKW {
    - anzahlSitze : int
    + PKW(marke: String, baujahr: int, anzahlSitze: int)
    + anzeigen() : void
}

class LKW {
    - ladegewicht : double
    + LKW(marke: String, baujahr: int, ladegewicht: double)
    + anzeigen() : void
}

Fahrzeug <|-- PKW
Fahrzeug <|-- LKW
@enduml
```

1. Klasse `Fahrzeug` mit `protected` Attributen `marke` und `baujahr`, Konstruktor und Methode `anzeigen()`.
2. Klasse `PKW extends Fahrzeug` mit zusätzlichem Attribut `anzahlSitze`. Konstruktor mit `super(...)`. Override von `anzeigen()` – zeigt zusätzlich die Sitzanzahl.
3. Klasse `LKW extends Fahrzeug` mit zusätzlichem Attribut `ladegewicht`. Konstruktor mit `super(...)`. Override von `anzeigen()` – zeigt zusätzlich das Ladegewicht.
4. Erzeuge in `main` je ein PKW- und LKW-Objekt und rufe `anzeigen()` auf.

<details>

<summary>**Tipp: Startercode**</summary>

```java
class Fahrzeug {
    protected String marke;
    protected int baujahr;

    public Fahrzeug(String marke, int baujahr) {
        // TODO
    }

    public void anzeigen() {
        // TODO: marke und baujahr ausgeben
    }
}

class PKW extends Fahrzeug {
    private int anzahlSitze;

    public PKW(String marke, int baujahr, int anzahlSitze) {
        // TODO: super(...) und eigenes Attribut
    }

    @Override
    public void anzeigen() {
        // TODO: super.anzeigen() + anzahlSitze
    }
}

// TODO: Klasse LKW analog

public class Main {
    public static void main(String[] args) {
        // TODO: Objekte erzeugen und anzeigen
    }
}
```

</details>

<details>

<summary>**Musterlösung**</summary>

```java Main.java
class Fahrzeug {
    protected String marke;
    protected int baujahr;

    public Fahrzeug(String marke, int baujahr) {
        this.marke = marke;
        this.baujahr = baujahr;
    }

    public void anzeigen() {
        System.out.println(marke + " (Baujahr " + baujahr + ")");
    }
}

class PKW extends Fahrzeug {
    private int anzahlSitze;

    public PKW(String marke, int baujahr, int anzahlSitze) {
        super(marke, baujahr);
        this.anzahlSitze = anzahlSitze;
    }

    @Override
    public void anzeigen() {
        super.anzeigen();
        System.out.println("  Typ: PKW, Sitze: " + anzahlSitze);
    }
}

class LKW extends Fahrzeug {
    private double ladegewicht;

    public LKW(String marke, int baujahr, double ladegewicht) {
        super(marke, baujahr);
        this.ladegewicht = ladegewicht;
    }

    @Override
    public void anzeigen() {
        super.anzeigen();
        System.out.println("  Typ: LKW, Ladegewicht: " + ladegewicht + " t");
    }
}

public class Main {
    public static void main(String[] args) {
        PKW auto = new PKW("VW Golf", 2022, 5);
        LKW laster = new LKW("MAN TGX", 2021, 18.0);

        auto.anzeigen();
        System.out.println();
        laster.anzeigen();
    }
}
```
@LIA.java(Main)

</details>

## Zusammenfassung

| Konzept | Java-Syntax | Bedeutung |
|---------|-------------|-----------|
| Vererbung | `class Hund extends Tier` | Unterklasse erbt von Oberklasse |
| super (Konstruktor) | `super(name, alter)` | Konstruktor der Oberklasse aufrufen |
| super (Methode) | `super.vorstellen()` | Methode der Oberklasse aufrufen |
| protected | `protected String name` | Sichtbar in eigener Klasse + Unterklassen |
| Override | `@Override` | Geerbte Methode überschreiben |
| "ist-ein" | Hund ist ein Tier | Semantik der Vererbung |

> **Vorschau Session 4:** Was passiert, wenn wir `Tier t = new Hund("Rex", 4)` schreiben? Die Variable ist vom Typ `Tier`, das Objekt aber ein `Hund`. Welche Version von `gibLaut()` wird aufgerufen? Das ist **Polymorphismus**!
