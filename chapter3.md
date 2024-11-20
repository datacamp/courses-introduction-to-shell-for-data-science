---
title: Combinaison d'outils
description: >-
  La véritable puissance du shell Unix ne réside pas dans les commandes
  individuelles, mais dans la facilité avec laquelle elles peuvent être
  combinées pour réaliser de nouvelles choses. Ce chapitre vous montrera comment
  utiliser cette puissance pour sélectionner les données que vous souhaitez, et
  vous présentera les commandes permettant de trier les valeurs et de supprimer
  les doublons.
lessons:
  - nb_of_exercises: 12
    title: Comment puis-je stocker la sortie d'une commande dans un fichier ?
---

## Comment puis-je stocker la sortie d'une commande dans un fichier ?

```yaml
type: ConsoleExercise
key: 07a427d50c
xp: 100
```

Tous les outils que vous avez vus jusqu'à présent vous permettent de nommer les fichiers d'entrée.
La plupart d'entre eux n'ont pas la possibilité de nommer le fichier de sortie, car ils n'en ont pas besoin.
Au lieu de cela,
vous pouvez utiliser la **redirection** pour enregistrer la sortie de n'importe quelle commande où vous le souhaitez.
Si vous exécutez cette commande :

```{shell}
head -n 5 seasonal/summer.csv
```

il imprime à l'écran les 5 premières lignes des données de l'été.
Si vous exécutez cette commande à la place :

```{shell}
head -n 5 seasonal/summer.csv > top.csv
```

rien n'apparaît à l'écran.
Au lieu de cela,
`head`est placé dans un nouveau fichier appelé `top.csv`.
Vous pouvez consulter le contenu de ce fichier en utilisant `cat`:

```{shell}
cat top.csv
```

Le signe plus grand que `>` indique au shell de rediriger la sortie de `head` vers un fichier.
Elle ne fait pas partie de la commande `head`;
au lieu de cela,
il fonctionne avec toutes les commandes shell qui produisent une sortie.

`@instructions`
Combinez `tail` avec la redirection pour enregistrer les 5 dernières lignes de `seasonal/winter.csv` dans un fichier appelé `last.csv`.

`@hint`
Utilisez `tail -n 5` pour obtenir les 5 dernières lignes.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
tail -n 5 seasonal/winter.csv > last.csv
```

`@sct`
```{python}
patt = "La ligne `%s` devrait être dans le fichier `last.csv`, mais elle ne l'est pas. Redirigez la sortie de `tail -n 5 seasonal/winter.csv` vers `last.csv` avec `>`."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/last.csv').multi(
        check_not(has_code('2017-07-01,incisor'), incorrect_msg='`last.csv` a trop de lignes. Avez-vous utilisé le drapeau `-n 5` avec `tail` ?'),
        has_code('2017-07-17,canine', incorrect_msg=patt%'2017-07-17,canine'),
        has_code('2017-08-13,canine', incorrect_msg=patt%'2017-08-13,canine')
    )
)
Ex().success_msg("Bien joué ! Pratiquons encore un peu !")
```

---

## Comment utiliser la sortie d'une commande comme entrée ?

```yaml
type: BulletConsoleExercise
key: f47d337593
xp: 100
```

Supposons que vous souhaitiez récupérer des lignes au milieu d'un fichier.
Plus précisément,
supposons que vous souhaitiez obtenir les lignes 3 à 5 de l'un de nos fichiers de données.
Vous pouvez commencer par utiliser `head` pour obtenir les 5 premières lignes
et la redirige vers un fichier,
puis utilisez `tail` pour sélectionner les 3 derniers :

```{shell}
head -n 5 seasonal/winter.csv > top.csv
tail -n 3 top.csv
```

Une vérification rapide confirme qu'il s'agit bien des lignes 3-5 de notre fichier original,
parce qu'il s'agit des 3 dernières lignes des 5 premières.

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
Sélectionnez les deux dernières lignes de `seasonal/winter.csv`
et sauvegardez-les dans un fichier appelé `bottom.csv`.

`@hint`
Utilisez `tail` pour sélectionner des lignes et `>` pour rediriger la sortie de `tail`.

`@solution`
```{shell}
tail -n 2 seasonal/winter.csv > bottom.csv

```

`@sct`
```{python}
patt="La ligne `%s` devrait être dans le fichier `bottom.csv`, mais elle ne l'est pas. Redirigez la sortie de `tail -n 2 seasonal/winter.csv` vers `bottom.csv` avec `>`."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/bottom.csv').multi(
        check_not(has_code('2017-08-11,bicuspid'), incorrect_msg = '`bottom.csv` a trop de lignes. Avez-vous utilisé le drapeau `-n 2` avec `tail` ?'),
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
Sélectionnez la première ligne de `bottom.csv`
afin d'obtenir l'avant-dernière ligne du fichier original.

`@hint`
Utilisez `head` pour sélectionner la ligne souhaitée.

`@solution`
```{shell}
head -n 1 bottom.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/bottom.csv').has_code('2017-08-11,wisdom', incorrect_msg="Il y a quelque chose qui ne va pas avec le fichier `bottom.csv`. Assurez-vous de ne pas le modifier !"),
    has_expr_output(strict=True, incorrect_msg="Avez-vous utilisé `head` correctement sur `bottom.csv` ? Assurez-vous d'utiliser correctement l'option `-n`.")
)

Ex().success_msg("Bien joué. Passez à l'exercice suivant pour découvrir de meilleures façons de combiner les commandes.")
```

---

## Quelle est la meilleure façon de combiner les commandes ?

```yaml
type: ConsoleExercise
key: b36aea9a1e
xp: 100
```

L'utilisation de la redirection pour combiner des commandes présente deux inconvénients :

1. Il laisse traîner de nombreux fichiers intermédiaires (comme `top.csv`).
2. Les commandes permettant d'obtenir le résultat final sont dispersées dans plusieurs lignes de l'histoire.

Le shell fournit un autre outil qui résout ces deux problèmes à la fois, appelé **pipe**.
Une fois de plus,
Commencez par lancer `head`:

```{shell}
head -n 5 seasonal/summer.csv
```

Au lieu d'envoyer la sortie de `head` dans un fichier,
ajoutez une barre verticale et la commande `tail` *sans* nom de fichier :

```{shell}
head -n 5 seasonal/summer.csv | tail -n 3
```

Le symbole du tuyau indique au shell d'utiliser la sortie de la commande située à gauche
comme entrée de la commande de droite.

`@instructions`
Utilisez `cut` pour sélectionner tous les noms de dents de la colonne 2 du fichier délimité par des virgules `seasonal/summer.csv`, puis dirigez le résultat vers `grep`, avec une correspondance inversée, pour exclure la ligne d'en-tête contenant le mot "Dent". *`cut` et `grep` ont été étudiés en détail dans le chapitre 2, exercices 8 et 11 respectivement.*

`@hint`
- La première partie de la commande se présente sous la forme `cut -d field_delimiter -f column_number filename`.
- La deuxième partie de la commande se présente sous la forme `grep -v thing_to_match`.

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
    has_expr_output(incorrect_msg = 'Avez-vous redirigé le résultat de `cut -d , -f 2 seasonal/summer.csv` dans `grep -v Tooth` avec `|` ?'),
    check_not(has_output("Tooth"), incorrect_msg = 'Avez-vous exclu la ligne d\'en-tête `"Tooth"` en utilisant `grep` ?')
)
Ex().success_msg("Parfait ! C'est peut-être la première fois que vous utilisez `|`, mais ce ne sera certainement pas la dernière !")
```

---

## Comment combiner plusieurs commandes ?

```yaml
type: ConsoleExercise
key: b8753881d6
xp: 100
```

Vous pouvez enchaîner un nombre illimité de commandes.
Par exemple :
cette commande :

```{shell}
cut -d , -f 1 seasonal/spring.csv | grep -v Date | head -n 10
```

volonté :

1. sélectionnez la première colonne des données du printemps ;
2. supprimer la ligne d'en-tête contenant le mot "Date" ; et
3. sélectionner les 10 premières lignes de données réelles.

`@instructions`
Dans l'exercice précédent, vous avez utilisé la commande suivante pour sélectionner tous les noms de dents de la colonne 2 de `seasonal/summer.csv`:

```
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth
```

Prolongez ce pipeline avec une commande `head` pour ne sélectionner que le tout premier nom de dent.

`@hint`
Copiez et collez le code dans les instructions, ajoutez un tuyau, puis appelez `head` avec le drapeau `-n`.

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
    has_output('^\s*canine\s*$', incorrect_msg = "Avez-vous utilisé `|` pour étendre le pipeline avec une commande `head` ? Assurez-vous de définir correctement le drapeau `-n`."),
    # by coincidence, tail -n 1 returns the same as head -n 1, so check that head was called
    has_code("head", "Avez-vous utilisé `|` pour étendre le pipeline avec une commande `head` ?")
)
Ex().success_msg("Enchaînement joyeux ! En enchaînant plusieurs commandes ensemble, vous pouvez construire des pipelines puissants de manipulation de données.")
```

---

## Comment compter les enregistrements d'un fichier ?

```yaml
type: ConsoleExercise
key: ae6a48d6aa
xp: 100
```

La commande `wc` (abréviation de "word count") affiche le nombre de **caractères**, de **mots**et de **lignes**d'un fichier.
Vous pouvez faire en sorte qu'il n'imprime que l'un d'entre eux en utilisant respectivement `-c`, `-w` ou `-l`.

`@instructions`
Comptez le nombre d'enregistrements dans `seasonal/spring.csv` dont la date se situe en juillet 2017 (`2017-07`). 
- Pour ce faire, utilisez `grep` avec une date partielle pour sélectionner les lignes et acheminez le résultat vers `wc` avec un indicateur approprié pour compter les lignes.

`@hint`
- Utilisez `head seasonal/spring.csv` pour vous rappeler le format de la date.
- La première partie de la commande se présente sous la forme `grep thing_to_match filename`.
- Après le tuyau, `|`, appelez `wc` avec le drapeau `-l`.

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
      has_code("grep", incorrect_msg = "Avez-vous appelé `grep` ?"),
      has_code("2017-07", incorrect_msg = "Avez-vous recherché `2017-07` ?"),
      has_code("seasonal/spring.csv", incorrect_msg = "Avez-vous recherché dans le fichier `seasonal/spring.csv` ?"),
      has_code("|", incorrect_msg = "Avez-vous utilisé un pipe vers `wc` avec `|` ?"),      
      has_code("wc", incorrect_msg = "Avez-vous appelé `wc` ?"),
      has_code("-l", incorrect_msg = "Avez-vous compté les lignes avec `-l` ?")
    )
  )
)
Ex().success_msg("Comptage minutieux ! Déterminer la quantité de données que vous avez est une excellente première étape dans toute analyse de données.")
```

---

## Comment puis-je spécifier plusieurs fichiers à la fois ?

```yaml
type: ConsoleExercise
key: 602d47e70c
xp: 100
```

La plupart des commandes du shell fonctionneront sur plusieurs fichiers si vous leur donnez plusieurs noms de fichiers.
Par exemple :
vous pouvez obtenir la première colonne de tous les fichiers de données saisonnières en une seule fois, comme suit :

```{shell}
cut -d , -f 1 seasonal/winter.csv seasonal/spring.csv seasonal/summer.csv seasonal/autumn.csv
```

Mais taper plusieurs fois le nom de nombreux fichiers n'est pas une bonne idée :
cela fait perdre du temps,
et tôt ou tard, vous oublierez un fichier ou vous répéterez le nom d'un fichier.
Pour améliorer votre vie,
le shell vous permet d'utiliser des **caractères génériques** pour spécifier une liste de fichiers à l'aide d'une seule expression.
Le caractère générique le plus courant est `*`,
ce qui signifie "correspondre à zéro ou plusieurs caractères".
L'utiliser,
nous pouvons raccourcir la commande `cut` ci-dessus à ceci :

```{shell}
cut -d , -f 1 seasonal/*
```

ou :

```{shell}
cut -d , -f 1 seasonal/*.csv
```

`@instructions`
Écrivez une commande unique utilisant `head` pour obtenir les trois premières lignes de `seasonal/spring.csv` et `seasonal/summer.csv`, soit un total de six lignes de données, mais *pas* des fichiers de données d'automne ou d'hiver.
Utilisez un caractère de remplacement au lieu d'épeler le nom des fichiers en entier.

`@hint`
- La commande se présente sous la forme suivante : `head -n number_of_lines filename_pattern`.
- Vous pouvez faire correspondre des fichiers du répertoire `a`, commençant par `b`, à l'aide de `a/b*`, par exemple.

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
    has_expr_output(incorrect_msg = "Vous pouvez utiliser `seasonal/s*` pour sélectionner `seasonal/spring.csv` et `seasonal/summer.csv`. Assurez-vous de n'inclure que les trois premières lignes de chaque fichier avec l'option `-n` !"),
    check_not(has_output('==> seasonal/autumn.csv <=='), incorrect_msg = "N'incluez pas la sortie pour `seasonal/autumn.csv`. Vous pouvez utiliser `seasonal/s*` pour sélectionner `seasonal/spring.csv` et `seasonal/summer.csv`"),
    check_not(has_output('==> seasonal/winter.csv <=='), incorrect_msg = "N'incluez pas la sortie pour `seasonal/winter.csv`. Vous pouvez utiliser `seasonal/s*` pour sélectionner `seasonal/spring.csv` et `seasonal/summer.csv`")
)
Ex().success_msg("Excellent travail avec les jokers ! Cela devient encore plus important si votre répertoire contient des centaines ou des milliers de fichiers.")
```

---

## Quels autres caractères génériques puis-je utiliser ?

```yaml
type: PureMultipleChoiceExercise
key: f8feeacd8c
xp: 50
```

Le shell dispose également d'autres caractères génériques,
bien qu'ils soient moins utilisés :

- `?` correspond à un seul caractère, donc `201?.txt` correspondra à `2017.txt` ou `2018.txt`, mais pas à `2017-01.txt`.
- `[...]` correspond à n'importe lequel des caractères placés entre crochets, de sorte que `201[78].txt` correspond à `2017.txt` ou `2018.txt`, mais pas à `2016.txt`.
- `{...}` correspond à n'importe quel motif séparé par des virgules à l'intérieur des crochets, donc `{*.txt, *.csv}` correspond à tout fichier dont le nom se termine par `.txt` ou `.csv`, mais pas aux fichiers dont le nom se termine par `.pdf`.

<hr/>

Quelle expression correspondrait à `singh.pdf` et `johel.txt` mais *pas à* `sandhu.pdf` ou `sandhu.txt`?

`@hint`
Faites correspondre chaque expression à chaque nom de fichier à tour de rôle.

`@possible_answers`
- `[sj]*.{.pdf, .txt}`
- `{s*.pdf, j*.txt}`
- `[singh,johel]{*.pdf, *.txt}`
- [`{singh.pdf, j*.txt}`]

`@feedback`
- Non : `.pdf` et `.txt` ne sont pas des noms de fichiers.
- Non : cela correspondra à `sandhu.pdf`.
- Non : l'expression entre crochets ne correspond qu'à un seul caractère, pas à des mots entiers.
- C'est exact !

---

## Comment trier des lignes de texte ?

```yaml
type: ConsoleExercise
key: f06d9e310e
xp: 100
```

Comme son nom l'indique,
`sort` met de l'ordre dans les données.
Par défaut, il le fait dans l'ordre alphabétique croissant,
mais les drapeaux `-n` et `-r` peuvent être utilisés pour trier numériquement et inverser l'ordre de sortie,
tandis que `-b` lui demande d'ignorer les blancs en début de ligne
et `-f` lui indique de ne pas **tenir**compte de la casse (c'est-à-dire d'être insensible à la casse).
Les pipelines utilisent souvent `grep` pour se débarrasser des enregistrements indésirables.
puis `sort` pour mettre en ordre les enregistrements restants.

`@instructions`
Vous souvenez-vous de la combinaison de `cut` et `grep` pour sélectionner tous les noms de dents de la colonne 2 de `seasonal/summer.csv`?

```
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth
```

À partir de cette recette, classez les noms des dents dans `seasonal/winter.csv` (et non `summer.csv`) par ordre alphabétique décroissant. Pour ce faire, prolongez le pipeline par une étape `sort`.

`@hint`
Copiez et collez la commande dans les instructions, modifiez le nom du fichier, ajoutez un tuyau, puis appelez `sort` avec l'indicateur `-r`.

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
      has_code("cut", incorrect_msg = "Avez-vous appelé `cut` ?"),
      has_code("-d", incorrect_msg = "Avez-vous spécifié un délimiteur de champ avec `-d` ?"),
      has_code("seasonal/winter.csv", incorrect_msg = "Avez-vous obtenu des données du fichier `seasonal/winter.csv` ?"),
      has_code("|", incorrect_msg = "Avez-vous utilisé un pipe de `cut` à `grep` à `sort` en utilisant `|` ?"),      
      has_code("grep", incorrect_msg = "Avez-vous appelé `grep` ?"),
      has_code("-v", incorrect_msg = "Avez-vous inversé la correspondance avec `-v` ?"),
      has_code("Tooth", incorrect_msg = "Avez-vous recherché `Tooth` ?"),
      has_code("sort", incorrect_msg = "Avez-vous appelé `sort` ?"),
      has_code("-r", incorrect_msg = "Avez-vous inversé l'ordre de tri avec `-r` ?")
    )
  )
)
Ex().success_msg("Trié ! `sort` a de nombreuses utilisations. Par exemple, en envoyant `sort -n` à `head`, vous affichez les plus grandes valeurs.")
```

---

## Comment supprimer les lignes en double ?

```yaml
type: ConsoleExercise
key: ed77aed337
xp: 100
```

Une autre commande souvent utilisée avec `sort` est `uniq`,
dont le rôle est de supprimer les lignes dupliquées.
Plus précisément,
il supprime les lignes *adjacentes* dupliquées.
Si un fichier contient :

```
2017-07-03
2017-07-03
2017-08-03
2017-08-03
```

alors `uniq` produira :

```
2017-07-03
2017-08-03
```

mais s'il contient :

```
2017-07-03
2017-08-03
2017-07-03
2017-08-03
```

alors `uniq` imprimera les quatre lignes.
La raison en est que `uniq` est conçu pour travailler avec de très gros fichiers.
Pour supprimer les lignes non adjacentes d'un fichier,
il devrait garder tout le fichier en mémoire
(ou au moins,
toutes les lignes uniques observées jusqu'à présent).
En ne supprimant que les doublons adjacents,
il doit seulement garder en mémoire la ligne unique la plus récente.

`@instructions`
Ecrivez un pipeline à :

- obtenir la deuxième colonne à partir de `seasonal/winter.csv`,
- supprimer le mot "Tooth" de la sortie afin que seuls les noms des dents soient affichés,
- trier les résultats de manière à ce que toutes les occurrences d'un nom de dent particulier soient adjacentes ; et
- affiche une fois le nom de chaque dent ainsi que le nombre de fois où elle apparaît.

Le début de votre pipeline est le même que celui de l'exercice précédent :

```
cut -d , -f 2 seasonal/winter.csv | grep -v Tooth
```

Étendez-le avec une commande `sort` et utilisez `uniq -c` pour afficher les lignes uniques avec un décompte de leur fréquence d'apparition plutôt que d'utiliser `uniq` et `wc`.

`@hint`
Copiez et collez la commande dans les instructions, envoyez-la à `sort` sans drapeau, puis envoyez-la à nouveau à `uniq` avec un drapeau `-c`.

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
                     incorrect_msg="Vous devriez commencer par cette commande : `cut -d , -f 2 seasonal/winter.csv | grep -v Tooth`. Maintenant, étendez-la !"),
            has_code('\|\s+sort', incorrect_msg="Avez-vous étendu la commande avec `| sort` ?"),
            has_code('\|\s+uniq', incorrect_msg="Avez-vous étendu la commande avec `| uniq` ?"),
            has_code('-c', incorrect_msg="Avez-vous inclus les comptes avec `-c` ?")
        )
    )
)
Ex().success_msg("Super ! Après tout ce travail sur un pipe, ce serait bien si nous pouvions stocker le résultat, non ?")
```

---

## Comment puis-je enregistrer la sortie d'un tube ?

```yaml
type: MultipleChoiceExercise
key: 4115aa25b2
xp: 50
```

Le shell nous permet de rediriger la sortie d'une séquence de commandes pipées :

```{shell}
cut -d , -f 2 seasonal/*.csv | grep -v Tooth > teeth-only.txt
```

Cependant, `>` doit apparaître à la fin du pipeline :
si nous essayons de l'utiliser au milieu, comme ceci :

```{shell}
cut -d , -f 2 seasonal/*.csv > teeth-only.txt | grep -v Tooth
```

toutes les données de sortie de `cut` sont alors écrites sur `teeth-only.txt`,
Il ne reste donc plus rien pour `grep`
et il attend indéfiniment une entrée.

<hr>

Que se passe-t-il si nous plaçons la redirection au début d'un pipeline, comme dans :

```{shell}
> result.txt head -n 3 seasonal/winter.csv
```

`@possible_answers`
- [La sortie de la commande est redirigée vers le fichier comme d'habitude.]
- Le shell signale qu'il s'agit d'une erreur.
- Le shell attend une entrée pour toujours.

`@hint`
Essayez-le dans le shell.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
Ex().has_chosen(1, ['Correct!', 'Non ; le shell peut en fait exécuter cela.', 'Non ; le shell peut en fait exécuter cela.'])
```

---

## Comment arrêter un programme en cours d'exécution ?

```yaml
type: ConsoleExercise
key: d1694dbdcd
xp: 100
```

Les commandes et les scripts que vous avez exécutés jusqu'à présent l'ont tous été rapidement,
mais certaines tâches peuvent prendre des minutes, des heures, voire des jours.
Vous pouvez également placer par erreur une redirection au milieu d'un pipeline,
ce qui a pour effet de le faire raccrocher.
Si vous décidez que vous ne voulez pas qu'un programme continue à fonctionner,
vous pouvez taper `Ctrl` + `C` pour y mettre fin.
Il est souvent écrit `^C` dans la documentation Unix ;
Notez que le "c" peut être en minuscule.

`@instructions`
Exécutez la commande :

```{shell}
head
```

sans arguments (de sorte qu'il attend une entrée qui ne viendra jamais)
puis arrêtez-la en tapant `Ctrl` + `C`.

`@hint`
Tapez simplement head, appuyez sur Enter et quittez le programme en cours avec `Ctrl` + `C`.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
# Simply type head, hit Enter and exit the running program with `Ctrl` + `C`.
```

`@sct`
```{python}
Ex().has_code(r'\s*head\s*', fixed=False, incorrect_msg="Avez-vous utilisé `head`?")
```

---

## Conclusion

```yaml
type: BulletConsoleExercise
key: 659d3caa48
xp: 100
```

Pour conclure,
vous construirez un pipeline pour déterminer combien d'enregistrements se trouvent dans le plus court des fichiers de données saisonnières.

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
Utilisez `wc` avec les paramètres appropriés pour répertorier le nombre de lignes dans tous les fichiers de données saisonnières.
(Utilisez un caractère de remplacement pour les noms de fichiers au lieu de les taper à la main).

`@hint`
Utilisez `-l` pour ne lister que les lignes et `*` pour faire correspondre les noms de fichiers.

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
      has_code("wc", incorrect_msg = "Avez-vous appelé `wc` ?"),
      has_code("-l", incorrect_msg = "Avez-vous compté le nombre de lignes avec `-l` ?"),
      has_code("seasonal/\*", incorrect_msg = "Avez-vous récupéré les données de tous les fichiers `seasonal/*` ?")
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
Ajoutez une autre commande à la précédente en utilisant un pipe pour supprimer la ligne contenant le mot "total".

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
      has_code("wc", incorrect_msg = "Avez-vous appelé `wc` ?"),
      has_code("-l", incorrect_msg = "Avez-vous compté le nombre de lignes avec `-l` ?"),
      has_code("seasonal/\*", incorrect_msg = "Avez-vous récupéré les données de tous les fichiers `seasonal/*` ?"),
      has_code("|", incorrect_msg = "Avez-vous utilisé un pipe de `wc` à `grep` avec `|` ?"),      
      has_code("grep", incorrect_msg = "Avez-vous appelé `grep` ?"),
      has_code("-v", incorrect_msg = "Avez-vous inversé la correspondance avec `-v` ?"),
      has_code("total", incorrect_msg = "Avez-vous recherché `total` ?")
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
Ajoutez deux étapes supplémentaires au pipeline qui utilisent `sort -n` et `head -n 1` pour trouver le fichier contenant le moins de lignes.

`@hint`
- Utilisez l'option `sort`'s `-n` pour effectuer un tri numérique.
- Utilisez l'option `head`'s `-n` ' pour ne conserver qu'une seule ligne.

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
      has_code("wc", incorrect_msg = "Avez-vous appelé `wc` ?"),
      has_code("-l", incorrect_msg = "Avez-vous compté le nombre de lignes avec `-l` ?"),
      has_code("seasonal/\*", incorrect_msg = "Avez-vous obtenu des données de tous les fichiers `seasonal/*` ?"),
      has_code("|", incorrect_msg = "Avez-vous utilisé `|` pour rediriger de `wc` à `grep` à `sort` à `head` ?"),      
      has_code("grep", incorrect_msg = "Avez-vous appelé `grep` ?"),
      has_code("-v", incorrect_msg = "Avez-vous inversé la correspondance avec `-v` ?"),
      has_code("total", incorrect_msg = "Avez-vous recherché `total` ?"),
      has_code("sort", incorrect_msg = "Avez-vous appelé `sort` ?"),
      has_code("-n", incorrect_msg = "Avez-vous spécifié le nombre de lignes à conserver avec `-n` ?"),
      has_code("1", incorrect_msg = "Avez-vous spécifié 1 ligne à conserver avec `-n 1` ?")
    )
  )
)
Ex().success_msg("Super ! Il s'avère que `autumn.csv` est le fichier avec le moins de lignes. Rendez-vous au chapitre 4 pour en savoir plus sur le traitement par lots !")
```
