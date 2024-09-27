---
title: Neue Werkzeuge erstellen
description: >-
  Mit dem Verlauf kannst du Dinge mit nur wenigen Tastendrücken wiederholen und
  mit Pipes kannst du bestehende Befehle kombinieren, um neue Befehle zu
  erstellen. In diesem Kapitel erfährst du, wie du noch einen Schritt weiter
  gehen und eigene Befehle erstellen kannst.
lessons:
  - nb_of_exercises: 9
    title: Wie kann ich eine Datei bearbeiten?
---

## Wie kann ich eine Datei bearbeiten?

```yaml
type: ConsoleExercise
key: 39eee3cfc0
xp: 100
```

Unix hat eine verwirrende Vielfalt an Texteditoren.
Für diesen Kurs,
werden wir ein einfaches Programm namens Nano verwenden.
Wenn du `nano filename` eingibst,
wird `filename` zur Bearbeitung geöffnet
(oder erstelle sie, wenn sie noch nicht existiert).
Du kannst dich mit den Pfeiltasten bewegen,
Zeichen mit der Rücktaste löschen,
und führe andere Operationen mit Steuerungstastenkombinationen durch:

- `Ctrl` + `K`: Lösche eine Zeile.
- `Ctrl` + `U`: Lösche eine Zeile.
- `Ctrl` + `O`: Speichere die Datei ("O" steht für "output"). _Du musst auch die Eingabetaste drücken, um den Dateinamen zu bestätigen!_
- `Ctrl` + `X`: Beende den Editor.

`@instructions`
Führe `nano names.txt` aus, um eine neue Datei in deinem Heimatverzeichnis zu bearbeiten
und gib die folgenden vier Zeilen ein:

```
Lovelace
Hopper
Johnson
Wilson
```

Um zu speichern, was du geschrieben hast,
Gib `Ctrl` + `O` ein, um die Datei auszuschreiben,
und dann Enter, um den Dateinamen zu bestätigen,
dann `Ctrl` + `X`, um den Editor zu verlassen.

`@hint`


`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/names.txt /home/repl
```

`@sct`
```{python}
patt = "Haben Sie die Zeile `%s` in die Datei `names.txt` eingefügt? Verwenden Sie `nano names.txt` erneut, um Ihre Datei zu aktualisieren. Verwenden Sie `Ctrl` + `O`, um zu speichern, und `Ctrl` + `X`, um zu beenden."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/names.txt').multi(
        has_code(r'Lovelace', incorrect_msg=patt%'Lovelace'),
        has_code(r'Hopper', incorrect_msg=patt%'Hopper'),
        has_code(r'Johnson', incorrect_msg=patt%'Johnson'),
        has_code(r'Wilson', incorrect_msg=patt%'Wilson')
    )
)
Ex().success_msg("Gut gemacht! Weiter zum nächsten!")

```

---

## Wie kann ich aufzeichnen, was ich gerade getan habe?

```yaml
type: BulletConsoleExercise
key: 80c3532985
xp: 100
```

Wenn du eine komplexe Analyse durchführst,
wirst du oft ein Protokoll über die verwendeten Befehle führen wollen.
Du kannst dies mit den Tools tun, die du bereits gesehen hast:

1. Run `history`.
2. Leite die Ausgabe an `tail -n 10` weiter (oder an die Anzahl der letzten Schritte, die du speichern möchtest).
3. Leite das in eine Datei um, die etwa `figure-5.history` heißt.

Das ist besser, als alles in ein Laborheft zu schreiben
weil er garantiert keinen Schritt verpasst.
Sie veranschaulicht auch die zentrale Idee der Schale:
einfache Werkzeuge, die Textzeilen produzieren und verbrauchen
können auf vielfältige Weise kombiniert werden
um eine breite Palette von Problemen zu lösen.

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: 144ca955ca
xp: 35
```

`@instructions`
Kopiere die Dateien `seasonal/spring.csv` und `seasonal/summer.csv` in dein Home-Verzeichnis.

`@hint`
Verwende `cp` zum Kopieren und `~` als Verknüpfung für den Pfad zu deinem Heimatverzeichnis.

`@solution`
```{shell}
cp seasonal/s* ~

```

`@sct`
```{python}
msg="Haben Sie `cp seasonal/s* ~` verwendet, um die erforderlichen Dateien in Ihr Home-Verzeichnis zu kopieren?"
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/spring.csv', missing_msg=msg).\
        has_code(r'2017-01-25,wisdom', incorrect_msg=msg),
    check_file('/home/repl/summer.csv', missing_msg=msg).\
        has_code(r'2017-01-11,canine', incorrect_msg=msg)
)
Ex().success_msg("Bemerkenswerte Aufzeichnungen! Wenn Sie irgendwelche Befehle falsch eingegeben haben, können Sie immer `nano` verwenden, um die gespeicherte Verlaufdatei danach zu bereinigen.")
```

***

```yaml
type: ConsoleExercise
key: 09a432e4df
xp: 35
```

`@instructions`
Verwende `grep` mit dem Flag `-h` (um die Ausgabe von Dateinamen zu verhindern)
und `-v Tooth` (um Zeilen auszuwählen, die *nicht* mit der Kopfzeile übereinstimmen)
um die Datensätze von `spring.csv` und `summer.csv` in dieser Reihenfolge auszuwählen
und leite die Ausgabe an `temp.csv` um.

`@hint`
Setze die Flaggen vor die Dateinamen.

`@solution`
```{shell}
grep -h -v Tooth spring.csv summer.csv > temp.csv

```

`@sct`
```{python}
msg1 = "Stellen Sie sicher, dass Sie die Ausgabe des `grep`-Befehls mit `>` auf `temp.csv` umleiten!"
msg2 = "Haben Sie `grep -h -v ___ ___ ___` (füllen Sie die Lücken aus) verwendet, um `temp.csv` zu füllen?"
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/temp.csv', missing_msg=msg1).multi(
        has_code(r'2017-08-04,canine', incorrect_msg=msg2),
        has_code(r'2017-03-14,incisor', incorrect_msg=msg2),
        has_code(r'2017-03-12,wisdom', incorrect_msg=msg2)
    )
)
```

***

```yaml
type: ConsoleExercise
key: c40348c1e5
xp: 30
```

`@instructions`
Rohr `history` in `tail -n 3`
und leiten die Ausgabe um an `steps.txt`
um die letzten drei Befehle in einer Datei zu speichern.
(Du musst drei statt nur zwei speichern
weil der Befehl `history` selbst in der Liste steht).

`@hint`
Denke daran, dass die Umleitung mit `>` am Ende der Abfolge von Piping-Befehlen steht.

`@solution`
```{shell}
history | tail -n 3 > steps.txt

```

`@sct`
```{python}
msg1="Stellen Sie sicher, dass Sie die Ausgabe Ihres Befehls in `steps.txt` umleiten."
msg2="Haben Sie `history | tail ___ ___` (füllen Sie die Lücken aus) verwendet, um `steps.txt` zu füllen?"
Ex().multi(
    has_cwd('/home/repl'),
    # When run by the validator, solution3 doesn't pass, so including a has_code for that
    check_or(
        check_file('/home/repl/steps.txt', missing_msg=msg1).multi(
            has_code(r'\s+1\s+', incorrect_msg=msg2),
            has_code(r'\s+3\s+history', incorrect_msg=msg2)
        ),
        has_code(r'history\s+|\s+tail\s+-n\s+4\s+>\s+steps\.txt')
    )
)
Ex().success_msg("Gut gemacht! Lassen Sie uns einen Schritt weiter gehen!")

```

---

## Wie kann ich Befehle speichern, um sie später erneut auszuführen?

```yaml
type: BulletConsoleExercise
key: 4507a0dbd8
xp: 100
```

Du hast die Shell bisher interaktiv genutzt.
Aber da die Befehle, die du eingibst, nur Text sind,
kannst du sie in Dateien speichern, damit die Shell sie immer wieder ausführen kann.
Um mit der Erkundung dieser mächtigen Fähigkeit zu beginnen,
Gib den folgenden Befehl in eine Datei namens `headers.sh` ein:

```{shell}
head -n 1 seasonal/*.csv
```

Dieser Befehl wählt die erste Zeile aus jeder der CSV Dateien im `seasonal` Verzeichnis aus.
Sobald du diese Datei erstellt hast,
kannst du es ausführen, indem du tippst:

```{shell}
bash headers.sh
```

Dies sagt der Shell (die nur ein Programm namens `bash` ist)
um die in der Datei `headers.sh` enthaltenen Befehle auszuführen,
was dieselbe Ausgabe erzeugt wie die direkte Ausführung der Befehle.

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: 316ad2fec6
xp: 50
```

`@instructions`
Verwenden Sie `nano dates.sh`, um eine Datei namens `dates.sh`
die diesen Befehl enthält:

```{shell}
cut -d , -f 1 seasonal/*.csv
```

um die erste Spalte aus allen CSV Dateien in `seasonal` zu extrahieren.

`@hint`
Füge die gezeigten Befehle ohne zusätzliche Leerzeilen oder Leerzeichen in die Datei ein.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/dates.sh ~

```

`@sct`
```{python}
msg = "Haben Sie die Zeile `cut -d , -f 1 seasonal/*.csv` in die Datei `dates.sh` eingefügt? Verwenden Sie `nano dates.sh` erneut, um Ihre Datei zu aktualisieren. Verwenden Sie `Ctrl` + `O`, um zu speichern, und `Ctrl` + `X`, um zu beenden."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/dates.sh').\
        has_code('cut -d *, *-f +1 +seasonal\/\*\.csv', incorrect_msg=msg)
)
```

***

```yaml
type: ConsoleExercise
key: 30a8fa953e
xp: 50
```

`@instructions`
Verwende `bash`, um die Datei `dates.sh` auszuführen.

`@hint`
Verwende `bash filename`, um die Datei auszuführen.

`@solution`
```{shell}
bash dates.sh

```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    has_expr_output(),
    multi(
      has_code("bash", incorrect_msg = 'Haben Sie `bash` aufgerufen?'),
      has_code("dates.sh", incorrect_msg = 'Haben Sie die Datei `dates.sh` angegeben?')
    )
  )
)
```

---

## Wie kann ich Rohre wiederverwenden?

```yaml
type: BulletConsoleExercise
key: da13667750
xp: 100
```

Eine Datei voller Shell-Befehle nennt man ein**\*Shell-Skript**,
oder manchmal auch nur ein "Skript". Skripte müssen keine Namen haben, die auf `.sh` enden,
aber diese Lektion wird diese Konvention verwenden
damit du den Überblick behältst, welche Dateien Skripte sind.

Skripte können auch Pipes enthalten.
Ein Beispiel:
wenn `all-dates.sh` diese Zeile enthält:

```{shell}
cut -d , -f 1 seasonal/*.csv | grep -v Date | sort | uniq
```

dann:

```{shell}
bash all-dates.sh > dates.out
```

extrahiert die eindeutigen Daten aus den saisonalen Datendateien
und speichere sie in `dates.out`.

`@pre_exercise_code`
```{python}
import shutil
shutil.copyfile('/solutions/teeth-start.sh', 'teeth.sh')
```

***

```yaml
type: ConsoleExercise
key: 6fae90f320
xp: 35
```

`@instructions`
Eine Datei `teeth.sh` in deinem Heimatverzeichnis wurde für dich vorbereitet, enthält aber einige Leerzeichen.
Verwende Nano, um die Datei zu bearbeiten und ersetze die beiden Platzhalter `____` 
mit `seasonal/*.csv` und `-c`, damit dieses Skript eine Zählung der
die Anzahl, wie oft jeder Zahnname in den CSV Dateien im `seasonal` Verzeichnis vorkommt.

`@hint`
Verwende `nano teeth.sh`, um die Datei zu bearbeiten.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/teeth.sh ~

```

`@sct`
```{python}
msg="Haben Sie die Lücken richtig ersetzt, sodass der Befehl in `teeth.sh` `cut -d , -f 2 seasonal/*.csv | grep -v Tooth | sort | uniq -c` lautet? Verwenden Sie `nano teeth.sh` erneut, um die erforderlichen Änderungen vorzunehmen."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/teeth.sh').\
        has_code(r'cut\s+-d\s+,\s+-f\s+2\s+seasonal/\*\.csv\s+\|\s+grep\s+-v\s+Tooth\s+\|\s+sort\s+\|\s+uniq\s+-c', incorrect_msg=msg)
)
```

***

```yaml
type: ConsoleExercise
key: dcfccb51e2
xp: 35
```

`@instructions`
Benutze `bash`, um `teeth.sh` auszuführen und `>`, um die Ausgabe an `teeth.out` umzuleiten.

`@hint`
Denke daran, dass `> teeth.out` *nach dem* Befehl kommen muss, der die Ausgabe erzeugt.

`@solution`
```{shell}
# We need to use 'cp' below to satisfy our automated tests.
# You should only use the last line that runs 'bash'.
cp /solutions/teeth.sh .
bash teeth.sh > teeth.out

```

`@sct`
```{python}
msg="Haben Sie das Ergebnis von `bash teeth.sh` mit `>` korrekt auf `teeth.out` umgeleitet?"
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    check_file('/home/repl/teeth.out').multi(
      has_code(r'31 canine', incorrect_msg=msg),
      has_code(r'17 wisdom', incorrect_msg=msg)
    ),
    multi(
      has_code("bash", incorrect_msg = 'Haben Sie `bash` aufgerufen?'),
      has_code("bash\s+teeth.sh", incorrect_msg = 'Haben Sie die Datei `teeth.sh` ausgeführt?'),
      has_code(">\s+teeth.out", incorrect_msg = 'Haben Sie auf die Datei `teeth.out` umgeleitet?')
    )
  )
)
```

***

```yaml
type: ConsoleExercise
key: c8c9a11e3c
xp: 30
```

`@instructions`
Führe `cat teeth.out` aus, um deine Ergebnisse zu überprüfen.

`@hint`
Denke daran, dass du die ersten paar Zeichen eines Dateinamens eingeben und dann die Tabulatortaste drücken kannst, um ihn automatisch zu vervollständigen.

`@solution`
```{shell}
cat teeth.out

```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    has_expr_output(),
    multi(
      has_code("cat", incorrect_msg = 'Haben Sie `cat` aufgerufen?'),
      has_code("teeth.out", incorrect_msg = 'Haben Sie die Datei `teeth.out` angegeben?')
    )
  )
)
Ex().success_msg("Schön! Das mag anfangs alles gekünstelt wirken, aber das Schöne ist, dass Sie Teile Ihres Workflows Schritt für Schritt automatisieren. Etwas, das als Data Scientist wirklich praktisch ist!")
```

---

## Wie kann ich Dateinamen an Skripte übergeben?

```yaml
type: BulletConsoleExercise
key: c2623b9c14
xp: 100
```

Ein Skript, das bestimmte Dateien verarbeitet, ist nützlich, um zu dokumentieren, was du getan hast, aber ein Skript, mit dem du jede beliebige Datei verarbeiten kannst, ist nützlicher.
Um dies zu unterstützen,
kannst du den speziellen Ausdruck `$@` (Dollarzeichen unmittelbar gefolgt von einem at-Zeichen) verwenden
für "alle dem Skript übergebenen Kommandozeilenparameter".

Wenn `unique-lines.sh` z.B. `sort $@ | uniq` enthält, kannst du es ausführen:

```{shell}
bash unique-lines.sh seasonal/summer.csv
```

ersetzt die Shell `$@` durch `seasonal/summer.csv` und verarbeitet eine Datei. Wenn du das machst:

```{shell}
bash unique-lines.sh seasonal/summer.csv seasonal/autumn.csv
```

verarbeitet er zwei Datendateien und so weiter.

_Zur Erinnerung: Um zu speichern, was du in Nano geschrieben hast, gibst du `Ctrl` + `O` ein, um die Datei auszuschreiben, dann Enter, um den Dateinamen zu bestätigen, und `Ctrl` + `X`, um den Editor zu verlassen._

`@pre_exercise_code`
```{python}
import shutil
shutil.copyfile('/solutions/count-records-start.sh', 'count-records.sh')
```

***

```yaml
type: ConsoleExercise
key: 7a893623af
xp: 50
```

`@instructions`
Bearbeite das Skript `count-records.sh` mit Nano und trage die beiden `____` Platzhalter ein
mit `$@` bzw. `-l` _(der Buchstabe_), damit er die Anzahl der Zeilen in einer oder mehreren Dateien zählt,
mit Ausnahme der jeweils ersten Zeile.

`@hint`
* Verwende `nano count-records.sh`, um den Dateinamen zu bearbeiten.
* Achte darauf, dass du den _Buchstaben_ `-l` angibst und nicht die Zahl eins.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/count-records.sh ~

```

`@sct`
```{python}
msg="Haben Sie die Leerstellen richtig ersetzt, sodass der Befehl in `count-records.sh` `tail -q -n +2 $@ | wc -l` lautet? Verwenden Sie `nano count-records.sh` erneut, um die erforderlichen Änderungen vorzunehmen."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/count-records.sh').\
        has_code('tail\s+-q\s+-n\s+\+2\s+\$\@\s+\|\s+wc\s+-l', incorrect_msg=msg)
)
```

***

```yaml
type: ConsoleExercise
key: d0da324516
xp: 50
```

`@instructions`
Führen Sie `count-records.sh` auf `seasonal/*.csv`
und leite die Ausgabe mit `>` an `num-records.out` um.

`@hint`
Verwende `>`, um die Ausgabe umzuleiten.

`@solution`
```{shell}
bash count-records.sh seasonal/*.csv > num-records.out

```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    check_file('/home/repl/num-records.out').has_code(r'92'),
    multi(
      has_code("bash", incorrect_msg = 'Haben Sie `bash` aufgerufen?'),
      has_code("bash\s+count-records.sh", incorrect_msg = 'Haben Sie die Datei `count-records.sh` ausgeführt?'),
      has_code("seasonal/\*", incorrect_msg = 'Haben Sie die zu verarbeitenden Dateien mit `seasonal/*` angegeben?'),
      has_code(">\s+num-records.out", incorrect_msg = 'Haben Sie in die Datei `num-records.out` umgeleitet?')
    )
  )
)
Ex().success_msg("Gut gemacht! Ihre Shell-Fähigkeiten erweitern sich ständig!")
```

---

## Wie kann ich ein einzelnes Argument verarbeiten?

```yaml
type: PureMultipleChoiceExercise
key: 4092cb4cda
xp: 50
```

Sowie `$@`,
In der Shell kannst du `$1`, `$2` und so weiter verwenden, um auf bestimmte Kommandozeilenparameter zu verweisen.
Damit kannst du Befehle schreiben, die sich einfacher und natürlicher anfühlen als die der Shell.
Ein Beispiel:
kannst du ein Skript namens `column.sh` erstellen, das eine einzelne Spalte aus einer CSV Datei auswählt
wenn der Benutzer den Dateinamen als ersten Parameter und die Spalte als zweiten Parameter angibt:

```{shell}
cut -d , -f $2 $1
```

und führe es dann mit aus:

```{shell}
bash column.sh seasonal/autumn.csv 1
```

Beachte, dass das Skript die beiden Parameter in umgekehrter Reihenfolge verwendet.

<hr>

Das Skript `get-field.sh` soll einen Dateinamen annehmen,
die Nummer der auszuwählenden Zeile,
die Nummer der auszuwählenden Spalte,
und drucke nur dieses Feld aus einer CSV Datei aus.
Beispiel:

```
bash get-field.sh seasonal/summer.csv 4 2
```

sollte das zweite Feld aus Zeile 4 von `seasonal/summer.csv` auswählen.
Welcher der folgenden Befehle sollte in `get-field.sh` eingegeben werden, um das zu tun?

`@hint`
Denke daran, dass die Befehlszeilenparameter von links nach rechts nummeriert sind.

`@possible_answers`
- `head -n $1 $2 | tail -n 1 | cut -d , -f $3`
- [`head -n $2 $1 | tail -n 1 | cut -d , -f $3`]
- `head -n $3 $1 | tail -n 1 | cut -d , -f $2`
- `head -n $2 $3 | tail -n 1 | cut -d , -f $1`

`@feedback`
- Nein: Damit wird versucht, den Dateinamen als Anzahl der Zeilen zu verwenden, die mit `head` ausgewählt werden sollen.
- Richtig!
- Nein: Dann wird versucht, die Spaltennummer als Zeilennummer zu verwenden und andersherum.
- Nein: Dann wird die Feldnummer als Dateiname verwendet und umgekehrt.

---

## Wie kann ein Shell-Skript viele Dinge tun?

```yaml
type: TabConsoleExercise
key: 846bc70e9d
xp: 100
```

Unsere bisherigen Shell-Skripte hatten einen einzigen Befehl oder eine Pipe, aber ein Skript kann viele Zeilen mit Befehlen enthalten. Du kannst z. B. eine Datei erstellen, die dir sagt, wie viele Datensätze in der kürzesten und längsten Datei enthalten sind, d. h., wie lang deine Datensätze sind.

Beachte, dass in Nano "Kopieren und Einfügen" erreicht wird, indem du zu der Zeile navigierst, die du kopieren willst, `CTRL` + `K` drückst, um die Zeile auszuschneiden, und dann `CTRL` + `U` zweimal drückst, um zwei Kopien davon einzufügen.

_Zur Erinnerung: Um zu speichern, was du in Nano geschrieben hast, gibst du `Ctrl` + `O` ein, um die Datei auszuschreiben, dann Enter, um den Dateinamen zu bestätigen, und `Ctrl` + `X`, um den Editor zu verlassen._

`@pre_exercise_code`
```{python}
import shutil
shutil.copyfile('/solutions/range-start-1.sh', 'range.sh')
```

***

```yaml
type: ConsoleExercise
key: a1e55487fb
xp: 25
```

`@instructions`
Verwende Nano, um das Skript zu bearbeiten `range.sh`
und ersetzen Sie die beiden `____` Platzhalter
mit `$@` und `-v`
so dass es die Namen und die Anzahl der Zeilen in allen Dateien auflistet, die in der Befehlszeile angegeben sind
*ohne* die Gesamtzahl der Zeilen in allen Dateien anzuzeigen.
(Versuche nicht, die Zeilen der Spaltenüberschriften von den Dateien zu subtrahieren).

`@hint`
Verwende `wc -l $@`, um die Zeilen in allen Dateien zu zählen, die in der Befehlszeile angegeben sind.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/range-1.sh range.sh

```

`@sct`
```{python}
msg="Haben Sie die Leerstellen richtig ersetzt, sodass der Befehl in `range.sh` `wc -l $@ | grep -v total` lautet? Verwenden Sie `nano range.sh` erneut, um die erforderlichen Änderungen vorzunehmen."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/range.sh').\
        has_code(r'wc\s+-l\s+\$@\s+\|\s+grep\s+-v\s+total', incorrect_msg=msg)
)
```

***

```yaml
type: ConsoleExercise
key: e8ece27fe7
xp: 25
```

`@instructions`
Verwende erneut Nano, um `sort -n` und `head -n 1` in dieser Reihenfolge hinzuzufügen.
zur Pipeline in `range.sh`
um den Namen und die Zeilenzahl der kürzesten Datei anzuzeigen, die ihr gegeben wurde.

`@hint`


`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/range-2.sh range.sh

```

`@sct`
```{python}
msg="Haben Sie `sort -n` und `head -n 1` mit Pipes zur Datei `range.sh` hinzugefügt? Verwenden Sie `nano range.sh` erneut, um die erforderlichen Änderungen vorzunehmen."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/range.sh').\
        has_code(r'wc\s+-l\s+\$@\s+\|\s+grep\s+-v\s+total\s+\|\s+sort\s+-n\s+|\s+head\s+-n\s+1', incorrect_msg=msg)
)
```

***

```yaml
type: ConsoleExercise
key: a3b36a746e
xp: 25
```

`@instructions`
Wiederum mit Nano fügst du eine zweite Zeile zu `range.sh` hinzu, um den Namen und die Anzahl der Datensätze von
die *längste* Datei im Verzeichnis *sowie* die kürzeste.
Diese Zeile sollte ein Duplikat der Zeile sein, die du bereits geschrieben hast,
aber mit `sort -n -r` und nicht mit `sort -n`.

`@hint`
Kopiere die erste Zeile und ändere die Sortierreihenfolge.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/range-3.sh range.sh

```

`@sct`
```{python}
msg1="Behalten Sie die erste Zeile in der Datei `range.sh`: `wc -l $@ | grep -v total | sort -n | head -n 1`"
msg2="Haben Sie die erste Zeile in `range.sh` dupliziert und eine kleine Änderung vorgenommen? `sort -n -r` anstelle von `sort -n`!"
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/range.sh').multi(
        has_code("wc -l $@ | grep -v total | sort -n | head -n 1", fixed=True, incorrect_msg = msg1),
        has_code(r'wc\s+-l\s+\$@\s+\|\s+grep\s+-v\s+total\s+\|\s+sort\s+-n\s+-r\s+|\s+head\s+-n\s+1', incorrect_msg=msg2)
    )
)

```

***

```yaml
type: ConsoleExercise
key: cba93a77c3
xp: 25
```

`@instructions`
Führe das Skript für die Dateien im Verzeichnis `seasonal` aus.
`seasonal/*.csv` verwenden, um alle Dateien abzugleichen
und leiten die Ausgabe um mit `>`
in eine Datei namens `range.out` in deinem Home-Verzeichnis.

`@hint`
Verwende `bash range.sh`, um dein Skript auszuführen, `seasonal/*.csv`, um Dateien anzugeben, und `> range.out`, um die Ausgabe umzuleiten.

`@solution`
```{shell}
bash range.sh seasonal/*.csv > range.out

```

`@sct`
```{python}
msg="Haben Sie das Ergebnis von `bash range.sh seasonal/*.csv` korrekt mit `>` nach `range.out` umgeleitet?"
Ex().multi(
has_cwd('/home/repl'),
multi(
has_code("bash", incorrect_msg = 'Haben Sie `bash` aufgerufen?'),
has_code("bash\s+range.sh", incorrect_msg = 'Haben Sie die Datei `range.sh` ausgeführt?'),
has_code("seasonal/\*", incorrect_msg = 'Haben Sie die zu verarbeitenden Dateien mit `seasonal/*` angegeben?'),
has_code(">\s+range.out", incorrect_msg = 'Haben Sie die Ausgabe in die Datei `range.out` umgeleitet?')
)
)

Ex().success_msg("Das läuft gut. Gehen Sie zur nächsten Übung, um mehr über das Schreiben von Schleifen zu lernen!")

```

---

## Wie kann ich Schleifen in ein Shell-Skript schreiben?

```yaml
type: BulletConsoleExercise
key: 6be8ca6009
xp: 100
```

Shell-Skripte können auch Schleifen enthalten. Du kannst sie mit Semikolons schreiben oder sie ohne Semikolons auf Zeilen aufteilen, um sie besser lesbar zu machen:

```{shell}
# Print the first and last data records of each file.
for filename in $@
do

    head -n 2 $filename | tail -n 1

    tail -n 1 $filename

done
```

(Du musst die Befehle innerhalb der Schleife nicht einrücken, aber das macht es übersichtlicher).

Die erste Zeile dieses Skripts ist ein **Kommentar**, der den Lesern sagt, was das Skript macht. Kommentare beginnen mit dem Zeichen `#` und laufen bis zum Ende der Zeile. Dein zukünftiges Ich wird es dir danken, wenn du jedem Skript, das du schreibst, eine kurze Erklärung wie die hier gezeigte beifügst.

_Zur Erinnerung: Um zu speichern, was du in Nano geschrieben hast, gibst du `Ctrl` + `O` ein, um die Datei auszuschreiben, dann Enter, um den Dateinamen zu bestätigen, und `Ctrl` + `X`, um den Editor zu verlassen._

`@pre_exercise_code`
```{python}
import shutil
shutil.copyfile('/solutions/date-range-start.sh', '/home/repl/date-range.sh')
```

***

```yaml
type: ConsoleExercise
key: 8ca2adb6c4
xp: 35
```

`@instructions`
Fülle die Platzhalter im Skript aus `date-range.sh`
mit `$filename` (zweimal), `head`, und `tail`
so dass er das erste und letzte Datum aus einer oder mehreren Dateien ausgibt.

`@hint`
Denke daran, `$filename` zu verwenden, um den aktuellen Wert der Schleifenvariablen zu erhalten.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/date-range.sh date-range.sh

```

`@sct`
```{python}
msgpatt="In `date-range.sh`, haben Sie die Zeile %s in der Schleife in `%s` geändert? Verwenden Sie `nano date-range.sh`, um Änderungen vorzunehmen."
cmdpatt = 'cut -d , -f 1 $filename | grep -v Date | sort | %s -n 1'
msg1=msgpatt%('first', cmdpatt%'head')
msg2=msgpatt%('second', cmdpatt%'tail')
patt='cut\s+-d\s+,\s+-f\s+1\s+\$filename\s+\|\s+grep\s+-v\s+Date\s+\|\s+sort\s+\|\s+%s\s+-n\s+1'
patt1 = patt%'head'
patt2 = patt%'tail'
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/date-range.sh').multi(
        has_code(patt1, incorrect_msg=msg1),
        has_code(patt2, incorrect_msg=msg2)
    )
)
```

***

```yaml
type: ConsoleExercise
key: ec1271356d
xp: 35
```

`@instructions`
Führe `date-range.sh` für alle vier saisonalen Datendateien aus.
mit `seasonal/*.csv`, um ihre Namen zu finden.

`@hint`
Der Platzhalterausdruck sollte mit dem Verzeichnisnamen beginnen.

`@solution`
```{shell}
bash date-range.sh seasonal/*.csv

```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    has_expr_output(),
    multi(
      has_code("bash", incorrect_msg = 'Haben Sie `bash` aufgerufen?'),
      has_code("bash\s+date-range.sh", incorrect_msg = 'Haben Sie die Datei `date-range.sh` ausgeführt?'),
      has_code("seasonal/\*", incorrect_msg = 'Haben Sie die zu verarbeitenden Dateien mit `seasonal/*` angegeben?')
    )
  )
)
```

***

```yaml
type: ConsoleExercise
key: 0323c7d68d
xp: 30
```

`@instructions`
Führe `date-range.sh` für alle vier saisonalen Datendateien aus und benutze `seasonal/*.csv`, um ihre Namen anzupassen,
und leite die Ausgabe an `sort` weiter, um zu sehen, dass deine Skripte genau wie die eingebauten Unix-Befehle verwendet werden können.

`@hint`
Verwende denselben Platzhalterausdruck wie zuvor.

`@solution`
```{shell}
bash date-range.sh seasonal/*.csv | sort

```

`@sct`
```{python}
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    has_expr_output(),
    multi(
      has_code("bash", incorrect_msg = 'Haben Sie `bash` aufgerufen?'),
      has_code("bash\s+date-range.sh", incorrect_msg = 'Haben Sie die Datei `date-range.sh` ausgeführt?'),
      has_code("seasonal/\*", incorrect_msg = 'Haben Sie die zu verarbeitenden Dateien mit `seasonal/*` angegeben?'),
      has_code("|", incorrect_msg = 'Haben Sie die Ausgabe des Skripts an `sort` weitergeleitet?'),
      has_code("sort", incorrect_msg = 'Haben Sie `sort` aufgerufen?')
    )
  )
)
Ex().success_msg("Magie! Beachten Sie, wie zusammensetzbar all die Dinge sind, die wir gelernt haben.")
```

---

## Was passiert, wenn ich keine Dateinamen angebe?

```yaml
type: MultipleChoiceExercise
key: 8a162c4d54
xp: 50
```

Ein häufiger Fehler in Shell-Skripten (und interaktiven Befehlen) ist es, Dateinamen an die falsche Stelle zu setzen.
Wenn du tippst:

```{shell}
tail -n 3
```

dann, da `tail` keine Dateinamen erhalten hat,
wartet er darauf, Eingaben von deiner Tastatur zu lesen.
Das bedeutet, dass du, wenn du tippst:

```{shell}
head -n 5 | tail -n 3 somefile.txt
```

dann macht `tail` weiter und druckt die letzten drei Zeilen von `somefile.txt`,
aber `head` wartet ewig auf Tastatureingaben,
da sie keinen Dateinamen erhalten hat und in der Pipeline nichts vor ihr liegt.

<hr>

Angenommen, du tippst aus Versehen:

```{shell}
head -n 5 | tail -n 3 somefile.txt
```

Was solltest du als Nächstes tun?

`@possible_answers`
- Warte 10 Sekunden, bis die Zeit von `head` abgelaufen ist.
- Gib `somefile.txt` ein und drücke die Eingabetaste, um `head` eine Eingabe zu geben.
- Verwende `Ctrl` + `C`, um das laufende Programm `head` anzuhalten.

`@hint`
Was macht `head`, wenn es keinen Dateinamen hat und nichts vorgelagert ist?

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
a1 = 'Nein, Befehle werden nicht ablaufen.'
a2 = 'Nein, das wird `head` den Text `somefile.txt` zur Verarbeitung geben, aber dann wird es hängen bleiben und auf weitere Eingaben warten.'
a3 = "Ja! Sie sollten `Ctrl` + `C` verwenden, um ein laufendes Programm zu stoppen. Damit ist dieser Einführungskurs abgeschlossen! Wenn Sie mehr über Kommandozeilen-Tools erfahren möchten, empfehlen wir Ihnen dringend, unseren kostenlosen Einführungskurs zu Git zu besuchen!"
Ex().has_chosen(3, [a1, a2, a3])
```
