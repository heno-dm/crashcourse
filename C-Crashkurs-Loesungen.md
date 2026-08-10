# Lösungen zum C-Crashkurs

Diese Lösungen sind Referenzlösungen. Andere korrekte, verständliche und warnungsfreie Lösungen sind ebenso gültig. Jede Datei wird beispielsweise so übersetzt:

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -Wconversion -g datei.c -o programm
```

## Lösung 1 - Umgebung und Hello World

```c
#include <stdio.h>

int main(void) {
    printf("Hello, World!\n");
    printf("Mein Name ist Ada.\n");
    return 0;
}
```

## Lösung 2 - Formatiertes C

```c
#include <stdio.h>

int main(void) {
    printf(" ####\n");
    printf("#\n");
    printf("#\n");
    printf("#\n");
    printf(" ####\n");
    printf("\nZusatz mit Tabulator:\tC\n");
    return 0;
}
```

## Lösung 3 - Visitenkarte

```c
#include <stdio.h>

int main(void) {
    printf("+--------------------------------+\n");
    printf("| Ada Lovelace                   |\n");
    printf("| Lehrbetrieb: Example AG        |\n");
    printf("| ada@example.ch                 |\n");
    printf("| Motto: \"Schritt fuer Schritt\" |\n");
    printf("+--------------------------------+\n");
    return 0;
}
```

## Lösung 4 - Zeichenfolge umkehren

```c
#include <stdio.h>

int main(void) {
    char first = 'C';
    char second = 'O';
    char third = 'D';

    printf("%c%c%c\n", first, second, third);
    printf("%c%c%c\n", third, second, first);
    return 0;
}
```

## Lösung 5 - Datentypen untersuchen

```c
#include <stddef.h>
#include <stdio.h>

int main(void) {
    printf("char:   %zu Byte\n", sizeof(char));
    printf("int:    %zu Byte\n", sizeof(int));
    printf("float:  %zu Byte\n", sizeof(float));
    printf("double: %zu Byte\n", sizeof(double));
    printf("size_t: %zu Byte\n", sizeof(size_t));

    // Der C-Standard definiert Mindestbereiche, aber nicht fuer jede Plattform
    // dieselbe Anzahl Bytes fuer int.
    return 0;
}
```

## Lösung 6 - Rechteck und Kreis

```c
#include <stdio.h>

int main(void) {
    const double PI = 3.141592653589793;
    double width = 7.0;
    double height = 5.0;
    double radius = 6.0;

    printf("Rechteckflaeche: %.2f\n", width * height);
    printf("Rechteckumfang:  %.2f\n", 2.0 * (width + height));
    printf("Kreisflaeche:    %.2f\n", PI * radius * radius);
    printf("Kreisumfang:     %.2f\n", 2.0 * PI * radius);
    return 0;
}
```

## Lösung 7 - Tage zerlegen

```c
#include <stdio.h>

int main(void) {
    int total_days = 800;
    int years = total_days / 365;
    int remaining = total_days % 365;
    int weeks = remaining / 7;
    int days = remaining % 7;

    printf("%d Jahre, %d Wochen, %d Tage\n", years, weeks, days);
    return 0;
}
```

## Lösung 8 - Rechner für zwei Zahlen

```c
#include <stdio.h>

int main(void) {
    int a = 0;
    int b = 0;

    printf("Zwei Ganzzahlen: ");
    if (scanf("%d %d", &a, &b) != 2) {
        fprintf(stderr, "Ungueltige Eingabe.\n");
        return 1;
    }

    printf("Summe:      %d\n", a + b);
    printf("Differenz:  %d\n", a - b);
    printf("Produkt:    %d\n", a * b);
    if (b == 0) {
        printf("Quotient und Rest: Division durch null nicht moeglich.\n");
    } else {
        printf("Quotient:   %d\n", a / b);
        printf("Rest:       %d\n", a % b);
    }
    return 0;
}
```

## Lösung 9 - Stückgewicht

```c
#include <stdio.h>

int main(void) {
    double total_weight = 0.0;
    int count = 0;

    printf("Gesamtgewicht und Anzahl: ");
    if (scanf("%lf %d", &total_weight, &count) != 2) {
        fprintf(stderr, "Ungueltige Eingabe.\n");
        return 1;
    }
    if (total_weight < 0.0 || count <= 0) {
        fprintf(stderr, "Gewicht muss nichtnegativ und Anzahl positiv sein.\n");
        return 1;
    }

    printf("Stueckgewicht: %.3f\n", total_weight / (double)count);
    return 0;
}
```

## Lösung 10 - Operatorlabor

```c
#include <stdio.h>

int main(void) {
    int a = 0;
    int b = 0;
    printf("a b: ");
    if (scanf("%d %d", &a, &b) != 2) {
        fprintf(stderr, "Ungueltige Eingabe.\n");
        return 1;
    }

    printf("a + b = %d\n", a + b);
    printf("a - b = %d\n", a - b);
    printf("a * b = %d\n", a * b);
    if (b != 0) {
        printf("Ganzzahldivision: %d\n", a / b);
        printf("Fliesskommadivision: %.3f\n", (double)a / (double)b);
        printf("Rest: %d\n", a % b);
    }
    printf("a == b: %d\n", a == b);
    printf("a < b:  %d\n", a < b);
    printf("a > b:  %d\n", a > b);
    return 0;
}
```

## Lösung 11 - Gültiges Dreieck

```c
#include <stdio.h>

int main(void) {
    double a = 0.0;
    double b = 0.0;
    double c = 0.0;

    printf("Drei Seiten: ");
    if (scanf("%lf %lf %lf", &a, &b, &c) != 3) {
        fprintf(stderr, "Ungueltige Eingabe.\n");
        return 1;
    }

    if (a > 0.0 && b > 0.0 && c > 0.0 &&
        a < b + c && b < a + c && c < a + b) {
        printf("Umfang: %.2f\n", a + b + c);
    } else {
        printf("Diese Seiten bilden kein Dreieck.\n");
    }
    return 0;
}
```

## Lösung 12 - Winkel zwischen Uhrzeigern

```c
#include <stdio.h>

int main(void) {
    int hour = 0;
    int minute = 0;

    printf("Uhrzeit (HH:MM): ");
    if (scanf("%d:%d", &hour, &minute) != 2 ||
        hour < 0 || hour > 23 || minute < 0 || minute > 59) {
        fprintf(stderr, "Ungueltige Uhrzeit.\n");
        return 1;
    }

    double minute_angle = 6.0 * (double)minute;
    double hour_angle = 30.0 * (double)(hour % 12) + 0.5 * (double)minute;
    double difference = hour_angle - minute_angle;
    if (difference < 0.0) {
        difference = -difference;
    }
    if (difference > 180.0) {
        difference = 360.0 - difference;
    }

    printf("Kleinerer Winkel: %.1f Grad\n", difference);
    return 0;
}
```

## Lösung 13 - Zahlenbereich

```c
#include <stdio.h>

int main(void) {
    int number = 0;
    printf("Zahl: ");
    if (scanf("%d", &number) != 1) {
        fprintf(stderr, "Ungueltige Eingabe.\n");
        return 1;
    }

    if (number < 0 || number > 80) {
        printf("Ausserhalb [0, 80]\n");
    } else if (number <= 20) {
        printf("[0, 20]\n");
    } else if (number <= 40) {
        printf("[21, 40]\n");
    } else if (number <= 60) {
        printf("[41, 60]\n");
    } else {
        printf("[61, 80]\n");
    }
    return 0;
}
```

## Lösung 14 - Gerade und ungerade Summen, noch ohne Schleife

```c
#include <stdio.h>

int main(void) {
    int a = 0, b = 0, c = 0, d = 0, e = 0;
    int even_sum = 0;
    int odd_sum = 0;

    printf("Fuenf Ganzzahlen: ");
    if (scanf("%d %d %d %d %d", &a, &b, &c, &d, &e) != 5) {
        fprintf(stderr, "Ungueltige Eingabe.\n");
        return 1;
    }

    if (a % 2 == 0) even_sum += a; else odd_sum += a;
    if (b % 2 == 0) even_sum += b; else odd_sum += b;
    if (c % 2 == 0) even_sum += c; else odd_sum += c;
    if (d % 2 == 0) even_sum += d; else odd_sum += d;
    if (e % 2 == 0) even_sum += e; else odd_sum += e;

    printf("Gerade Summe: %d\nUngerade Summe: %d\n", even_sum, odd_sum);
    return 0;
}
```

Die Wiederholung ist hier bewusst sichtbar. Lösung 19 zeigt die besser skalierbare Variante.

## Lösung 15 - Gerade Zahlen bis zu einer Grenze

```c
#include <stdio.h>

int main(void) {
    int limit = 0;
    printf("Obergrenze: ");
    if (scanf("%d", &limit) != 1 || limit < 1) {
        fprintf(stderr, "Erwartet wird eine Ganzzahl ab 1.\n");
        return 1;
    }

    for (int number = 2; number <= limit; number += 2) {
        printf("%d ", number);
    }
    printf("\n");
    return 0;
}
```

## Lösung 16 - Zahlenmuster

```c
#include <stdio.h>

int main(void) {
    int height = 0;
    printf("Hoehe (1-20): ");
    if (scanf("%d", &height) != 1 || height < 1 || height > 20) {
        fprintf(stderr, "Ungueltige Hoehe.\n");
        return 1;
    }

    for (int row = 1; row <= height; ++row) {
        for (int number = 1; number <= row; ++number) {
            printf("%d%s", number, number == row ? "" : " ");
        }
        printf("\n");
    }
    return 0;
}
```

## Lösung 17 - Notenrechner

```c
#include <stdio.h>

int main(void) {
    double sum = 0.0;
    int count = 0;

    while (1) {
        double grade = 0.0;
        printf("Note (0 beendet): ");
        if (scanf("%lf", &grade) != 1) {
            fprintf(stderr, "Ungueltige Eingabe.\n");
            return 1;
        }
        if (grade == 0.0) {
            break;
        }
        if (grade < 1.0 || grade > 6.0) {
            printf("Note muss zwischen 1.0 und 6.0 liegen.\n");
            continue;
        }
        sum += grade;
        ++count;
    }

    if (count == 0) {
        printf("Keine Noten erfasst.\n");
    } else {
        printf("Anzahl: %d\nDurchschnitt: %.2f\n", count, sum / (double)count);
    }
    return 0;
}
```

## Lösung 18 - Primzahltest

```c
#include <stdbool.h>
#include <stdio.h>

bool is_prime(int number) {
    if (number < 2) {
        return false;
    }
    for (int divisor = 2; divisor <= number / divisor; ++divisor) {
        if (number % divisor == 0) {
            return false;
        }
    }
    return true;
}

int main(void) {
    int limit = 0;
    printf("Obergrenze: ");
    if (scanf("%d", &limit) != 1 || limit < 2) {
        fprintf(stderr, "Erwartet wird eine Ganzzahl ab 2.\n");
        return 1;
    }

    printf("%d ist %sprim.\n", limit, is_prime(limit) ? "" : "nicht ");
    printf("Primzahlen bis %d:\n", limit);
    for (int number = 2; number <= limit; ++number) {
        if (is_prime(number)) {
            printf("%d ", number);
        }
    }
    printf("\n");
    return 0;
}
```

## Lösung 19 - Gerade und ungerade Summen mit Array

```c
#include <stdio.h>

int main(void) {
    int values[5] = {0};
    int even_sum = 0;
    int odd_sum = 0;
    int even_count = 0;
    int odd_count = 0;

    for (size_t i = 0; i < 5; ++i) {
        printf("Zahl %zu: ", i + 1);
        if (scanf("%d", &values[i]) != 1) {
            fprintf(stderr, "Ungueltige Eingabe.\n");
            return 1;
        }
        if (values[i] % 2 == 0) {
            even_sum += values[i];
            ++even_count;
        } else {
            odd_sum += values[i];
            ++odd_count;
        }
    }

    printf("Gerade:   Anzahl %d, Summe %d\n", even_count, even_sum);
    printf("Ungerade: Anzahl %d, Summe %d\n", odd_count, odd_sum);
    return 0;
}
```

## Lösung 20 - Minimum und Position

```c
#include <stdio.h>

#define MAX_VALUES 100

int main(void) {
    int values[MAX_VALUES] = {0};
    int input_length = 0;

    printf("Laenge (1-100): ");
    if (scanf("%d", &input_length) != 1 ||
        input_length < 1 || input_length > MAX_VALUES) {
        fprintf(stderr, "Ungueltige Laenge.\n");
        return 1;
    }
    size_t length = (size_t)input_length;

    for (size_t i = 0; i < length; ++i) {
        printf("Wert %zu: ", i);
        if (scanf("%d", &values[i]) != 1) {
            fprintf(stderr, "Ungueltige Eingabe.\n");
            return 1;
        }
    }

    int minimum = values[0];
    size_t minimum_index = 0;
    for (size_t i = 1; i < length; ++i) {
        if (values[i] < minimum) {
            minimum = values[i];
            minimum_index = i;
        }
    }

    printf("Minimum %d an Index %zu\n", minimum, minimum_index);
    return 0;
}
```

## Lösung 21 - Sichere Ganzzahleingabe

```c
#include <ctype.h>
#include <errno.h>
#include <limits.h>
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>

bool read_int(const char *prompt, int min, int max, int *result) {
    char line[128];
    printf("%s", prompt);
    if (fgets(line, sizeof line, stdin) == NULL) {
        return false;
    }

    errno = 0;
    char *end = NULL;
    long value = strtol(line, &end, 10);
    if (end == line || errno == ERANGE || value < min || value > max ||
        value < INT_MIN || value > INT_MAX) {
        return false;
    }
    while (isspace((unsigned char)*end)) {
        ++end;
    }
    if (*end != '\0') {
        return false;
    }

    *result = (int)value;
    return true;
}

int main(void) {
    int number = 0;
    if (!read_int("Zahl (1-100): ", 1, 100, &number)) {
        fprintf(stderr, "Ungueltige Eingabe.\n");
        return 1;
    }
    printf("Eingelesen: %d\n", number);
    return 0;
}
```

Hinweis: Eine produktionsreife Variante würde zusätzlich erkennen und leeren, wenn die Eingabezeile den Puffer überschreitet.

## Lösung 22 - Fehlerjagd

```c
#include <stdio.h>

int main(void) {
    int values[3] = {10, 20, 30};
    int sum = 0; // Akkumulator muss initialisiert sein.

    // Der letzte gueltige Index ist 2, deshalb muss i < 3 gelten.
    for (int i = 0; i < 3; ++i) {
        sum += values[i]; // += statt =+
    }

    printf("Durchschnitt: %.2f\n", (double)sum / 3.0);
    return 0;
}
```

Gefundene Probleme:

- `sum` war nicht initialisiert.
- `i <= 3` griff ausserhalb des Arrays auf Index 3 zu.
- `sum =+ values[i]` wies in jedem Durchlauf nur einen positiven Wert zu.
- Ganzzahldivision hätte Nachkommastellen abgeschnitten.
- Ein explizites `return 0` macht den erfolgreichen Abschluss für Einsteigende sichtbar.

## Lösung 23 - Tic-Tac-Toe

```c
#include <stdbool.h>
#include <stdio.h>

void print_board(const char board[9]) {
    printf("\n %c | %c | %c\n", board[0], board[1], board[2]);
    printf("---+---+---\n");
    printf(" %c | %c | %c\n", board[3], board[4], board[5]);
    printf("---+---+---\n");
    printf(" %c | %c | %c\n\n", board[6], board[7], board[8]);
}

bool has_won(const char board[9], char player) {
    const int lines[8][3] = {
        {0, 1, 2}, {3, 4, 5}, {6, 7, 8},
        {0, 3, 6}, {1, 4, 7}, {2, 5, 8},
        {0, 4, 8}, {2, 4, 6}
    };
    for (size_t i = 0; i < 8; ++i) {
        if (board[lines[i][0]] == player &&
            board[lines[i][1]] == player &&
            board[lines[i][2]] == player) {
            return true;
        }
    }
    return false;
}

bool make_move(char board[9], int field, char player) {
    int index = field - 1;
    if (index < 0 || index >= 9 || board[index] == 'X' || board[index] == 'O') {
        return false;
    }
    board[index] = player;
    return true;
}

int main(void) {
    char board[9] = {'1', '2', '3', '4', '5', '6', '7', '8', '9'};
    char player = 'X';

    for (int turn = 0; turn < 9;) {
        print_board(board);
        int field = 0;
        printf("%c waehlt ein Feld: ", player);
        if (scanf("%d", &field) != 1) {
            fprintf(stderr, "Ungueltige Eingabe.\n");
            return 1;
        }
        if (!make_move(board, field, player)) {
            printf("Feld ist ungueltig oder belegt.\n");
            continue;
        }

        ++turn;
        if (has_won(board, player)) {
            print_board(board);
            printf("%c gewinnt!\n", player);
            return 0;
        }
        player = player == 'X' ? 'O' : 'X';
    }

    print_board(board);
    printf("Unentschieden.\n");
    return 0;
}
```

### Testideen für Tic-Tac-Toe

| Fall | Zugfolge | Erwartung |
| --- | --- | --- |
| Sieg in Reihe | 1, 4, 2, 5, 3 | X gewinnt |
| Sieg in Spalte | 1, 2, 4, 3, 7 | X gewinnt |
| Sieg diagonal | 1, 2, 5, 3, 9 | X gewinnt |
| Feld doppelt | 1, 1 | zweiter Zug wird abgelehnt |
| Ausser Bereich | 0 oder 10 | Zug wird abgelehnt |

## Hinweise zur Bewertung

Eine mögliche Gewichtung pro Aufgabe:

- 40 Prozent: korrektes Verhalten und Grenzfälle
- 25 Prozent: verständliche Struktur und Namen
- 20 Prozent: Eingabeprüfung und Fehlerbehandlung
- 15 Prozent: warnungsfreier, formatierter Code und dokumentierte Tests

Bei Einsteigeraufgaben zählt eine nachvollziehbare Herleitung mehr als eine besonders kurze Lösung.
