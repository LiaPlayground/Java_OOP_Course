<!--
author:   Ihr Name
email:    your@email.de
version:  0.1.0
language: de
narrator: Deutsch Female
mode:     Textbook
comment:  Session 1: Klassen und Objekte in Java – KommZuMint Schulversuch (BSZ Julius Weißbach / TU Bergakademie Freiberg)

import:   https://raw.githubusercontent.com/LiaScript/CodeRunner/master/README.md
          https://raw.githubusercontent.com/liascript-templates/plantUML/master/README.md
-->

# Session 1: Klassen und Objekte in Java

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://raw.githubusercontent.com/LiaPlayground/Java_OOP_Course/refs/heads/main/Session_01_Klassen_und_Objekte.md)

> **Lernziele dieser Session:**
>
> - Das bekannte Modell Klasse–Objekt–Attribut–Methode in Java-Syntax umsetzen
> - Klassen mit Attributen und Methoden definieren
> - Objekte erzeugen und nutzen
> - Konstruktoren verstehen und schreiben

## Einstiegsdiagnose: Was kannst du schon?

Bevor wir loslegen, wollen wir herausfinden, wo ihr steht. Keine Noten – nur eine ehrliche Bestandsaufnahme!

                   {{0-1}}
***********************************************

**Was ist eine Variable?**

- [( )] Ein fester Wert, der sich nie ändert
- [(x)] Ein benannter Speicherplatz für einen Wert
- [( )] Eine Anweisung, die etwas auf dem Bildschirm ausgibt
- [( )] Ein Programm, das automatisch läuft

***********************************************

                   {{1-2}}
***********************************************

**Welches Ergebnis liefert folgender Pseudocode?**

```
x = 10
wenn x > 5 dann
    x = x + 3
sonst
    x = x - 1
ausgabe x
```

[[13]]


***********************************************

                   {{2-3}}
***********************************************

**Was gibt folgender Pseudocode aus?**

```
ergebnis = 0
für i von 1 bis 4
    ergebnis = ergebnis + i
ausgabe ergebnis
```

[[10]]
*********

Die Schleife addiert 1 + 2 + 3 + 4 = **10**. Die Variable `ergebnis` wird in jedem Durchlauf um den aktuellen Wert von `i` erhöht.

*********

***********************************************

                   {{3-4}}
***********************************************

**Was gibt dieser Pseudocode aus?**

```
x = 1
wiederhole solange x < 50
    x = x * 2
ausgabe x
```

- [( )] 32
- [(x)] 64
- [( )] 50
- [( )] 128
*********

`x` verdoppelt sich: 1 → 2 → 4 → 8 → 16 → 32 → **64**. Bei 64 ist die Bedingung `x < 50` nicht mehr erfüllt, die Schleife endet und 64 wird ausgegeben.

*********

***********************************************

## Fehlersuche: Kannst du die Bugs finden?

Das folgende Java-Programm soll die Zahlen 1 bis 5 und ihre Quadrate ausgeben. Es enthält aber **6 Fehler**! Öffne [OnlineGDB](https://www.onlinegdb.com/) (Sprache: **Java**), kopiere den Code und versuche, alle Fehler zu finden und zu beheben.

```java
public class Main {
    public static void main(String[] args)
        int zahl = 1

        while (zahl < 5) {
            int quadrat = zahl * zahl
            System.out.println(zahl + " zum Quadrat ist " quadrat);
            zahl = zahl + 1;
        }

        System.out.Println("Fertig!");
    }
}
```

<details>

<summary>**Auflösung: Die 6 Fehler (erst selbst versuchen!)**</summary>

1. **Zeile 2:** `main(String[] args)` → fehlt die **öffnende geschweifte Klammer** `{`
2. **Zeile 3:** `int zahl = 1` → fehlt das **Semikolon** `;`
3. **Zeile 5:** `zahl < 5` → muss `zahl <= 5` sein, sonst fehlt die 5 in der Ausgabe
4. **Zeile 6:** `zahl * zahl` → fehlt das **Semikolon** `;`
5. **Zeile 7:** `"zum Quadrat ist " quadrat` → fehlt der **Verknüpfungsoperator** `+` zwischen String und Variable
6. **Zeile 11:** `System.out.Println` → Java ist case-sensitive! Es muss `println` heißen (kleines p)

**So sieht der korrigierte Code aus:**

```java Main.java
public class Main {
    public static void main(String[] args) {
        int zahl = 1;

        while (zahl <= 5) {
            int quadrat = zahl * zahl;
            System.out.println(zahl + " zum Quadrat ist " + quadrat);
            zahl = zahl + 1;
        }

        System.out.println("Fertig!");
    }
}
```
@LIA.java(Main)

</details>

> **Wie lief es?** Kein Stress, wenn ihr nicht alle Fehler gefunden habt. Die Fehlersuche in Code ist eine Fähigkeit, die mit Übung kommt. In diesem Kurs werdet ihr viel davon machen!

## Mini-Aufgabe: Mein erstes Java-Programm

Schreibe auf [OnlineGDB](https://www.onlinegdb.com/) (Sprache: **Java**) ein Programm, das folgende Ausgabe erzeugt:

```text
=== Steckbrief ===
Name: Anna
Alter: 16
Lieblingsfach: Informatik
In 2 Jahren bin ich 18 Jahre alt.
==================
```

**Anforderungen:**

1. Lege drei Variablen an: `name` (String), `alter` (int), `fach` (String) – mit deinen eigenen Werten.
2. Berechne das Alter in 2 Jahren und gib es mit aus.
3. Nutze `System.out.println(...)` für die Ausgabe.

<details>

<summary>**Tipp: Grundgerüst**</summary>

```java
public class Main {
    public static void main(String[] args) {
        String name = "Anna";
        // TODO: weitere Variablen

        System.out.println("=== Steckbrief ===");
        // TODO: Ausgaben
        System.out.println("==================");
    }
}
```

</details>

<details>

<summary>**Musterlösung**</summary>

```java Main.java
public class Main {
    public static void main(String[] args) {
        String name = "Anna";
        int alter = 16;
        String fach = "Informatik";

        System.out.println("=== Steckbrief ===");
        System.out.println("Name: " + name);
        System.out.println("Alter: " + alter);
        System.out.println("Lieblingsfach: " + fach);
        System.out.println("In 2 Jahren bin ich " + (alter + 2) + " Jahre alt.");
        System.out.println("==================");
    }
}
```
@LIA.java(Main)

</details>

## Java im Vergleich

**Java ist auch eine Insel** – so heißt das bekannteste deutschsprachige Java-Buch. Aber Java ist natürlich nicht allein auf der Welt. Schauen wir uns an, wie Java im Vergleich zu anderen Sprachen aussieht.

Das gleiche Programm – Zahlen von 1 bis 5 ausgeben – sieht in drei Sprachen sehr unterschiedlich aus. Probiert es aus!

> **Python** (interpretiert)

```python main.py
for i in range(1, 6):
    print(i)
```
@LIA.python3


> **C++** (kompiliert zu Maschinencode)

```cpp main.cpp
#include <iostream>
using namespace std;

int main() {
    for (int i = 1; i <= 5; i++) {
        cout << i << endl;
    }
    return 0;
}
```
@LIA.cpp

### Drei Sprachen im Vergleich

<!-- data-type="none" -->
| | Python | Java | C++ |
|---|--------|------|-----|
| **Ausführung** | **Interpretiert** – Zeile für Zeile | **Bytecode** – kompiliert, dann in JVM ausgeführt | **Kompiliert** – direkt in Maschinencode |
| Fehlererkennung | Erst zur **Laufzeit** | Viele Fehler schon beim **Kompilieren** | Viele Fehler schon beim **Kompilieren** |
| Typsystem | Dynamisch – Typen automatisch erkannt | Statisch – Typen müssen angegeben werden | Statisch – Typen müssen angegeben werden |
| Plattform | Läuft überall, wo Python installiert ist | Läuft überall, wo eine JVM installiert ist | Muss für jedes OS **neu kompiliert** werden |
| Speicher | Automatisch (Garbage Collector) | Automatisch (Garbage Collector) | **Manuell** – Programmierer gibt Speicher frei |
| Geschwindigkeit | Langsamer | Schnell (JVM optimiert zur Laufzeit) | Sehr schnell (nativer Maschinencode) |
| Einstieg | Einfach, wenig Schreibarbeit | Mehr Struktur nötig (`class`, `main`) | Am meisten Verantwortung beim Programmierer |

### Interpretiert vs. Kompiliert

```text
C++:      Quellcode  ──► Compiler ──► Maschinencode ──► CPU
          (.cpp)         (g++)        (.exe / ELF)

Java:     Quellcode  ──► Compiler ──► Bytecode ──► JVM ──► CPU
          (.java)        (javac)      (.class)

Python:   Quellcode  ──► Interpreter ──► CPU
          (.py)          (python)
```

> **C++** wird direkt in **Maschinencode** übersetzt – die Sprache, die der Prozessor versteht. Das ist sehr schnell, aber der Code muss für jedes Betriebssystem separat kompiliert werden.
>
> **Python** wird Zeile für Zeile vom **Interpreter** gelesen und ausgeführt. Praktisch zum Ausprobieren, aber Fehler fallen erst auf, wenn die betroffene Zeile erreicht wird.
>
> **Java steht dazwischen:** Der Compiler erzeugt **Bytecode** – eine Art Zwischensprache. Dieser wird von der **Java Virtual Machine (JVM)** ausgeführt. Das macht Java **plattformunabhängig**: "Write once, run anywhere."

> Als Programmierprofis kleben wir nicht an einer Sprache fest – wir verstehen die Konzepte dahinter und können sie in verschiedenen Sprachen anwenden. In diesem Kurs konzentrieren wir uns auf Java, aber die Ideen von Klassen, Objekten, Methoden, etc. sind universell!

## Wiederholung

In Java besteht jedes Programm aus mindestens einer **Klasse** mit einer **main-Methode** als Einstiegspunkt.

### Hello World

```java Main.java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hallo Welt!");
    }
}
```
@LIA.java(Main)

> **Aufbau erklärt:**
>
> - `public class Main` – Jede Datei enthält eine Klasse. Der Dateiname muss dem Klassennamen entsprechen.
> - `public static void main(String[] args)` – Der Startpunkt des Programms. Das `static` erklären wir später genauer.
> - `System.out.println(...)` – Gibt Text auf der Konsole aus.
> - Jede Anweisung endet mit einem **Semikolon** `;`

### Datentypen – Kurzübersicht

| Typ | Beschreibung | Beispiel |
|-----|-------------|---------|
| `int` | Ganzzahl | `int alter = 16;` |
| `double` | Kommazahl | `double preis = 9.99;` |
| `boolean` | Wahrheitswert | `boolean aktiv = true;` |
| `String` | Zeichenkette | `String name = "Max";` |

```java Main.java
public class Main {
    public static void main(String[] args) {
        int alter = 16;
        double groesse = 1.75;
        boolean hungrig = true;
        String name = "Anna";

        System.out.println(name + " ist " + alter + " Jahre alt.");
        System.out.println("Groesse: " + groesse + " m");
        System.out.println("Hungrig? " + hungrig);
    }
}
```
@LIA.java(Main)

### Arrays

Was, wenn wir nicht nur **eine** Zahl speichern wollen, sondern gleich **mehrere**? Dafür gibt es **Arrays** – eine feste Sammlung von Werten desselben Typs.

```java Main.java
public class Main {
    public static void main(String[] args) {
        // Array mit 5 Noten
        int[] noten = {2, 1, 3, 2, 1};

        // Alle Noten ausgeben
        for (int i = 0; i < noten.length; i++) {
            System.out.println("Note " + (i + 1) + ": " + noten[i]);
        }

        // Durchschnitt berechnen
        int summe = 0;
        for (int i = 0; i < noten.length; i++) {
            summe += noten[i];
        }
        double schnitt = (double) summe / noten.length;
        System.out.println("Durchschnitt: " + schnitt);
    }
}
```
@LIA.java(Main)

> **Wichtig:**
>
> - `int[] noten = {2, 1, 3, 2, 1};` – Erstellt ein Array mit 5 Elementen
> - `noten[0]` – Das **erste** Element (Index beginnt bei **0**!)
> - `noten.length` – Gibt die Anzahl der Elemente zurück (hier: 5)
> - Die Größe eines Arrays ist **fest** – sie kann nach der Erzeugung nicht mehr geändert werden

### Operatoren

<!-- data-type="none" -->
| Kategorie | Operator | Beispiel | Ergebnis |
|-----------|----------|----------|----------|
| Arithmetik | `+` `-` `*` `/` `%` | `7 / 2` | `3` (Ganzzahldivision!) |
| | | `7.0 / 2` | `3.5` |
| | | `7 % 2` | `1` (Rest) |
| Vergleich | `==` `!=` `<` `>` `<=` `>=` | `5 == 5` | `true` |
| Logisch | `&&` (und), `!` (nicht), &#124;&#124; (oder) | `true && false` | `false` |
| Zuweisung | `=` `+=` `-=` `*=` `/=` | `x += 3` | `x = x + 3` |
| Inkrement | `++` `--` | `i++` | `i = i + 1` |

```java Main.java
public class Main {
    public static void main(String[] args) {
        int a = 10;
        int b = 3;

        System.out.println("a + b = " + (a + b));
        System.out.println("a / b = " + (a / b));       // Ganzzahldivision!
        System.out.println("a % b = " + (a % b));       // Rest
        System.out.println("a > b? " + (a > b));
        System.out.println("a == b? " + (a == b));
    }
}
```
@LIA.java(Main)

> **Achtung:** `7 / 2` ergibt in Java `3` und nicht `3.5`! Bei der Division von zwei Ganzzahlen wird das Ergebnis **abgeschnitten**. Damit eine Kommazahl herauskommt, muss mindestens ein Operand ein `double` sein: `7.0 / 2` ergibt `3.5`.

### Kontrollstrukturen

Ihr kennt bereits Selektion und Zyklus – so sieht das in Java aus:

**if / else – Selektion**

```java Main.java
public class Main {
    public static void main(String[] args) {
        int alter = 16;

        if (alter >= 18) {
            System.out.println("Volljaehrig");
        } else if (alter >= 16) {
            System.out.println("Fast volljaehrig");
        } else {
            System.out.println("Minderjaehrig");
        }
    }
}
```
@LIA.java(Main)

**for-Schleife – Zählschleife**

```java Main.java
public class Main {
    public static void main(String[] args) {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Durchlauf " + i);
        }
    }
}
```
@LIA.java(Main)

**while-Schleife – Bedingungsschleife**

```java Main.java
public class Main {
    public static void main(String[] args) {
        int zahl = 1;
        while (zahl < 100) {
            zahl = zahl * 2;
        }
        System.out.println("Ergebnis: " + zahl);
    }
}
```
@LIA.java(Main)

### Methoden (Funktionen)

In Java heißen Funktionen **Methoden**. Eine Methode hat einen **Rückgabetyp**, einen **Namen** und optional **Parameter**.

```java Main.java
public class Main {

    // Methode mit Rueckgabewert
    static int quadrat(int zahl) {
        return zahl * zahl;
    }

    // Methode ohne Rueckgabewert (void)
    static void begruessen(String name) {
        System.out.println("Hallo, " + name + "!");
    }

    // Methode mit mehreren Parametern
    static double durchschnitt(int a, int b) {
        return (a + b) / 2.0;
    }

    public static void main(String[] args) {
        begruessen("Anna");
        System.out.println("3 zum Quadrat = " + quadrat(3));
        System.out.println("Durchschnitt von 7 und 4 = " + durchschnitt(7, 4));
    }
}
```
@LIA.java(Main)

## Aufgabe 1: Häufigkeit von Zahlen in einem Array

Erstelle diese Aufgabe auf [OnlineGDB](https://www.onlinegdb.com/) (Sprache: **Java**).

Gegeben ist ein Array mit Würfelergebnissen (Zahlen von 1 bis 6). Schreibe ein Programm, das zählt, **wie oft jede Zahl** vorkommt.

**Erwartete Ausgabe** für das Array `{3, 1, 4, 1, 5, 3, 2, 1, 4, 6, 2, 3}`:

```text
Wuerfelergebnisse: 3 1 4 1 5 3 2 1 4 6 2 3
Zahl 1: 3 mal
Zahl 2: 2 mal
Zahl 3: 3 mal
Zahl 4: 2 mal
Zahl 5: 1 mal
Zahl 6: 1 mal
```

**Hinweise:**

1. Lege ein Array `wuerfe` mit den gegebenen Werten an.
2. Lege ein zweites Array `haeufigkeit` der Länge 7 an (Index 0 bleibt ungenutzt, Index 1–6 für die Zahlen).
3. Durchlaufe `wuerfe` mit einer Schleife und erhöhe den passenden Zähler in `haeufigkeit`.
4. Gib am Ende die Häufigkeiten aus.

<details>

<summary>**Tipp: So funktioniert das Zählen**</summary>

Der Trick: Der Würfelwert selbst ist der **Index** im Häufigkeits-Array!

```java
// Wenn wuerfe[i] den Wert 3 hat, dann:
haeufigkeit[3]++;  // Zähler für die 3 um 1 erhöhen
// Allgemein:
haeufigkeit[wuerfe[i]]++;
```

</details>

<details>

<summary>**Musterlösung**</summary>

```java Main.java
public class Main {
    public static void main(String[] args) {
        int[] wuerfe = {3, 1, 4, 1, 5, 3, 2, 1, 4, 6, 2, 3};

        // Haeufigkeiten zaehlen (Index 0 bleibt ungenutzt)
        int[] haeufigkeit = new int[7];

        // Alle Wuerfe ausgeben
        System.out.print("Wuerfelergebnisse: ");
        for (int i = 0; i < wuerfe.length; i++) {
            System.out.print(wuerfe[i] + " ");
            haeufigkeit[wuerfe[i]]++;
        }
        System.out.println();

        // Haeufigkeiten ausgeben
        for (int i = 1; i <= 6; i++) {
            System.out.println("Zahl " + i + ": " + haeufigkeit[i] + " mal");
        }
    }
}
```
@LIA.java(Main)

</details>

## OOP Motivation: Daten, Daten, Daten

Bevor wir Klassen kennenlernen, schauen wir uns an, wie man es **ohne** Klassen machen müsste – und warum das schnell problematisch wird.

### Versuch 1: Einzelne Variablen

Stellt euch vor, wir wollen drei Hunde verwalten:

```java Main.java
public class Main {
    public static void main(String[] args) {
        // Hund 1
        String name1 = "Bello";
        int alter1 = 5;
        String rasse1 = "Labrador";

        // Hund 2
        String name2 = "Rex";
        int alter2 = 3;
        String rasse2 = "Schaeferhund";

        // Hund 3
        String name3 = "Luna";
        int alter3 = 7;
        String rasse3 = "Dackel";

        System.out.println(name1 + " (" + rasse1 + "), " + alter1 + " Jahre");
        System.out.println(name2 + " (" + rasse2 + "), " + alter2 + " Jahre");
        System.out.println(name3 + " (" + rasse3 + "), " + alter3 + " Jahre");
    }
}
```
@LIA.java(Main)

> **Frage an euch:** Was fällt euch an diesem Code auf? Was passiert, wenn wir 20 Hunde verwalten wollen?

### Versuch 2: Methoden mit langen Parameterlisten

Vielleicht helfen Methoden? Wir schreiben eine Methode, die einen Hund vorstellt:

```java Main.java
public class Main {

    static void vorstellen(String name, int alter, String rasse) {
        System.out.println(name + " (" + rasse + "), " + alter + " Jahre");
    }

    static boolean istAelterAls(String name1, int alter1, String rasse1,
                                String name2, int alter2, String rasse2) {
        return alter1 > alter2;
    }

    public static void main(String[] args) {
        String name1 = "Bello";  int alter1 = 5;  String rasse1 = "Labrador";
        String name2 = "Rex";    int alter2 = 3;   String rasse2 = "Schaeferhund";

        vorstellen(name1, alter1, rasse1);
        vorstellen(name2, alter2, rasse2);

        // Ist Bello aelter als Rex?
        boolean ergebnis = istAelterAls(name1, alter1, rasse1,
                                         name2, alter2, rasse2);
        System.out.println(name1 + " aelter als " + name2 + "? " + ergebnis);
    }
}
```
@LIA.java(Main)

### Reflexion: Was gefällt euch daran nicht?

**Welche Probleme seht ihr bei diesem Ansatz? (Mehrfachauswahl)**

- [[x]] Für jeden Hund braucht man viele einzelne Variablen
- [[x]] Die Parameterlisten der Methoden werden sehr lang
- [[x]] Es ist unklar, welche Variablen zusammengehören
- [[x]] Bei einem 4. Attribut (z.B. Gewicht) muss man ALLE Methoden anpassen
- [[ ]] Der Code ist kurz und übersichtlich

> **Kernproblem:** Daten, die zusammengehören (`name`, `alter`, `rasse` eines Hundes), sind über viele einzelne Variablen verstreut. Nichts im Code sagt: "Diese drei Variablen gehören zusammen!"
>
> **Die Lösung:** Wir brauchen eine Möglichkeit, zusammengehörige Daten **und** die passenden Funktionen zu einer Einheit zu bündeln. Genau das ist eine **Klasse**.

## Die Lösung: Klassen

Eine Klasse bündelt **Daten** (Attribute) und **Funktionen** (Methoden) zu einer Einheit.

### UML-Diagramm

Unified Modeling Language (UML) ist eine standardisierte Sprache zur Visualisierung von Softwarestrukturen. So könnte die Klasse `Hund` in UML aussehen:

```text @plantUML
@startuml
class Hund {
    + name : String
    + alter : int
    + bellen() : void
    + vorstellen() : void
}
@enduml
```

Solche Diagramme helfen uns, die Struktur unserer Klassen zu planen und zu verstehen.


### Umsetzung in Java

```java Main.java
class Hund {
    String name;
    int alter;

    void bellen() {
        System.out.println(name + " sagt: Wuff!");
    }

    void vorstellen() {
        System.out.println("Ich bin " + name + ", " + alter + " Jahre alt.");
    }
}

public class Main {
    public static void main(String[] args) {
        Hund h = new Hund();
        h.name = "Bello";
        h.alter = 5;

        h.vorstellen();
        h.bellen();
    }
}
```
@LIA.java(Main)

> **Was passiert hier?**
>
> 1. Wir definieren die Klasse `Hund` mit zwei **Attributen** (`name`, `alter`) und zwei **Methoden** (`bellen()`, `vorstellen()`).
> 2. In der `main`-Methode erzeugen wir ein **Objekt** mit `new Hund()`.
> 3. Wir setzen die Attribute über die **Punktnotation** (`h.name = "Bello"`).
> 4. Wir rufen die Methoden des Objekts auf (`h.vorstellen()`).

### Vergleich: Vorher vs. Nachher

| | Ohne Klasse (prozedural) | Mit Klasse (objektorientiert) |
|---|---|---|
| Daten | `String name1; int alter1;` | `Hund h = new Hund();` |
| Zusammengehörigkeit | Unklar – nur durch Namenskonvention | Klar – alles in einem Objekt |
| Methoden | `vorstellen(name, alter)` | `h.vorstellen()` – das Objekt kennt sich selbst! |
| Neues Attribut hinzufügen | Alle Methoden anpassen | Nur die Klasse erweitern |

> **Der entscheidende Unterschied:** Das Objekt `h` **kennt seine eigenen Daten**. Es muss nicht erst gesagt bekommen, welcher Name und welches Alter gemeint ist – es weiß das selbst!

## Objekte erzeugen

Aus einer Klasse können wir **beliebig viele Objekte** erzeugen – jedes mit eigenen Attributwerten.

```java Main.java
class Hund {
    String name;
    int alter;

    void vorstellen() {
        System.out.println("Ich bin " + name + ", " + alter + " Jahre alt.");
    }
}

public class Main {
    public static void main(String[] args) {
        Hund hund1 = new Hund();
        hund1.name = "Bello";
        hund1.alter = 5;

        Hund hund2 = new Hund();
        hund2.name = "Rex";
        hund2.alter = 3;

        Hund hund3 = new Hund();
        hund3.name = "Luna";
        hund3.alter = 7;

        hund1.vorstellen();
        hund2.vorstellen();
        hund3.vorstellen();
    }
}
```
@LIA.java(Main)

> Nun haben wir nicht mehr 6 einzelne Variablen, sondern 3 Objekte, die jeweils ihre eigenen Daten und Methoden haben. Das ist die Stärke der Objektorientierung!

**Wie viele Objekte der Klasse Hund werden hier erzeugt?**

```java
Hund a = new Hund();
Hund b = new Hund();
Hund c = a;
```

[[2]]
*********

Es werden nur **2 Objekte** erzeugt (durch die zwei `new`-Aufrufe). Die Variable `c` zeigt auf **dasselbe Objekt** wie `a` – es wird kein neues erzeugt!

*********

## Konstruktoren

Das Setzen von Attributen Zeile für Zeile ist umständlich. Ein **Konstruktor** initialisiert ein Objekt direkt bei der Erzeugung.

**Ohne Konstruktor (umständlich)**

```java
Hund h = new Hund();
h.name = "Bello";
h.alter = 5;
```

> Das Problem: Es ist leicht, Attribute zu vergessen oder falsch zu setzen. Außerdem ist es nicht klar, welche Werte zusammengehören.

**Mit Konstruktor**

```java
Hund h = new Hund("Bello", 5);
```

```java Main.java
class Hund {
    String name;
    int alter;

    // Konstruktor
    Hund(String name, int alter) {
        this.name = name;
        this.alter = alter;
    }

    void vorstellen() {
        System.out.println("Ich bin " + name + ", " + alter + " Jahre alt.");
    }

    void bellen() {
        System.out.println(name + " sagt: Wuff!");
    }
}

public class Main {
    public static void main(String[] args) {
        Hund bello = new Hund("Bello", 5);
        Hund rex = new Hund("Rex", 3);

        bello.vorstellen();
        rex.vorstellen();
        bello.bellen();
    }
}
```
@LIA.java(Main)

> **Das Schlüsselwort `this`:**
>
> Im Konstruktor haben der Parameter `name` und das Attribut `name` denselben Namen. Mit `this.name` verweisen wir auf das **Attribut des Objekts**, um es vom Parameter zu unterscheiden.

**Was passiert, wenn wir `this.` im Konstruktor weglassen?**

```java
Hund(String name, int alter) {
    name = name;       // Was passiert hier?
    alter = alter;
}
```

Ohne `this.` wird der Parameter sich selbst zugewiesen. Das Attribut des Objekts bleibt auf dem Standardwert (`null` für String, `0` für int). Das ist ein häufiger Fehler!

## Aufgabe 2: Klasse Buch

Jetzt bist du dran! Erstelle diese Aufgabe auf [OnlineGDB](https://www.onlinegdb.com/) (wähle **Java** als Sprache).

```text @plantUML
@startuml
class Buch {
    + titel : String
    + autor : String
    + seitenanzahl : int
    + Buch(titel: String, autor: String, seitenanzahl: int)
    + info() : void
}
@enduml
```

1. Erstelle die Klasse `Buch` mit den Attributen `titel`, `autor` und `seitenanzahl`.
2. Schreibe einen **Konstruktor**, der alle drei Attribute initialisiert.
3. Schreibe eine Methode `info()`, die alle Daten des Buches ausgibt, z.B.:

   `"Der Hobbit" von J.R.R. Tolkien, 310 Seiten`

4. Erzeuge in der `main`-Methode **mindestens drei** Buch-Objekte und rufe `info()` auf jedem auf.

<details>

<summary>**Tipp: Startercode (klicke zum Aufklappen)**</summary>

```java
class Buch {
    String titel;
    String autor;
    int seitenanzahl;

    // TODO: Konstruktor schreiben

    void info() {
        // TODO: Alle Daten ausgeben
    }
}

public class Main {
    public static void main(String[] args) {
        // TODO: Mindestens 3 Bücher erzeugen und info() aufrufen
    }
}
```

</details>

<details>

<summary>**Musterlösung (erst versuchen, dann schauen!)**</summary>

```java Main.java
class Buch {
    String titel;
    String autor;
    int seitenanzahl;

    Buch(String titel, String autor, int seitenanzahl) {
        this.titel = titel;
        this.autor = autor;
        this.seitenanzahl = seitenanzahl;
    }

    void info() {
        System.out.println("\"" + titel + "\" von " + autor + ", " + seitenanzahl + " Seiten");
    }
}

public class Main {
    public static void main(String[] args) {
        Buch buch1 = new Buch("Der Hobbit", "J.R.R. Tolkien", 310);
        Buch buch2 = new Buch("Harry Potter", "J.K. Rowling", 336);
        Buch buch3 = new Buch("1984", "George Orwell", 328);

        buch1.info();
        buch2.info();
        buch3.info();
    }
}
```
@LIA.java(Main)

</details>

## Zusammenfassung

| Konzept | Java-Syntax | Bedeutung |
|---------|-------------|-----------|
| Klasse definieren | `class Hund { ... }` | Bauplan für Objekte |
| Attribut | `String name;` | Eigenschaft eines Objekts |
| Methode | `void bellen() { ... }` | Verhalten eines Objekts |
| Konstruktor | `Hund(String name) { ... }` | Initialisierung bei Erzeugung |
| Objekt erzeugen | `new Hund("Bello", 5)` | Konkrete Instanz erstellen |
| Punktnotation | `h.bellen()` | Zugriff auf Attribute/Methoden |
| this | `this.name = name` | Verweis auf das aktuelle Objekt |

> **Vorschau Session 2:** Was passiert, wenn jemand `hund.alter = -5` schreibt? Das sollte nicht möglich sein! In der nächsten Session lernt ihr, wie ihr eure Daten mit **Kapselung** schützt.
