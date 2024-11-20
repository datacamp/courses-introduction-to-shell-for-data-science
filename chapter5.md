---
title: Créer de nouveaux outils
description: >-
  L'historique vous permet de répéter des choses en quelques frappes, et les
  tuyaux vous permettent de combiner des commandes existantes pour en créer de
  nouvelles. Dans ce chapitre, vous verrez comment aller plus loin et créer vos
  propres commandes.
lessons:
  - nb_of_exercises: 9
    title: Comment puis-je modifier un fichier ?
---

## Comment puis-je modifier un fichier ?

```yaml
type: ConsoleExercise
key: 39eee3cfc0
xp: 100
```

Unix dispose d'une variété déconcertante d'éditeurs de texte.
Pour ce cours,
nous en utiliserons un simple appelé Nano.
Si vous tapez `nano filename`,
Le site `filename` s'ouvrira pour être édité.
(ou créez-le s'il n'existe pas encore).
Vous pouvez vous déplacer à l'aide des touches fléchées,
supprimer des caractères en utilisant l'espacement arrière,
et effectuer d'autres opérations à l'aide de combinaisons de touches de contrôle :

- `Ctrl` + `K`: supprimer une ligne.
- `Ctrl` + `U`: supprimer une ligne.
- `Ctrl` + `O`: enregistrer le fichier ('O' signifie 'output'). _Vous devrez également appuyer sur la touche Entrée pour confirmer le nom du fichier !_
- `Ctrl` + `X`: quitter l'éditeur.

`@instructions`
Lancez `nano names.txt` pour éditer un nouveau fichier dans votre répertoire personnel.
et saisissez les quatre lignes suivantes :

```
Lovelace
Hopper
Johnson
Wilson
```

Pour sauvegarder ce que vous avez écrit,
tapez `Ctrl` + `O` pour écrire le fichier,
puis Enter pour confirmer le nom du fichier,
puis `Ctrl` + `X` pour quitter l'éditeur.

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
patt = "Avez-vous inclus la ligne `%s` dans le fichier `names.txt` ? Utilisez à nouveau `nano names.txt` pour mettre à jour votre fichier. Utilisez `Ctrl` + `O` pour enregistrer et `Ctrl` + `X` pour quitter."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/names.txt').multi(
        has_code(r'Lovelace', incorrect_msg=patt%'Lovelace'),
        has_code(r'Hopper', incorrect_msg=patt%'Hopper'),
        has_code(r'Johnson', incorrect_msg=patt%'Johnson'),
        has_code(r'Wilson', incorrect_msg=patt%'Wilson')
    )
)
Ex().success_msg("Bien joué ! Passons au suivant !")
```

---

## Comment puis-je enregistrer ce que je viens de faire ?

```yaml
type: BulletConsoleExercise
key: 80c3532985
xp: 100
```

Lorsque vous effectuez une analyse complexe,
vous voudrez souvent garder une trace des commandes que vous avez utilisées.
Vous pouvez le faire avec les outils que vous avez déjà vus :

1. Exécutez `history`.
2. Dirigez sa sortie vers `tail -n 10` (ou vers le nombre d'étapes récentes que vous souhaitez sauvegarder).
3. Redirigez-le vers un fichier appelé quelque chose comme `figure-5.history`.

C'est mieux que d'écrire dans un cahier de laboratoire.
car il est garanti qu'aucune étape ne sera manquée.
Il illustre également l'idée centrale du shell :
des outils simples qui produisent et consomment des lignes de texte
peuvent être combinés de multiples façons
pour résoudre un large éventail de problèmes.

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
Copiez les fichiers `seasonal/spring.csv` et `seasonal/summer.csv` dans votre répertoire personnel.

`@hint`
Utilisez `cp` pour copier et `~` comme raccourci pour le chemin d'accès à votre répertoire personnel.

`@solution`
```{shell}
cp seasonal/s* ~

```

`@sct`
```{python}
msg="Avez-vous utilisé `cp seasonal/s* ~` pour copier les fichiers requis dans votre répertoire personnel ?"
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/spring.csv', missing_msg=msg).\
        has_code(r'2017-01-25,wisdom', incorrect_msg=msg),
    check_file('/home/repl/summer.csv', missing_msg=msg).\
        has_code(r'2017-01-11,canine', incorrect_msg=msg)
)
Ex().success_msg("Enregistrement remarquable ! Si vous avez mal tapé des commandes, vous pouvez toujours utiliser `nano` pour nettoyer le fichier d'historique des sauvegardes par la suite.")
```

***

```yaml
type: ConsoleExercise
key: 09a432e4df
xp: 35
```

`@instructions`
Utilisez `grep` avec l'option `-h` (pour empêcher l'impression des noms de fichiers).
et `-v Tooth` (pour sélectionner les lignes qui ne correspondent *pas* à la ligne d'en-tête)
pour sélectionner les enregistrements de données de `spring.csv` et `summer.csv` dans cet ordre
et rediriger la sortie vers `temp.csv`.

`@hint`
Placez les drapeaux avant les noms de fichiers.

`@solution`
```{shell}
grep -h -v Tooth spring.csv summer.csv > temp.csv

```

`@sct`
```{python}
msg1 = "Assurez-vous de rediriger la sortie de la commande `grep` vers `temp.csv` avec `>` !"
msg2 = "Avez-vous utilisé `grep -h -v ___ ___ ___` (remplissez les blancs) pour remplir `temp.csv` ?"
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
Pipez `history` dans `tail -n 3`
et rediriger la sortie vers `steps.txt`
pour enregistrer les trois dernières commandes dans un fichier.
(Vous devez en enregistrer trois au lieu de deux seulement
car la commande `history` elle-même figurera dans la liste).

`@hint`
Rappelez-vous que la redirection avec `>` vient à la fin de la séquence de commandes pipées.

`@solution`
```{shell}
history | tail -n 3 > steps.txt

```

`@sct`
```{python}
msg1="Assurez-vous de rediriger la sortie de votre commande vers `steps.txt`."
msg2="Avez-vous utilisé `history | tail ___ ___` (remplissez les blancs) pour remplir `steps.txt` ?"
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
Ex().success_msg("Bien joué ! Passons à la vitesse supérieure !")
```

---

## Comment puis-je enregistrer des commandes pour les réexécuter plus tard ?

```yaml
type: BulletConsoleExercise
key: 4507a0dbd8
xp: 100
```

Jusqu'à présent, vous avez utilisé le shell de manière interactive.
Mais comme les commandes que vous saisissez ne sont que du texte,
vous pouvez les stocker dans des fichiers pour que le shell les exécute encore et encore.
Pour commencer à explorer cette puissante capacité,
mettez la commande suivante dans un fichier appelé `headers.sh`:

```{shell}
head -n 1 seasonal/*.csv
```

Cette commande sélectionne la première ligne de chacun des fichiers CSV dans le répertoire `seasonal`.
Une fois que vous avez créé ce fichier,
vous pouvez l'exécuter en tapant :

```{shell}
bash headers.sh
```

Cela indique au shell (qui n'est autre qu'un programme appelé `bash`)
pour exécuter les commandes contenues dans le fichier `headers.sh`,
ce qui produit le même résultat que l'exécution directe des commandes.

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
Utilisez `nano dates.sh` pour créer un fichier appelé `dates.sh`
qui contient cette commande :

```{shell}
cut -d , -f 1 seasonal/*.csv
```

pour extraire la première colonne de tous les fichiers CSV dans `seasonal`.

`@hint`
Insérez les commandes indiquées dans le fichier sans lignes vides ni espaces supplémentaires.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/dates.sh ~

```

`@sct`
```{python}
msg = "Avez-vous inclus la ligne `cut -d , -f 1 seasonal/*.csv` dans le fichier `dates.sh` ? Utilisez à nouveau `nano dates.sh` pour mettre à jour votre fichier. Utilisez `Ctrl` + `O` pour enregistrer et `Ctrl` + `X` pour quitter."
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
Utilisez `bash` pour exécuter le fichier `dates.sh`.

`@hint`
Utilisez `bash filename` pour exécuter le fichier.

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
      has_code("bash", incorrect_msg = 'Avez-vous appelé `bash` ?'),
      has_code("dates.sh", incorrect_msg = 'Avez-vous spécifié le fichier `dates.sh` ?')
    )
  )
)
```

---

## Comment réutiliser les tuyaux ?

```yaml
type: BulletConsoleExercise
key: da13667750
xp: 100
```

Un fichier rempli de commandes shell s'appelle un**\*script** shell,
ou parfois simplement un "script". Les scripts ne doivent pas nécessairement avoir un nom se terminant par `.sh`,
mais cette leçon utilisera cette convention
pour vous aider à garder une trace des fichiers qui sont des scripts.

Les scripts peuvent également contenir des tuyaux.
Par exemple :
si `all-dates.sh` contient cette ligne :

```{shell}
cut -d , -f 1 seasonal/*.csv | grep -v Date | sort | uniq
```

ensuite :

```{shell}
bash all-dates.sh > dates.out
```

extraira les dates uniques des fichiers de données saisonnières
et sauvegardez-les dans `dates.out`.

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
Un fichier `teeth.sh` dans votre répertoire personnel a été préparé pour vous, mais il contient des blancs.
Utilisez Nano pour éditer le fichier et remplacez les deux placeholders `____` 
avec `seasonal/*.csv` et `-c` afin que ce script imprime un décompte des
nombre de fois où chaque nom de dent apparaît dans les fichiers CSV du répertoire `seasonal`.

`@hint`
Utilisez `nano teeth.sh` pour éditer le fichier.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/teeth.sh ~

```

`@sct`
```{python}
msg="Avez-vous remplacé les blancs correctement pour que la commande dans `teeth.sh` soit `cut -d , -f 2 seasonal/*.csv | grep -v Tooth | sort | uniq -c` ? Utilisez à nouveau `nano teeth.sh` pour effectuer les modifications nécessaires."
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
Utilisez `bash` pour exécuter `teeth.sh` et `>` pour rediriger sa sortie vers `teeth.out`.

`@hint`
N'oubliez pas que `> teeth.out` doit être placé *après la* commande qui produit la sortie.

`@solution`
```{shell}
# We need to use 'cp' below to satisfy our automated tests.
# You should only use the last line that runs 'bash'.
cp /solutions/teeth.sh .
bash teeth.sh > teeth.out

```

`@sct`
```{python}
msg="Avez-vous correctement redirigé le résultat de `bash teeth.sh` vers `teeth.out` avec le `>` ?"
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    check_file('/home/repl/teeth.out').multi(
      has_code(r'31 canine', incorrect_msg=msg),
      has_code(r'17 wisdom', incorrect_msg=msg)
    ),
    multi(
      has_code("bash", incorrect_msg = 'Avez-vous appelé `bash` ?'),
      has_code("bash\s+teeth.sh", incorrect_msg = 'Avez-vous exécuté le fichier `teeth.sh` ?'),
      has_code(">\s+teeth.out", incorrect_msg = 'Avez-vous redirigé vers le fichier `teeth.out` ?')
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
Lancez `cat teeth.out` pour vérifier vos résultats.

`@hint`
N'oubliez pas que vous pouvez taper les premiers caractères d'un nom de fichier et appuyer sur la touche de tabulation pour le compléter automatiquement.

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
      has_code("cat", incorrect_msg = 'Avez-vous appelé `cat` ?'),
      has_code("teeth.out", incorrect_msg = 'Avez-vous spécifié le fichier `teeth.out` ?')
    )
  )
)
Ex().success_msg("Bien ! Tout cela peut sembler artificiel au début, mais l'avantage est que vous automatisez progressivement des parties de votre flux de travail. Quelque chose de très utile en tant que data scientist !")
```

---

## Comment puis-je transmettre des noms de fichiers à des scripts ?

```yaml
type: BulletConsoleExercise
key: c2623b9c14
xp: 100
```

Un script qui traite des fichiers spécifiques est utile pour garder une trace de ce que vous avez fait, mais un script qui vous permet de traiter tous les fichiers que vous souhaitez est plus utile.
A l'appui de cette démarche,
vous pouvez utiliser l'expression spéciale `$@` (signe du dollar immédiatement suivi du signe at)
pour signifier "tous les paramètres de la ligne de commande donnés au script".

Par exemple, si `unique-lines.sh` contient `sort $@ | uniq`, lorsque vous exécutez :

```{shell}
bash unique-lines.sh seasonal/summer.csv
```

le shell remplace `$@` par `seasonal/summer.csv` et traite un fichier. Si vous exécutez ceci :

```{shell}
bash unique-lines.sh seasonal/summer.csv seasonal/autumn.csv
```

il traite deux fichiers de données, et ainsi de suite.

_Pour rappel, pour sauvegarder ce que vous avez écrit dans Nano, tapez `Ctrl` + `O` pour écrire le fichier, puis Enter pour confirmer le nom du fichier, puis `Ctrl` + `X` pour quitter l'éditeur._

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
Editez le script `count-records.sh` avec Nano et remplissez les deux espaces réservés de `____`.
avec respectivement `$@` et `-l` _(la lettre_) pour qu'il compte le nombre de lignes dans un ou plusieurs fichiers,
à l'exclusion de la première ligne de chacun d'entre eux.

`@hint`
* Utilisez `nano count-records.sh` pour modifier le nom du fichier.
* Assurez-vous que vous spécifiez la _lettre_ `-l`, et non le chiffre un.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/count-records.sh ~

```

`@sct`
```{python}
msg="Avez-vous remplacé les blancs correctement pour que la commande dans `count-records.sh` soit `tail -q -n +2 $@ | wc -l` ? Utilisez `nano count-records.sh` à nouveau pour effectuer les modifications requises."
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
Lancez `count-records.sh` sur `seasonal/*.csv`
et rediriger la sortie vers `num-records.out` en utilisant `>`.

`@hint`
Utilisez `>` pour rediriger la sortie.

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
      has_code("bash", incorrect_msg = 'Avez-vous appelé `bash` ?'),
      has_code("bash\s+count-records.sh", incorrect_msg = 'Avez-vous exécuté le fichier `count-records.sh` ?'),
      has_code("seasonal/\*", incorrect_msg = 'Avez-vous spécifié les fichiers à traiter avec `seasonal/*` ?'),
      has_code(">\s+num-records.out", incorrect_msg = 'Avez-vous redirigé vers le fichier `num-records.out` ?')
    )
  )
)
Ex().success_msg("Un travail bien fait ! Votre maîtrise du shell ne cesse de s\'étendre !")
```

---

## Comment traiter un seul argument ?

```yaml
type: PureMultipleChoiceExercise
key: 4092cb4cda
xp: 50
```

Ainsi que `$@`,
le shell vous permet d'utiliser `$1`, `$2`, et ainsi de suite pour faire référence à des paramètres de ligne de commande spécifiques.
Vous pouvez l'utiliser pour écrire des commandes qui semblent plus simples ou plus naturelles que celles du shell.
Par exemple :
vous pouvez créer un script appelé `column.sh` qui sélectionne une seule colonne dans un fichier CSV 
lorsque l'utilisateur fournit le nom du fichier comme premier paramètre et la colonne comme second :

```{shell}
cut -d , -f $2 $1
```

puis exécutez-le en utilisant :

```{shell}
bash column.sh seasonal/autumn.csv 1
```

Remarquez que le script utilise les deux paramètres dans l'ordre inverse.

<hr>

Le script `get-field.sh` est censé prendre un nom de fichier,
le numéro de la ligne à sélectionner,
le numéro de la colonne à sélectionner,
et d'imprimer uniquement ce champ à partir d'un fichier CSV.
Par exemple :

```
bash get-field.sh seasonal/summer.csv 4 2
```

doit sélectionner le deuxième champ de la ligne 4 de `seasonal/summer.csv`.
Laquelle des commandes suivantes doit être placée dans `get-field.sh` pour y parvenir ?

`@hint`
Rappelez-vous que les paramètres de la ligne de commande sont numérotés de gauche à droite.

`@possible_answers`
- `head -n $1 $2 | tail -n 1 | cut -d , -f $3`
- [`head -n $2 $1 | tail -n 1 | cut -d , -f $3`]
- `head -n $3 $1 | tail -n 1 | cut -d , -f $2`
- `head -n $2 $3 | tail -n 1 | cut -d , -f $1`

`@feedback`
- Non : cela essaiera d'utiliser le nom du fichier comme nombre de lignes à sélectionner avec `head`.
- C'est exact !
- Non : il essaiera d'utiliser le numéro de colonne comme numéro de ligne et vice versa.
- Non : le numéro de champ sera utilisé comme nom de fichier et vice versa.

---

## Comment un script shell peut-il faire plusieurs choses ?

```yaml
type: TabConsoleExercise
key: 846bc70e9d
xp: 100
```

Jusqu'à présent, nos scripts shells ne comportaient qu'une seule commande ou un seul tuyau, mais un script peut contenir de nombreuses lignes de commandes. Par exemple, vous pouvez en créer un qui vous indique le nombre d'enregistrements contenus dans le plus court et le plus long de vos fichiers de données, c'est-à-dire l'éventail des longueurs de vos ensembles de données.

Notez que dans Nano, le "copier-coller" s'effectue en naviguant jusqu'à la ligne que vous souhaitez copier, en appuyant sur `CTRL` + `K` pour couper la ligne, puis sur `CTRL` + `U` deux fois pour coller deux copies de la ligne.

_Pour rappel, pour sauvegarder ce que vous avez écrit dans Nano, tapez `Ctrl` + `O` pour écrire le fichier, puis Enter pour confirmer le nom du fichier, puis `Ctrl` + `X` pour quitter l'éditeur._

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
Utilisez Nano pour éditer le script `range.sh`
et remplacez les deux placeholders `____` 
avec `$@` et `-v`
afin qu'il énumère les noms et le nombre de lignes de tous les fichiers indiqués sur la ligne de commande
*sans* afficher le nombre total de lignes dans tous les fichiers.
(N'essayez pas de soustraire les lignes d'en-tête de colonne des fichiers).

`@hint`
Utilisez `wc -l $@` pour compter les lignes dans tous les fichiers indiqués sur la ligne de commande.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/range-1.sh range.sh

```

`@sct`
```{python}
msg="Avez-vous remplacé les blancs correctement pour que la commande dans `range.sh` soit `wc -l $@ | grep -v total` ? Utilisez à nouveau `nano range.sh` pour effectuer les modifications requises."
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
Utilisez à nouveau Nano pour ajouter `sort -n` et `head -n 1` dans cet ordre.
à la canalisation en `range.sh`
pour afficher le nom et le nombre de lignes du fichier le plus court qui lui a été donné.

`@hint`


`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/range-2.sh range.sh

```

`@sct`
```{python}
msg="Avez-vous ajouté `sort -n` et `head -n 1` avec des pipes au fichier `range.sh` ? Utilisez à nouveau `nano range.sh` pour effectuer les modifications requises."
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
Toujours à l'aide de Nano, ajoutez une deuxième ligne à `range.sh` pour imprimer le nom et le nombre d'enregistrements de
le fichier *le plus long* du répertoire *ainsi que* le plus court.
Cette ligne doit être une copie de celle que vous avez déjà écrite,
mais avec `sort -n -r` au lieu de `sort -n`.

`@hint`
Copiez la première ligne et modifiez l'ordre de tri.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/range-3.sh range.sh

```

`@sct`
```{python}
msg1="Conservez la première ligne dans le fichier `range.sh` : `wc -l $@ | grep -v total | sort -n | head -n 1`"
msg2="Avez-vous dupliqué la première ligne dans `range.sh` et fait un petit changement ? `sort -n -r` au lieu de `sort -n` !"
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
Exécutez le script sur les fichiers du répertoire `seasonal` 
en utilisant `seasonal/*.csv` pour faire correspondre tous les fichiers
et rediriger la sortie en utilisant `>`
dans un fichier appelé `range.out` dans votre répertoire personnel.

`@hint`
Utilisez `bash range.sh` pour exécuter votre script, `seasonal/*.csv` pour spécifier des fichiers et `> range.out` pour rediriger la sortie.

`@solution`
```{shell}
bash range.sh seasonal/*.csv > range.out

```

`@sct`
```{python}
msg="Avez-vous correctement redirigé le résultat de `bash range.sh seasonal/*.csv` vers `range.out` avec le `>`?"
Ex().multi(
has_cwd('/home/repl'),
multi(
has_code("bash", incorrect_msg = 'Avez-vous appelé `bash` ?'),
has_code("bash\s+range.sh", incorrect_msg = 'Avez-vous exécuté le fichier `range.sh` ?'),
has_code("seasonal/\*", incorrect_msg = 'Avez-vous spécifié les fichiers à traiter avec `seasonal/*` ?'),
has_code(">\s+range.out", incorrect_msg = 'Avez-vous redirigé vers le fichier `range.out` ?')
)
)

Ex().success_msg("Tout se passe bien. Passez à l'exercice suivant pour apprendre à écrire des boucles !")
```

---

## Comment écrire des boucles dans un script shell ?

```yaml
type: BulletConsoleExercise
key: 6be8ca6009
xp: 100
```

Les scripts shell peuvent également contenir des boucles. Vous pouvez les écrire en utilisant des points-virgules ou les répartir sur plusieurs lignes sans point-virgule pour les rendre plus lisibles :

```{shell}
# Print the first and last data records of each file.
for filename in $@
do

    head -n 2 $filename | tail -n 1

    tail -n 1 $filename

done
```

(Il n'est pas nécessaire d'indenter les commandes à l'intérieur de la boucle, mais cela rend les choses plus claires).

La première ligne de ce script est un **commentaire** qui indique aux lecteurs ce que fait le script. Les commentaires commencent par le caractère `#` et se poursuivent jusqu'à la fin de la ligne. Votre futur vous remerciera d'ajouter de brèves explications comme celle présentée ici à chaque script que vous écrivez.

_Pour rappel, pour sauvegarder ce que vous avez écrit dans Nano, tapez `Ctrl` + `O` pour écrire le fichier, puis Enter pour confirmer le nom du fichier, puis `Ctrl` + `X` pour quitter l'éditeur._

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
Remplir les espaces réservés dans le script `date-range.sh`
avec `$filename` (deux fois), `head` et `tail`
pour qu'il imprime la première et la dernière date d'un ou de plusieurs fichiers.

`@hint`
N'oubliez pas d'utiliser `$filename` pour obtenir la valeur actuelle de la variable de la boucle.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/date-range.sh date-range.sh

```

`@sct`
```{python}
msgpatt="Dans `date-range.sh`, avez-vous modifié la ligne %s dans la boucle pour être `%s` ? Utilisez `nano date-range.sh` pour apporter des modifications."
cmdpatt = 'cut -d , -f 1 $filename | grep -v Date | sort | %s -n 1'
msg1=msgpatt%('première', cmdpatt%'head')
msg2=msgpatt%('deuxième', cmdpatt%'tail')
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
Lancez `date-range.sh` sur les quatre fichiers de données saisonnières.
en utilisant `seasonal/*.csv` pour faire correspondre leurs noms.

`@hint`
L'expression générique doit commencer par le nom du répertoire.

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
      has_code("bash", incorrect_msg = 'Avez-vous appelé `bash` ?'),
      has_code("bash\s+date-range.sh", incorrect_msg = 'Avez-vous exécuté le fichier `date-range.sh` ?'),
      has_code("seasonal/\*", incorrect_msg = 'Avez-vous spécifié les fichiers à traiter avec `seasonal/*` ?')
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
Exécutez `date-range.sh` sur les quatre fichiers de données saisonnières en utilisant `seasonal/*.csv` pour faire correspondre leurs noms,
et envoyez sa sortie à `sort` pour voir que vos scripts peuvent être utilisés comme les commandes intégrées d'Unix.

`@hint`
Utilisez la même expression de caractères génériques que celle que vous avez utilisée précédemment.

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
      has_code("bash", incorrect_msg = 'Avez-vous appelé `bash` ?'),
      has_code("bash\s+date-range.sh", incorrect_msg = 'Avez-vous exécuté le fichier `date-range.sh` ?'),
      has_code("seasonal/\*", incorrect_msg = 'Avez-vous spécifié les fichiers à traiter avec `seasonal/*` ?'),
      has_code("|", incorrect_msg = 'Avez-vous redirigé la sortie du script vers `sort` ?'),
      has_code("sort", incorrect_msg = 'Avez-vous appelé `sort` ?')
    )
  )
)
Ex().success_msg("Magique ! Remarquez à quel point tout ce que nous avons appris est modulable.")
```

---

## Que se passe-t-il si je ne fournis pas de noms de fichiers ?

```yaml
type: MultipleChoiceExercise
key: 8a162c4d54
xp: 50
```

Une erreur fréquente dans les scripts shell (et les commandes interactives) consiste à placer les noms de fichiers au mauvais endroit.
Si vous tapez :

```{shell}
tail -n 3
```

puisque `tail` n'a reçu aucun nom de fichier,
il attend de lire les données de votre clavier.
Cela signifie que si vous tapez :

```{shell}
head -n 5 | tail -n 3 somefile.txt
```

alors `tail` imprime les trois dernières lignes de `somefile.txt`,
mais `head` attend indéfiniment la saisie du clavier,
puisqu'il n'a pas reçu de nom de fichier et qu'il n'y a rien avant lui dans le pipeline.

<hr>

Supposons que vous tapiez accidentellement :

```{shell}
head -n 5 | tail -n 3 somefile.txt
```

Que faire ensuite ?

`@possible_answers`
- Attendez 10 secondes pour que `head` se termine.
- Tapez `somefile.txt` et appuyez sur Entrée pour donner à `head` quelques informations.
- Utilisez `Ctrl` + `C` pour arrêter le programme `head` en cours d'exécution.

`@hint`
Que fait `head` s'il n'a pas de nom de fichier et qu'il n'y a rien en amont ?

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
a1 = 'Non, les commandes ne s\'arrêteront pas par expiration du délai.'
a2 = 'Non, cela donnera à `head` le texte `somefile.txt` à traiter, mais ensuite il se bloquera en attendant encore plus d\'entrée.'
a3 = "Oui ! Vous devriez utiliser `Ctrl` + `C` pour arrêter un programme en cours d'exécution. Cela conclut ce cours d'introduction ! Si vous êtes intéressé à apprendre plus d'outils en ligne de commande, nous vous recommandons vivement de suivre notre cours d'introduction gratuit à Git !"
Ex().has_chosen(3, [a1, a2, a3])
```
