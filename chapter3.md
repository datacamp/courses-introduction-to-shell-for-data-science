---
title: Werkzeuge kombinieren
description: >-
  Die wahre Stärke der Unix-Shell liegt nicht in den einzelnen Befehlen, sondern
  darin, wie einfach sie kombiniert werden können, um neue Dinge zu tun. In
  diesem Kapitel erfährst du, wie du diese Fähigkeit nutzen kannst, um die
  gewünschten Daten auszuwählen, und lernst Befehle zum Sortieren von Werten und
  Entfernen von Duplikaten kennen.
lessons:
  - nb_of_exercises: 12
    title: Wie kann ich die Ausgabe eines Befehls in einer Datei speichern?
---

## Wie kann ich die Ausgabe eines Befehls in einer Datei speichern?

```yaml
type: ConsoleExercise
key: 07a427d50c
xp: 100
```

Bei allen Tools, die du bisher gesehen hast, kannst du Eingabedateien benennen.
Die meisten haben keine Option für die Benennung einer Ausgabedatei, weil sie keine brauchen.
Stattdessen,
kannst du **die Umleitung** nutzen, um die Ausgabe eines Befehls an einem beliebigen Ort zu speichern.
Wenn du diesen Befehl ausführst:

```{shell}
head -n 5 seasonal/summer.csv
```

druckt er die ersten 5 Zeilen der Sommerdaten auf dem Bildschirm aus.
Wenn du stattdessen diesen Befehl ausführst:

```{shell}
head -n 5 seasonal/summer.csv > top.csv
```

erscheint nichts auf dem Bildschirm.
Stattdessen,
`head`Die Ausgabe des Programms wird in einer neuen Datei namens `top.csv` gespeichert.
Du kannst dir den Inhalt dieser Datei mit `cat` ansehen:

```{shell}
cat top.csv
```

Das Größer-als-Zeichen `>` weist die Shell an, die Ausgabe von `head` in eine Datei umzuleiten.
Sie ist nicht Teil des `head` Befehls;
stattdessen,
Es funktioniert mit jedem Shell-Befehl, der eine Ausgabe erzeugt.

`@instructions`
Kombiniere `tail` mit der Umleitung, um die letzten 5 Zeilen von `seasonal/winter.csv` in einer Datei namens `last.csv` zu speichern.

`@hint`
Verwende `tail -n 5`, um die letzten 5 Zeilen zu erhalten.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
tail -n 5 seasonal/winter.csv > last.csv
```

`@sct`
```{python}
patt = "Die Zeile `%s` sollte in der Datei `last.csv` sein, aber sie ist es nicht. Leite die Ausgabe von `tail -n 5 seasonal/winter.csv` mit `>` nach `last.csv` um."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/last.csv').multi(
        check_not(has_code('2017-07-01,incisor'), incorrect_msg='`last.csv` hat zu viele Zeilen. Hast du das Flag `-n 5` mit `tail` verwendet?'),
        has_code('2017-07-17,canine', incorrect_msg=patt%'2017-07-17,canine'),
        has_code('2017-08-13,canine', incorrect_msg=patt%'2017-08-13,canine')
    )
)
Ex().success_msg("Gut gemacht! Lass uns noch ein bisschen üben!")

```

---

## Wie kann ich die Ausgabe eines Befehls als Eingabe verwenden?

```yaml
type: BulletConsoleExercise
key: f47d337593
xp: 100
```

Angenommen, du willst Zeilen aus der Mitte einer Datei holen.
Um genau zu sein,
Nehmen wir an, du willst die Zeilen 3-5 aus einer unserer Datendateien holen.
Du kannst mit `head` beginnen, um die ersten 5 Zeilen zu erhalten
und leite sie in eine Datei um,
und benutze dann `tail`, um die letzten 3 auszuwählen:

```{shell}
head -n 5 seasonal/winter.csv > top.csv
tail -n 3 top.csv
```

Eine kurze Überprüfung bestätigt, dass dies die Zeilen 3-5 unserer Originaldatei sind,
denn es sind die letzten 3 Zeilen der ersten 5.

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: 35bbb5520e
xp: 50
```

`@instructions`
Wähle die letzten beiden Zeilen aus `seasonal/winter.csv`
und speichere sie in einer Datei namens `bottom.csv`.

`@hint`
Verwende `tail`, um Zeilen auszuwählen und `>`, um die Ausgabe von `tail` umzuleiten.

`@solution`
```{shell}
tail -n 2 seasonal/winter.csv > bottom.csv

```

`@sct`
```{python}
patt="Die Zeile `%s` sollte in der Datei `bottom.csv` sein, aber sie ist es nicht. Leiten Sie die Ausgabe von `tail -n 2 seasonal/winter.csv` mit `>` nach `bottom.csv` um."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/bottom.csv').multi(
        check_not(has_code('2017-08-11,bicuspid'), incorrect_msg = '`bottom.csv` hat zu viele Zeilen. Haben Sie das Flag `-n 2` mit `tail` verwendet?'),
        has_code('2017-08-11,wisdom', incorrect_msg=patt%"2017-08-11,wisdom"),
        has_code('2017-08-13,canine', incorrect_msg=patt%"2017-08-13,canine")
    )
)

```

***

```yaml
type: ConsoleExercise
key: c94d3936a7
xp: 50
```

`@instructions`
Wählen Sie die erste Zeile aus `bottom.csv`
um die vorletzte Zeile der Originaldatei zu erhalten.

`@hint`
Verwende `head`, um die gewünschte Zeile auszuwählen.

`@solution`
```{shell}
head -n 1 bottom.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/bottom.csv').has_code('2017-08-11,wisdom', incorrect_msg="Mit der Datei `bottom.csv` stimmt etwas nicht. Stellen Sie sicher, dass Sie sie nicht ändern!"),
    has_expr_output(strict=True, incorrect_msg="Haben Sie `head` korrekt auf `bottom.csv` verwendet? Stellen Sie sicher, dass Sie das `-n` Flag korrekt verwenden.")
)

Ex().success_msg("Gut gemacht. Gehen Sie zur nächsten Übung, um bessere Möglichkeiten zum Kombinieren von Befehlen zu erfahren.")                             
```

---

## Wie kann man Befehle besser kombinieren?

```yaml
type: ConsoleExercise
key: b36aea9a1e
xp: 100
```

Die Umleitung zur Kombination von Befehlen hat zwei Nachteile:

1. Dabei bleiben viele Zwischendateien liegen (wie `top.csv`).
2. Die Befehle, die zu deinem Endergebnis führen, sind über mehrere Zeilen der Geschichte verstreut.

Die Shell bietet ein weiteres Werkzeug, das beide Probleme auf einmal löst: die **Pipe**.
Noch einmal,
Beginne damit, `head` auszuführen:

```{shell}
head -n 5 seasonal/summer.csv
```

Anstatt die Ausgabe von `head` in eine Datei zu senden,
füge einen vertikalen Balken und den Befehl `tail` *ohne* Dateinamen hinzu:

```{shell}
head -n 5 seasonal/summer.csv | tail -n 3
```

Das Pipe-Symbol weist die Shell an, die Ausgabe des Befehls auf der linken Seite zu verwenden
als Eingabe für den Befehl auf der rechten Seite.

`@instructions`
Verwende `cut`, um alle Zahnnamen aus Spalte 2 der kommagetrennten Datei `seasonal/summer.csv` auszuwählen, und leite das Ergebnis dann über die Pipeline an `grep` weiter, um die Kopfzeile, die das Wort "Zahn" enthält, auszuschließen. *`cut` und `grep` wurden in Kapitel 2, Übungen 8 bzw. 11 ausführlich behandelt.*

`@hint`
- Der erste Teil des Befehls hat die Form `cut -d field_delimiter -f column_number filename`.
- Der zweite Teil des Befehls hat die Form `grep -v thing_to_match`.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth
```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_expr_output(incorrect_msg = 'Haben Sie das Ergebnis von `cut -d , -f 2 seasonal/summer.csv` mit `|` in `grep -v Tooth` geleitet?'),
    check_not(has_output("Tooth"), incorrect_msg = 'Haben Sie die Kopfzeile `"Tooth"` mit `grep` ausgeschlossen?')
)
Ex().success_msg("Perfektes Piping! Dies ist vielleicht das erste Mal, dass Sie `|` verwendet haben, aber es wird definitiv nicht das letzte Mal sein!")
```

---

## Wie kann ich viele Befehle kombinieren?

```yaml
type: ConsoleExercise
key: b8753881d6
xp: 100
```

Du kannst eine beliebige Anzahl von Befehlen aneinanderreihen.
Ein Beispiel:
diesen Befehl:

```{shell}
cut -d , -f 1 seasonal/spring.csv | grep -v Date | head -n 10
```

wird:

1. wähle die erste Spalte aus den Frühlingsdaten aus;
2. die Kopfzeile mit dem Wort "Datum" entfernen; und
3. wähle die ersten 10 Zeilen der tatsächlichen Daten aus.

`@instructions`
In der vorherigen Übung hast du den folgenden Befehl verwendet, um alle Zahnnamen aus Spalte 2 von `seasonal/summer.csv` auszuwählen:

```
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth
```

Erweitere diese Pipeline mit einem `head` Befehl, um nur den ersten Zahnnamen auszuwählen.

`@hint`
Kopiere den Code in der Anleitung, füge eine Pipe an und rufe dann `head` mit dem `-n` Flag auf.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth | head -n 1
```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    # for some reason has_expr_output with strict=True does not work here...
    has_output('^\s*canine\s*$', incorrect_msg = "Haben Sie `|` verwendet, um die Pipeline mit einem `head`-Befehl zu erweitern? Stellen Sie sicher, dass das `-n`-Flag korrekt gesetzt ist."),
    # by coincidence, tail -n 1 returns the same as head -n 1, so check that head was called
    has_code("head", "Haben Sie `|` verwendet, um die Pipeline mit einem `head`-Befehl zu erweitern?")
)
Ex().success_msg("Fröhliches Verketten! Durch das Verketten mehrerer Befehle können Sie leistungsstarke Datenmanipulations-Pipelines erstellen.")

```

---

## Wie kann ich die Datensätze in einer Datei zählen?

```yaml
type: ConsoleExercise
key: ae6a48d6aa
xp: 100
```

Der Befehl `wc` (kurz für "word count") gibt die Anzahl der **Zeichen**, **Wörter**und **Zeilen**in einer Datei aus.
Mit `-c`, `-w` bzw. `-l` kannst du nur eines davon drucken lassen.

`@instructions`
Zähle, wie viele Datensätze in `seasonal/spring.csv` ein Datum im Juli 2017 haben (`2017-07`). 
- Verwende dazu `grep` mit einem Teildatum, um die Zeilen auszuwählen, und leite das Ergebnis in `wc` mit einem entsprechenden Flag, um die Zeilen zu zählen.

`@hint`
- Verwende `head seasonal/spring.csv`, um dich an das Datumsformat zu erinnern.
- Der erste Teil des Befehls hat die Form `grep thing_to_match filename`.
- Nach der Pipe, `|`, rufst du `wc` mit dem Flag `-l` auf.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
grep 2017-07 seasonal/spring.csv | wc -l
```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    has_expr_output(strict=True),
    multi(
      has_code("grep", incorrect_msg = "Haben Sie `grep` aufgerufen?"),
      has_code("2017-07", incorrect_msg = "Haben Sie nach `2017-07` gesucht?"),
      has_code("seasonal/spring.csv", incorrect_msg = "Haben Sie die Datei `seasonal/spring.csv` durchsucht?"),
      has_code("|", incorrect_msg = "Haben Sie mit `|` zu `wc` gepiped?"),      
      has_code("wc", incorrect_msg = "Haben Sie `wc` aufgerufen?"),
      has_code("-l", incorrect_msg = "Haben Sie Zeilen mit `-l` gezählt?")
    )
  )
)
Ex().success_msg("Sorgfältiges Zählen! Zu bestimmen, wie viele Daten Sie haben, ist ein großartiger erster Schritt in jeder Datenanalyse.")

```

---

## Wie kann ich viele Dateien auf einmal angeben?

```yaml
type: ConsoleExercise
key: 602d47e70c
xp: 100
```

Die meisten Shell-Befehle funktionieren mit mehreren Dateien, wenn du ihnen mehrere Dateinamen gibst.
Ein Beispiel:
kannst du die erste Spalte aus allen saisonalen Datendateien auf einmal wie folgt abrufen:

```{shell}
cut -d , -f 1 seasonal/winter.csv seasonal/spring.csv seasonal/summer.csv seasonal/autumn.csv
```

Aber es ist keine gute Idee, die Namen vieler Dateien immer wieder einzutippen:
es verschwendet Zeit,
und früher oder später wirst du entweder eine Datei auslassen oder den Namen einer Datei wiederholen.
Um dein Leben besser zu machen,
Die Shell erlaubt es dir, **Platzhalter** zu verwenden, um eine Liste von Dateien mit einem einzigen Ausdruck anzugeben.
Der häufigste Platzhalter ist `*`,
was so viel bedeutet wie: "Stimme mit null oder mehr Zeichen überein".
Benutze es,
können wir den obigen `cut` Befehl wie folgt abkürzen:

```{shell}
cut -d , -f 1 seasonal/*
```

oder:

```{shell}
cut -d , -f 1 seasonal/*.csv
```

`@instructions`
Schreibe einen einzigen Befehl mit `head`, um die ersten drei Zeilen aus `seasonal/spring.csv` und `seasonal/summer.csv` zu erhalten, also insgesamt sechs Zeilen Daten, aber *nicht* aus den Herbst- oder Winterdateien.
Verwende einen Platzhalter, anstatt die Namen der Dateien vollständig auszuschreiben.

`@hint`
- Der Befehl hat die Form `head -n number_of_lines filename_pattern`.
- Du könntest z.B. Dateien im Verzeichnis `a`, beginnend mit `b`, mit `a/b*` abgleichen.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
head -n 3 seasonal/s* # ...or seasonal/s*.csv, or even s*/s*.csv
```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_expr_output(incorrect_msg = "Sie können `seasonal/s*` verwenden, um `seasonal/spring.csv` und `seasonal/summer.csv` auszuwählen. Stellen Sie sicher, dass Sie nur die ersten drei Zeilen jeder Datei mit dem `-n`-Flag einbeziehen!"),
    check_not(has_output('==> seasonal/autumn.csv <=='), incorrect_msg = "Schließen Sie die Ausgabe für `seasonal/autumn.csv` nicht ein. Sie können `seasonal/s*` verwenden, um `seasonal/spring.csv` und `seasonal/summer.csv` auszuwählen"),
    check_not(has_output('==> seasonal/winter.csv <=='), incorrect_msg = "Schließen Sie die Ausgabe für `seasonal/winter.csv` nicht ein. Sie können `seasonal/s*` verwenden, um `seasonal/spring.csv` und `seasonal/summer.csv` auszuwählen")
)
Ex().success_msg("Wildes Wildcard-Arbeiten! Dies wird noch wichtiger, wenn Ihr Verzeichnis Hunderte oder Tausende von Dateien enthält.")

```

---

## Welche anderen Platzhalter kann ich verwenden?

```yaml
type: PureMultipleChoiceExercise
key: f8feeacd8c
xp: 50
```

Die Shell hat auch andere Platzhalter,
obwohl sie weniger häufig verwendet werden:

- `?` passt auf ein einzelnes Zeichen, also passt `201?.txt` auf `2017.txt` oder `2018.txt`, aber nicht auf `2017-01.txt`.
- `[...]` passt auf ein beliebiges Zeichen innerhalb der eckigen Klammern, also passt `201[78].txt` auf `2017.txt` oder `2018.txt`, aber nicht auf `2016.txt`.
- `{...}` passt auf jedes der kommagetrennten Muster innerhalb der geschweiften Klammern, also passt `{*.txt, *.csv}` auf jede Datei, deren Name mit `.txt` oder `.csv` endet, aber nicht auf Dateien, deren Namen mit `.pdf` enden.

<hr/>

Welcher Ausdruck würde auf `singh.pdf` und `johel.txt` passen, aber *nicht* auf `sandhu.pdf` oder `sandhu.txt`?

`@hint`
Vergleiche jeden Ausdruck mit jedem Dateinamen der Reihe nach.

`@possible_answers`
- `[sj]*.{.pdf, .txt}`
- `{s*.pdf, j*.txt}`
- `[singh,johel]{*.pdf, *.txt}`
- [`{singh.pdf, j*.txt}`]

`@feedback`
- Nein: `.pdf` und `.txt` sind keine Dateinamen.
- Nein: Dies wird `sandhu.pdf` entsprechen.
- Nein: Der Ausdruck in eckigen Klammern passt nur auf ein Zeichen, nicht auf ganze Wörter.
- Richtig!

---

## Wie kann ich Textzeilen sortieren?

```yaml
type: ConsoleExercise
key: f06d9e310e
xp: 100
```

Wie der Name schon sagt,
`sort` bringt die Daten in Ordnung.
Standardmäßig geschieht dies in aufsteigender alphabetischer Reihenfolge,
aber die Flags `-n` und `-r` können verwendet werden, um numerisch zu sortieren und die Reihenfolge der Ausgabe umzukehren,
während `-b` anweist, führende Leerzeichen zu ignorieren
und `-f` weist es an, **die**Groß- und Kleinschreibung **zu ignorieren**(d.h. zu **falten**).
Pipelines verwenden häufig `grep`, um unerwünschte Datensätze loszuwerden
und dann `sort`, um die restlichen Datensätze in die richtige Reihenfolge zu bringen.

`@instructions`
Erinnerst du dich an die Kombination von `cut` und `grep`, um alle Zahnnamen aus Spalte 2 von `seasonal/summer.csv` auszuwählen?

```
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth
```

Ausgehend von diesem Rezept sortierst du die Namen der Zähne in `seasonal/winter.csv` (nicht `summer.csv`) in absteigender alphabetischer Reihenfolge. Dazu erweiterst du die Pipeline um einen `sort` Schritt.

`@hint`
Kopiere den Befehl in der Anleitung, ändere den Dateinamen, füge eine Pipe an und rufe dann `sort` mit dem Flag `-r` auf.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
cut -d , -f 2 seasonal/winter.csv | grep -v Tooth | sort -r
```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    has_expr_output(strict=True),
    multi(
      has_code("cut", incorrect_msg = "Haben Sie `cut` aufgerufen?"),
      has_code("-d", incorrect_msg = "Haben Sie einen Feldtrennzeichen mit `-d` angegeben?"),
      has_code("seasonal/winter.csv", incorrect_msg = "Haben Sie Daten aus der Datei `seasonal/winter.csv` geholt?"),
      has_code("|", incorrect_msg = "Haben Sie von `cut` zu `grep` zu `sort` mit `|` gepiped?"),      
      has_code("grep", incorrect_msg = "Haben Sie `grep` aufgerufen?"),
      has_code("-v", incorrect_msg = "Haben Sie die Übereinstimmung mit `-v` invertiert?"),
      has_code("Tooth", incorrect_msg = "Haben Sie nach `Tooth` gesucht?"),
      has_code("sort", incorrect_msg = "Haben Sie `sort` aufgerufen?"),
      has_code("-r", incorrect_msg = "Haben Sie die Sortierreihenfolge mit `-r` umgekehrt?")
    )
  )
)
Ex().success_msg("Sortiert! `sort` hat viele Verwendungsmöglichkeiten. Zum Beispiel zeigt das Piping von `sort -n` zu `head` die größten Werte an.")

```

---

## Wie kann ich doppelte Zeilen entfernen?

```yaml
type: ConsoleExercise
key: ed77aed337
xp: 100
```

Ein weiterer Befehl, der oft mit `sort` verwendet wird, ist `uniq`,
dessen Aufgabe es ist, doppelte Zeilen zu entfernen.
Um genau zu sein,
werden *benachbarte* doppelte Zeilen entfernt.
Wenn eine Datei enthält:

```
2017-07-03
2017-07-03
2017-08-03
2017-08-03
```

dann wird `uniq` produzieren:

```
2017-07-03
2017-08-03
```

aber wenn es enthält:

```
2017-07-03
2017-08-03
2017-07-03
2017-08-03
```

dann wird `uniq` alle vier Zeilen ausdrucken.
Der Grund dafür ist, dass `uniq` für die Arbeit mit sehr großen Dateien ausgelegt ist.
Um nicht benachbarte Zeilen aus einer Datei zu entfernen,
es müsste die gesamte Datei im Speicher behalten
(oder zumindest,
alle bisher gesehenen einzigartigen Linien).
Indem du nur benachbarte Duplikate entfernst,
es muss nur die letzte eindeutige Zeile im Speicher behalten.

`@instructions`
Schreibe eine Pipeline zu:

- hole die zweite Spalte von `seasonal/winter.csv`,
- das Wort "Zahn" aus der Ausgabe entfernen, damit nur die Zahnnamen angezeigt werden,
- die Ausgabe so sortieren, dass alle Vorkommen eines bestimmten Zahnnamens nebeneinander liegen; und
- zeige jeden Zahnnamen einmal an, zusammen mit der Anzahl, wie oft er vorkommt.

Der Beginn deiner Pipeline ist derselbe wie bei der vorherigen Übung:

```
cut -d , -f 2 seasonal/winter.csv | grep -v Tooth
```

Erweitere sie um den Befehl `sort` und verwende `uniq -c`, um einzelne Zeilen mit einer Zählung der Häufigkeit ihres Auftretens anzuzeigen, anstatt `uniq` und `wc` zu verwenden.

`@hint`
Kopiere den Befehl in der Anleitung und füge ihn ein, leite ihn ohne Flags an `sort` weiter und leite ihn dann mit einem `-c` Flag an `uniq` weiter.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
cut -d , -f 2 seasonal/winter.csv | grep -v Tooth | sort | uniq -c
```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_correct(
        has_expr_output(),
        multi(
            has_code('cut\s+-d\s+,\s+-f\s+2\s+seasonal/winter.csv\s+\|\s+grep\s+-v\s+Tooth',
                     incorrect_msg="Sie sollten mit diesem Befehl beginnen: `cut -d , -f 2 seasonal/winter.csv | grep -v Tooth`. Jetzt erweitern Sie ihn!"),
            has_code('\|\s+sort', incorrect_msg="Haben Sie den Befehl mit `| sort` erweitert?"),
            has_code('\|\s+uniq', incorrect_msg="Haben Sie den Befehl mit `| uniq` erweitert?"),
            has_code('-c', incorrect_msg="Haben Sie die Zählungen mit `-c` eingeschlossen?")
        )
    )
)
Ex().success_msg("Großartig! Nach all dieser Arbeit an einer Pipe wäre es schön, wenn wir das Ergebnis speichern könnten, oder?")

```

---

## Wie kann ich die Ausgabe einer Pipe speichern?

```yaml
type: MultipleChoiceExercise
key: 4115aa25b2
xp: 50
```

Mit der Shell können wir die Ausgabe einer Reihe von Piping-Befehlen umleiten:

```{shell}
cut -d , -f 2 seasonal/*.csv | grep -v Tooth > teeth-only.txt
```

Allerdings muss `>` am Ende der Pipeline erscheinen:
wenn wir versuchen, sie in der Mitte zu verwenden, wie hier:

```{shell}
cut -d , -f 2 seasonal/*.csv > teeth-only.txt | grep -v Tooth
```

dann wird die gesamte Ausgabe von `cut` auf `teeth-only.txt` geschrieben,
so dass nichts mehr übrig ist für `grep`
und es wartet ewig auf eine Eingabe.

<hr>

Was passiert, wenn wir die Umleitung an den Anfang einer Pipeline stellen, wie in:

```{shell}
> result.txt head -n 3 seasonal/winter.csv
```

`@possible_answers`
- [Die Ausgabe des Befehls wird wie üblich in die Datei umgeleitet.]
- Die Shell meldet dies als Fehler.
- Die Shell wartet ewig auf Eingaben.

`@hint`
Probiere es in der Schale aus.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
Ex().has_chosen(1, ['Richtig!', 'Nein; die Shell kann dies tatsächlich ausführen.', 'Nein; die Shell kann dies tatsächlich ausführen.'])
```

---

## Wie kann ich ein laufendes Programm anhalten?

```yaml
type: ConsoleExercise
key: d1694dbdcd
xp: 100
```

Die Befehle und Skripte, die du bis jetzt ausgeführt hast, wurden alle schnell ausgeführt,
aber manche Aufgaben brauchen Minuten, Stunden oder sogar Tage, um erledigt zu werden.
Es kann auch vorkommen, dass du die Umleitung fälschlicherweise in die Mitte einer Pipeline setzt,
was dazu führt, dass er sich aufhängt.
Wenn du beschließt, dass du nicht willst, dass ein Programm weiterläuft,
kannst du `Ctrl` + `C` eingeben, um sie zu beenden.
In der Unix-Dokumentation steht das oft unter `^C`;
beachte, dass das "c" klein geschrieben werden kann.

`@instructions`
Führe den Befehl aus:

```{shell}
head
```

ohne Argumente (so dass er auf Eingaben wartet, die nie kommen werden)
und beende es dann, indem du `Ctrl` + `C` eingibst.

`@hint`
Gib einfach head ein, drücke Enter und beende das laufende Programm mit `Ctrl` + `C`.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
# Simply type head, hit Enter and exit the running program with `Ctrl` + `C`.
```

`@sct`
```{python}
Ex().has_code(r'\s*head\s*', fixed=False, incorrect_msg="Haben Sie `head` verwendet?")
```

---

## Einpacken

```yaml
type: BulletConsoleExercise
key: 659d3caa48
xp: 100
```

Um es kurz zu machen,
erstellst du eine Pipeline, um herauszufinden, wie viele Datensätze in der kürzesten der saisonalen Datendateien enthalten sind.

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: b1f9c8ff84
xp: 35
```

`@instructions`
Verwende `wc` mit den entsprechenden Parametern, um die Anzahl der Zeilen in allen saisonalen Datendateien aufzulisten.
(Verwende einen Platzhalter für die Dateinamen, anstatt sie alle von Hand einzutippen).

`@hint`
Verwende `-l`, um nur die Zeilen aufzulisten, und `*`, um die Dateinamen abzugleichen.

`@solution`
```{shell}
wc -l seasonal/*.csv

```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    has_expr_output(strict=True),
    multi(
      has_code("wc", incorrect_msg = "Haben Sie `wc` aufgerufen?"),
      has_code("-l", incorrect_msg = "Haben Sie die Anzahl der Zeilen mit `-l` gezählt?"),
      has_code("seasonal/\*", incorrect_msg = "Haben Sie Daten aus allen `seasonal/*` Dateien geholt?")
    )
  )
)
```

***

```yaml
type: ConsoleExercise
key: 7f94acc679
xp: 35
```

`@instructions`
Füge einen weiteren Befehl an den vorherigen an und verwende eine Pipe, um die Zeile mit dem Wort "total" zu entfernen.

`@hint`


`@solution`
```{shell}
wc -l seasonal/*.csv | grep -v total

```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    has_expr_output(strict=True),
    multi(
      has_code("wc", incorrect_msg = "Haben Sie `wc` aufgerufen?"),
      has_code("-l", incorrect_msg = "Haben Sie die Anzahl der Zeilen mit `-l` gezählt?"),
      has_code("seasonal/\*", incorrect_msg = "Haben Sie Daten aus allen `seasonal/*` Dateien geholt?"),
      has_code("|", incorrect_msg = "Haben Sie von `wc` zu `grep` mit `|` gepiped?"),      
      has_code("grep", incorrect_msg = "Haben Sie `grep` aufgerufen?"),
      has_code("-v", incorrect_msg = "Haben Sie die Übereinstimmung mit `-v` invertiert?"),
      has_code("total", incorrect_msg = "Haben Sie nach `total` gesucht?")
    )
  )
)

```

***

```yaml
type: ConsoleExercise
key: c5f55bff6b
xp: 30
```

`@instructions`
Füge zwei weitere Stufen zur Pipeline hinzu, die `sort -n` und `head -n 1` verwenden, um die Datei mit den wenigsten Zeilen zu finden.

`@hint`
- Verwende `sort`'s `-n` Flag, um numerisch zu sortieren.
- Verwende `head`'s `-n` Flag, um nur 1 Zeile zu behalten.

`@solution`
```{shell}
wc -l seasonal/*.csv | grep -v total | sort -n | head -n 1

```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    has_expr_output(strict=True),
    multi(
      has_code("wc", incorrect_msg = "Haben Sie `wc` aufgerufen?"),
      has_code("-l", incorrect_msg = "Haben Sie die Anzahl der Zeilen mit `-l` gezählt?"),
      has_code("seasonal/\*", incorrect_msg = "Haben Sie Daten aus allen `seasonal/*` Dateien geholt?"),
      has_code("|", incorrect_msg = "Haben Sie von `wc` zu `grep` zu `sort` zu `head` mit `|` gepiped?"),      
      has_code("grep", incorrect_msg = "Haben Sie `grep` aufgerufen?"),
      has_code("-v", incorrect_msg = "Haben Sie die Übereinstimmung mit `-v` invertiert?"),
      has_code("total", incorrect_msg = "Haben Sie nach `total` gesucht?"),
      has_code("sort", incorrect_msg = "Haben Sie `sort` aufgerufen?"),
      has_code("-n", incorrect_msg = "Haben Sie die Anzahl der zu behaltenden Zeilen mit `-n` angegeben?"),
      has_code("1", incorrect_msg = "Haben Sie 1 Zeile mit `-n 1` angegeben?")
    )
  )
)
Ex().success_msg("Großartig! Es stellt sich heraus, dass `autumn.csv` die Datei mit den wenigsten Zeilen ist. Eilen Sie zu Kapitel 4, um mehr über die Stapelverarbeitung zu erfahren!")

```
