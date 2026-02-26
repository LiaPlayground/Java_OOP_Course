<!--
author:   Sebastian Zug
email:    your@email.de
version:  0.1.0
language: de
narrator: Deutsch Female
mode:     Textbook
comment:  Session 2: Kapselung und Modularisierung – KommZuMint Schulversuch (BSZ Julius Weißbach / TU Bergakademie Freiberg)

import:   https://raw.githubusercontent.com/LiaScript/CodeRunner/master/README.md
          https://raw.githubusercontent.com/liascript-templates/plantUML/master/README.md
-->

# Session 2: Kapselung und Modularisierung

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://raw.githubusercontent.com/LiaPlayground/Java_OOP_Course/refs/heads/main/Session_02_Kapselung.md)

> **Lernziele dieser Session:**
>
> - Das Prinzip der Kapselung (Information Hiding) verstehen
> - Zugriffsmodifikatoren `private` und `public` korrekt einsetzen
> - Getter und Setter schreiben und deren Nutzen erkennen
> - UML-Klassendiagramme lesen und erstellen

## Recap: Klassen und Objekte

Kurze Wiederholung aus Session 1:

**Welche Zeile erzeugt ein neues Objekt?**

- [( )] `class Hund { }`
- [( )] `Hund h;`
- [(x)] `Hund h = new Hund("Bello", 5);`
- [( )] `h.bellen();`

**Was macht ein Konstruktor?**

- [( )] Er löscht ein Objekt aus dem Speicher
- [( )] Er definiert die Klasse
- [(x)] Er initialisiert ein neues Objekt mit Startwerten
- [( )] Er gibt Attribute auf der Konsole aus

## Das Problem: Unkontrollierter Zugriff

In Session 1 waren alle Attribute direkt zugänglich – jeder konnte sie von außen beliebig ändern. Das ist so, als hätte euer Smartphone **kein Sperrbildschirm**: Jeder könnte eure Nachrichten lesen, Apps löschen oder euer Profilbild ändern.

Schaut euch an, was passiert, wenn wir Attribute ohne Schutz lassen:

```java Main.java
class Hund {
    String name;
    int alter;

    Hund(String name, int alter) {
        this.name = name;
        this.alter = alter;
    }

    void vorstellen() {
        System.out.println(name + ", " + alter + " Jahre alt.");
    }
}

public class Main {
    public static void main(String[] args) {
        Hund bello = new Hund("Bello", 5);
        bello.vorstellen();

        // Das sollte nicht moeglich sein!
        bello.alter = -5;
        bello.name = "";
        bello.vorstellen();
    }
}
```
@LIA.java(Main)

> **Führt den Code aus!** Ein Hund mit Alter **-5** und **leerem** Namen? Das ergibt keinen Sinn! Weil die Attribute `name` und `alter` frei zugänglich sind, kann jeder beliebige (auch unsinnige) Werte reinschreiben. In einer echten Anwendung – z.B. einer Datenbank oder einem Onlineshop – wäre das fatal.

## Kapselung: private und public

Die Lösung heißt **Kapselung** (englisch: *Encapsulation*). Die Idee: Wir **verstecken** die Attribute vor dem direkten Zugriff und bieten stattdessen **kontrollierte Methoden** an, die prüfen, ob ein Wert gültig ist, bevor er gesetzt wird.

> **Analogie:** Denkt an einen Geldautomaten. Ihr könnt nicht einfach in den Tresor greifen und Geld nehmen. Stattdessen nutzt ihr die Schnittstelle (PIN eingeben, Betrag wählen) – und der Automat prüft intern, ob genug Geld da ist und ob eure PIN stimmt.

In Java bedeutet das konkret: Wir machen Attribute **`private`** und kontrollieren den Zugriff über **`public`-Methoden**.

Beispiele:

+ hat die Postleitzahl genau 5 Ziffern?
+ besteht eine Telefonnummer nur aus Zahlen und Leerzeichen?
+ ist das Alter eines Hundes zwischen 0 und 30 Jahren?

### Zugriffsmodifikatoren

Java kennt Schlüsselwörter, mit denen wir die **Sichtbarkeit** von Attributen und Methoden steuern. Für den Anfang brauchen wir nur zwei:

<!-- data-type="none" -->
| Modifikator | Bedeutung |
|-------------|-----------|
| `private` | Nur innerhalb der eigenen Klasse sichtbar |
| `public` | Von überall zugreifbar |

So sieht das Prinzip im UML-Diagramm aus: Die Attribute sind `private` (von außen unsichtbar), der Zugriff läuft über `public`-Methoden:

```text @plantUML
@startuml
class Hund {
    - name : String
    - alter : int
    + Hund(name: String, alter: int)
    + getName() : String
    + setName(name: String) : void
    + getAlter() : int
    - setAlter(alter: int) : void
    + vorstellen() : void
}
note right of Hund
  - = private
  + = public
end note
@enduml
```

> **UML-Notation:** `-` steht für `private`, `+` steht für `public`. In einem UML-Diagramm erkennt ihr also sofort, welche Teile einer Klasse von außen erreichbar sind und welche versteckt bleiben.

### Umsetzung in Java

Schauen wir uns an, wie die Klasse `Hund` mit Kapselung in Java aussieht. Achtet besonders auf die Schlüsselwörter `private` und `public` sowie auf die **Validierung** im Setter:

```java Main.java
class Hund {
    private String name;
    private int alter;

    public Hund(String name, int alter) {
        this.name = name;
        this.setAlter(alter);  // Validierung nutzen!
    }

    // Getter
    public String getName() {
        return name;
    }

    public int getAlter() {
        return alter;
    }

    // Setter mit Validierung
    public void setName(String name) {
        if (name != null && !name.isEmpty()) {
            this.name = name;
        } else {
            System.out.println("Fehler: Name darf nicht leer sein!");
        }
    }

    public void setAlter(int alter) {
        if (alter >= 0 && alter <= 30) {
            this.alter = alter;
        } else {
            System.out.println("Fehler: Alter muss zwischen 0 und 30 liegen!");
        }
    }

    public void vorstellen() {
        System.out.println(name + ", " + alter + " Jahre alt.");
    }
}

public class Main {
    public static void main(String[] args) {
        Hund bello = new Hund("Bello", 5);
        bello.vorstellen();

        bello.setAlter(-5);      // Wird abgelehnt!
        bello.setName("");       // Wird abgelehnt!
        bello.vorstellen();      // Bello ist immer noch korrekt

        bello.setAlter(6);       // Das funktioniert
        bello.vorstellen();
    }
}
```
@LIA.java(Main)

> **Führt den Code aus und schaut, was passiert!** Beachtet:
>
> - `bello.setAlter(-5)` wird **abgelehnt** – der Setter prüft, ob der Wert zwischen 0 und 30 liegt
> - `bello.setName("")` wird **abgelehnt** – der Setter prüft, ob der Name nicht leer ist
> - `bello.setAlter(6)` wird **akzeptiert** – der Wert ist gültig
>
> Der direkte Zugriff `bello.alter = -5` ist jetzt **nicht mehr möglich**, weil `alter` als `private` deklariert ist. Man **muss** den Setter nutzen – und genau dort findet die Prüfung statt.

## Getter und Setter: Wann und warum?

Getter und Setter folgen einer festen **Namenskonvention** in Java: `get` + Attributname für den Lesezugriff, `set` + Attributname für den Schreibzugriff. Der erste Buchstabe des Attributnamens wird dabei großgeschrieben.

**Getter** (Lesezugriff): Geben den Wert eines Attributs zurück, ohne ihn zu verändern.

```java
public int getAlter() {
    return alter;
}
```

**Setter** (Schreibzugriff): Setzen einen neuen Wert – mit optionaler **Validierung**. Das ist der entscheidende Vorteil gegenüber direktem Zugriff: Wir können **prüfen**, ob der neue Wert sinnvoll ist.

```java
public void setAlter(int alter) {
    if (alter >= 0) {
        this.alter = alter;
    }
}
```

> **Nicht jedes Attribut braucht einen Setter!** Wenn ein Attribut nach der Erzeugung nicht mehr geändert werden soll, bieten wir keinen Setter an. Beispiel: Ein `Benutzerkonto` hat vielleicht keinen Setter für den Benutzernamen – der soll nach der Registrierung nicht mehr änderbar sein. Ein Attribut **nur** mit Getter (ohne Setter) nennt man **read-only**.

## Private Methoden: Auch Logik kann man verstecken

Bisher haben wir Attribute als `private` geschützt. Aber auch **Methoden** können `private` sein! Das sind **Hilfsmethoden**, die nur intern gebraucht werden – von außen soll niemand sie aufrufen können.

Ein typischer Anwendungsfall: Wir haben eine Prüfung (z.B. „ist das Alter gültig?"), die wir an **mehreren Stellen** brauchen – im Konstruktor und im Setter. Statt den gleichen Code doppelt zu schreiben, lagern wir ihn in eine private Hilfsmethode aus:

```java Main.java
class Hund {
    private String name;
    private int alter;

    public Hund(String name, int alter) {
        this.name = name;
        if (istGueltigesAlter(alter)) {
            this.alter = alter;
        } else {
            this.alter = 0;
            System.out.println("Ungueltiges Alter! Setze auf 0.");
        }
    }

    // Private Hilfsmethode - nur intern nutzbar!
    private boolean istGueltigesAlter(int alter) {
        return alter >= 0 && alter <= 30;
    }

    public void setAlter(int alter) {
        if (istGueltigesAlter(alter)) {    // Wiederverwendung!
            this.alter = alter;
        } else {
            System.out.println("Fehler: Alter muss zwischen 0 und 30 liegen!");
        }
    }

    public int getAlter() { return alter; }
    public String getName() { return name; }

    public void vorstellen() {
        System.out.println(name + ", " + alter + " Jahre alt.");
    }
}

public class Main {
    public static void main(String[] args) {
        Hund bello = new Hund("Bello", 5);
        bello.vorstellen();

        bello.setAlter(35);     // Wird abgelehnt (gleiche Pruefung!)
        bello.vorstellen();

        // bello.istGueltigesAlter(10);  // Compilerfehler! Methode ist private
    }
}
```
@LIA.java(Main)

> **Warum private Methoden?**
>
> - Die Validierungslogik steht nur **einmal** an einer Stelle (statt doppelt in Konstruktor und Setter)
> - Von außen kann niemand `istGueltigesAlter()` aufrufen – das ist ein **internes Detail**
> - Wenn sich die Regel ändert (z.B. Alter bis 25), muss man nur **eine** Stelle anpassen

## UML lesen und schreiben

> Vom UML-Diagramm zum Code und zurück – das ist eine wichtige Fähigkeit.

```text @plantUML
@startuml
class Benutzerkonto {
    - benutzername : String
    - passwortLaenge : int
    + Benutzerkonto(name: String, passwort: String)
    + getBenutzername() : String
    + passwortAendern(neuesPasswort: String) : boolean
    - istSicheresPasswort(passwort: String) : boolean
}
@enduml
```

> **So lest ihr das Diagramm:**
>
> - `- benutzername : String` → Das Minus bedeutet `private`. Das Attribut ist von außen **nicht** direkt zugänglich.
> - `+ getBenutzername() : String` → Das Plus bedeutet `public`. Diese Methode kann von außen aufgerufen werden.
> - `- istSicheresPasswort(passwort: String) : boolean` → Auch Methoden können `private` sein! Diese Hilfsmethode wird nur **intern** verwendet – z.B. im Konstruktor und in `passwortAendern()`, um zu prüfen, ob ein Passwort sicher genug ist (mindestens 8 Zeichen).
>
> **Faustregel:** Attribute sind fast immer `private` (`-`). Getter, Setter und die "Haupt-Methoden" sind `public` (`+`). Hilfsmethoden, die nur intern gebraucht werden, sind `private` (`-`).

## Aufgabe: Klasse Bankkonto

Jetzt seid ihr dran! Setzt die Konzepte aus dieser Session selbstständig um. Erstelle diese Aufgabe auf [OnlineGDB](https://www.onlinegdb.com/) (Sprache: **Java**).

> **So geht ihr vor:**
>
> 1. Lest zuerst das UML-Diagramm: Welche Attribute und Methoden hat die Klasse? Was ist `private`, was ist `public`?
> 2. Schreibt das **Grundgerüst** der Klasse: Attribute, Konstruktor, leere Methoden.
> 3. Füllt die Methoden **nacheinander** mit Logik – fangt mit den einfachen Gettern an.
> 4. Denkt bei jeder Methode an **Validierung**: Was könnte schiefgehen?

```text @plantUML
@startuml
class Bankkonto {
    - inhaber : String
    - kontostand : double
    + Bankkonto(inhaber: String, startguthaben: double)
    + getInhaber() : String
    + getKontostand() : double
    + einzahlen(betrag: double) : void
    + abheben(betrag: double) : boolean
    + kontoInfo() : void
}
@enduml
```

1. Erstelle die Klasse `Bankkonto` gemäß dem UML-Diagramm.
2. Der Konstruktor soll den Inhaber und ein Startguthaben setzen. Das Startguthaben darf nicht negativ sein.
3. `einzahlen(betrag)`: Erhöht den Kontostand. Nur positive Beträge erlaubt.
4. `abheben(betrag)`: Verringert den Kontostand. Gibt `true` zurück wenn erfolgreich, `false` wenn nicht genug Geld vorhanden oder der Betrag ungültig ist.
5. `kontoInfo()`: Gibt Inhaber und Kontostand aus.
6. Teste mit mehreren Kontoobjekten: Einzahlen, Abheben, ungültige Operationen.

<details>

<summary>**Tipp: Startercode**</summary>

```java
class Bankkonto {
    private String inhaber;
    private double kontostand;

    public Bankkonto(String inhaber, double startguthaben) {
        // TODO
    }

    public String getInhaber() {
        // TODO
    }

    public double getKontostand() {
        // TODO
    }

    public void einzahlen(double betrag) {
        // TODO: Nur positive Betraege!
    }

    public boolean abheben(double betrag) {
        // TODO: Pruefe ob genug Geld vorhanden
    }

    public void kontoInfo() {
        // TODO
    }
}

public class Main {
    public static void main(String[] args) {
        // TODO: Teste das Bankkonto
    }
}
```

</details>

<details>

<summary>**Musterlösung**</summary>

```java Main.java
class Bankkonto {
    private String inhaber;
    private double kontostand;

    public Bankkonto(String inhaber, double startguthaben) {
        this.inhaber = inhaber;
        if (startguthaben >= 0) {
            this.kontostand = startguthaben;
        } else {
            this.kontostand = 0;
            System.out.println("Startguthaben darf nicht negativ sein. Setze auf 0.");
        }
    }

    public String getInhaber() {
        return inhaber;
    }

    public double getKontostand() {
        return kontostand;
    }

    public void einzahlen(double betrag) {
        if (betrag > 0) {
            kontostand += betrag;
            System.out.println(betrag + " Euro eingezahlt. Neuer Stand: " + kontostand);
        } else {
            System.out.println("Fehler: Betrag muss positiv sein!");
        }
    }

    public boolean abheben(double betrag) {
        if (betrag > 0 && betrag <= kontostand) {
            kontostand -= betrag;
            System.out.println(betrag + " Euro abgehoben. Neuer Stand: " + kontostand);
            return true;
        } else {
            System.out.println("Abhebung nicht moeglich! Kontostand: " + kontostand);
            return false;
        }
    }

    public void kontoInfo() {
        System.out.println("Konto von " + inhaber + ": " + kontostand + " Euro");
    }
}

public class Main {
    public static void main(String[] args) {
        Bankkonto konto1 = new Bankkonto("Anna Mueller", 1000.0);
        Bankkonto konto2 = new Bankkonto("Max Schmidt", 500.0);

        konto1.kontoInfo();
        konto2.kontoInfo();

        System.out.println();
        konto1.einzahlen(250.0);
        konto1.abheben(100.0);
        konto1.abheben(2000.0);    // Nicht genug Geld!
        konto1.einzahlen(-50.0);   // Ungueltiger Betrag!

        System.out.println();
        konto1.kontoInfo();
        konto2.kontoInfo();
    }
}
```
@LIA.java(Main)

</details>

## Zusammenfassung

> **Die 4 Grundprinzipien der OOP:**
>
> 1. **Abstraktion** – Wir modellieren nur die relevanten Eigenschaften (Session 1)
> 2. **Kapselung** – Wir schützen Daten vor unkontrolliertem Zugriff (Session 2) ✓
> 3. **Vererbung** – Klassen können Eigenschaften von anderen erben (Session 3)
> 4. **Polymorphismus** – Gleicher Aufruf, verschiedenes Verhalten (Session 4)
>
> Zwei von vier habt ihr jetzt kennengelernt!

<!-- data-type="none" -->
| Konzept | Java-Syntax | Bedeutung |
|---------|-------------|-----------|
| private | `private int alter;` | Nur in der eigenen Klasse sichtbar |
| public | `public void bellen()` | Von überall zugreifbar |
| Getter | `public int getAlter()` | Kontrollierter Lesezugriff |
| Setter | `public void setAlter(int a)` | Kontrollierter Schreibzugriff mit Validierung |
| UML: `-` | `- alter : int` | Attribut ist private |
| UML: `+` | `+ getAlter() : int` | Methode/Attribut ist public |

> **Vorschau Session 3:** Was, wenn mehrere Klassen dieselben Attribute haben? `Hund`, `Katze` und `Vogel` haben alle `name` und `alter`. Muss man den Code dreimal schreiben? Nein – dafür gibt es **Vererbung**!
