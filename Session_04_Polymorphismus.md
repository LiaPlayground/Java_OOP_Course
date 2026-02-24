<!--
author:   Ihr Name
email:    your@email.de
version:  0.1.0
language: de
narrator: Deutsch Female
mode:     Textbook
comment:  Session 4: Polymorphismus und Zusammenfassung – KommZuMint Schulversuch (BSZ Julius Weißbach / TU Bergakademie Freiberg)

import:   https://raw.githubusercontent.com/LiaScript/CodeRunner/master/README.md
          https://raw.githubusercontent.com/liascript-templates/plantUML/master/README.md
-->

# Session 4: Polymorphismus und Zusammenfassung

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://raw.githubusercontent.com/LiaPlayground/Java_OOP_Course/refs/heads/main/Session_04_Polymorphismus.md)

> **Lernziele dieser Session:**
>
> - Polymorphismus verstehen: gleicher Aufruf, verschiedenes Verhalten
> - Referenzvariablen vom Typ der Oberklasse nutzen
> - Alle OOP-Konzepte im Zusammenhang anwenden

## Recap: Vererbung

**Was bedeutet `extends` in Java?**

- [( )] Eine Klasse wird gelöscht und neu erstellt
- [(x)] Eine Klasse erbt Attribute und Methoden einer Oberklasse
- [( )] Zwei Klassen werden zusammengeführt
- [( )] Eine Methode wird überschrieben

**Was macht `super(name, alter)` im Konstruktor einer Unterklasse?**

- [( )] Es erstellt ein neues Objekt der Oberklasse
- [(x)] Es ruft den Konstruktor der Oberklasse auf
- [( )] Es kopiert alle Methoden der Oberklasse
- [( )] Es gibt die Attribute auf der Konsole aus

## Einstieg: Ein überraschendes Experiment

Schaut euch diesen Code genau an und überlegt, **bevor** ihr ihn ausführt:

```java Main.java
class Tier {
    protected String name;

    public Tier(String name) {
        this.name = name;
    }

    public void gibLaut() {
        System.out.println(name + " macht ein Geraeusch.");
    }
}

class Hund extends Tier {
    public Hund(String name) {
        super(name);
    }

    @Override
    public void gibLaut() {
        System.out.println(name + " sagt: Wuff!");
    }
}

class Katze extends Tier {
    public Katze(String name) {
        super(name);
    }

    @Override
    public void gibLaut() {
        System.out.println(name + " sagt: Miau!");
    }
}

public class Main {
    public static void main(String[] args) {
        Tier tier1 = new Hund("Bello");    // Typ: Tier, Objekt: Hund!
        Tier tier2 = new Katze("Minka");   // Typ: Tier, Objekt: Katze!
        Tier tier3 = new Tier("Unbekannt");

        tier1.gibLaut();
        tier2.gibLaut();
        tier3.gibLaut();
    }
}
```
@LIA.java(Main)

**Was wird ausgegeben?**

- [( )] Alle drei geben "macht ein Geraeusch" aus
- [(x)] "Wuff", "Miau", "macht ein Geraeusch"
- [( )] Compilerfehler – ein Tier kann kein Hund sein
- [( )] "Wuff", "Wuff", "Wuff"
*********

Obwohl alle drei Variablen den Typ `Tier` haben, wird die **überschriebene** Methode des **tatsächlichen Objekttyps** aufgerufen. Das ist **Polymorphismus** – "Vielgestaltigkeit"!

*********

## Was ist Polymorphismus?

> **Polymorphismus** (griech. "Vielgestaltigkeit"): Objekte verschiedener Unterklassen können über eine gemeinsame Oberklassen-Referenz angesprochen werden. Die aufgerufene Methode richtet sich nach dem **tatsächlichen Typ des Objekts**, nicht nach dem Typ der Variable.

### Variablentyp vs. Objekttyp

```java
Tier meinTier = new Hund("Rex");
//│              └── Objekttyp: Hund (das tatsächliche Objekt)
//└── Variablentyp: Tier (die "Sicht" auf das Objekt)
```

- Die **Variable** `meinTier` ist vom Typ `Tier`
- Das **Objekt** im Speicher ist ein `Hund`
- Bei Methodenaufrufen zählt der **Objekttyp**

### Warum funktioniert das?

Weil jeder `Hund` ein `Tier` **ist** (Vererbung = "ist-ein"-Beziehung). Deshalb kann eine `Tier`-Variable auf ein `Hund`-Objekt zeigen.

## Polymorphismus in der Praxis: Arrays

Die wahre Stärke zeigt sich, wenn wir **verschiedene Objekte einheitlich behandeln**:

```java Main.java
class Tier {
    protected String name;

    public Tier(String name) {
        this.name = name;
    }

    public void gibLaut() {
        System.out.println(name + " macht ein Geraeusch.");
    }
}

class Hund extends Tier {
    public Hund(String name) { super(name); }

    @Override
    public void gibLaut() {
        System.out.println(name + " sagt: Wuff!");
    }
}

class Katze extends Tier {
    public Katze(String name) { super(name); }

    @Override
    public void gibLaut() {
        System.out.println(name + " sagt: Miau!");
    }
}

class Vogel extends Tier {
    public Vogel(String name) { super(name); }

    @Override
    public void gibLaut() {
        System.out.println(name + " sagt: Piep!");
    }
}

public class Main {
    public static void main(String[] args) {
        // Array vom Typ Tier - kann verschiedene Tierarten enthalten!
        Tier[] tiere = new Tier[4];
        tiere[0] = new Hund("Bello");
        tiere[1] = new Katze("Minka");
        tiere[2] = new Vogel("Tweety");
        tiere[3] = new Hund("Rex");

        // Alle Tiere durchgehen - jedes reagiert anders!
        System.out.println("=== Alle Tiere geben Laut ===");
        for (int i = 0; i < tiere.length; i++) {
            tiere[i].gibLaut();
        }
    }
}
```
@LIA.java(Main)

> **Das ist die Stärke von Polymorphismus:** Die Schleife ruft auf jedem Objekt `gibLaut()` auf, ohne zu wissen (oder wissen zu müssen), welches konkrete Tier es ist. Jedes Tier reagiert **automatisch richtig**.

## Erweiterbarkeit

Der große Vorteil: Wir können **neue Tierarten** hinzufügen, ohne den bestehenden Code ändern zu müssen!

```java Main.java
class Tier {
    protected String name;
    public Tier(String name) { this.name = name; }
    public void gibLaut() {
        System.out.println(name + " macht ein Geraeusch.");
    }
}

class Hund extends Tier {
    public Hund(String name) { super(name); }
    @Override public void gibLaut() {
        System.out.println(name + " sagt: Wuff!");
    }
}

class Katze extends Tier {
    public Katze(String name) { super(name); }
    @Override public void gibLaut() {
        System.out.println(name + " sagt: Miau!");
    }
}

// NEU: Einfach eine neue Klasse hinzufuegen!
class Frosch extends Tier {
    public Frosch(String name) { super(name); }
    @Override public void gibLaut() {
        System.out.println(name + " sagt: Quak!");
    }
}

public class Main {
    // Diese Methode funktioniert mit JEDEM Tier - auch mit Frosch!
    static void alleGibtLaut(Tier[] tiere) {
        for (int i = 0; i < tiere.length; i++) {
            tiere[i].gibLaut();
        }
    }

    public static void main(String[] args) {
        Tier[] tiere = {
            new Hund("Bello"),
            new Katze("Minka"),
            new Frosch("Kermit")    // Funktioniert sofort!
        };

        alleGibtLaut(tiere);
    }
}
```
@LIA.java(Main)

> Die Methode `alleGibtLaut()` wurde **nicht verändert** – trotzdem funktioniert sie mit der neuen Klasse `Frosch`. Das ist die Kraft der Kombination aus **Vererbung** und **Polymorphismus**.

**Was muss man tun, um eine neue Tierart (z.B. Pferd) hinzuzufügen?**

- [( )] Die Klasse Tier ändern
- [( )] Die main-Methode komplett umschreiben
- [(x)] Eine neue Klasse erstellen, die von Tier erbt und gibLaut() überschreibt
- [( )] Alle bestehenden Klassen anpassen

## Die 4 Säulen der OOP

Ihr habt jetzt alle vier Grundprinzipien der Objektorientierung kennengelernt:

```text @plantUML
@startuml
rectangle "**Abstraktion**\n(Session 1)" as A #LightBlue
rectangle "**Kapselung**\n(Session 2)" as K #LightGreen
rectangle "**Vererbung**\n(Session 3)" as V #LightYellow
rectangle "**Polymorphismus**\n(Session 4)" as P #LightCoral

A -[hidden]right-> K
K -[hidden]right-> V
V -[hidden]right-> P

note bottom of A
  Nur relevante
  Eigenschaften
  modellieren
end note

note bottom of K
  Daten schuetzen
  durch private +
  Getter/Setter
end note

note bottom of V
  Gemeinsame
  Eigenschaften
  in Oberklasse
end note

note bottom of P
  Gleicher Aufruf,
  verschiedenes
  Verhalten
end note
@enduml
```

| Prinzip | Kurzbeschreibung | Java-Beispiel |
|---------|-----------------|---------------|
| **Abstraktion** | Nur relevante Eigenschaften modellieren | Klasse `Hund` statt aller Einzelvariablen |
| **Kapselung** | Daten vor unkontrolliertem Zugriff schützen | `private` + Getter/Setter |
| **Vererbung** | Gemeinsame Eigenschaften in Oberklasse auslagern | `class Hund extends Tier` |
| **Polymorphismus** | Gleicher Aufruf, verschiedenes Verhalten | `Tier t = new Hund(); t.gibLaut();` |

**Ordne die Szenarien den OOP-Prinzipien zu:**

- [[Abstraktion]    [Kapselung]     [Vererbung]      [Polymorphismus]]
- [( )              (x)             ( )              ( )             ] Ein Attribut ist private und wird nur über Setter geändert
- [( )              ( )             (x)              ( )             ] Klasse PKW erbt von Klasse Fahrzeug
- [(x)              ( )             ( )              ( )             ] Wir modellieren nur Name und Alter eines Tieres, nicht die DNA
- [( )              ( )             ( )              (x)             ] Ein Array von Tier-Objekten, jedes reagiert anders auf gibLaut()

## Abschlussprojekt: Schul-Verwaltung

Erstelle diese Aufgabe auf [OnlineGDB](https://www.onlinegdb.com/) (Sprache: **Java**).

### Aufgabenstellung

Implementiere eine kleine Schulverwaltung, die alle OOP-Konzepte vereint:

```text @plantUML
@startuml
class Person {
    # name : String
    # alter : int
    + Person(name: String, alter: int)
    + getName() : String
    + getAlter() : int
    + toString() : String
}

class Schueler {
    - klasse : String
    - notendurchschnitt : double
    + Schueler(name: String, alter: int, klasse: String, notendurchschnitt: double)
    + getKlasse() : String
    + getNotendurchschnitt() : double
    + istVersetzt() : boolean
    + toString() : String
}

class Lehrer {
    - fach : String
    - kuerzel : String
    + Lehrer(name: String, alter: int, fach: String, kuerzel: String)
    + getFach() : String
    + getKuerzel() : String
    + toString() : String
}

Person <|-- Schueler
Person <|-- Lehrer
@enduml
```

1. **Klasse `Person`** (Oberklasse): Attribute `name` und `alter` (protected), Konstruktor, Getter, `toString()` gibt "Name (Alter Jahre)" aus.

2. **Klasse `Schueler extends Person`**: Zusätzliche private Attribute `klasse` und `notendurchschnitt`. Methode `istVersetzt()` gibt `true` zurück wenn Notendurchschnitt ≤ 4.0. `toString()` überschreiben: "Schüler: Name (Alter), Klasse X, Schnitt: Y".

3. **Klasse `Lehrer extends Person`**: Zusätzliche private Attribute `fach` und `kuerzel`. `toString()` überschreiben: "Lehrer: Name (Alter), Fach: X [Kürzel]".

4. **In main**: Erstelle ein Array `Person[]` mit mindestens 2 Schülern und 2 Lehrern. Gib alle Personen in einer Schleife mit `toString()` aus (**Polymorphismus**!).

<details>

<summary>**Tipp: Startercode**</summary>

```java
class Person {
    protected String name;
    protected int alter;

    public Person(String name, int alter) {
        // TODO
    }

    public String getName() { return name; }
    public int getAlter() { return alter; }

    public String toString() {
        // TODO: "Name (Alter Jahre)"
        return "";
    }
}

class Schueler extends Person {
    private String klasse;
    private double notendurchschnitt;

    public Schueler(String name, int alter, String klasse, double notendurchschnitt) {
        // TODO: super(...) + eigene Attribute
    }

    // TODO: Getter

    public boolean istVersetzt() {
        // TODO: true wenn Schnitt <= 4.0
        return false;
    }

    @Override
    public String toString() {
        // TODO
        return "";
    }
}

// TODO: Klasse Lehrer analog

public class Main {
    public static void main(String[] args) {
        // TODO: Array Person[] mit Schuelern und Lehrern
        // TODO: Schleife: alle ausgeben (Polymorphismus!)
    }
}
```

</details>

<details>

<summary>**Musterlösung**</summary>

```java Main.java
class Person {
    protected String name;
    protected int alter;

    public Person(String name, int alter) {
        this.name = name;
        this.alter = alter;
    }

    public String getName() { return name; }
    public int getAlter() { return alter; }

    public String toString() {
        return name + " (" + alter + " Jahre)";
    }
}

class Schueler extends Person {
    private String klasse;
    private double notendurchschnitt;

    public Schueler(String name, int alter, String klasse, double notendurchschnitt) {
        super(name, alter);
        this.klasse = klasse;
        this.notendurchschnitt = notendurchschnitt;
    }

    public String getKlasse() { return klasse; }
    public double getNotendurchschnitt() { return notendurchschnitt; }

    public boolean istVersetzt() {
        return notendurchschnitt <= 4.0;
    }

    @Override
    public String toString() {
        return "Schueler: " + name + " (" + alter + "), Klasse " + klasse
             + ", Schnitt: " + notendurchschnitt
             + (istVersetzt() ? " [versetzt]" : " [nicht versetzt]");
    }
}

class Lehrer extends Person {
    private String fach;
    private String kuerzel;

    public Lehrer(String name, int alter, String fach, String kuerzel) {
        super(name, alter);
        this.fach = fach;
        this.kuerzel = kuerzel;
    }

    public String getFach() { return fach; }
    public String getKuerzel() { return kuerzel; }

    @Override
    public String toString() {
        return "Lehrer: " + name + " (" + alter + "), Fach: " + fach
             + " [" + kuerzel + "]";
    }
}

public class Main {
    public static void main(String[] args) {
        Person[] schule = {
            new Schueler("Anna Mueller", 16, "11a", 2.3),
            new Schueler("Max Schmidt", 17, "11b", 4.5),
            new Lehrer("Frau Weber", 42, "Informatik", "WEB"),
            new Lehrer("Herr Fischer", 35, "Mathematik", "FIS"),
            new Schueler("Lisa Braun", 16, "11a", 1.7)
        };

        System.out.println("=== Schulverwaltung ===");
        System.out.println();

        // Polymorphismus: Jede Person wird passend ausgegeben!
        for (int i = 0; i < schule.length; i++) {
            System.out.println(schule[i].toString());
        }
    }
}
```
@LIA.java(Main)

</details>

## Gesamtzusammenfassung des Kurses

### Was ihr gelernt habt

| Session | Konzepte | Schlüsselwörter |
|---------|----------|-----------------|
| 1 | Klassen, Objekte, Konstruktoren | `class`, `new`, `this` |
| 2 | Kapselung, Getter/Setter, UML | `private`, `public`, `-`, `+` |
| 3 | Vererbung, Überschreiben | `extends`, `super`, `protected`, `@Override` |
| 4 | Polymorphismus | Oberklassen-Referenz, dynamische Bindung |

### Die Kernidee

```text
   Klasse (Bauplan)
       │
       ▼
   Objekt (Instanz)      ──►  Kapselung (Schutz)
       │
       ▼
   Vererbung (ist-ein)   ──►  Code-Wiederverwendung
       │
       ▼
   Polymorphismus         ──►  Flexibilität & Erweiterbarkeit
```

### Ausblick

Es gibt noch viele spannende OOP-Konzepte, die auf dem Gelernten aufbauen:

- **Abstrakte Klassen** – Klassen, von denen man keine Objekte erzeugen kann
- **Interfaces** – "Verträge", die Klassen erfüllen müssen
- **Collections** (z.B. `ArrayList`) – Flexible Listen statt fester Arrays
- **Design Patterns** – Bewährte Lösungsmuster für häufige Probleme

All das baut auf den vier Säulen auf, die ihr jetzt beherrscht!

## Abschluss-Quiz

**Was ist der Unterschied zwischen einer Klasse und einem Objekt?**

- [( )] Kein Unterschied – beides ist dasselbe
- [(x)] Eine Klasse ist der Bauplan, ein Objekt eine konkrete Instanz davon
- [( )] Ein Objekt definiert Methoden, eine Klasse nutzt sie
- [( )] Klassen existieren nur im UML-Diagramm

**Was passiert bei `Tier t = new Hund("Rex"); t.gibLaut();`?**

- [( )] Die Methode gibLaut() der Klasse Tier wird aufgerufen
- [(x)] Die Methode gibLaut() der Klasse Hund wird aufgerufen (Polymorphismus)
- [( )] Compilerfehler, weil Tier und Hund verschiedene Typen sind
- [( )] Beide Versionen von gibLaut() werden aufgerufen

**Welche Aussagen sind korrekt?**

- [[x]] Konstruktoren werden nicht vererbt
- [[x]] `private` Attribute sind in Unterklassen nicht direkt zugänglich
- [[ ]] Eine Unterklasse kann von mehreren Oberklassen erben (in Java)
- [[x]] `@Override` markiert eine überschriebene Methode
- [[x]] Polymorphismus ermöglicht es, verschiedene Objekte einheitlich zu behandeln
- [[ ]] `super` erzeugt ein neues Objekt der Oberklasse
