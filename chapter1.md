---
title: Dateien und Verzeichnisse manipulieren
description: >-
  Dieses Kapitel ist eine kurze Einführung in die Unix-Shell. Du erfährst, warum
  sie nach fast 50 Jahren immer noch verwendet wird, wie sie sich von den
  grafischen Tools unterscheidet, mit denen du vielleicht vertrauter bist, wie
  du dich in der Shell bewegst und wie du Dateien und Ordner erstellst,
  veränderst und löschst.
free_preview: true
lessons:
  - nb_of_exercises: 12
    title: Wie ist die Shell im Vergleich zu einer Desktop-Oberfläche?
---

## Wie ist die Shell im Vergleich zu einer Desktop-Oberfläche?

```yaml
type: PureMultipleChoiceExercise
key: badd717ea4
xp: 50
```

Ein Betriebssystem wie Windows, Linux oder Mac OS ist eine besondere Art von Programm.
Er steuert den Prozessor des Computers, die Festplatte und die Netzwerkverbindung,
aber seine wichtigste Aufgabe ist es, andere Programme auszuführen.

Denn Menschen sind nicht digital,
sie brauchen eine Schnittstelle, um mit dem Betriebssystem zu interagieren.
Die gängigste Variante ist heutzutage ein grafischer Datei-Explorer,
die Klicks und Doppelklicks in Befehle zum Öffnen von Dateien und Ausführen von Programmen umwandelt.
Bevor Computer grafische Displays hatten,
allerdings,
tippten die Menschen Anweisungen in ein Programm ein, das als **Kommandozeilen-Shell** bezeichnet wurde.
Jedes Mal, wenn ein Befehl eingegeben wird,
die Shell führt einige andere Programme aus,
gibt ihre Ausgabe in menschenlesbarer Form aus,
und zeigt dann eine *Eingabeaufforderung* an, um zu signalisieren, dass er bereit ist, den nächsten Befehl anzunehmen.
(Sein Name kommt daher, dass er die "äußere Hülle" des Computers ist.)

Befehle zu tippen, anstatt zu klicken und zu ziehen, mag zunächst umständlich erscheinen,
aber wie du sehen wirst,
wenn du anfängst, zu formulieren, was der Computer tun soll,
Du kannst alte Befehle kombinieren, um neue Befehle zu erstellen
und automatisieren sich wiederholende Vorgänge
mit nur wenigen Tastendrucken.

<hr>
Welche Beziehung besteht zwischen dem grafischen Dateiexplorer, den die meisten Leute benutzen, und der Kommandozeilen-Shell?

`@hint`
Denke daran, dass ein Benutzer nur über ein Programm mit einem Betriebssystem interagieren kann.

`@possible_answers`
- Mit dem Datei-Explorer kannst du Dateien ansehen und bearbeiten, während du mit der Shell Programme ausführen kannst.
- Der Datei-Explorer ist auf der Shell aufgebaut.
- Die Shell ist Teil des Betriebssystems, während der Datei-Explorer separat ist.
- [Beides sind Schnittstellen, um Befehle an das Betriebssystem zu geben.]

`@feedback`
- Mit beiden kannst du Dateien ansehen und bearbeiten und Programme ausführen.
- Grafische Dateiexplorer und die Shell rufen beide die gleichen Funktionen des Betriebssystems auf.
- Die Shell und der Datei-Explorer sind beides Programme, die Benutzerbefehle (getippt oder geklickt) in Aufrufe an das Betriebssystem übersetzen.
- Richtig! Beide nehmen die Befehle des Benutzers (egal ob getippt oder geklickt) entgegen und senden sie an das Betriebssystem.

---

## Wo bin ich?

```yaml
type: MultipleChoiceExercise
key: 7c1481dbd3
xp: 50
```

Das **Dateisystem** verwaltet Dateien und Verzeichnisse (oder Ordner).
Jeder wird durch einen **absoluten Pfad** identifiziert
die zeigt, wie man sie vom **Stammverzeichnis** des Dateisystems aus erreicht:
`/home/repl` ist das Verzeichnis `repl` im Verzeichnis `home`,
während `/home/repl/course.txt` eine Datei `course.txt` in diesem Verzeichnis ist,
und `/` allein ist das Stammverzeichnis.

So findest du heraus, wo du dich im Dateisystem befindest,
den Befehl ausführen `pwd`
(kurz für " **Arbeitsverzeichnis****drucken**").
Dies gibt den absoluten Pfad deines **aktuellen Arbeitsverzeichnisses** aus,
in der die Shell standardmäßig Befehle ausführt und nach Dateien sucht.

<hr>
Run `pwd`.
Wo bist du jetzt gerade?

`@possible_answers`
- `/home`
- `/repl`
- `/home/repl`

`@hint`
Unix-Systeme legen die Home-Verzeichnisse aller Benutzer normalerweise unter `/home` ab.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err = "Das ist nicht der richtige Pfad."
correct = "Korrekt - Sie befinden sich in `/home/repl`."

Ex().has_chosen(3, [err, err, correct])
```

---

## Wie kann ich Dateien und Verzeichnisse identifizieren?

```yaml
type: MultipleChoiceExercise
key: f5b0499835
xp: 50
```

`pwd` sagt dir, wo du bist.
Um herauszufinden, was es dort gibt,
tippe `ls` (das ist die Abkürzung für "**Listing**") und drücke die Eingabetaste.
Für sich allein,
`ls` listet den Inhalt deines aktuellen Verzeichnisses auf
(die, die von `pwd` angezeigt wird).
Wenn du die Namen von einigen Dateien hinzufügst,
`ls` wird sie auflisten,
und wenn du die Namen von Verzeichnissen hinzufügst,
wird ihr Inhalt aufgelistet.
Ein Beispiel:
`ls /home/repl` zeigt dir, was sich in deinem Startverzeichnis befindet
(normalerweise dein **Heimatverzeichnis**).

<hr>
Verwende `ls` mit einem entsprechenden Argument, um die Dateien im Verzeichnis `/home/repl/seasonal` aufzulisten
(hier findest du Informationen über Zahnarztpraxen nach Datum und Jahreszeit).
Welche dieser Dateien befindet sich *nicht* in diesem Verzeichnis?

`@possible_answers`
- `autumn.csv`
- `fall.csv`
- `spring.csv`
- `winter.csv`

`@hint`
Wenn du `ls` einen Pfad gibst, zeigt es dir, was sich in diesem Pfad befindet.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err = "Diese Datei befindet sich im Verzeichnis `seasonal`."
correct = "Korrekt - diese Datei befindet sich *nicht* im Verzeichnis `seasonal`."

Ex().has_chosen(2, [err, correct, err, err])
```

---

## Wie kann ich sonst noch Dateien und Verzeichnisse identifizieren?

```yaml
type: BulletConsoleExercise
key: a766184b59
xp: 100
```

Ein absoluter Pfad ist wie ein Längen- und Breitengrad: Er hat den gleichen Wert, egal wo du bist. Ein **relativer Pfad** hingegen gibt einen Ort an, der von dem Ort ausgeht, an dem du dich befindest: Es ist so, als würdest du sagen "20 Kilometer nördlich".

Als Beispiele:
- Wenn du dich im Verzeichnis `/home/repl` befindest, gibt der **relative** Pfad `seasonal` das gleiche Verzeichnis an wie der **absolute** Pfad `/home/repl/seasonal`. 
- Wenn du dich im Verzeichnis `/home/repl/seasonal` befindest, gibt der **relative** Pfad `winter.csv` die gleiche Datei an wie der **absolute** Pfad `/home/repl/seasonal/winter.csv`.

Die Shell entscheidet anhand des ersten Zeichens, ob ein Pfad absolut oder relativ ist: Wenn sie mit `/` beginnt, ist sie absolut. Wenn sie *nicht* mit `/` beginnt, ist sie relativ.

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: 9db1ed7afd
xp: 35
```

`@instructions`
Du bist auf `/home/repl`. Verwende `ls` mit einem **relativen Pfad**, um die Datei aufzulisten, die den absoluten Pfad `/home/repl/course.txt` hat (und nur diese Datei).

`@hint`
Oft kannst du den relativen Pfad zu einer Datei oder einem Verzeichnis unterhalb deines aktuellen Standorts konstruieren
indem du den absoluten Pfad deines aktuellen Standorts abziehst
aus dem absoluten Pfad der Sache, die du willst.

`@solution`
```{shell}
ls course.txt

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/home/repl"),
    has_code("ls", incorrect_msg = "Sie haben `ls` nicht aufgerufen, um die Dateiliste zu erstellen."), # to prevent `echo "course.txt"`
    check_correct(
      has_expr_output(strict=True),
      has_code("ls +course.txt", incorrect_msg = "Ihr Befehl hat nicht die richtige Dateiliste erstellt. Verwenden Sie `ls` gefolgt von einem relativen Pfad zu `/home/repl/course.txt`.")
    )
)
```

***

```yaml
type: ConsoleExercise
key: 4165425bf6
xp: 35
```

`@instructions`
Du bist auf `/home/repl`.
`ls` mit einem **relativen** Pfad verwenden
um die Datei `/home/repl/seasonal/summer.csv` (und nur diese Datei) aufzulisten.

`@hint`
Relative Pfade beginnen *nicht* mit einem führenden "/".

`@solution`
```{shell}
ls seasonal/summer.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/home/repl"),
    has_code("ls", incorrect_msg = "Sie haben `ls` nicht aufgerufen, um die Dateiliste zu erstellen."), 
    check_correct(
      has_expr_output(strict=True),
      has_code("ls +seasonal/summer.csv", incorrect_msg = "Ihr Befehl hat nicht die richtige Dateiliste erstellt. Verwenden Sie `ls` gefolgt von einem relativen Pfad zu `/home/repl/seasonal/summer.csv`.")
    )
)
```

***

```yaml
type: ConsoleExercise
key: b5e66d3741
xp: 30
```

`@instructions`
Du bist auf `/home/repl`.
`ls` mit einem **relativen** Pfad verwenden
um den Inhalt des Verzeichnisses `/home/repl/people` aufzulisten.

`@hint`
Relative Pfade beginnen nicht mit einem führenden "/".

`@solution`
```{shell}
ls people

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/home/repl"),
    has_code("ls", incorrect_msg = "Sie haben `ls` nicht aufgerufen, um die Dateiliste zu erstellen."), 
    check_correct(
      has_expr_output(strict=True),
      has_code("ls +people", incorrect_msg = "Ihr Befehl hat nicht die richtige Dateiliste erstellt. Verwenden Sie `ls` gefolgt von einem relativen Pfad zu `/home/repl/people`.")
    )
)
Ex().success_msg("Gut gemacht. Jetzt, da Sie wissen, wie man Dateien und Verzeichnisse auflistet, sehen wir uns an, wie Sie sich im Dateisystem bewegen können!")

```

---

## Wie kann ich in ein anderes Verzeichnis wechseln?

```yaml
type: BulletConsoleExercise
key: dbdaec5610
xp: 100
```

So wie du dich in einem Dateibrowser bewegen kannst, indem du auf Ordner doppelklickst,
kannst du dich im Dateisystem bewegen, indem du den Befehl `cd`
(das steht für "Verzeichnis wechseln").

Wenn du `cd seasonal` und dann `pwd` eingibst,
Die Shell wird dir sagen, dass du dich jetzt in `/home/repl/seasonal` befindest.
Wenn du dann `ls` alleine ausführst,
Es zeigt dir den Inhalt von `/home/repl/seasonal`,
denn genau dort bist du.
Wenn du zu deinem Heimatverzeichnis `/home/repl` zurückkehren willst,
kannst du den Befehl `cd /home/repl` verwenden.

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: 3d0bfdd77d
xp: 35
```

`@instructions`
Du bist in `/home/repl`/.
Wechsle das Verzeichnis zu `/home/repl/seasonal` unter Verwendung eines relativen Pfades.

`@hint`
Denke daran, dass `cd` für "Verzeichnis wechseln" steht und dass relative Pfade nicht mit einem führenden "/" beginnen.

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
key: e69c8eac15
xp: 35
```

`@instructions`
Nutze `pwd`, um zu überprüfen, ob du dort bist.

`@hint`
Denke daran, nach der Eingabe des Befehls "Enter" oder "Return" zu drücken.

`@solution`
```{shell}
pwd

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    check_correct(
      has_expr_output(),
      has_code('pwd')
    )
)


```

***

```yaml
type: ConsoleExercise
key: f6b265bd7f
xp: 30
```

`@instructions`
Verwende `ls` ohne Pfadangaben, um zu sehen, was sich in dem Verzeichnis befindet.

`@hint`
Denke daran, nach dem Befehl "Enter" oder "Return" zu drücken.

`@solution`
```{shell}
ls

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    check_correct(
      has_expr_output(),
      has_code('ls', incorrect_msg="Ihr Befehl hat nicht die korrekte Ausgabe erzeugt. Haben Sie `ls` ohne Pfade verwendet, um den Inhalt des aktuellen Verzeichnisses anzuzeigen?")
    )
)

Ex().success_msg("Sauber! Hier ging es darum, in Unterverzeichnisse zu navigieren. Wie wäre es mit dem Aufstieg? Finden wir es heraus!")
```

---

## Wie kann ich in einem Verzeichnis aufsteigen?

```yaml
type: PureMultipleChoiceExercise
key: 09c717ef76
xp: 50
```

Das **übergeordnete** Verzeichnis eines Verzeichnisses ist das Verzeichnis über ihm.
Zum Beispiel ist `/home` der Elternteil von `/home/repl`,
und `/home/repl` ist die Muttergesellschaft von `/home/repl/seasonal`.
Du kannst Befehlen wie `cd` und `ls` immer den absoluten Pfad deines übergeordneten Verzeichnisses angeben.
Häufiger,
allerdings,
wirst du den Vorteil nutzen, dass der spezielle Pfad `..`
(zwei Punkte ohne Leerzeichen) bedeutet "das Verzeichnis über dem, in dem ich mich gerade befinde".
Wenn du in `/home/repl/seasonal` bist,
`cd ..` bringt dich dann auf `/home/repl`.
Wenn du `cd ..` noch einmal verwendest,
Es bringt dich auf `/home`.
Eine weitere `cd ..` bringt dich in das *Stammverzeichnis* `/`,
das ist der oberste Punkt des Dateisystems.
(Denk daran, ein Leerzeichen zwischen `cd` und `..` zu setzen - es ist ein Befehl und ein Pfad, nicht ein einzelner Befehl mit vier Buchstaben).

Ein einzelner Punkt allein, `.`, bedeutet immer "das aktuelle Verzeichnis",
`ls` allein und `ls .` tun also dasselbe,
während `cd .` keine Wirkung hat
(weil es dich in das Verzeichnis bringt, in dem du dich gerade befindest).

Ein letzter spezieller Pfad ist `~` (das Tilde-Zeichen),
was "dein Heimatverzeichnis" bedeutet,
wie `/home/repl`.
Egal, wo du bist,
`ls ~` wird immer den Inhalt deines Home-Verzeichnisses auflisten,
und `cd ~` wird dich immer nach Hause bringen.

<hr>
Wenn du in `/home/repl/seasonal` bist,
Wohin führt dich `cd ~/../.`?

`@hint`
Verfolge den Pfad ein Verzeichnis nach dem anderen.

`@possible_answers`
- `/home/repl`
- [`/home`]
- `/home/repl/seasonal`
- `/` (das Stammverzeichnis)

`@feedback`
- Nein, aber entweder `~` oder `..` allein würden dich dorthin bringen.
- Richtig! Der Pfad bedeutet "Heimatverzeichnis", "eine Ebene höher", "hier".
- Nein, aber `.` würde das von alleine tun.
- Nein, der letzte Teil des Weges ist `.` (bedeutet "hier") und nicht `..` (bedeutet "oben").

---

## Wie kann ich Dateien kopieren?

```yaml
type: BulletConsoleExercise
key: 832de9e74c
xp: 100
```

Du wirst oft Dateien kopieren wollen,
verschiebe sie in andere Verzeichnisse, um sie zu organisieren,
oder sie umbenennen.
Ein Befehl dafür ist `cp`, was die Abkürzung für "copy" ist.
Wenn `original.txt` eine bestehende Datei ist,
dann:

```{shell}
cp original.txt duplicate.txt
```

erstellt eine Kopie von `original.txt` mit dem Namen `duplicate.txt`.
Wenn es bereits eine Datei namens `duplicate.txt` gab,
wird sie überschrieben.
Wenn der letzte Parameter auf `cp` ein bestehendes Verzeichnis ist,
dann einen Befehl wie:

```{shell}
cp seasonal/autumn.csv seasonal/winter.csv backup
```

kopiert *alle* Dateien in dieses Verzeichnis.

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: 6ab3fb1e25
xp: 50
```

`@instructions`
Erstelle eine Kopie von `seasonal/summer.csv` im Verzeichnis `backup` (das sich auch in `/home/repl` befindet),
Aufruf der neuen Datei `summer.bck`.

`@hint`
Kombiniere den Namen des Zielverzeichnisses und den Namen der kopierten Datei
um einen relativen Pfad für die neue Datei zu erstellen.

`@solution`
```{shell}
cp seasonal/summer.csv backup/summer.bck

```

`@sct`
```{python}
Ex().check_correct(
    check_file('/home/repl/backup/summer.bck', missing_msg="`summer.bck` scheint nicht im Verzeichnis `backup` zu existieren. Geben Sie zwei Pfade an `cp` an: die vorhandene Datei (`seasonal/summer.csv`) und die Zieldatei (`backup/summer.bck`)."),
    has_cwd('/home/repl')
)
```

***

```yaml
type: ConsoleExercise
key: d9e1214bb0
xp: 50
```

`@instructions`
Kopiere `spring.csv` und `summer.csv` aus dem Verzeichnis `seasonal` in das Verzeichnis `backup` 
*ohne* dein aktuelles Arbeitsverzeichnis zu ändern (`/home/repl`).

`@hint`
Verwende `cp` mit den Namen der Dateien, die du kopieren willst
und *dann* den Namen des Verzeichnisses, in das sie kopiert werden sollen.

`@solution`
```{shell}
cp seasonal/spring.csv seasonal/summer.csv backup

```

`@sct`
```{python}
patt = "`%s` scheint nicht in das `backup` Verzeichnis kopiert worden zu sein. Geben Sie zwei Dateinamen und einen Verzeichnisnamen an `cp` an."
Ex().multi(
    has_cwd('/home/repl', incorrect_msg="Stellen Sie sicher, dass Sie die Dateien im Verzeichnis `{{dir}}` kopieren! Verwenden Sie `cd {{dir}}`, um dorthin zurück zu navigieren."),
    check_file('/home/repl/backup/spring.csv', missing_msg=patt%'spring.csv'),
    check_file('/home/repl/backup/summer.csv', missing_msg=patt%'summer.csv')
)
Ex().success_msg("Gut gemacht. Neben dem Kopieren sollten wir auch in der Lage sein, Dateien von einem Verzeichnis in ein anderes zu verschieben. Lernen Sie mehr darüber in der nächsten Übung!")
```

---

## Wie kann ich eine Datei verschieben?

```yaml
type: ConsoleExercise
key: 663a083a3c
xp: 100
```

Während `cp` eine Datei kopiert,
`mv` verschiebt es von einem Verzeichnis in ein anderes,
als ob du sie in einen grafischen Dateibrowser gezogen hättest.
Sie behandelt ihre Parameter auf die gleiche Weise wie `cp`,
also den Befehl:

```{shell}
mv autumn.csv winter.csv ..
```

verschiebt die Dateien `autumn.csv` und `winter.csv` aus dem aktuellen Arbeitsverzeichnis
eine Ebene nach oben zum übergeordneten Verzeichnis
(denn `..` bezieht sich immer auf das Verzeichnis oberhalb deines aktuellen Standorts).

`@instructions`
Du befindest dich in `/home/repl`, das Unterverzeichnisse `seasonal` und `backup` hat.
Verschiebe `spring.csv` und `summer.csv` mit einem einzigen Befehl von `seasonal` nach `backup`.

`@hint`


`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
mv seasonal/spring.csv seasonal/summer.csv backup
```

`@sct`
```{python}
backup_patt="Die Datei `%s` befindet sich nicht im Verzeichnis `backup`. Haben Sie `mv` korrekt verwendet? Verwenden Sie zwei Dateinamen und ein Verzeichnis als Parameter für `mv`."
seasonal_patt="Die Datei `%s` befindet sich noch im Verzeichnis `seasonal`. Stellen Sie sicher, dass Sie die Dateien mit `mv` verschieben, anstatt sie mit `cp` zu kopieren!"
Ex().multi(
    check_file('/home/repl/backup/spring.csv', missing_msg=backup_patt%'spring.csv'),
    check_file('/home/repl/backup/summer.csv', missing_msg=backup_patt%'summer.csv'),
    check_not(check_file('/home/repl/seasonal/spring.csv'), incorrect_msg=seasonal_patt%'spring.csv'),
    check_not(check_file('/home/repl/seasonal/summer.csv'), incorrect_msg=seasonal_patt%'summer.csv')
)
Ex().success_msg("Gut gemacht, lassen Sie uns diesen Shell-Zug weiterfahren!")
```

---

## Wie kann ich Dateien umbenennen?

```yaml
type: BulletConsoleExercise
key: 001801a652
xp: 100
```

`mv` kann auch zum Umbenennen von Dateien verwendet werden. Wenn du läufst:

```{shell}
mv course.txt old-course.txt
```

dann wird die Datei `course.txt` im aktuellen Arbeitsverzeichnis in die Datei `old-course.txt`"verschoben".
Dies unterscheidet sich von der Art und Weise, wie Dateibrowser funktionieren,
ist aber oft praktisch.

Eine Warnung:
genau wie `cp`,
`mv` werden bestehende Dateien überschrieben.
If,
zum Beispiel,
hast du bereits eine Datei namens `old-course.txt`,
dann wird der oben gezeigte Befehl sie durch das ersetzen, was in `course.txt` steht.

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: 710187c8c7
xp: 35
```

`@instructions`
Gehe in das Verzeichnis `seasonal`.

`@hint`
Denke daran, dass `cd` für "Verzeichnis wechseln" steht und dass relative Pfade nicht mit einem führenden "/" beginnen.

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
key: ed5fe1df23
xp: 35
```

`@instructions`
Benenne die Datei `winter.csv` in `winter.csv.bck` um.

`@hint`
Verwende `mv` mit dem aktuellen Namen der Datei und dem Namen, den sie in dieser Reihenfolge haben soll.

`@solution`
```{shell}
mv winter.csv winter.csv.bck

```

`@sct`
```{python}
hint = " Verwenden Sie `mv` mit zwei Argumenten: die Datei, die Sie umbenennen möchten (`winter.csv`), und den neuen Namen für die Datei (`winter.csv.bck`)."
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    multi(
        check_file('/home/repl/seasonal/winter.csv.bck', missing_msg="Wir erwarteten, `winter.csv.bck` im Verzeichnis zu finden." + hint),
        check_not(check_file('/home/repl/seasonal/winter.csv'), incorrect_msg="Wir erwarteten nicht mehr, `winter.csv` im Verzeichnis zu finden." + hint)
    )
)
```

***

```yaml
type: ConsoleExercise
key: 1deee4c768
xp: 30
```

`@instructions`
Führe `ls` aus, um zu überprüfen, ob alles funktioniert hat.

`@hint`
Denke daran, "Enter" oder "Return" zu drücken, um den Befehl auszuführen.

`@solution`
```{shell}
ls

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    has_expr_output(incorrect_msg="Haben Sie `ls` verwendet, um den Inhalt Ihres aktuellen Arbeitsverzeichnisses aufzulisten?")
)
Ex().multi(
    has_cwd("/home/repl/seasonal"),
    check_correct(
      has_expr_output(strict=True),
      has_code("ls", incorrect_msg = "Ihr Befehl hat nicht die korrekte Dateiliste erzeugt. Verwenden Sie `ls` ohne Argumente, um den Inhalt Ihres aktuellen Arbeitsverzeichnisses aufzulisten.")
    )
)
Ex().success_msg("Kopieren, verschieben, umbenennen, Sie haben alles im Griff! Als nächstes: Dateien löschen.")
```

---

## Wie kann ich Dateien löschen?

```yaml
type: BulletConsoleExercise
key: '2734680614'
xp: 100
```

Wir können Dateien kopieren und sie verschieben;
um sie zu löschen,
wir verwenden `rm`,
was für "entfernen" steht.
Wie bei `cp` und `mv`,
kannst du `rm` die Namen von so vielen Dateien geben, wie du möchtest, also:

```{shell}
rm thesis.txt backup/thesis-2017-08.txt
```

beseitigt sowohl `thesis.txt` als auch `backup/thesis-2017-08.txt`

`rm` tut genau das, was sein Name sagt,
und er tut es sofort:
im Gegensatz zu grafischen Dateibrowsern,
die Schale hat keinen Mülleimer,
Wenn du also den obigen Befehl eingibst,
ist deine These für immer vom Tisch.

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: d7580f7bd4
xp: 25
```

`@instructions`
Du bist auf `/home/repl`.
Gehe in das Verzeichnis `seasonal`.

`@hint`
Denke daran, dass `cd` für "Verzeichnis wechseln" steht und dass ein relativer Pfad nicht mit einem führenden "/" beginnt.

`@solution`
```{shell}
cd seasonal

```

`@sct`
```{python}
Ex().has_cwd('/home/repl/seasonal')
```

***

```yaml
type: ConsoleExercise
key: 1c21cc7039
xp: 25
```

`@instructions`
`autumn.csv` entfernen.

`@hint`
Denk daran, dass `rm` für "entfernen" steht.

`@solution`
```{shell}
rm autumn.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    check_not(check_file('/home/repl/seasonal/autumn.csv'), incorrect_msg="Wir haben nicht erwartet, dass `autumn.csv` noch im Verzeichnis `seasonal` ist. Verwenden Sie `rm` mit dem Pfad zu der Datei, die Sie entfernen möchten."),
    has_code('rm', incorrect_msg = 'Verwenden Sie `rm`, um die Datei zu entfernen, anstatt sie zu verschieben.')
)
```

***

```yaml
type: ConsoleExercise
key: 09f2d105cd
xp: 25
```

`@instructions`
Gehe zurück in dein Heimatverzeichnis.

`@hint`
Wenn du `cd` ohne Wege benutzt, bringt er dich nach Hause.

`@solution`
```{shell}
cd

```

`@sct`
```{python}
Ex().has_cwd('/home/repl', incorrect_msg="Verwenden Sie `cd ..` oder `cd ~`, um in das Home-Verzeichnis zurückzukehren.")
```

***

```yaml
type: ConsoleExercise
key: 9eaf49744c
xp: 25
```

`@instructions`
Entferne `seasonal/summer.csv`, ohne die Verzeichnisse erneut zu ändern.

`@hint`
Denk daran, dass `rm` für "entfernen" steht.

`@solution`
```{shell}
rm seasonal/summer.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_not(check_file('/home/repl/seasonal/summer.csv'), incorrect_msg="Wir haben nicht erwartet, dass `summer.csv` noch im Verzeichnis `seasonal` ist. Verwenden Sie `rm` mit dem Pfad zur Datei, die Sie entfernen möchten."),
    has_code('rm', incorrect_msg = 'Verwenden Sie `rm`, um die Datei zu entfernen, anstatt sie zu verschieben.')
)
Ex().success_msg("Beeindruckende Sache! Weiter zum nächsten!")
```

---

## Wie kann ich Verzeichnisse erstellen und löschen?

```yaml
type: BulletConsoleExercise
key: 63e8fbd0c2
xp: 100
```

`mv` behandelt Verzeichnisse auf die gleiche Weise wie Dateien:
wenn du dich in deinem Heimatverzeichnis befindest und `mv seasonal by-season` ausführst,
zum Beispiel,
`mv` ändert den Namen des Verzeichnisses `seasonal` in `by-season`.
Allerdings
`rm` funktioniert anders.

Wenn du versuchst, ein Verzeichnis zu `rm`,
gibt die Shell eine Fehlermeldung aus, die besagt, dass sie das nicht tun kann,
vor allem, um zu verhindern, dass du versehentlich ein ganzes Verzeichnis voller Arbeit löschst.
Stattdessen,
kannst du einen separaten Befehl namens `rmdir` verwenden.
Für zusätzliche Sicherheit,
es funktioniert nur, wenn das Verzeichnis leer ist,
Du musst also die Dateien in einem Verzeichnis löschen *, bevor* du das Verzeichnis löschst.
(Erfahrene Nutzer können die Option `-r` auf `rm` verwenden, um denselben Effekt zu erzielen;
wir werden die Befehlsoptionen im nächsten Kapitel besprechen).

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: 5a81bb8589
xp: 25
```

`@instructions`
Ohne die Verzeichnisse zu wechseln,
lösche die Datei `agarwal.txt` im Verzeichnis `people`.

`@hint`
Denk daran, dass `rm` für "entfernen" steht und dass ein relativer Pfad nicht mit einem führenden "/" beginnt.

`@solution`
```{shell}
rm people/agarwal.txt

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_not(check_file('/home/repl/people/agarwal.txt'), incorrect_msg="`agarwal.txt` sollte nicht mehr in `/home/repl/people` sein. Hast du `rm` korrekt verwendet?"),
    has_expr_output(expr = 'ls people', output = '', incorrect_msg = 'Es gibt immer noch Dateien im Verzeichnis `people`. Wenn du `agarwal.txt` nur verschoben oder neue Dateien erstellt hast, lösche sie alle.')
)
```

***

```yaml
type: ConsoleExercise
key: 661633e531
xp: 25
```

`@instructions`
Jetzt, wo das Verzeichnis `people` leer ist,
einen einzigen Befehl verwenden, um sie zu löschen.

`@hint`
Denke daran, dass `rm` nur mit Dateien funktioniert.

`@solution`
```{shell}
rmdir people

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_not(has_dir('/home/repl/people'),
              incorrect_msg = "Das Verzeichnis 'people' sollte nicht mehr in Ihrem Home-Verzeichnis sein. Verwenden Sie `rmdir`, um es zu entfernen!")
)
```

***

```yaml
type: ConsoleExercise
key: 89f7ffc1da
xp: 25
```

`@instructions`
Da ein Verzeichnis keine Datei ist,
musst du den Befehl verwenden `mkdir directory_name`
um ein neues (leeres) Verzeichnis zu erstellen.
Benutze diesen Befehl, um ein neues Verzeichnis namens `yearly` unterhalb deines Home-Verzeichnisses zu erstellen.

`@hint`
Starte `mkdir` mit dem Namen des Verzeichnisses, das du erstellen möchtest.

`@solution`
```{shell}
mkdir yearly

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_dir('/home/repl/yearly', msg="Es gibt kein `yearly` Verzeichnis in Ihrem Home-Verzeichnis. Verwenden Sie `mkdir yearly`, um eines zu erstellen!")
)
```

***

```yaml
type: ConsoleExercise
key: 013a5ff2dc
xp: 25
```

`@instructions`
Jetzt gibt es die `yearly`,
Erstelle darin ein weiteres Verzeichnis namens `2017` 
*ohne* dein Heimatverzeichnis zu verlassen.

`@hint`
Verwende einen relativen Pfad für das Unterverzeichnis, das du erstellen möchtest.

`@solution`
```{shell}
mkdir yearly/2017

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_dir('/home/repl/yearly/2017',
            msg="Kann das Verzeichnis '2017' in '/home/repl/yearly' nicht finden. Sie können dieses Verzeichnis mit dem relativen Pfad `yearly/2017` erstellen.")
)
Ex().success_msg("Cool! Lassen Sie uns dieses Kapitel mit einer Übung abschließen, die einige seiner Konzepte wiederholt!")
```

---

## Einpacken

```yaml
type: BulletConsoleExercise
key: b1990e9a42
xp: 100
```

Wenn du Daten analysierst, erstellst du oft Zwischendateien.
Anstatt sie in deinem Heimatverzeichnis zu speichern,
Du kannst sie in `/tmp` einstellen,
wo Menschen und Programme oft Dateien aufbewahren, die sie nur kurz brauchen.
(Beachte, dass `/tmp` direkt unter dem Stammverzeichnis `/` liegt,
*nicht* unterhalb deines Heimatverzeichnisses).
Diese Nachbereitungsübung zeigt dir, wie du das machen kannst.

`@pre_exercise_code`
```{python}

```

***

```yaml
type: ConsoleExercise
key: 59781bc43b
xp: 25
```

`@instructions`
Verwende `cd`, um `/tmp` aufzurufen.

`@hint`
Denke daran, dass `cd` für "Verzeichnis wechseln" steht und dass ein absoluter Pfad mit einem "/" beginnt.

`@solution`
```{shell}
cd /tmp

```

`@sct`
```{python}
Ex().check_correct(
  has_cwd('/tmp'),
  has_code('cd +/tmp', incorrect_msg = 'Sie befinden sich im falschen Verzeichnis. Verwenden Sie `cd`, um das Verzeichnis zu `/tmp` zu ändern.')
)
```

***

```yaml
type: ConsoleExercise
key: 7e6ada440d
xp: 25
```

`@instructions`
Listet den Inhalt von `/tmp` auf *, ohne* einen Verzeichnisnamen einzugeben.

`@hint`
Wenn du `ls` nicht sagst, was aufgelistet werden soll, zeigt es dir, was sich in deinem aktuellen Verzeichnis befindet.

`@solution`
```{shell}
ls

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/tmp"),
    has_code("ls", incorrect_msg = "Sie haben `ls` nicht aufgerufen, um die Dateiliste zu erstellen."),
    check_correct(
      has_expr_output(strict=True),
      has_code("^\s*ls\s*$", incorrect_msg = "Ihr Befehl hat nicht die richtige Dateiliste erstellt. Verwenden Sie `ls` ohne `. ")
    )
)
```

***

```yaml
type: ConsoleExercise
key: edaf1bcf96
xp: 25
```

`@instructions`
Lege innerhalb von `/tmp` ein neues Verzeichnis mit dem Namen `scratch` an.

`@hint`
Verwende `mkdir`, um Verzeichnisse zu erstellen.

`@solution`
```{shell}
mkdir scratch

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/tmp'),
    check_correct(
      has_dir('/tmp/scratch'),
      has_code('mkdir +scratch', incorrect_msg="Kann kein 'scratch'-Verzeichnis unter '/tmp' finden. Stellen Sie sicher, dass Sie `mkdir` korrekt verwenden.")
    )
)
```

***

```yaml
type: ConsoleExercise
key: a904a3a719
xp: 25
```

`@instructions`
Verschiebe `/home/repl/people/agarwal.txt` in `/tmp/scratch`.
Wir empfehlen dir, die Verknüpfung `~` für dein Home-Verzeichnis und einen relativen Pfad für das zweite Verzeichnis zu verwenden, anstatt des absoluten Pfades.

`@hint`


`@solution`
```{shell}
mv ~/people/agarwal.txt scratch

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/tmp'),
    check_file('/tmp/scratch/agarwal.txt', missing_msg="Kann 'agarwal.txt' in '/tmp/scratch' nicht finden. Verwenden Sie `mv` mit `~/people/agarwal.txt` als ersten Parameter und `scratch` als zweiten.")
)
Ex().success_msg("Dies schließt Kapitel 1 der Einführung in die Shell ab! Eilen Sie zum nächsten Kapitel, um mehr über die Datenmanipulation zu erfahren!")
```
