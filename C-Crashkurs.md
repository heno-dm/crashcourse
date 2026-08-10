# C-Crashkurs für Lernende

## Ziel und Rahmen

Dieser Kurs führt schrittweise in die prozedurale Programmierung mit C ein. Gearbeitet wird mit Visual Studio Code in einem Devcontainer. Alle Lernenden erhalten dadurch dieselbe Linux-Umgebung mit Compiler, Debugger und Formatierungswerkzeugen.

Der Kurs ist für erste Programmiererfahrungen ausgelegt. Die Kapitel bauen aufeinander auf. Zu jedem Thema gehören kurze Theorie, ein Beispiel und Praxisaufgaben.

Nach dem Kurs kannst du:

- C-Programme übersetzen, starten und debuggen
- Variablen, Datentypen, Operatoren und Ein-/Ausgaben verwenden
- Entscheidungen mit `if`, `else` und `switch` formulieren
- Wiederholungen mit Schleifen programmieren
- Programme mit Funktionen strukturieren
- Arrays und Zeichenketten grundlegend einsetzen
- Eingaben prüfen und typische Fehler systematisch finden
- ein kleines Programm selbstständig planen und umsetzen

## 1. Entwicklungsumgebung

### 1.1 Benötigte Werkzeuge

- Visual Studio Code
- Docker Desktop oder eine kompatible Container-Laufzeit
- VS-Code-Erweiterung **Dev Containers**

### 1.2 Projektstruktur

Lege folgende Struktur an:

```text
crashcourse/
├── .devcontainer/
│   ├── devcontainer.json
│   └── Dockerfile
├── .vscode/
│   ├── launch.json
│   └── tasks.json
└── src/
    └── hello_world.c
```

`.devcontainer/devcontainer.json`:

```json
{
  "name": "C Crashkurs",
  "build": {
    "dockerfile": "Dockerfile"
  },
  "customizations": {
    "vscode": {
      "extensions": [
        "ms-vscode.cpptools",
        "ms-vscode.makefile-tools"
      ],
      "settings": {
        "C_Cpp.default.cStandard": "c23",
        "editor.formatOnSave": true
      }
    }
  },
  "remoteUser": "vscode"
}
```

`.devcontainer/Dockerfile`:

```dockerfile
FROM mcr.microsoft.com/devcontainers/cpp:debian13

RUN apt-get update \
    && apt-get install -y --no-install-recommends clang-format valgrind \
    && rm -rf /var/lib/apt/lists/*
```

`.vscode/tasks.json` übersetzt die aktuell geöffnete C-Datei:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "C: aktuelle Datei bauen",
      "type": "shell",
      "command": "gcc",
      "args": [
        "-std=c23", "-Wall", "-Wextra", "-Wpedantic", "-Wconversion", "-g",
        "${file}", "-o", "${fileDirname}/${fileBasenameNoExtension}"
      ],
      "options": { "cwd": "${fileDirname}" },
      "problemMatcher": ["$gcc"],
      "group": { "kind": "build", "isDefault": true }
    }
  ]
}
```

`.vscode/launch.json` startet die aktuell geöffnete Datei im Debugger:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "C: aktuelle Datei debuggen",
      "type": "cppdbg",
      "request": "launch",
      "program": "${fileDirname}/${fileBasenameNoExtension}",
      "cwd": "${fileDirname}",
      "MIMode": "gdb",
      "preLaunchTask": "C: aktuelle Datei bauen",
      "externalConsole": false
    }
  ]
}
```

Öffne den Projektordner in VS Code und wähle **Dev Containers: Reopen in Container**. Prüfe danach im Terminal:

```bash
gcc --version
gdb --version
```

### 1.3 Übersetzen und starten

Ein C-Quelltext wird vom Compiler in ein ausführbares Programm übersetzt:

```bash
gcc -std=c23 -Wall -Wextra -Wpedantic -Wconversion -g src/hello_world.c -o hello_world
./hello_world
```

- `-std=c23` wählt den aktuellen ISO-Sprachstandard C23.
- `-Wall -Wextra -Wpedantic` aktiviert wichtige Warnungen.
- `-Wconversion` warnt vor riskanten Typumwandlungen.
- `-g` fügt Informationen für den Debugger hinzu.
- `-o hello_world` bestimmt den Namen des Programms.

Warnungen werden im Kurs wie Fehler behandelt: Ursache verstehen und beheben.

### Praxis 1 - Umgebung und Hello World

Erstelle `src/hello_world.c`. Das Programm soll exakt die Zeile `Hello, World!` ausgeben. Übersetze es mit allen oben genannten Warnoptionen und starte es.

Akzeptanzkriterien:

- Das Programm lässt sich ohne Warnung übersetzen.
- Die Ausgabe endet mit einem Zeilenumbruch.
- `main` liefert den Wert `0` zurück.

Zusatz: Ändere die Ausgabe so, dass dein Name in einer zweiten Zeile erscheint.

## 2. Aufbau eines C-Programms und Ausgaben

```c
#include <stdio.h>

int main(void) {
    printf("Hallo!\n");
    return 0;
}
```

- `#include <stdio.h>` macht Deklarationen für Ein-/Ausgabe verfügbar.
- `main` ist der Einstiegspunkt des Programms.
- Geschweifte Klammern bilden einen Block.
- Anweisungen enden meistens mit `;`.
- `printf` schreibt formatierten Text auf die Standardausgabe.
- `\n`, `\t`, `\\` und `\"` sind Escape-Sequenzen.
- `return 0` meldet dem Betriebssystem einen erfolgreichen Abschluss.

Kommentare erklären Absicht und Entscheidungen:

```c
// Einzeiliger Kommentar
/* Mehrzeiliger
   Kommentar */
```

Kommentare sollen nicht bloss wiederholen, was der Code bereits sagt.

### Praxis 2 - Formatiertes C

Gib mit mehreren `printf`-Aufrufen und dem Zeichen `#` ein grosses C aus. Beispiel:

```text
 ####
#
#
#
 ####
```

Zusatz: Erzeuge zusätzlich ein F und verwende mindestens einmal `\t`.

### Praxis 3 - Visitenkarte

Gib eine Visitenkarte mit Rahmen aus. Sie enthält Name, Lehrbetrieb, E-Mail-Adresse und einen frei gewählten Satz. Sonderzeichen im Text müssen korrekt escaped sein.

## 3. Variablen, Datentypen und Operatoren

Eine Variable besitzt einen Typ, einen Namen und einen Wert:

```c
int age = 16;
double temperature = 21.5;
char grade = 'A';
```

Wichtige Typen:

| Typ | Typischer Zweck | Ausgabe mit `printf` |
| --- | --- | --- |
| `char` | einzelnes Zeichen / kleine Ganzzahl | `%c` |
| `int` | Ganzzahl | `%d` |
| `unsigned int` | nichtnegative Ganzzahl | `%u` |
| `size_t` | Grössen und Indizes | `%zu` |
| `float` | Fliesskommazahl | `%f` |
| `double` | genauere Fliesskommazahl | `%f` |

Die tatsächliche Speichergrösse hängt von der Plattform ab und kann mit `sizeof` ermittelt werden. `sizeof` liefert `size_t`.

Konstanten erhalten mit `const` einen nicht mehr veränderbaren Wert:

```c
const double PI = 3.141592653589793;
```

Arithmetische Operatoren sind `+`, `-`, `*`, `/` und `%`. Bei zwei Ganzzahlen führt `/` eine Ganzzahldivision aus: `7 / 2` ergibt `3`. Mindestens ein Operand muss eine Fliesskommazahl sein, wenn `3.5` erwartet wird.

Vergleiche verwenden `==`, `!=`, `<`, `<=`, `>` und `>=`. Logische Operatoren sind `&&` (und), `||` (oder) und `!` (nicht). Eine Zuweisung verwendet `=`, ein Vergleich auf Gleichheit `==`.

### Praxis 4 - Zeichenfolge umkehren

Speichere drei Zeichen in drei `char`-Variablen. Gib sie zuerst in normaler, danach in umgekehrter Reihenfolge aus.

### Praxis 5 - Datentypen untersuchen

Gib die Grösse von `char`, `int`, `float`, `double` und `size_t` in Bytes aus. Verwende `sizeof` und `%zu`. Beantworte als Kommentar im Quelltext: Weshalb soll man sich nicht auf eine überall identische Grösse von `int` verlassen?

### Praxis 6 - Rechteck und Kreis

Teil A: Speichere Höhe und Breite eines Rechtecks als `double`. Berechne Fläche und Umfang.

Teil B: Speichere den Radius eines Kreises als `double`. Berechne Fläche und Umfang mit einer `const`-Konstante für Pi.

Alle Ergebnisse werden mit zwei Nachkommastellen ausgegeben. Teste unter anderem Rechteck `7.0 x 5.0` und Radius `6.0`.

### Praxis 7 - Tage zerlegen

Speichere eine nichtnegative Anzahl Tage. Zerlege sie mit Ganzzahldivision und Modulo in Jahre zu 365 Tagen, Wochen und Resttage.

Beispiel: `800` Tage ergeben `2` Jahre, `10` Wochen und `0` Tage.

## 4. Eingaben und ihre Prüfung

`scanf` kann für erste numerische Eingaben verwendet werden. Der Rückgabewert sagt, wie viele Werte erfolgreich gelesen wurden:

```c
int value = 0;
printf("Ganzzahl: ");
if (scanf("%d", &value) != 1) {
    fprintf(stderr, "Ungueltige Eingabe.\n");
    return 1;
}
```

Das `&` liefert die Adresse der Variablen. Ohne passende Adresse kann das Programm abstürzen oder unvorhersehbar reagieren. Formatbezeichner und Variablentyp müssen zusammenpassen.

Für robuste Programme sind `fgets` und anschliessendes Parsen mit `strtol` oder `strtod` besser geeignet. Im ersten Kursteil genügt geprüftes `scanf`; in der Vertiefung wird robustes Parsen verwendet.

Fehlermeldungen gehören auf `stderr`. Ein Rückgabewert ungleich `0` signalisiert einen Fehler.

### Praxis 8 - Rechner für zwei Zahlen

Lies zwei Ganzzahlen ein. Gib Summe, Differenz, Produkt, Quotient und Rest aus. Prüfe beide Eingaben. Eine Division durch null muss verhindert und verständlich gemeldet werden.

### Praxis 9 - Stückgewicht

Lies das Gesamtgewicht als `double` und die Anzahl Gegenstände als `int` ein. Berechne das Gewicht eines Gegenstands. Lehne ein negatives Gewicht sowie eine Anzahl kleiner oder gleich null ab.

### Praxis 10 - Operatorlabor

Lies zwei Ganzzahlen `a` und `b`. Gib Ergebnisse für alle arithmetischen Operatoren sowie für die Vergleiche `a == b`, `a < b` und `a > b` aus. Beobachte den Unterschied zwischen Ganzzahl- und Fliesskommadivision.

## 5. Verzweigungen

Mit Bedingungen wählt ein Programm einen Pfad:

```c
if (temperature < 0.0) {
    printf("Frost\n");
} else if (temperature < 20.0) {
    printf("Kuehl\n");
} else {
    printf("Warm\n");
}
```

Mehrere diskrete Fälle können mit `switch` übersichtlich sein. Ein `break` verhindert, dass der nächste Fall ebenfalls ausgeführt wird.

Gleitkommazahlen sollen wegen Rundungsfehlern normalerweise nicht mit `==` verglichen werden. Stattdessen prüft man, ob ihre Differenz kleiner als eine gewählte Toleranz ist.

### Praxis 11 - Gültiges Dreieck

Lies drei positive Seitenlängen als `double`. Ein Dreieck ist möglich, wenn jede Seite kleiner als die Summe der beiden anderen Seiten ist. Gib bei einem gültigen Dreieck den Umfang aus, sonst eine Fehlermeldung.

Teste mindestens: `3 4 5`, `1 2 3`, `-1 2 2`.

### Praxis 12 - Winkel zwischen Uhrzeigern

Lies eine Uhrzeit im Format `HH:MM`, zum Beispiel `11:34`. Prüfe `0 <= HH <= 23` und `0 <= MM <= 59`.

Berechne den kleineren Winkel zwischen Stunden- und Minutenzeiger. Beachte: Der Stundenzeiger bewegt sich auch innerhalb einer Stunde. Das Resultat liegt zwischen 0 und 180 Grad.

### Praxis 13 - Zahlenbereich

Lies eine Ganzzahl und ordne sie genau einem Bereich zu: `[0, 20]`, `[21, 40]`, `[41, 60]` oder `[61, 80]`. Werte ausserhalb werden separat gemeldet. Teste die Grenzwerte `-1`, `0`, `20`, `21`, `80`, `81`.

### Praxis 14 - Gerade und ungerade Summen, noch ohne Schleife

Lies fünf Ganzzahlen in fünf einzelne Variablen. Addiere gerade Zahlen zu einer geraden Summe und ungerade Zahlen zu einer ungeraden Summe. Diese Aufgabe wird später mit Schleife und Array verbessert.

## 6. Schleifen

Schleifen wiederholen Anweisungen:

```c
for (int i = 0; i < 5; ++i) {
    printf("%d\n", i);
}
```

- `for` eignet sich, wenn Start, Ende und Schritt bekannt sind.
- `while` prüft vor jedem Durchlauf.
- `do ... while` führt den Block mindestens einmal aus.
- `break` beendet eine Schleife.
- `continue` springt zum nächsten Durchlauf.

Wichtige Fehlerquellen sind Endlosschleifen, eine falsche Abbruchbedingung und ein Zugriff um eine Position neben dem gültigen Bereich.

### Praxis 15 - Gerade Zahlen bis zu einer Grenze

Lies eine Ganzzahl `limit >= 1`. Gib alle geraden Zahlen von 1 bis einschliesslich `limit` aus. Ungültige Eingaben werden abgelehnt.

### Praxis 16 - Zahlenmuster

Erzeuge mit verschachtelten Schleifen dieses Muster, ohne jede Zeile einzeln zu programmieren:

```text
1
1 2
1 2 3
1 2 3 4
1 2 3 4 5
```

Zusatz: Lies die Höhe zwischen 1 und 20 ein.

### Praxis 17 - Notenrechner

Lies nacheinander Schweizer Schulnoten von `1.0` bis `6.0`. Die Eingabe `0` beendet die Erfassung. Ungültige Noten werden gemeldet und nicht mitgezählt. Gib Anzahl und Durchschnitt aus. Verhindere eine Division durch null, falls sofort `0` eingegeben wird.

## 7. Funktionen

Funktionen zerlegen ein Problem in benannte, testbare Teile:

```c
int maximum(int a, int b);

int main(void) {
    printf("%d\n", maximum(3, 7));
    return 0;
}

int maximum(int a, int b) {
    return a > b ? a : b;
}
```

Die erste Zeile ist der Funktionsprototyp. Parameter sind lokale Variablen. C übergibt Werte standardmässig als Kopie. Eine Funktion soll möglichst eine klar beschriebene Aufgabe haben.

### Praxis 18 - Primzahltest

Implementiere `bool is_prime(int number)`. Eine Primzahl ist eine natürliche Zahl grösser als 1 mit genau zwei positiven Teilern. Prüfe mögliche Teiler nur so lange, wie `divisor <= number / divisor`; damit werden Überlauf und unnötige Arbeit vermieden.

Teil A: Lies eine Zahl und gib aus, ob sie prim ist.

Teil B: Gib mit derselben Funktion alle Primzahlen bis zu einer eingegebenen Obergrenze aus.

## 8. Arrays und Zeichenketten

Ein Array speichert mehrere Werte desselben Typs zusammen:

```c
int values[5] = {4, 8, 15, 16, 23};
size_t length = sizeof values / sizeof values[0];
```

Gültige Indizes reichen von `0` bis `length - 1`. C prüft Arraygrenzen nicht automatisch. Ein Zugriff ausserhalb ist undefiniertes Verhalten.

Eine Zeichenkette ist ein `char`-Array mit dem Abschlusszeichen `\0`:

```c
char name[40] = "Ada";
```

Für Text eignet sich `fgets`, weil eine maximale Länge angegeben wird:

```c
char line[100];
if (fgets(line, sizeof line, stdin) == NULL) {
    fprintf(stderr, "Lesefehler.\n");
    return 1;
}
```

### Praxis 19 - Gerade und ungerade Summen mit Array

Lies fünf Ganzzahlen in ein Array. Ermittle die gerade und ungerade Summe mit genau einer Schleife. Zusatz: Gib auch die Anzahl gerader und ungerader Werte aus.

### Praxis 20 - Minimum und Position

Lies zuerst eine Länge von 1 bis 100, danach entsprechend viele Ganzzahlen in ein Array fester Maximalgrösse. Ermittle den kleinsten Wert und seinen Index. Kommt das Minimum mehrfach vor, wird der erste Index ausgegeben.

## 9. Robuste Texteingabe

`scanf` ist kompakt, lässt aber bei fehlerhaften Eingaben leicht Zeichen im Eingabepuffer zurück. Für zuverlässige Programme wird zuerst eine ganze Zeile mit `fgets` gelesen und danach konvertiert. `strtol` meldet unter anderem, wo die Konvertierung endete und ob der Zahlenbereich überschritten wurde.

Beim Parsen sind mindestens diese Fälle zu prüfen:

- Es wurde keine Zahl erkannt.
- Nach der Zahl stehen unerwartete Zeichen.
- Die Zahl liegt ausserhalb des gewünschten Bereichs.
- Die Eingabezeile war länger als der Puffer.

### Praxis 21 - Sichere Ganzzahleingabe

Schreibe eine Funktion mit diesem Vertrag:

```c
bool read_int(const char *prompt, int min, int max, int *result);
```

Sie liest mit `fgets`, konvertiert mit `strtol`, akzeptiert Leerraum am Ende und liefert nur Werte im Bereich `[min, max]`. Bei Erfolg wird das Ergebnis über `result` zurückgegeben.

Verwende die Funktion, um eine Zahl von 1 bis 100 einzulesen.

## 10. Debugging und Qualität

Ein guter Ablauf bei Fehlern:

1. Fehler reproduzieren und Eingabe notieren.
2. Compilerwarnungen vollständig lesen.
3. Eine Hypothese bilden.
4. Mit Breakpoint, schrittweiser Ausführung und Variablenansicht prüfen.
5. Ursache beheben und den ursprünglichen sowie angrenzende Testfälle erneut testen.

Nützliche Befehle:

```bash
gcc -std=c23 -Wall -Wextra -Wpedantic -Wconversion -g -fsanitize=address,undefined src/program.c -o program
./program
valgrind --leak-check=full ./program
```

Sanitizer und Valgrind werden getrennt verwendet. Automatische Formatierung:

```bash
clang-format -i src/program.c
```

### Praxis 22 - Fehlerjagd

Der folgende Code enthält mehrere Fehler. Kopiere ihn, übersetze ihn mit Warnungen und Sanitizern, finde die Ursachen und dokumentiere jede Korrektur in einem kurzen Kommentar.

```c
#include <stdio.h>

int main(void) {
    int values[3] = {10, 20, 30};
    int sum;

    for (int i = 0; i <= 3; ++i) {
        sum =+ values[i];
    }

    printf("Durchschnitt: %d\n", sum / 3);
}
```

## 11. Abschlussprojekt - Tic-Tac-Toe

Entwickle ein Tic-Tac-Toe-Spiel für zwei Personen im Terminal.

Mindestanforderungen:

- Spielfeld als Array
- abwechselnde Spielzüge für `X` und `O`
- Eingabeprüfung für Feldnummern und bereits belegte Felder
- Erkennung von Sieg und Unentschieden
- sinnvolle Funktionen, zum Beispiel `print_board`, `make_move` und `winner`
- Übersetzung ohne Warnungen

Vorgehen:

1. Definiere Datenmodell und Ein-/Ausgabe mit einem kleinen Beispiel.
2. Implementiere und teste die Spielfeldausgabe.
3. Implementiere einen gültigen Spielzug.
4. Implementiere die Siegesprüfung.
5. Verbinde alles in der Spielschleife.
6. Teste Reihen, Spalten, Diagonalen, ungültige Eingaben und Unentschieden.

Mögliche Erweiterungen:

- erneute Partie ohne Neustart
- Spielstand über mehrere Partien
- Computergegner
- frei wählbare Spielfeldgrösse

## 12. Eigene Programmidee

Plane ein kleines Konsolenprogramm mit Eingaben, Verarbeitung und Ausgaben. Beispiele sind Quiz, Taschenrechner, Zahlenratespiel oder einfache Aufgabenverwaltung. Formuliere vor dem Programmieren:

- Ziel und Zielgruppe
- mindestens drei Anforderungen
- benötigte Daten
- mindestens fünf Testfälle
- Aufteilung in Funktionen

## Abgabe-Checkliste

- Der Code lässt sich im Devcontainer reproduzierbar ausführen.
- Der Compiler meldet keine Warnungen.
- Variablen und Funktionen haben verständliche Namen.
- Eingaben und Grenzfälle werden geprüft.
- Wiederholter Code wurde sinnvoll in Schleifen oder Funktionen zusammengefasst.
- Der Code ist mit `clang-format` formatiert.
- Zu jeder Aufgabe sind verwendete Testeingaben und Resultate notiert.
