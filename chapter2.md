---
title: Daten manipulieren
description: >-
  Mit den Befehlen, die du im vorherigen Kapitel gesehen hast, kannst du Dinge
  im Dateisystem verschieben. In diesem Kapitel erfährst du, wie du mit den
  Daten in diesen Dateien arbeiten kannst. Die Werkzeuge, die wir verwenden
  werden, sind ziemlich einfach, aber solide Bausteine.
lessons:
  - nb_of_exercises: 12
    title: Wie kann ich den Inhalt einer Datei einsehen?
---

## Wie kann ich den Inhalt einer Datei einsehen?

```yaml
type: ConsoleExercise
key: 8acc09ede3
xp: 100
```

Bevor du Dateien umbenennst oder löschst,
kannst du einen Blick auf ihren Inhalt werfen.
Am einfachsten geht das mit `cat`,
die nur den Inhalt von Dateien auf dem Bildschirm ausgibt.
(Der Name ist eine Abkürzung für "concatenate", was so viel bedeutet wie "Dinge miteinander verbinden",
da er alle Dateien, deren Namen du ihm gibst, nacheinander ausgibt).

```{shell}
cat agarwal.txt
```
```
name: Agarwal, Jasmine
position: RCT2
start: 2017-04-01
benefits: full
```

`@instructions`
Drucke den Inhalt von `course.txt` auf dem Bildschirm aus.

`@hint`


`@pre_exercise_code`
```{python}

```

`@solution`
```{bash}
cat course.txt
```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_expr_output(incorrect_msg="Ihr Befehl hat nicht die richtige Ausgabe erzeugt. Haben Sie `cat` gefolgt vom Namen der Datei, `course.txt`, verwendet?")
)
Ex().success_msg("Gut gemacht! Schauen wir uns andere Möglichkeiten an, den Inhalt einer Datei anzuzeigen.")
```

---

## Wie kann ich mir den Inhalt einer Datei Stück für Stück ansehen?

```yaml
type: ConsoleExercise
key: d8a30a3f81
xp: 100
```

Du kannst `cat` verwenden, um große Dateien zu drucken und dann durch die Ausgabe zu blättern,
aber es ist meist bequemer, die Ausgabe **auszublättern**.
Der ursprüngliche Befehl dafür hieß `more`,
aber er wurde durch einen leistungsfähigeren Befehl namens `less` ersetzt.
(Diese Art der Namensgebung ist das, was in der Unix-Welt als Humor gilt).
Wenn du eine Datei `less`,
Es wird immer nur eine Seite auf einmal angezeigt;
kannst du die Leertaste drücken, um nach unten zu blättern oder `q` eingeben, um zu beenden.

Wenn du `less` die Namen von mehreren Dateien gibst,
kannst du `:n` (Doppelpunkt und ein kleines 'n') eingeben, um zur nächsten Datei zu wechseln,
`:p` um zum vorherigen zurückzukehren,
oder `:q` zu kündigen.

Hinweis: Wenn du dir Lösungen zu Übungen ansiehst, die `less` verwenden,
siehst du am Ende einen zusätzlichen Befehl, der das Paging *ausschaltet*
damit wir deine Lösungen effizient testen können.

`@instructions`
Verwende `less seasonal/spring.csv seasonal/summer.csv`, um diese beiden Dateien in dieser Reihenfolge anzuzeigen.
Drücke die Leertaste, um nach unten zu blättern, `:n`, um zur zweiten Datei zu gehen, und `:q`, um zu beenden.

`@hint`


`@pre_exercise_code`
```{python}

```

`@solution`
```{bash}
# You can leave out the '| cat' part here:
less seasonal/spring.csv seasonal/summer.csv | cat
```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_or(
        has_code(r'\s*less\s+seasonal/spring\.csv\s+seasonal/summer\.csv\s*',
                 incorrect_msg='Verwenden Sie `less` und die Dateinamen. Denken Sie daran, dass `:n` Sie zur nächsten Datei bringt.'),
        has_code(r'\s*less\s+seasonal/summer\.csv\s+seasonal/spring\.csv\s*')
    )
)
```

---

## Wie kann ich mir den Anfang einer Datei ansehen?

```yaml
type: MultipleChoiceExercise
key: 82bdc9af65
lang: shell
xp: 50
skills:
  - 1
```

Das erste, was die meisten Datenwissenschaftler tun, wenn sie einen neuen Datensatz zu analysieren haben, ist
herausfinden, welche Felder er enthält und welche Werte diese Felder haben.
Wenn der Datensatz aus einer Datenbank oder Tabellenkalkulation exportiert wurde,
werden sie oft als **kommagetrennte Werte** gespeichert (CSV).
Ein schneller Weg, um herauszufinden, was er enthält, ist ein Blick auf die ersten paar Zeilen.

Wir können dies in der Shell mit einem Befehl namens `head` tun.
Wie der Name schon sagt,
Es druckt die ersten Zeilen einer Datei
(wobei "ein paar" 10 bedeutet),
also den Befehl:

```{shell}
head seasonal/summer.csv
```

Anzeigen:

```
Date,Tooth
2017-01-11,canine
2017-01-18,wisdom
2017-01-21,bicuspid
2017-02-02,molar
2017-02-27,wisdom
2017-02-27,wisdom
2017-03-07,bicuspid
2017-03-15,wisdom
2017-03-20,canine
```

<hr>

Was macht `head`, wenn es keine 10 Zeilen in der Datei gibt?
(Um das herauszufinden, schaue damit oben auf `people/agarwal.txt`.)

`@possible_answers`
- Eine Fehlermeldung ausgeben, weil die Datei zu kurz ist.
- Zeige so viele Zeilen an, wie es gibt.
- Zeige so viele Leerzeilen an, dass die Gesamtzahl 10 beträgt.

`@hint`
Was ist das Nützlichste, was es tun kann?

`@pre_exercise_code`
```{python}

```

`@sct`
```{shell}
Ex().has_chosen(2, ["Falsch: Das ist nicht das Nützlichste, was es tun könnte.",
                    "Richtig!",
                    "Falsch: Das wäre nicht von einer Datei zu unterscheiden, die mit einer Menge leerer Zeilen endet."])
```

---

## Wie kann ich weniger tippen?

```yaml
type: BulletConsoleExercise
key: 0b7b8ca8f7
xp: 100
```

Eines der wichtigsten Werkzeuge der Shell ist die **Registerkartenvervollständigung**.
Wenn du beginnst, den Namen einer Datei einzugeben und dann die Tabulatortaste drückst,
wird die Shell ihr Bestes tun, um den Pfad automatisch zu vervollständigen.
Ein Beispiel:
wenn du `sea` eingibst und die Tabulatortaste drückst,
wird der Verzeichnisname `seasonal/` (mit einem abschließenden Schrägstrich) eingegeben.
Wenn du dann `a` und Tab eintippst,
wird er den Pfad als `seasonal/autumn.csv` vervollständigen.

Wenn der Pfad zweideutig ist,
wie `seasonal/s`,
Wenn du ein zweites Mal die Tabulatortaste drückst, wird eine Liste mit Möglichkeiten angezeigt.
Gib ein oder zwei weitere Zeichen ein, um deinen Pfad zu präzisieren
und dann Tabulator drücken
wird den Rest des Namens ausfüllen.

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: 4e30296c27
xp: 50
```

`@instructions`
Führe `head seasonal/autumn.csv` aus, ohne den vollständigen Dateinamen einzugeben.

`@hint`
Tippe so viel von dem Pfad ein, wie du brauchst, drücke dann die Tabulatortaste und wiederhole.

`@solution`
```{shell}
head seasonal/autumn.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_expr_output(incorrect_msg="Der Prüfer konnte die richtige Ausgabe in Ihrem Befehl nicht finden. Sind Sie sicher, dass Sie `head` auf `seasonal/autumn.csv` aufgerufen haben?")
)
```

***

```yaml
type: ConsoleExercise
key: e249266733
xp: 50
```

`@instructions`
Führe `head seasonal/spring.csv` aus, ohne den vollständigen Dateinamen einzugeben.

`@hint`
Tippe so viel von dem Pfad ein, wie du brauchst, drücke dann die Tabulatortaste und wiederhole.

`@solution`
```{shell}
head seasonal/spring.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_expr_output(incorrect_msg="Der Prüfer konnte die richtige Ausgabe in Ihrem Befehl nicht finden. Sind Sie sicher, dass Sie `head` auf `seasonal/spring.csv` angewendet haben?")
)
Ex().success_msg("Gute Arbeit! Sobald Sie sich an die Verwendung der Tab-Vervollständigung gewöhnt haben, wird sie Ihnen viel Zeit sparen!")
```

---

## Wie kann ich kontrollieren, was die Befehle tun?

```yaml
type: ConsoleExercise
key: 9eb608f6c9
xp: 100
```

Du wirst dir nicht immer die ersten 10 Zeilen einer Datei ansehen wollen,
Mit der Shell kannst du also das Verhalten von `head` ändern.
indem du ihm ein **Kommandozeilen-Flag** (oder kurz "Flag") gibst.
Wenn du den Befehl ausführst:

```{shell}
head -n 3 seasonal/summer.csv
```

`head` werden nur die ersten drei Zeilen der Datei angezeigt.
Wenn du `head -n 100` ausführst,
werden die ersten 100 angezeigt (vorausgesetzt, es gibt so viele),
und so weiter.

Der Name einer Flagge gibt normalerweise ihren Zweck an
(zum Beispiel bedeutet `-n` "**Anzahl**der Zeilen").
Befehlsflaggen müssen nicht aus einem `-` gefolgt von einem einzelnen Buchstaben bestehen,
aber es ist eine weit verbreitete Konvention.

Hinweis: Es gehört zum guten Stil, alle Flaggen *vor den* Dateinamen zu setzen,
so in diesem Kurs,
wir akzeptieren nur Antworten, die das tun.

`@instructions`
Zeige die ersten 5 Zeilen von `winter.csv` im Verzeichnis `seasonal` an.

`@hint`


`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
head -n 5 seasonal/winter.csv
```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_correct(
        has_expr_output(incorrect_msg="Bist du sicher, dass du `head` auf die Datei `seasonal/winter.csv` anwendest?"),
        has_expr_output(strict=True, incorrect_msg="Bist du sicher, dass du die Option `-n 5` verwendet hast?")
    ),
    check_not(has_output("2017-02-17,incisor"), incorrect_msg = "Bist du sicher, dass du die Option `-n 5` verwendet hast?")
)
Ex().success_msg("Schön! Mit dieser Technik kannst du verhindern, dass deine Shell abstürzt, wenn du größere Textdateien ansehen möchtest.")
```

---

## Wie kann ich alles unterhalb eines Verzeichnisses auflisten?

```yaml
type: ConsoleExercise
key: f830d46419
xp: 100
```

Um alles zu sehen, was sich unter einem Verzeichnis befindet,
unabhängig davon, wie tief sie verschachtelt ist,
kannst du `ls` die Flagge geben `-R`
(was "rekursiv" bedeutet).
Wenn du `ls -R` in deinem Heimatverzeichnis verwendest,
siehst du so etwas wie das hier:

```
backup          course.txt      people          seasonal

./backup:

./people:
agarwal.txt

./seasonal:
autumn.csv      spring.csv      summer.csv      winter.csv
```

Hier werden alle Dateien und Verzeichnisse auf der aktuellen Ebene angezeigt,
dann alles in den einzelnen Unterverzeichnissen,
und so weiter.

`@instructions`
Damit du weißt, was was ist,
`ls` hat ein weiteres Flag `-F`, das ein `/` nach dem Namen jedes Verzeichnisses ausgibt
und ein `*` hinter dem Namen jedes ausführbaren Programms.
Führe `ls` mit den beiden Flags `-R` und `-F` und dem absoluten Pfad zu deinem Heimatverzeichnis aus
um alles zu sehen, was er enthält.
(Die Reihenfolge der Flaggen spielt keine Rolle, aber der Verzeichnisname muss an letzter Stelle stehen).

`@hint`
Dein Heimatverzeichnis kann mit `~` oder `.` oder dem absoluten Pfad angegeben werden.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
ls -R -F /home/repl
```

`@sct`
```{python}
Ex().check_or(
  has_expr_output(incorrect_msg='Verwenden Sie entweder `ls -R -F` oder `ls -F -R` und den Pfad `/home/repl`.'),
  has_expr_output(expr = "ls -R -F .", incorrect_msg='Verwenden Sie entweder `ls -R -F` oder `ls -F -R` und den Pfad `/home/repl`.')
)
Ex().success_msg("Das ist doch ein ziemlich übersichtlicher Überblick, oder?")

```

---

## Wie kann ich Hilfe für ein Kommando bekommen?

```yaml
type: BulletConsoleExercise
key: 7b90b8a7cd
xp: 100
```

Um herauszufinden, was Befehle bewirken,
wurde früher der Befehl `man` verwendet.
(kurz für "manuell").
Ein Beispiel:
Mit dem Befehl `man head` erhältst du diese Informationen:

```
HEAD(1)               BSD General Commands Manual              HEAD(1)

NAME

     head -- display first lines of a file


SYNOPSIS

     head [-n count | -c bytes] [file ...]


DESCRIPTION

     This filter displays the first count lines or bytes of each of

     the specified files, or of the standard input if no files are

     specified.  If count is omitted it defaults to 10.


     If more than a single file is specified, each file is preceded by

     a header consisting of the string ``==> XXX <=='' where ``XXX''

     is the name of the file.


SEE ALSO

     tail(1)

```

`man` ruft automatisch `less` auf,
Du musst also eventuell die Leertaste drücken, um durch die Informationen zu blättern.
und `:q` zu kündigen.

Die einzeilige Beschreibung unter `NAME` sagt dir kurz, was der Befehl macht,
und die Zusammenfassung unter `SYNOPSIS` listet alle Flaggen auf, die es versteht.
Alles, was optional ist, steht in eckigen Klammern `[...]`,
Entweder/Oder-Alternativen werden durch `|` getrennt,
und Dinge, die wiederholt werden können, werden von `...` angezeigt,
Die Handbuchseite von `head` sagt dir also, dass du *entweder* eine Zeilenzahl mit `-n`
oder eine Byte-Zählung mit `-c`,
und dass du ihm eine beliebige Anzahl von Dateinamen geben kannst.

Das Problem mit dem Unix-Handbuch ist, dass du wissen musst, wonach du suchst.
Wenn du es nicht tust,
kannst du [Stack Overflow](https://stackoverflow.com/) durchsuchen,
stelle eine Frage in den Slack-Kanälen von DataCamp,
oder sieh dir die `SEE ALSO` Abschnitte der Befehle an, die du bereits kennst.

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: 52d629048a
xp: 50
```

`@instructions`
Lies die Handbuchseite für den Befehl `tail`, um herauszufinden
was das Voranstellen des Zeichens `+` vor die mit der `-n` verwendete Zahl bewirkt.
(Denke daran, die Leertaste zu drücken, um nach unten zu blättern und/oder `q` einzugeben, um zu beenden).

`@hint`
Denk daran: `man` ist die Abkürzung für "manuell".

`@solution`
```{shell}
# Run the following command *without* '| cat':
man tail | cat

```

`@sct`
```{python}
Ex().has_code(r'\s*man\s+tail.*', incorrect_msg='Verwenden Sie `man` und den Befehlsnamen.')
```

***

```yaml
type: ConsoleExercise
key: 6a07958ae0
xp: 50
```

`@instructions`
Verwende `tail` mit dem Flag `-n +7`, um alle Zeilen *außer den* ersten sechs Zeilen von `seasonal/spring.csv` anzuzeigen.

`@hint`
Setze ein Pluszeichen "+" vor die Anzahl der Zeilen, die du anzeigen möchtest.

`@solution`
```{shell}
tail -n +7 seasonal/spring.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_output('2017-09-07,molar', incorrect_msg="Rufen Sie `tail` auf `seasonal/spring.csv` auf?"),
    has_expr_output(strict=True, incorrect_msg="Sind Sie sicher, dass Sie die Flagge `-n +7` verwendet haben?")
)
```

---

## Wie kann ich Spalten aus einer Datei auswählen?

```yaml
type: MultipleChoiceExercise
key: 925e9d645a
xp: 50
```

`head` und `tail` kannst du Zeilen aus einer Textdatei auswählen.
Wenn du Spalten auswählen möchtest,
kannst du den Befehl `cut` verwenden.
Es gibt verschiedene Optionen (benutze `man cut`, um sie zu erkunden),
aber die häufigste ist so etwas wie:

```{shell}
cut -f 2-5,8 -d , values.csv
```

Das bedeutet
" Wähle die Spalten 2 bis 5 und die Spalte 8 aus,
mit Komma als Trennzeichen".
`cut` verwendet `-f` (was "Felder" bedeutet), um Spalten anzugeben
und `-d` (bedeutet "Trennzeichen"), um das Trennzeichen anzugeben.
Letzteres musst du angeben, weil manche Dateien Leerzeichen, Tabulatoren oder Doppelpunkte zum Trennen von Spalten verwenden können.

<hr>

Welcher Befehl wählt die erste Spalte (mit den Daten) in der Datei `spring.csv` aus?

`@possible_answers`
- `cut -d , -f 1 seasonal/spring.csv`
- `cut -d, -f1 seasonal/spring.csv`
- Eine der oben genannten Möglichkeiten.
- Weder das eine noch das andere, denn `-f` muss vor `-d` kommen.

`@hint`
Die Reihenfolge der Flaggen spielt keine Rolle.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
Ex().has_chosen(3, ['Ja, aber das ist nicht alles', 'Ja, aber das ist nicht alles', 'Korrekt! Ein Leerzeichen nach der Flagge hinzuzufügen ist guter Stil, aber nicht zwingend erforderlich.', 'Nein, die Reihenfolge der Flags spielt keine Rolle'])
```

---

## Was kann man nicht schneiden?

```yaml
type: MultipleChoiceExercise
key: b9bb10ae87
xp: 50
```

`cut` ist ein einfältiger Befehl.
Im Besonderen,
er versteht keine Zeichenketten in Anführungszeichen.
Wenn deine Datei zum Beispiel lautet:

```
Name,Age
"Johel,Ranjit",28
"Sharma,Rupinder",26
```

dann:

```{shell}
cut -f 2 -d , everyone.csv
```

produzieren wird:

```
Age
Ranjit"
Rupinder"
```

und nicht das Alter eines jeden,
weil es das Komma zwischen Vor- und Nachnamen für ein Spaltentrennzeichen hält.

<hr>

Wie hoch ist die Leistung von `cut -d : -f 2-4` auf der Strecke:

```
first:second:third:
```

(Beachte den Doppelpunkt am Ende.)

`@possible_answers`
- `second`
- `second:third`
- `second:third:`
- Nichts von alledem, denn es gibt keine vier Felder.

`@hint`
Achte auf den nachgestellten Doppelpunkt.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
Ex().has_chosen(3, ['Nein, da ist mehr.', 'Nein, da ist mehr.', 'Korrekt! Der nachgestellte Doppelpunkt erzeugt ein viertes leeres Feld.', 'Nein, `cut` tut sein Bestes.'])
```

---

## Wie kann ich Befehle wiederholen?

```yaml
type: TabConsoleExercise
key: 32c0d30049
xp: 100
```

Einer der größten Vorteile der Verwendung der Shell ist, dass
Es macht es dir leicht, die Dinge wieder zu tun.
Wenn du einige Befehle ausführst,
kannst du die Pfeil-nach-oben-Taste drücken, um sie wieder zu durchlaufen.
Du kannst auch die Pfeiltasten nach links und rechts sowie die Löschtaste verwenden, um sie zu bearbeiten.
Wenn du Return drückst, wird der geänderte Befehl ausgeführt.

Noch besser ist, dass `history` eine Liste der Befehle ausgibt, die du kürzlich ausgeführt hast.
Jedem Befehl ist eine Seriennummer vorangestellt, damit du bestimmte Befehle leicht wiederholen kannst:
Gib einfach `!55` ein, um den 55. Befehl in deinem Verlauf erneut auszuführen (wenn du so viele hast).
Du kannst einen Befehl auch erneut ausführen, indem du ein Ausrufezeichen gefolgt von dem Namen des Befehls eingibst,
wie `!head` oder `!cut`,
der die letzte Verwendung des Befehls erneut ausführt.

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: 188a2fab38
xp: 20
```

`@instructions`
Führe `head summer.csv` in deinem Home-Verzeichnis aus (was fehlschlagen sollte).

`@hint`
Die Tabulatorvervollständigung funktioniert nicht, wenn es keinen passenden Dateinamen gibt.

`@solution`
```{shell}
head summer.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_code(r'\s*head\s+summer.csv\s*', incorrect_msg="Verwenden Sie `head` und einen Dateinamen, `summer.csv`. Machen Sie sich keine Sorgen, wenn es fehlschlägt. Es sollte so sein.")
)
```

***

```yaml
type: ConsoleExercise
key: cba6bf99a5
xp: 20
```

`@instructions`
Wechsle das Verzeichnis zu `seasonal`.

`@hint`
Denke daran, dass `cd` für "Verzeichnis wechseln" steht.

`@solution`
```{shell}
cd seasonal

```

`@sct`
```{python}
Ex().check_correct(
  has_cwd('/home/repl/seasonal'),
  has_code('cd +seasonal', incorrect_msg="Wenn Ihr aktuelles Arbeitsverzeichnis (finden Sie mit `pwd` heraus) `/home/repl` ist, können Sie mit `cd seasonal` in den Ordner `seasonal` wechseln.")
)
```

***

```yaml
type: ConsoleExercise
key: 74f5c8d2fc
xp: 20
```

`@instructions`
Führe den Befehl `head` mit `!head` erneut aus.

`@hint`
Gib keine Leerzeichen zwischen `!` und dem folgenden Text ein.

`@solution`
```{shell}
!head

```

`@sct`
```{python}
# !head is expanded into head summer.csv by the terminal, so manually specify expression
# This won't work for the validator though, so we have to use check_or to satisfy it.
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    check_or(
        has_expr_output(expr = 'head summer.csv',
                        incorrect_msg='Verwenden Sie `!head`, um den `head`-Befehl zu wiederholen.'),
        has_code('!head')
    )
)
```

***

```yaml
type: ConsoleExercise
key: a28555575a
xp: 20
```

`@instructions`
Nutze `history`, um zu sehen, was du getan hast.

`@hint`
Beachte, dass `history` die neuesten Befehle zuletzt anzeigt, damit sie auf deinem Bildschirm verbleiben, wenn das Programm beendet ist.

`@solution`
```{shell}
history

```

`@sct`
```{python}
Ex().has_code(r'history', incorrect_msg='Verwenden Sie `history` ohne Flags, um eine Liste der vorherigen Befehle zu erhalten.')
```

***

```yaml
type: ConsoleExercise
key: 0629b2adf3
xp: 20
```

`@instructions`
Führe `head` erneut aus, indem du `!` gefolgt von einer Befehlsnummer verwendest.

`@hint`
Gib *keine* Leerzeichen zwischen `!` und dem folgenden Text ein.

`@solution`
```{shell}
!3

```

`@sct`
```{python}
# !3 is expanded into head summer.csv by the terminal, so manually specify expression
# This won't work for the validator though, so we have to use check_or to satisfy it.
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    check_or(
        has_expr_output(expr = 'head summer.csv',
                        incorrect_msg='Haben Sie `!<eine_Zahl>` verwendet, um das letzte `head` aus der Historie erneut auszuführen?'),
        # The head cmd should appear twice, at positions 1 and 3, though this will change 
        # if the student typed a wrong answer.
        # Since we're also checking output, this should be niche enough to ignore.
        has_code(r'!3'),
        has_code(r'!1') 
    )
)
Ex().success_msg("Gut gemacht! Weiter zum nächsten!")

```

---

## Wie kann ich Zeilen mit bestimmten Werten auswählen?

```yaml
type: BulletConsoleExercise
key: adf1516acf
xp: 100
```

`head` und `tail` Zeilen auswählen,
`cut` wählt Spalten aus,
und `grep` wählt die Zeilen danach aus, was sie enthalten.
In seiner einfachsten Form,
`grep` nimmt ein Stück Text gefolgt von einem oder mehreren Dateinamen
und gibt alle Zeilen in diesen Dateien aus, die diesen Text enthalten.
Ein Beispiel:
`grep bicuspid seasonal/winter.csv`
druckt Zeilen aus `winter.csv`, die "bicuspid" enthalten.

`grep` kann auch nach Mustern suchen;
Diese werden wir im nächsten Kurs erkunden.
Was im Moment wichtiger ist, sind einige der häufigeren Flaggen von `grep`:

- `-c`: eine Zählung der übereinstimmenden Zeilen und nicht die Zeilen selbst ausgeben
- `-h`: die Namen der Dateien *nicht* ausdrucken, wenn mehrere Dateien durchsucht werden
- `-i`: Groß- und Kleinschreibung ignorieren (z.B. "Regression" und "regression" als Übereinstimmung behandeln)
- `-l`: Druckt die Namen der Dateien, die Übereinstimmungen enthalten, nicht die Übereinstimmungen
- `-n`: Zeilennummern für übereinstimmende Zeilen drucken
- `-v`: den Abgleich umkehren, d.h. nur Zeilen anzeigen, die *nicht* übereinstimmen

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: 0d7ef2baa0
xp: 35
```

`@instructions`
Druckt den Inhalt aller Zeilen aus, die das Wort `molar` in `seasonal/autumn.csv`
indem du einen einzigen Befehl in deinem Heimatverzeichnis ausführst. Verwende keine Flaggen.

`@hint`
Verwende `grep` mit dem Wort, nach dem du suchst, und dem Namen der Datei(en), in der du suchen willst.

`@solution`
```{shell}
grep molar seasonal/autumn.csv

```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    has_expr_output(),
    multi(
      has_code("grep", incorrect_msg = "Haben Sie `grep` aufgerufen?"),
      has_code("molar", incorrect_msg = "Haben Sie nach `molar` gesucht?"),
      has_code("seasonal/autumn.csv", incorrect_msg = "Haben Sie die Datei `seasonal/autumn.csv` durchsucht?")
    )
  )
)
```

***

```yaml
type: ConsoleExercise
key: a0eee34d1e
xp: 35
```

`@instructions`
Drehe die Übereinstimmung um, um alle Zeilen zu finden, die das Wort `molar` in `seasonal/spring.csv`*nicht* enthalten, und zeige ihre Zeilennummern.
Denke daran, dass es zum guten Stil gehört, alle Flaggen *vor* andere Werte wie Dateinamen oder den Suchbegriff "molar" zu setzen.

`@hint`


`@solution`
```{shell}
grep -v -n molar seasonal/spring.csv

```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    has_expr_output(),
    multi(
      has_code("grep", incorrect_msg = "Haben Sie `grep` aufgerufen?"),
      has_code("-v", incorrect_msg = "Haben Sie die Übereinstimmung mit `-v` invertiert?"),
      has_code("-n", incorrect_msg = "Haben Sie die Zeilennummern mit `-n` angezeigt?"),
      has_code("molar", incorrect_msg = "Haben Sie nach `molar` gesucht?"),
      has_code("seasonal/spring.csv", incorrect_msg = "Haben Sie die Datei `seasonal/spring.csv` durchsucht?")
    )
  )
)

```

***

```yaml
type: ConsoleExercise
key: f5641234fe
xp: 30
```

`@instructions`
Zähle, wie viele Zeilen das Wort `incisor` in `autumn.csv` und `winter.csv` zusammen enthalten.
(Auch hier führst du einen einzigen Befehl aus deinem Home-Verzeichnis aus).

`@hint`
Denke daran, `-c` mit `grep` zu verwenden, um Zeilen zu zählen.

`@solution`
```{shell}
grep -c incisor seasonal/autumn.csv seasonal/winter.csv

```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    has_expr_output(),
    multi(
      has_code("grep", incorrect_msg = "Haben Sie `grep` aufgerufen?"),
      has_code("-c", incorrect_msg = "Haben Sie die Anzahl mit `-c` ermittelt?"),
      has_code("incisor", incorrect_msg = "Haben Sie nach `incisor` gesucht?"),
      has_code("seasonal/autumn.csv", incorrect_msg = "Haben Sie die Datei `seasonal/autumn.csv` durchsucht?"),
      has_code("seasonal/winter.csv", incorrect_msg = "Haben Sie die Datei `seasonal/winter.csv` durchsucht?")
    )
  )
)
```

---

## Warum ist es nicht immer sicher, Daten als Text zu behandeln?

```yaml
type: MultipleChoiceExercise
key: 11914639fc
xp: 50
```

Der Abschnitt `SEE ALSO` auf der Handbuchseite für `cut` verweist auf einen Befehl namens `paste`
die verwendet werden kann, um Datendateien zu kombinieren, anstatt sie zu zerschneiden.

<hr>

Lies die Handbuchseite für `paste`,
und führe dann `paste` aus, um die Herbst- und Winterdateien in einer einzigen Tabelle zu kombinieren
mit einem Komma als Trennzeichen.
Was ist an der Ausgabe aus Sicht der Datenanalyse falsch?

`@possible_answers`
- Die Spaltenüberschriften werden wiederholt.
- Die letzten paar Zeilen haben die falsche Anzahl von Spalten.
- Einige der Daten von `winter.csv` fehlen.

`@hint`
Wenn du `cut` die Ausgabe von `paste` mit Kommas als Trennzeichen versiehst,
würde es die richtige Antwort ergeben?

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err1 = 'Stimmt, aber es ist nicht unbedingt ein Fehler.'
correct2 = 'Korrekt: Das Zusammenfügen der Zeilen mit Spalten erzeugt nur eine leere Spalte am Anfang, nicht zwei.'
err3 = 'Nein, alle Winterdaten sind vorhanden.'
Ex().has_chosen(2, [err1, correct2, err3])
```
