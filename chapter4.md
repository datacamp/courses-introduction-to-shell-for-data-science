---
title: Traitement par lots
description: >-
  La plupart des commandes du shell traitent plusieurs fichiers à la fois. Ce
  chapitre vous montre comment créer vos propres pipelines. En cours de route,
  vous verrez comment le shell utilise des variables pour stocker des
  informations.
lessons:
  - nb_of_exercises: 10
    title: Comment le shell stocke-t-il les informations ?
---

## Comment le shell stocke-t-il les informations ?

```yaml
type: MultipleChoiceExercise
key: e4d5f4adea
xp: 50
```

Comme les autres programmes, le shell stocke des informations dans des variables.
Certains d'entre eux,
appelées **variables d'environnement**,
sont disponibles en permanence.
Les noms des variables d'environnement sont traditionnellement écrits en majuscules,
et quelques-unes des plus courantes sont présentées ci-dessous.

| Variable | Objectif                           | Valeur                 |
|----------|-----------------------------------|-----------------------|
| `HOME`   | Répertoire personnel de l'utilisateur             | `/home/repl`          |
| `PWD `   | Répertoire de travail actuel         | Identique à la commande `pwd`  |
| `SHELL`  | Quel est le programme shell utilisé ? | `/bin/bash`           |
| `USER`   | Utilisateur ID                         | `repl`                |

Pour obtenir une liste complète (qui est assez longue),
vous pouvez taper `set` dans le shell.

<hr>

Utilisez `set` et `grep` avec un tuyau pour afficher la valeur de `HISTFILESIZE`,
qui détermine le nombre d'anciennes commandes stockées dans votre historique de commandes.
Quelle est sa valeur ?

`@possible_answers`
- 10
- 500
- [2000]
- La variable n'existe pas.

`@hint`
Utilisez `set | grep HISTFILESIZE` pour obtenir la ligne dont vous avez besoin.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err1 = "Non : le shell enregistre plus d'historique que cela."
err2 = "Non : le shell enregistre plus d'historique que cela."
correct3 = "Correct : le shell enregistre 2000 anciennes commandes par défaut sur ce système."
err4 = "Non : la variable `HISTFILESIZE` est présente."
Ex().has_chosen(3, [err1, err2, correct3, err4])
```

---

## Comment imprimer la valeur d'une variable ?

```yaml
type: ConsoleExercise
key: afae0f33a7
xp: 100
```

Une façon plus simple de trouver la valeur d'une variable est d'utiliser une commande appelée `echo`, qui imprime ses arguments. Dactylographie

```{shell}
echo hello DataCamp!
```

empreintes

```
hello DataCamp!
```

Si vous essayez de l'utiliser pour imprimer la valeur d'une variable comme ceci :

```{shell}
echo USER
```

il imprimera le nom de la variable, `USER`.

Pour obtenir la valeur de la variable, vous devez la faire précéder du signe du dollar `$`. Dactylographie 

```{shell}
echo $USER
```

empreintes

```
repl
```

C'est le cas partout :
pour obtenir la valeur d'une variable appelée `X`,
vous devez écrire `$X`.
(Cela permet au shell de savoir si vous voulez dire "un fichier nommé X"
ou "la valeur d'une variable nommée X").

`@instructions`
La variable `OSTYPE` contient le nom du type de système d'exploitation que vous utilisez.
Affichez sa valeur à l'aide de `echo`.

`@hint`
Appelez `echo` avec la variable `OSTYPE` précédée de `$`.

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
            has_code('echo', incorrect_msg="Avez-vous appelé `echo` ?"),
            has_code('OSTYPE', incorrect_msg="Avez-vous imprimé la variable d'environnement `OSTYPE` ?"),
            has_code(r'\$OSTYPE', incorrect_msg="Assurez-vous de faire précéder `OSTYPE` par un `$`.")
        )
    )
)
Ex().success_msg("Excellent écho des variables d'environnement ! Vous commencez bien. Continuons !")
```

---

## Comment le shell stocke-t-il d'autres informations ?

```yaml
type: BulletConsoleExercise
key: e925da48e4
xp: 100
```

L'autre type de variable est appelé **variable shell**,
qui est comme une variable locale dans un langage de programmation.

Pour créer une variable shell,
vous attribuez simplement une valeur à un nom :

```{shell}
training=seasonal/summer.csv
```

*sans* espace avant ou après le signe `=`.
Une fois que vous avez fait cela,
vous pouvez vérifier la valeur de la variable avec :

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
Définissez une variable appelée `testing` avec la valeur `seasonal/winter.csv`.

`@hint`
Il *ne* doit *pas* y avoir d'espace entre le nom de la variable et sa valeur.

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
        has_code('testing', incorrect_msg='Avez-vous défini une variable shell nommée `testing` ?'),
        has_code('testing=', incorrect_msg='Avez-vous écrit `=` directement après testing, sans espaces ?'),
        has_code('=seasonal/winter\.csv', incorrect_msg='Avez-vous défini la valeur de `testing` à `seasonal/winter.csv` ?')
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
Utilisez `head -n 1 SOMETHING` pour obtenir la première ligne de `seasonal/winter.csv`
en utilisant la valeur de la variable `testing` au lieu du nom du fichier.

`@hint`
N'oubliez pas d'utiliser `$testing` au lieu de `testing`
(le site `$` est nécessaire pour obtenir la valeur de la variable).

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
    has_code(r'\$testing', incorrect_msg="Avez-vous référencé la variable shell en utilisant `$testing` ?"),
    check_correct(
        has_output('^Date,Tooth\s*$'),
        multi(
            has_code('head', incorrect_msg="Avez-vous appelé `head` ?"),
            has_code('-n', incorrect_msg="Avez-vous limité le nombre de lignes avec `-n` ?"),
            has_code(r'-n\s+1', incorrect_msg="Avez-vous choisi de conserver 1 ligne avec `-n 1` ?")     
        )
    )
)
Ex().success_msg("Stellaire ! Voyons comment vous pouvez répéter les commandes facilement.")
```

---

## Comment puis-je répéter une commande plusieurs fois ?

```yaml
type: ConsoleExercise
key: 920d1887e3
xp: 100
```

Les variables shell sont également utilisées dans les **boucles**,
qui répètent les commandes plusieurs fois.
Si nous exécutons cette commande :

```{shell}
for filetype in gif jpg png; do echo $filetype; done
```

qu'il produit :

```
gif
jpg
png
```

Remarquez les éléments suivants concernant la boucle :

1. La structure est la suivante : `for`...variable... `in`...liste... `; do`...corps... `; done`
2. La liste des éléments que la boucle doit traiter (dans notre cas, les mots `gif`, `jpg`, et `png`).
3. La variable qui indique la chose que la boucle est en train de traiter (dans notre cas, `filetype`).
4. Le corps de la boucle qui effectue le traitement (dans notre cas, `echo $filetype`).

Remarquez que le corps utilise `$filetype` pour obtenir la valeur de la variable au lieu de `filetype`,
comme avec n'importe quelle autre variable du shell.
Remarquez également l'emplacement des points-virgules :
le premier vient entre la liste et le mot-clé `do`,
et le second se situe entre le corps et le mot-clé `done`.

`@instructions`
Modifiez la boucle pour qu'elle s'imprime :

```
docx
odt
pdf
```

Veuillez utiliser `filetype` comme nom de la variable de la boucle.

`@hint`
Utilisez la structure de code du texte d'introduction, en remplaçant les types de fichiers images par des types de fichiers documents.

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
      has_code('for', incorrect_msg='Avez-vous appelé `for` ?'),
      has_code('filetype', incorrect_msg='Avez-vous utilisé `filetype` comme variable de boucle ?'),
      has_code('in', incorrect_msg='Avez-vous utilisé `in` avant la liste des types de fichiers ?'),
      has_code('docx odt pdf', incorrect_msg='Avez-vous bouclé sur `docx`, `odt` et `pdf` dans cet ordre ?'),
      has_code(r'pdf\s*;', incorrect_msg='Avez-vous mis un point-virgule après le dernier élément de la boucle ?'),
      has_code(r';\s*do', incorrect_msg='Avez-vous utilisé `do` après le premier point-virgule ?'),
      has_code('echo', incorrect_msg='Avez-vous appelé `echo` ?'),
      has_code(r'\$filetype', incorrect_msg='Avez-vous fait écho de `$filetype` ?'),
      has_code(r'filetype\s*;', incorrect_msg='Avez-vous mis un point-virgule après le corps de la boucle ?'),
      has_code('; done', incorrect_msg='Avez-vous terminé avec `done` ?')
    )
  )
)
Ex().success_msg("Excellent pour la boucle ! Les boucles sont brillantes si vous voulez faire la même chose des centaines ou des milliers de fois.")
```

---

## Comment puis-je répéter une commande pour chaque fichier ?

```yaml
type: ConsoleExercise
key: 8468b70a71
xp: 100
```

Vous pouvez toujours saisir les noms des fichiers que vous souhaitez traiter lorsque vous écrivez la boucle,
mais il est généralement préférable d'utiliser des caractères génériques.
Essayez d'exécuter cette boucle dans la console :

```{shell}
for filename in seasonal/*.csv; do echo $filename; done
```

Il imprime :

```
seasonal/autumn.csv
seasonal/spring.csv
seasonal/summer.csv
seasonal/winter.csv
```

parce que le shell interprète `seasonal/*.csv` comme une liste de quatre noms de fichiers
avant d'exécuter la boucle.

`@instructions`
Modifiez l'expression générique en `people/*`
pour que la boucle imprime les noms des fichiers dans le répertoire `people` 
indépendamment du suffixe qu'ils ont ou n'ont pas.
Veuillez utiliser `filename` comme nom de votre variable de boucle.

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
      has_code('for', incorrect_msg='Avez-vous appelé `for` ?'),
      has_code('filename', incorrect_msg='Avez-vous utilisé `filename` comme variable de boucle ?'),
      has_code('in', incorrect_msg='Avez-vous utilisé `in` avant la liste des types de fichiers ?'),
      has_code('people/\*', incorrect_msg='Avez-vous spécifié une liste de fichiers avec `people/*` ?'),
      has_code(r'people/\*\s*;', incorrect_msg='Avez-vous mis un point-virgule après la liste des fichiers ?'),
      has_code(r';\s*do', incorrect_msg='Avez-vous utilisé `do` après le premier point-virgule ?'),
      has_code('echo', incorrect_msg='Avez-vous appelé `echo` ?'),
      has_code(r'\$filename', incorrect_msg='Avez-vous fait écho à `$filename` ?'),
      has_code(r'filename\s*;', incorrect_msg='Avez-vous mis un point-virgule après le corps de la boucle ?'),
      has_code('; done', incorrect_msg='Avez-vous terminé avec `done` ?')
    )
  )
)
Ex().success_msg("Bouclage en boucle ! Les jokers et les boucles forment une combinaison puissante.")
```

---

## Comment enregistrer les noms d'un ensemble de fichiers ?

```yaml
type: MultipleChoiceExercise
key: 153ca10317
xp: 50
```

Il est fréquent de définir une variable à l'aide d'une expression générique pour enregistrer une liste de noms de fichiers.
Par exemple :
si vous définissez `datasets` comme suit :

```{shell}
datasets=seasonal/*.csv
```

vous pouvez afficher les noms des fichiers ultérieurement :

```{shell}
for filename in $datasets; do echo $filename; done
```

Cela permet d'économiser la frappe et de réduire les risques d'erreur.

<hr>

Si vous exécutez ces deux commandes dans votre répertoire personnel,
combien de lignes de sortie seront-elles imprimées ?

```{shell}
files=seasonal/*.csv
for f in $files; do echo $f; done
```

`@possible_answers`
- None : puisque `files` est défini sur une ligne distincte, il n'a pas de valeur dans la deuxième ligne.
- Premièrement, le mot "dossiers".
- Quatre : les noms des quatre fichiers de données saisonnières.

`@hint`
Rappelez-vous que `X` est simplement "X", tandis que `$X` est la valeur de la variable `X`.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err1 = "Non : vous n'avez pas à définir une variable sur la même ligne que vous l'utilisez."
err2 = "Non : cet exemple définit et utilise la variable `files` dans le même shell."
correct3 = "Correct. La commande est équivalente à `for f in seasonal/*.csv; do echo $f; done`."
Ex().has_chosen(3, [err1, err2, correct3])
```

---

## Le nom d'une variable par rapport à sa valeur

```yaml
type: PureMultipleChoiceExercise
key: 4fcfb63c4f
xp: 50
```

Une erreur fréquente est d'oublier d'utiliser `$` avant le nom d'une variable.
Lorsque vous faites cela,
le shell utilise le nom que vous avez saisi
plutôt que la valeur de cette variable.

Une erreur plus courante pour les utilisateurs expérimentés est de mal taper le nom de la variable.
Par exemple :
si vous définissez `datasets` comme suit :

```{shell}
datasets=seasonal/*.csv
```

puis tapez :

```{shell}
echo $datsets
```

le shell n'imprime rien,
car `datsets` (sans le deuxième "a") n'est pas défini.

<hr>

Si vous exécutez ces deux commandes dans votre répertoire personnel,
quelle est la sortie imprimée ?

```{shell}
files=seasonal/*.csv
for f in files; do echo $f; done
```

(Lisez attentivement la première partie de la boucle avant de répondre).

`@hint`
Rappelez-vous que `X` est simplement "X", tandis que `$X` est la valeur de la variable `X`.

`@possible_answers`
- [Une ligne : le mot "fichiers".]
- Quatre lignes : les noms des quatre fichiers de données saisonnières.
- Quatre lignes vides : la variable `f` n'a pas de valeur.

`@feedback`
- Correct : la boucle utilise `files` au lieu de `$files`, de sorte que la liste se compose du mot "fichiers".
- Non : la boucle utilise `files` au lieu de `$files`, de sorte que la liste se compose du mot "files" plutôt que de l'expansion de `files`.
- Non : la variable `f` est définie automatiquement par la boucle `for`.

---

## Comment puis-je exécuter plusieurs commandes dans une seule boucle ?

```yaml
type: ConsoleExercise
key: 39b5dcf81a
xp: 100
```

L'impression des noms de fichiers est utile pour le débogage,
mais l'objectif réel des boucles est de faire des choses avec plusieurs fichiers.
Cette boucle imprime la deuxième ligne de chaque fichier de données :

```{shell}
for file in seasonal/*.csv; do head -n 2 $file | tail -n 1; done
```

Elle a la même structure que les autres boucles que vous avez déjà vues :
tout ce qui est différent, c'est que son corps est un pipeline de deux commandes au lieu d'une seule.

`@instructions`
Ecrivez une boucle qui imprime la dernière entrée de juillet 2017 (`2017-07`) dans chaque fichier saisonnier. Il devrait produire un résultat similaire à :

```{shell}
grep 2017-07 seasonal/winter.csv | tail -n 1
```

mais pour **_chaque_** saisonnier séparément. Veuillez utiliser `file` comme nom de la variable de boucle et n'oubliez pas de parcourir en boucle la liste des fichiers `seasonal/*.csv` _(au lieu de "seasonal/winter.csv" comme dans l'exemple_).

`@hint`
Le corps de la boucle est la commande grep présentée dans les instructions, avec `seasonal/winter.csv` remplacé par `$file`.

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
  has_code('for', incorrect_msg='Avez-vous appelé `for` ?'),
  check_correct(
    has_expr_output(),
    multi(
      has_code('file', incorrect_msg='Avez-vous utilisé `file` comme variable de boucle ?'),
      has_code('in', incorrect_msg='Avez-vous utilisé `in` avant la liste des fichiers ?'),
      has_code('seasonal/\*', incorrect_msg='Avez-vous spécifié une liste de fichiers avec `seasonal/*` ?'),
      has_code(r'seasonal\/\*\.csv\s*;', incorrect_msg='Avez-vous mis un point-virgule après la liste des fichiers ?'),
      has_code(r';\s*do', incorrect_msg='Avez-vous utilisé `do` après le premier point-virgule ?'),
      has_code('grep', incorrect_msg='Avez-vous appelé `grep` ?'),
      has_code('2017-07', incorrect_msg='Avez-vous fait correspondre `2017-07` ?'),
      has_code(r'\$file', incorrect_msg='Avez-vous utilisé `$file` comme nom de la variable de boucle ?'),
      has_code(r'file\s*|', incorrect_msg='Avez-vous utilisé un pipe pour connecter votre deuxième commande ?'),
      has_code(r'tail\s*-n\s*1', incorrect_msg='Avez-vous utilisé `tail -n 1` pour imprimer la dernière entrée de chaque recherche dans votre deuxième commande ?'),
      has_code('; done', incorrect_msg='Avez-vous terminé avec `done` ?')
    )
  )
)

Ex().success_msg("Bouclage en boucle ! Les jokers et les boucles forment une combinaison puissante.")
```

---

## Pourquoi ne pas utiliser d'espaces dans les noms de fichiers ?

```yaml
type: PureMultipleChoiceExercise
key: b974b7f45a
xp: 50
```

Il est facile et judicieux de donner aux fichiers des noms à plusieurs mots comme `July 2017.csv`
lorsque vous utilisez un explorateur de fichiers graphique.
Cependant,
cela pose des problèmes lorsque vous travaillez dans le shell.
Par exemple :
supposons que vous souhaitiez renommer `July 2017.csv` en `2017 July data.csv`.
Vous ne pouvez pas taper :

```{shell}
mv July 2017.csv 2017 July data.csv
```

parce que le shell a l'impression que vous essayez de vous déplacer
quatre fichiers appelés `July`, `2017.csv`, `2017`, et `July` (à nouveau)
dans un répertoire appelé `data.csv`.
Au lieu de cela,
vous devez citer les noms des fichiers
de sorte que le shell traite chacun d'entre eux comme un seul paramètre :

```{shell}
mv 'July 2017.csv' '2017 July data.csv'
```

<hr>

Si vous avez deux fichiers appelés `current.csv` et `last year.csv`
(avec un espace dans son nom)
et vous tapez :

```{shell}
rm current.csv last year.csv
```

ce qui se passera :

`@hint`
Que pensez-vous qu'il va se passer si quelqu'un vous montre la commande et que vous ne savez pas quels fichiers existent ?

`@possible_answers`
- Le shell affiche un message d'erreur car `last` et `year.csv` n'existent pas.
- Le shell supprimera `current.csv`.
- [Les deux.]
- Rien.

`@feedback`
- Oui, mais ce n'est pas tout.
- Oui, mais ce n'est pas tout.
- C'est exact. Vous pouvez utiliser des guillemets simples, `'`, ou des guillemets doubles, `"`, autour des noms de fichiers.
- Malheureusement, ce n'est pas le cas.

---

## Comment puis-je faire plusieurs choses dans une seule boucle ?

```yaml
type: MultipleChoiceExercise
key: f6d0530991
xp: 50
```

Les boucles que vous avez vues jusqu'à présent ont toutes une seule commande ou un seul pipeline dans leur corps,
mais une boucle peut contenir n'importe quel nombre de commandes.
Pour dire à la coquille où se termine l'un et où commence le suivant,
vous devez les séparer par des points-virgules :

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

Supposons que vous oubliez le point-virgule entre les commandes `echo` et `head` dans la boucle précédente,
afin de demander au shell de s'exécuter :

```{shell}
for f in seasonal/*.csv; do echo $f head -n 2 $f | tail -n 1; done
```

Que fera le shell ?

`@possible_answers`
- Imprimer un message d'erreur.
- Imprimez une ligne pour chacun des quatre fichiers.
- Imprimez une ligne pour `autumn.csv` (le premier fichier).
- Imprimez la dernière ligne de chaque fichier.

`@hint`
Vous pouvez diriger la sortie de `echo` vers `tail`.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err1 = "Non : la boucle s'exécutera, elle ne fera simplement rien de sensé."
correct2 = "Oui : `echo` produit une ligne qui inclut le nom de fichier deux fois, que `tail` copie ensuite."
err3 = "Non : la boucle s'exécute une fois pour chacun des quatre noms de fichiers."
err4 = "Non : l'entrée de `tail` est la sortie de `echo` pour chaque nom de fichier."
Ex().has_chosen(2, [err1, correct2, err3, err4])
```
