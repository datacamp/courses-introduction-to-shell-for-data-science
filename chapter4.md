---
title: Stapelverarbeitung
description: >-
  Die meisten Shell-Befehle verarbeiten viele Dateien auf einmal. In diesem
  Kapitel erfährst du, wie du deine eigenen Pipelines dazu machen kannst. Auf
  dem Weg dorthin wirst du sehen, wie die Shell Variablen verwendet, um
  Informationen zu speichern.
lessons:
  - nb_of_exercises: 10
    title: Wie speichert die Shell Informationen?
---

## Wie speichert die Shell Informationen?

```yaml
type: MultipleChoiceExercise
key: e4d5f4adea
xp: 50
```

Wie andere Programme auch, speichert die Shell Informationen in Variablen.
Einige davon,
genannt **Umgebungsvariablen**,
sind die ganze Zeit über verfügbar.
Die Namen von Umgebungsvariablen werden üblicherweise in Großbuchstaben geschrieben,
und einige der am häufigsten verwendeten sind unten aufgeführt.

| Variabel | Zweck                           | Wert                 |
|----------|-----------------------------------|-----------------------|
| `HOME`   | Heimatverzeichnis des Benutzers             | `/home/repl`          |
| `PWD `   | Aktuelles Arbeitsverzeichnis         | Dasselbe wie der Befehl `pwd`  |
| `SHELL`  | Welches Shell-Programm verwendet wird | `/bin/bash`           |
| `USER`   | Der Benutzer ID                         | `repl`                |

Um eine vollständige Liste zu erhalten (die ziemlich lang ist),
kannst du in der Shell `set` eingeben.

<hr>

Verwende `set` und `grep` mit einer Pipe, um den Wert von `HISTFILESIZE` anzuzeigen,
die bestimmt, wie viele alte Befehle in deinem Befehlsverlauf gespeichert werden.
Was ist ihr Wert?

`@possible_answers`
- 10
- 500
- [2000]
- Die Variable ist nicht vorhanden.

`@hint`
Verwende `set | grep HISTFILESIZE`, um die Zeile zu bekommen, die du brauchst.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err1 = "Nein: Die Shell speichert mehr Verlauf als das."
err2 = "Nein: Die Shell speichert mehr Verlauf als das."
correct3 = "Korrekt: Die Shell speichert standardmäßig 2000 alte Befehle auf diesem System."
err4 = "Nein: Die Variable `HISTFILESIZE` ist vorhanden."
Ex().has_chosen(3, [err1, err2, correct3, err4])
```

---

## Wie kann ich den Wert einer Variablen drucken?

```yaml
type: ConsoleExercise
key: afae0f33a7
xp: 100
```

Eine einfachere Methode, den Wert einer Variablen zu finden, ist der Befehl `echo`, der seine Argumente ausgibt. Tippen

```{shell}
echo hello DataCamp!
```

druckt

```
hello DataCamp!
```

Wenn du versuchst, damit den Wert einer Variablen zu drucken, wie hier:

```{shell}
echo USER
```

wird der Name der Variablen gedruckt, `USER`.

Um den Wert der Variable zu erhalten, musst du ein Dollarzeichen `$` vor die Variable setzen. Tippen 

```{shell}
echo $USER
```

druckt

```
repl
```

Das ist überall so:
um den Wert einer Variablen namens `X` zu erhalten,
musst du `$X` schreiben.
(So kann die Shell erkennen, ob du "eine Datei namens X" meinst.
oder "der Wert einer Variablen namens X").

`@instructions`
Die Variable `OSTYPE` enthält den Namen des Betriebssystems, das du verwendest.
Zeige seinen Wert mit `echo` an.

`@hint`
Rufe `echo` mit der Variable `OSTYPE` auf, der `$` vorangestellt ist.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
echo $OSTYPE
```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_correct(
        has_expr_output(strict = True),
        multi(
            has_code('echo', incorrect_msg="Haben Sie `echo` aufgerufen?"),
            has_code('OSTYPE', incorrect_msg="Haben Sie die Umgebungsvariable `OSTYPE` ausgegeben?"),
            has_code(r'\$OSTYPE', incorrect_msg="Stellen Sie sicher, dass `OSTYPE` mit einem `$` vorangestellt ist.")
        )
    )
)
Ex().success_msg("Ausgezeichnetes Echo der Umgebungsvariablen! Sie haben einen guten Start hingelegt. Machen wir weiter!")
```

---

## Wie speichert die Shell sonst noch Informationen?

```yaml
type: BulletConsoleExercise
key: e925da48e4
xp: 100
```

Die andere Art von Variablen wird **Shell-Variable** genannt,
was so etwas wie eine lokale Variable in einer Programmiersprache ist.

Um eine Shell-Variable zu erstellen,
weist du einem Namen einfach einen Wert zu:

```{shell}
training=seasonal/summer.csv
```

*ohne* Leerzeichen vor oder nach dem Zeichen `=`.
Sobald du dies getan hast,
kannst du den Wert der Variablen mit überprüfen:

```{shell}
echo $training
```
```
seasonal/summer.csv
```

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: 78f7fd446f
xp: 50
```

`@instructions`
Definiere eine Variable namens `testing` mit dem Wert `seasonal/winter.csv`.

`@hint`
Zwischen dem Variablennamen und dem Wert sollten *keine* Leerzeichen stehen.

`@solution`
```{shell}
testing=seasonal/winter.csv

```

`@sct`
```{python}
# For some reason, testing the shell variable directly always passes, so we can't do the following.
# Ex().multi(
#     has_cwd('/home/repl'),
#     has_expr_output(
#         expr='echo $testing',
#         output='seasonal/winter.csv',
#         incorrect_msg="Have you used `testing=seasonal/winter.csv` to define the `testing` variable?"
#     )
# )
Ex().multi(
    has_cwd('/home/repl'),
    multi(
        has_code('testing', incorrect_msg='Haben Sie eine Shell-Variable namens `testing` definiert?'),
        has_code('testing=', incorrect_msg='Haben Sie `=` direkt nach testing geschrieben, ohne Leerzeichen?'),
        has_code('=seasonal/winter\.csv', incorrect_msg='Haben Sie den Wert von `testing` auf `seasonal/winter.csv` gesetzt?')
    )
)
```

***

```yaml
type: ConsoleExercise
key: d5e7224f55
xp: 50
```

`@instructions`
Benutze `head -n 1 SOMETHING`, um die erste Zeile von `seasonal/winter.csv`
indem du den Wert der Variable `testing` anstelle des Dateinamens verwendest.

`@hint`
Denke daran, `$testing` zu verwenden und nicht nur `testing`
(die `$` wird benötigt, um den Wert der Variablen zu erhalten).

`@solution`
```{shell}
# We need to re-set the variable for testing purposes for this exercise
# you should only run "head -n 1 $testing"
testing=seasonal/winter.csv
head -n 1 $testing

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_code(r'\$testing', incorrect_msg="Haben Sie die Shell-Variable mit `$testing` referenziert?"),
    check_correct(
        has_output('^Date,Tooth\s*$'),
        multi(
            has_code('head', incorrect_msg="Haben Sie `head` aufgerufen?"),
            has_code('-n', incorrect_msg="Haben Sie die Anzahl der Zeilen mit `-n` begrenzt?"),
            has_code(r'-n\s+1', incorrect_msg="Haben Sie sich entschieden, 1 Zeile mit `-n 1` zu behalten?")     
        )
    )
)
Ex().success_msg("Stellar! Mal sehen, wie Sie Befehle einfach wiederholen können.")

```

---

## Wie kann ich einen Befehl viele Male wiederholen?

```yaml
type: ConsoleExercise
key: 920d1887e3
xp: 100
```

Shell-Variablen werden auch in **Schleifen** verwendet,
die Befehle viele Male wiederholen.
Wenn wir diesen Befehl ausführen:

```{shell}
for filetype in gif jpg png; do echo $filetype; done
```

es produziert:

```
gif
jpg
png
```

Beachte diese Dinge über die Schleife:

1. Die Struktur ist `for`...Variable... `in`...Liste... `; do`...Körper... `; done`
2. Die Liste der Dinge, die die Schleife verarbeiten soll (in unserem Fall die Wörter `gif`, `jpg`, und `png`).
3. Die Variable, die festhält, was die Schleife gerade bearbeitet (in unserem Fall `filetype`).
4. Der Körper der Schleife, die die Verarbeitung vornimmt (in unserem Fall `echo $filetype`).

Beachte, dass der Textkörper `$filetype` verwendet, um den Wert der Variablen zu erhalten, anstatt nur `filetype`,
genau wie bei jeder anderen Shell-Variablen.
Beachte auch, wo die Semikolons stehen:
die erste kommt zwischen der Liste und dem Schlüsselwort `do`,
und die zweite kommt zwischen dem Körper und dem Schlüsselwort `done`.

`@instructions`
Ändere die Schleife so, dass sie gedruckt wird:

```
docx
odt
pdf
```

Bitte benutze `filetype` als Namen für die Schleifenvariable.

`@hint`
Verwende die Codestruktur aus dem Einführungstext und tausche die Bilddateitypen gegen Dokumentendateitypen aus.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
for filetype in docx odt pdf; do echo $filetype; done
```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    has_expr_output(),
    multi(
      has_code('for', incorrect_msg='Haben Sie `for` aufgerufen?'),
      has_code('filetype', incorrect_msg='Haben Sie `filetype` als Schleifenvariable verwendet?'),
      has_code('in', incorrect_msg='Haben Sie `in` vor der Liste der Dateitypen verwendet?'),
      has_code('docx odt pdf', incorrect_msg='Haben Sie über `docx`, `odt` und `pdf` in dieser Reihenfolge iteriert?'),
      has_code(r'pdf\s*;', incorrect_msg='Haben Sie ein Semikolon nach dem letzten Schleifenelement gesetzt?'),
      has_code(r';\s*do', incorrect_msg='Haben Sie `do` nach dem ersten Semikolon verwendet?'),
      has_code('echo', incorrect_msg='Haben Sie `echo` aufgerufen?'),
      has_code(r'\$filetype', incorrect_msg='Haben Sie `$filetype` ausgegeben?'),
      has_code(r'filetype\s*;', incorrect_msg='Haben Sie ein Semikolon nach dem Schleifenkörper gesetzt?'),
      has_code('; done', incorrect_msg='Haben Sie mit `done` abgeschlossen?')
    )
  )
)
Ex().success_msg("Erstklassig für Schleifen! Schleifen sind genial, wenn Sie dasselbe Hunderte oder Tausende Male tun möchten.")

```

---

## Wie kann ich einen Befehl für jede Datei einmal wiederholen?

```yaml
type: ConsoleExercise
key: 8468b70a71
xp: 100
```

Du kannst die Namen der Dateien, die du verarbeiten willst, immer beim Schreiben der Schleife eingeben,
aber normalerweise ist es besser, Wildcards zu verwenden.
Versuche, diese Schleife in der Konsole auszuführen:

```{shell}
for filename in seasonal/*.csv; do echo $filename; done
```

Er druckt:

```
seasonal/autumn.csv
seasonal/spring.csv
seasonal/summer.csv
seasonal/winter.csv
```

weil die Shell `seasonal/*.csv` als eine Liste von vier Dateinamen expandiert
bevor es die Schleife durchläuft.

`@instructions`
Ändere den Platzhalterausdruck in `people/*`
damit die Schleife die Namen der Dateien im Verzeichnis `people` ausgibt
unabhängig davon, welches Suffix sie haben oder nicht haben.
Bitte benutze `filename` als Namen für deine Schleifenvariable.

`@hint`


`@pre_exercise_code`
```{python}

```

`@solution`
```{bash}
for filename in people/*; do echo $filename; done
```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    has_expr_output(),
    multi(
      has_code('for', incorrect_msg='Haben Sie `for` aufgerufen?'),
      has_code('filename', incorrect_msg='Haben Sie `filename` als Schleifenvariable verwendet?'),
      has_code('in', incorrect_msg='Haben Sie `in` vor der Liste der Dateitypen verwendet?'),
      has_code('people/\*', incorrect_msg='Haben Sie eine Liste von Dateien mit `people/*` angegeben?'),
      has_code(r'people/\*\s*;', incorrect_msg='Haben Sie ein Semikolon nach der Liste der Dateien gesetzt?'),
      has_code(r';\s*do', incorrect_msg='Haben Sie `do` nach dem ersten Semikolon verwendet?'),
      has_code('echo', incorrect_msg='Haben Sie `echo` aufgerufen?'),
      has_code(r'\$filename', incorrect_msg='Haben Sie `$filename` ausgegeben?'),
      has_code(r'filename\s*;', incorrect_msg='Haben Sie ein Semikolon nach dem Schleifenkörper gesetzt?'),
      has_code('; done', incorrect_msg='Haben Sie mit `done` abgeschlossen?')
    )
  )
)
Ex().success_msg("Schleifen! Platzhalter und Schleifen sind eine mächtige Kombination.")

```

---

## Wie kann ich die Namen einer Gruppe von Dateien aufzeichnen?

```yaml
type: MultipleChoiceExercise
key: 153ca10317
xp: 50
```

Oft wird eine Variable mit einem Platzhalterausdruck gesetzt, um eine Liste von Dateinamen aufzuzeichnen.
Ein Beispiel:
wenn du `datasets` so definierst:

```{shell}
datasets=seasonal/*.csv
```

kannst du die Namen der Dateien später anzeigen:

```{shell}
for filename in $datasets; do echo $filename; done
```

Das spart Tipparbeit und macht Fehler unwahrscheinlicher.

<hr>

Wenn du diese beiden Befehle in deinem Heimatverzeichnis ausführst,
wie viele Zeilen der Ausgabe werden gedruckt?

```{shell}
files=seasonal/*.csv
for f in $files; do echo $f; done
```

`@possible_answers`
- Keine: Da `files` in einer separaten Zeile definiert ist, hat es in der zweiten Zeile keinen Wert.
- Erstens: das Wort "Dateien".
- Vier: die Namen aller vier saisonalen Datendateien.

`@hint`
Erinnere dich daran, dass `X` für sich genommen nur "X" ist, während `$X` der Wert der Variablen `X` ist.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err1 = "Nein: Sie müssen keine Variable in derselben Zeile definieren, in der Sie sie verwenden."
err2 = "Nein: Dieses Beispiel definiert und verwendet die Variable `files` in derselben Shell."
correct3 = "Korrekt. Der Befehl ist gleichbedeutend mit `for f in seasonal/*.csv; do echo $f; done`."
Ex().has_chosen(3, [err1, err2, correct3])
```

---

## Der Name einer Variablen versus ihr Wert

```yaml
type: PureMultipleChoiceExercise
key: 4fcfb63c4f
xp: 50
```

Ein häufiger Fehler ist, dass du vergisst, `$` vor dem Namen einer Variablen zu verwenden.
Wenn du das tust,
die Shell verwendet den Namen, den du eingegeben hast
und nicht den Wert dieser Variablen.

Ein häufiger Fehler für erfahrene Benutzer ist es, den Namen der Variablen falsch zu schreiben.
Ein Beispiel:
wenn du `datasets` so definierst:

```{shell}
datasets=seasonal/*.csv
```

und tippe dann:

```{shell}
echo $datsets
```

druckt die Shell nichts aus,
denn `datsets` (ohne das zweite "a") ist nicht definiert.

<hr>

Wenn du diese beiden Befehle in deinem Home-Verzeichnis ausführen würdest,
Welche Ausgabe würde gedruckt werden?

```{shell}
files=seasonal/*.csv
for f in files; do echo $f; done
```

(Lies den ersten Teil der Schleife sorgfältig durch, bevor du antwortest).

`@hint`
Erinnere dich daran, dass `X` für sich genommen nur "X" ist, während `$X` der Wert der Variablen `X` ist.

`@possible_answers`
- [Eine Zeile: das Wort "Dateien".]
- Vier Zeilen: die Namen aller vier saisonalen Datendateien.
- Vier Leerzeilen: Der Variablen `f` ist kein Wert zugewiesen.

`@feedback`
- Richtig: Die Schleife verwendet `files` anstelle von `$files`, sodass die Liste aus dem Wort "Dateien" besteht.
- Nein: Die Schleife verwendet `files` anstelle von `$files`, so dass die Liste aus dem Wort "Dateien" und nicht aus der Erweiterung von `files` besteht.
- Nein: Die Variable `f` wird automatisch von der `for` Schleife definiert.

---

## Wie kann ich viele Befehle in einer einzigen Schleife ausführen?

```yaml
type: ConsoleExercise
key: 39b5dcf81a
xp: 100
```

Die Ausgabe von Dateinamen ist nützlich für die Fehlersuche,
aber der eigentliche Zweck von Schleifen ist es, Dinge mit mehreren Dateien zu tun.
Diese Schleife druckt die zweite Zeile jeder Datendatei aus:

```{shell}
for file in seasonal/*.csv; do head -n 2 $file | tail -n 1; done
```

Sie hat die gleiche Struktur wie die anderen Schleifen, die du bereits gesehen hast:
Der einzige Unterschied besteht darin, dass es sich um eine Pipeline aus zwei Befehlen handelt, anstatt um einen einzigen Befehl.

`@instructions`
Schreibe eine Schleife, die den letzten Eintrag vom Juli 2017 (`2017-07`) in jeder Saisondatei ausgibt. Es sollte ein ähnliches Ergebnis liefern wie:

```{shell}
grep 2017-07 seasonal/winter.csv | tail -n 1
```

aber für **_jede_** saisonale Datei separat. Bitte verwende `file` als Namen der Schleifenvariablen und denke daran, die Liste der Dateien `seasonal/*.csv` in einer Schleife zu durchlaufen_(statt "seasonal/winter.csv" wie im Beispiel_).

`@hint`
Der Schleifenkörper ist der in der Anleitung gezeigte grep-Befehl, wobei `seasonal/winter.csv` durch `$file` ersetzt wird.

`@pre_exercise_code`
```{python}

```

`@solution`
```{bash}
for file in seasonal/*.csv; do grep 2017-07 $file | tail -n 1; done
```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  # Enforce use of for loop, so students can't just use grep -h 2017-07 seasonal/*.csv
  has_code('for', incorrect_msg='Haben Sie `for` aufgerufen?'),
  check_correct(
    has_expr_output(),
    multi(
      has_code('file', incorrect_msg='Haben Sie `file` als Schleifenvariable verwendet?'),
      has_code('in', incorrect_msg='Haben Sie `in` vor der Liste der Dateien verwendet?'),
      has_code('seasonal/\*', incorrect_msg='Haben Sie eine Liste von Dateien mit `seasonal/*` angegeben?'),
      has_code(r'seasonal\/\*\.csv\s*;', incorrect_msg='Haben Sie ein Semikolon nach der Liste der Dateien gesetzt?'),
      has_code(r';\s*do', incorrect_msg='Haben Sie `do` nach dem ersten Semikolon verwendet?'),
      has_code('grep', incorrect_msg='Haben Sie `grep` aufgerufen?'),
      has_code('2017-07', incorrect_msg='Haben Sie auf `2017-07` abgeglichen?'),
      has_code(r'\$file', incorrect_msg='Haben Sie `$file` als Namen der Schleifenvariable verwendet?'),
      has_code(r'file\s*|', incorrect_msg='Haben Sie eine Pipe verwendet, um Ihren zweiten Befehl zu verbinden?'),
      has_code(r'tail\s*-n\s*1', incorrect_msg='Haben Sie `tail -n 1` verwendet, um den letzten Eintrag jeder Suche in Ihrem zweiten Befehl auszugeben?'),
      has_code('; done', incorrect_msg='Haben Sie mit `done` abgeschlossen?')
    )
  )
)

Ex().success_msg("Schleifen! Platzhalter und Schleifen sind eine mächtige Kombination.")

```

---

## Warum sollte ich keine Leerzeichen in Dateinamen verwenden?

```yaml
type: PureMultipleChoiceExercise
key: b974b7f45a
xp: 50
```

Es ist einfach und sinnvoll, Dateien Mehrwortnamen zu geben wie `July 2017.csv`
wenn du einen grafischen Dateiexplorer verwendest.
Allerdings
Dies führt zu Problemen, wenn du in der Shell arbeitest.
Ein Beispiel:
Angenommen, du möchtest `July 2017.csv` in `2017 July data.csv` umbenennen.
Du kannst nicht tippen:

```{shell}
mv July 2017.csv 2017 July data.csv
```

weil es für die Hülle so aussieht, als würdest du versuchen, dich zu bewegen
vier Dateien namens `July`, `2017.csv`, `2017` und `July` (wieder)
in ein Verzeichnis namens `data.csv`.
Stattdessen,
du musst die Namen der Dateien angeben
so dass die Shell jeden einzelnen Parameter als einen einzigen behandelt:

```{shell}
mv 'July 2017.csv' '2017 July data.csv'
```

<hr>

Wenn du zwei Dateien mit den Namen `current.csv` und `last year.csv`
(mit einem Leerzeichen in seinem Namen)
und du tippst:

```{shell}
rm current.csv last year.csv
```

was passieren wird:

`@hint`
Was würdest du denken, was passiert, wenn dir jemand den Befehl zeigt und du nicht weißt, welche Dateien existieren?

`@possible_answers`
- Die Shell wird eine Fehlermeldung ausgeben, weil `last` und `year.csv` nicht existieren.
- Die Shell wird `current.csv` löschen.
- [Beide der oben genannten.]
- Nichts.

`@feedback`
- Ja, aber das ist noch nicht alles.
- Ja, aber das ist noch nicht alles.
- Richtig. Du kannst einfache Anführungszeichen, `'`, oder doppelte Anführungszeichen, `"`, um die Dateinamen herum verwenden.
- Leider nicht.

---

## Wie kann ich viele Dinge in einer einzigen Schleife tun?

```yaml
type: MultipleChoiceExercise
key: f6d0530991
xp: 50
```

Die Schleifen, die du bisher gesehen hast, haben alle einen einzigen Befehl oder eine Pipeline in ihrem Körper,
aber eine Schleife kann eine beliebige Anzahl von Befehlen enthalten.
Um der Muschel zu sagen, wo eine endet und die nächste beginnt,
musst du sie mit Semikolons trennen:

```{shell}
for f in seasonal/*.csv; do echo $f; head -n 2 $f | tail -n 1; done
```

```
seasonal/autumn.csv
2017-01-05,canine
seasonal/spring.csv
2017-01-25,wisdom
seasonal/summer.csv
2017-01-11,canine
seasonal/winter.csv
2017-01-03,bicuspid
```

<hr>

Angenommen, du vergisst das Semikolon zwischen den Befehlen `echo` und `head` in der vorherigen Schleife,
damit du die Shell aufforderst, zu starten:

```{shell}
for f in seasonal/*.csv; do echo $f head -n 2 $f | tail -n 1; done
```

Was wird die Muschel tun?

`@possible_answers`
- Drucke eine Fehlermeldung.
- Gib für jede der vier Dateien eine Zeile aus.
- Drucke eine Zeile für `autumn.csv` (die erste Datei).
- Drucke die letzte Zeile jeder Datei aus.

`@hint`
Du kannst die Ausgabe von `echo` über eine Pipeline an `tail` weiterleiten.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err1 = "Nein: Die Schleife wird ausgeführt, sie wird nur nichts Sinnvolles tun."
correct2 = "Ja: `echo` erzeugt eine Zeile, die den Dateinamen zweimal enthält, die `tail` dann kopiert."
err3 = "Nein: Die Schleife läuft einmal für jeden der vier Dateinamen."
err4 = "Nein: Die Eingabe von `tail` ist die Ausgabe von `echo` für jeden Dateinamen."
Ex().has_chosen(2, [err1, correct2, err3, err4])
```
