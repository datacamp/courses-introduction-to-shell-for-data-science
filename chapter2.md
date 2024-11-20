---
title: Manipulation des données
description: >-
  Les commandes que vous avez vues dans le chapitre précédent vous permettent de
  déplacer des éléments dans le système de fichiers. Ce chapitre vous montrera
  comment travailler avec les données contenues dans ces fichiers. Les outils
  que nous utiliserons sont relativement simples, mais constituent des éléments
  de base solides.
lessons:
  - nb_of_exercises: 12
    title: Comment visualiser le contenu d'un fichier ?
---

## Comment visualiser le contenu d'un fichier ?

```yaml
type: ConsoleExercise
key: 8acc09ede3
xp: 100
```

Avant de renommer ou de supprimer des fichiers,
vous pouvez consulter leur contenu.
La manière la plus simple d'y parvenir est de consulter le site `cat`,
qui se contente d'afficher le contenu des fichiers à l'écran.
(Son nom est l'abréviation de "concaténer", qui signifie "relier des choses entre elles",
car il imprimera tous les fichiers dont vous lui donnez le nom, l'un après l'autre).

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
Imprimez le contenu de `course.txt` à l'écran.

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
    has_expr_output(incorrect_msg="Votre commande n'a pas généré la bonne sortie. Avez-vous utilisé `cat` suivi du nom du fichier, `course.txt` ?")
)
Ex().success_msg("Bien ! Examinons d'autres moyens de visualiser le contenu d'un fichier.")
```

---

## Comment visualiser le contenu d'un fichier morceau par morceau ?

```yaml
type: ConsoleExercise
key: d8a30a3f81
xp: 100
```

Vous pouvez utiliser `cat` pour imprimer des fichiers volumineux et faire défiler les résultats,
mais il est généralement plus pratique de **paginer la** sortie.
La commande originale pour ce faire s'appelait `more`,
mais elle a été remplacée par une commande plus puissante appelée `less`.
(Ce type de dénomination est ce qui passe pour de l'humour dans le monde Unix).
Lorsque vous `less` un fichier,
une page est affichée à la fois ;
vous pouvez appuyer sur la barre d'espacement pour revenir à la page précédente ou taper `q` pour quitter.

Si vous donnez à `less` les noms de plusieurs fichiers,
vous pouvez taper `:n` (deux points et un "n" minuscule) pour passer au fichier suivant,
`:p` pour revenir à la précédente,
ou `:q` pour arrêter.

Note : Si vous consultez les solutions des exercices qui utilisent `less`,
vous verrez une commande supplémentaire à la fin qui *désactive* la pagination
afin que nous puissions tester vos solutions de manière efficace.

`@instructions`
Utilisez `less seasonal/spring.csv seasonal/summer.csv` pour visualiser ces deux fichiers dans cet ordre.
Appuyez sur la barre d'espacement pour passer à la page suivante, sur `:n` pour passer au deuxième fichier et sur `:q` pour quitter.

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
                 incorrect_msg='Utilisez `less` et les noms de fichiers. N\'oubliez pas que `:n` vous déplace vers le fichier suivant.'),
        has_code(r'\s*less\s+seasonal/summer\.csv\s+seasonal/spring\.csv\s*')
    )
)
```

---

## Comment puis-je consulter le début d'un fichier ?

```yaml
type: MultipleChoiceExercise
key: 82bdc9af65
lang: shell
xp: 50
skills:
  - 1
```

La première chose que font la plupart des scientifiques des données lorsqu'ils reçoivent un nouvel ensemble de données à analyser est de
déterminer les champs qu'il contient et les valeurs de ces champs.
Si le jeu de données a été exporté d'une base de données ou d'une feuille de calcul,
il sera souvent stocké sous forme de **valeurs séparées par des virgules** (CSV).
Un moyen rapide de savoir ce qu'il contient est de regarder les premières lignes.

Nous pouvons le faire dans le shell à l'aide d'une commande appelée `head`.
Comme son nom l'indique,
il imprime les premières lignes d'un fichier
(où "quelques" signifie 10),
donc la commande :

```{shell}
head seasonal/summer.csv
```

affichent :

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

Que fait `head` s'il n'y a pas 10 lignes dans le fichier ?
(Pour le savoir, utilisez-le pour consulter le haut du site `people/agarwal.txt`).

`@possible_answers`
- Imprimer un message d'erreur car le fichier est trop court.
- Affichez autant de lignes qu'il y en a.
- Affichez suffisamment de lignes vierges pour porter le total à 10.

`@hint`
Quelle est la chose la plus utile qu'il puisse faire ?

`@pre_exercise_code`
```{python}

```

`@sct`
```{shell}
Ex().has_chosen(2, ["Incorrect : ce n'est pas la chose la plus utile qu'il pourrait faire.",
                    "Correct !",
                    "Incorrect : il serait impossible de le distinguer d'un fichier qui se termine par un tas de lignes vides."])
```

---

## Comment puis-je taper moins ?

```yaml
type: BulletConsoleExercise
key: 0b7b8ca8f7
xp: 100
```

L'un des outils du shell est la **complétion des tabulations**.
Si vous commencez à taper le nom d'un fichier et que vous appuyez ensuite sur la touche de tabulation,
le shell fera de son mieux pour compléter automatiquement le chemin.
Par exemple :
si vous tapez `sea` et appuyez sur la touche tabulation,
il remplira le nom du répertoire `seasonal/` (avec une barre oblique à la fin).
Si vous tapez ensuite `a` et tab,
il complétera le chemin comme `seasonal/autumn.csv`.

Si le chemin est ambigu,
comme `seasonal/s`,
en appuyant une deuxième fois sur la touche tab, vous obtiendrez une liste de possibilités.
Tapez un ou deux caractères supplémentaires pour rendre votre chemin plus précis.
puis appuyez sur la touche de tabulation
remplira le reste du nom.

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
Exécutez `head seasonal/autumn.csv` sans taper le nom complet du fichier.

`@hint`
Tapez autant de chemin que nécessaire, puis appuyez sur la touche de tabulation et répétez l'opération.

`@solution`
```{shell}
head seasonal/autumn.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_expr_output(incorrect_msg="Le vérificateur n'a pas pu trouver la bonne sortie dans votre commande. Êtes-vous sûr d'avoir appelé `head` sur `seasonal/autumn.csv` ?")
)
```

***

```yaml
type: ConsoleExercise
key: e249266733
xp: 50
```

`@instructions`
Exécutez `head seasonal/spring.csv` sans taper le nom complet du fichier.

`@hint`
Tapez autant de chemin que nécessaire, puis appuyez sur la touche de tabulation et répétez l'opération.

`@solution`
```{shell}
head seasonal/spring.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_expr_output(incorrect_msg="Le vérificateur n'a pas pu trouver la bonne sortie dans votre commande. Êtes-vous sûr d'avoir appelé `head` sur `seasonal/spring.csv` ?")
)
Ex().success_msg("Bon travail ! Une fois que vous vous habituez à utiliser la complétion par tabulation, cela vous fera gagner beaucoup de temps !")
```

---

## Comment puis-je contrôler ce que font les commandes ?

```yaml
type: ConsoleExercise
key: 9eb608f6c9
xp: 100
```

Vous ne voudrez pas toujours consulter les 10 premières lignes d'un fichier,
Le shell vous permet donc de modifier le comportement de `head`.
en lui attribuant un **drapeau de ligne de commande** (ou "drapeau" en abrégé).
Si vous exécutez la commande :

```{shell}
head -n 3 seasonal/summer.csv
```

`head` n'affichera que les trois premières lignes du fichier.
Si vous exécutez `head -n 100`,
il affichera les 100 premiers (en supposant qu'il y en ait autant),
et ainsi de suite.

Le nom d'un drapeau indique généralement son objectif
(par exemple, `-n` signifie "**nombre de**lignes").
Les drapeaux de commande ne doivent pas nécessairement être un `-` suivi d'une seule lettre,
mais c'est une convention largement utilisée.

Remarque : il est de bon ton de placer tous les drapeaux *avant les* noms de fichiers,
donc dans ce cours,
nous n'acceptons que les réponses qui vont dans ce sens.

`@instructions`
Affiche les 5 premières lignes de `winter.csv` dans le répertoire `seasonal`.

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
        has_expr_output(incorrect_msg="Êtes-vous sûr d'appeler `head` sur le fichier `seasonal/winter.csv` ?"),
        has_expr_output(strict=True, incorrect_msg="Êtes-vous sûr d'avoir utilisé le drapeau `-n 5` ?")
    ),
    check_not(has_output("2017-02-17,incisor"), incorrect_msg = "Êtes-vous sûr d'avoir utilisé le drapeau `-n 5` ?")
)
Ex().success_msg("Bien ! Avec cette technique, vous pouvez éviter que votre shell ne plante si vous souhaitez jeter un œil à de plus gros fichiers texte.")
```

---

## Comment répertorier tout ce qui se trouve sous un répertoire ?

```yaml
type: ConsoleExercise
key: f830d46419
xp: 100
```

Pour voir tout ce qui se trouve sous un répertoire,
quelle que soit sa profondeur d'imbrication,
vous pouvez donner le drapeau à `ls`  `-R`
(qui signifie "récursif").
Si vous utilisez `ls -R` dans votre répertoire personnel,
vous verrez quelque chose comme ceci :

```
backup          course.txt      people          seasonal

./backup:

./people:
agarwal.txt

./seasonal:
autumn.csv      spring.csv      summer.csv      winter.csv
```

Cette fonction permet d'afficher tous les fichiers et répertoires du niveau actuel,
puis tout ce qui se trouve dans chaque sous-répertoire,
et ainsi de suite.

`@instructions`
Pour vous aider à faire la part des choses,
`ls` a un autre drapeau `-F` qui imprime un `/` après le nom de chaque répertoire
et un `*` après le nom de chaque programme exécutable.
Exécutez `ls` avec deux drapeaux, `-R` et `-F`, et le chemin absolu vers votre répertoire personnel.
pour voir tout ce qu'il contient.
(L'ordre des drapeaux n'a pas d'importance, mais le nom du répertoire doit venir en dernier).

`@hint`
Votre répertoire personnel peut être spécifié en utilisant `~` ou `.` ou son chemin absolu.

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
  has_expr_output(incorrect_msg='Utilisez soit `ls -R -F` soit `ls -F -R` et le chemin `/home/repl`.'),
  has_expr_output(expr = "ls -R -F .", incorrect_msg='Utilisez soit `ls -R -F` soit `ls -F -R` et le chemin `/home/repl`.')
)
Ex().success_msg("C'est un aperçu assez soigné, n'est-ce pas ?")
```

---

## Comment puis-je obtenir de l'aide pour une commande ?

```yaml
type: BulletConsoleExercise
key: 7b90b8a7cd
xp: 100
```

Pour savoir ce que font les commandes,
Les gens avaient l'habitude d'utiliser la commande `man` 
(abréviation de "manuel").
Par exemple :
la commande `man head` permet d'obtenir ces informations :

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

`man` invoque automatiquement `less`,
Il se peut donc que vous deviez appuyer sur la barre d'espacement pour parcourir les informations.
et `:q` pour arrêter.

La description d'une ligne sous `NAME` vous indique brièvement ce que fait la commande,
et le résumé sous `SYNOPSIS` liste tous les drapeaux qu'il comprend.
Tout ce qui est facultatif est indiqué entre crochets `[...]`,
Les alternatives soit/soit sont séparées par `|`,
et les choses qui peuvent être répétées sont indiquées par `...`,
Ainsi, la page de manuel de `head` vous indique que vous pouvez *soit* donner un nombre de lignes avec `-n`
ou un comptage d'octets avec `-c`,
et que vous pouvez lui donner n'importe quel nombre de noms de fichiers.

Le problème du manuel Unix est que vous devez savoir ce que vous cherchez.
Si vous ne le faites pas,
vous pouvez faire une recherche sur [Stack Overflow](https://stackoverflow.com/),
poser une question sur les canaux Slack de DataCamp,
ou consultez les sections `SEE ALSO` des commandes que vous connaissez déjà.

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
Lisez la page de manuel de la commande `tail` pour en savoir plus.
ce que fait le signe `+` devant le numéro utilisé avec le drapeau `-n`.
(N'oubliez pas d'appuyer sur la barre d'espacement pour descendre dans la page et/ou de taper `q` pour quitter).

`@hint`
N'oubliez pas que `man` est l'abréviation de "manuel".

`@solution`
```{shell}
# Run the following command *without* '| cat':
man tail | cat

```

`@sct`
```{python}
Ex().has_code(r'\s*man\s+tail.*', incorrect_msg='Utilisez `man` et le nom de la commande.')
```

***

```yaml
type: ConsoleExercise
key: 6a07958ae0
xp: 50
```

`@instructions`
Utilisez `tail` avec l'indicateur `-n +7` pour afficher toutes les lignes de `seasonal/spring.csv`*à l'exception des* six premières.

`@hint`
Utilisez le signe "+" devant le nombre de lignes que vous souhaitez afficher.

`@solution`
```{shell}
tail -n +7 seasonal/spring.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_output('2017-09-07,molar', incorrect_msg="Appelez-vous `tail` sur `seasonal/spring.csv` ?"),
    has_expr_output(strict=True, incorrect_msg="Êtes-vous sûr d'avoir utilisé le drapeau `-n +7` ?")
)
```

---

## Comment sélectionner des colonnes dans un fichier ?

```yaml
type: MultipleChoiceExercise
key: 925e9d645a
xp: 50
```

`head` et `tail` vous permettent de sélectionner des lignes dans un fichier texte.
Si vous souhaitez sélectionner des colonnes,
vous pouvez utiliser la commande `cut`.
Il propose plusieurs options (utilisez `man cut` pour les explorer),
mais le plus courant est quelque chose comme :

```{shell}
cut -f 2-5,8 -d , values.csv
```

ce qui signifie
"sélectionnez les colonnes 2 à 5 et les colonnes 8,
en utilisant la virgule comme séparateur".
`cut` utilise `-f` (signifiant "champs") pour spécifier les colonnes
et `-d` (signifiant "délimiteur") pour spécifier le séparateur.
Vous devez spécifier ce dernier car certains fichiers peuvent utiliser des espaces, des tabulations ou des deux-points pour séparer les colonnes.

<hr>

Quelle commande permet de sélectionner la première colonne (contenant les dates) du fichier `spring.csv`?

`@possible_answers`
- `cut -d , -f 1 seasonal/spring.csv`
- `cut -d, -f1 seasonal/spring.csv`
- L'un ou l'autre.
- Ni l'un ni l'autre, car `-f` doit précéder `-d`.

`@hint`
L'ordre des drapeaux n'a pas d'importance.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
Ex().has_chosen(3, ['Oui, mais ce n\'est pas tout', 'Oui, mais ce n\'est pas tout', 'Correct ! Ajouter un espace après le drapeau est une bonne pratique, mais pas obligatoire.', 'Non, l\'ordre des drapeaux n\'a pas d\'importance'])
```

---

## Qu'est-ce qui ne peut pas être coupé ?

```yaml
type: MultipleChoiceExercise
key: b9bb10ae87
xp: 50
```

`cut` est un commandement simple d'esprit.
En particulier,
il ne comprend pas les chaînes de caractères entre guillemets.
Si, par exemple, votre dossier est :

```
Name,Age
"Johel,Ranjit",28
"Sharma,Rupinder",26
```

ensuite :

```{shell}
cut -f 2 -d , everyone.csv
```

produira :

```
Age
Ranjit"
Rupinder"
```

plutôt que l'âge de chacun,
car il pensera que la virgule entre le nom et le prénom est un séparateur de colonnes.

<hr>

Quelle est la sortie de `cut -d : -f 2-4` sur la ligne :

```
first:second:third:
```

(Notez les deux points à la fin).

`@possible_answers`
- `second`
- `second:third`
- `second:third:`
- Rien de tout cela, car il n'y a pas quatre champs.

`@hint`
Faites attention aux deux-points qui suivent.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
Ex().has_chosen(3, ['Non, il y en a plus.', 'Non, il y en a plus.', 'Correct ! Les deux-points à la fin créent un quatrième champ vide.', 'Non, `cut` fait de son mieux.'])
```

---

## Comment puis-je répéter les ordres ?

```yaml
type: TabConsoleExercise
key: 32c0d30049
xp: 100
```

L'un des principaux avantages de l'utilisation du shell est que
il vous permet de refaire les choses facilement.
Si vous exécutez quelques commandes,
vous pouvez ensuite appuyer sur la flèche vers le haut pour revenir en arrière.
Vous pouvez également utiliser les touches fléchées gauche et droite et la touche de suppression pour les modifier.
En appuyant sur la touche retour, vous exécuterez la commande modifiée.

Mieux encore, `history` imprimera une liste des commandes que vous avez exécutées récemment.
Chacune d'entre elles est précédée d'un numéro de série afin de faciliter la ré-exécution de certaines commandes :
tapez simplement `!55` pour réexécuter la 55ème commande de votre historique (si vous en avez autant).
Vous pouvez également réexécuter une commande en tapant un point d'exclamation suivi du nom de la commande,
tels que `!head` ou `!cut`,
qui réexécutera la dernière utilisation de cette commande.

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
Exécutez `head summer.csv` dans votre répertoire personnel (ce qui devrait échouer).

`@hint`
La complétion de la tabulation ne fonctionnera pas s'il n'y a pas de nom de fichier correspondant.

`@solution`
```{shell}
head summer.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_code(r'\s*head\s+summer.csv\s*', incorrect_msg="Utilisez `head` et un nom de fichier, `summer.csv`. Ne vous inquiétez pas si cela échoue. Cela devrait.")
)
```

***

```yaml
type: ConsoleExercise
key: cba6bf99a5
xp: 20
```

`@instructions`
Changez de répertoire pour `seasonal`.

`@hint`
N'oubliez pas que `cd` signifie "changer de répertoire".

`@solution`
```{shell}
cd seasonal

```

`@sct`
```{python}
Ex().check_correct(
  has_cwd('/home/repl/seasonal'),
  has_code('cd +seasonal', incorrect_msg="Si votre répertoire de travail actuel (vérifiez avec `pwd`) est `/home/repl`, vous pouvez vous déplacer vers le dossier `seasonal` avec `cd seasonal`.")
)
```

***

```yaml
type: ConsoleExercise
key: 74f5c8d2fc
xp: 20
```

`@instructions`
Exécutez à nouveau la commande `head` avec `!head`.

`@hint`
Ne tapez pas d'espace entre `!` et ce qui suit.

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
                        incorrect_msg='Utilisez `!head` pour répéter la commande `head`.'),
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
Utilisez `history` pour examiner ce que vous avez fait.

`@hint`
Notez que `history` affiche les commandes les plus récentes en dernier, de sorte qu'elles restent affichées à l'écran à la fin de l'exécution du programme.

`@solution`
```{shell}
history

```

`@sct`
```{python}
Ex().has_code(r'history', incorrect_msg='Utilisez `history` sans indicateurs pour obtenir une liste des commandes précédentes.')
```

***

```yaml
type: ConsoleExercise
key: 0629b2adf3
xp: 20
```

`@instructions`
Exécutez à nouveau `head` en utilisant `!` suivi d'un numéro de commande.

`@hint`
Ne tapez *pas* d'espace entre `!` et ce qui suit.

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
                        incorrect_msg='Avez-vous utilisé `!<un_nombre>` pour relancer le dernier `head` de l\'historique ?'),
        # The head cmd should appear twice, at positions 1 and 3, though this will change 
        # if the student typed a wrong answer.
        # Since we're also checking output, this should be niche enough to ignore.
        has_code(r'!3'),
        has_code(r'!1') 
    )
)
Ex().success_msg("Bien joué ! Passons au suivant !")
```

---

## Comment puis-je sélectionner des lignes contenant des valeurs spécifiques ?

```yaml
type: BulletConsoleExercise
key: adf1516acf
xp: 100
```

`head` et `tail` sélectionner des lignes,
`cut` sélectionne les colonnes,
et `grep` sélectionne les lignes en fonction de leur contenu.
Dans sa forme la plus simple,
`grep` prend un morceau de texte suivi d'un ou plusieurs noms de fichiers
et imprime toutes les lignes de ces fichiers qui contiennent ce texte.
Par exemple :
`grep bicuspid seasonal/winter.csv`
imprime les lignes de `winter.csv` qui contiennent "bicuspid".

`grep` peut également rechercher des modèles ;
nous les étudierons dans le prochain cours.
Ce qui est plus important pour l'instant, ce sont certains des drapeaux les plus courants de `grep`:

- `-c`: imprime un nombre de lignes correspondantes plutôt que les lignes elles-mêmes
- `-h`Pour la recherche de fichiers multiples : n *'* imprimez *pas* les noms des fichiers lors de la recherche de plusieurs fichiers
- `-i`: ignorez la casse (par exemple, traitez "Régression" et "régression" comme des correspondances).
- `-l`: imprime les noms des fichiers qui contiennent des correspondances, et non les correspondances.
- `-n`: imprime les numéros de ligne pour les lignes correspondantes
- `-v`: inverser la correspondance, c'est-à-dire n'afficher que les lignes qui ne correspondent *pas*.

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
Imprimez le contenu de toutes les lignes contenant le mot `molar` dans `seasonal/autumn.csv`
en exécutant une seule commande dans votre répertoire personnel. N'utilisez pas de drapeaux.

`@hint`
Utilisez `grep` avec le mot que vous recherchez et le nom du ou des fichiers à rechercher.

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
      has_code("grep", incorrect_msg = "Avez-vous appelé `grep` ?"),
      has_code("molar", incorrect_msg = "Avez-vous recherché `molar` ?"),
      has_code("seasonal/autumn.csv", incorrect_msg = "Avez-vous recherché dans le fichier `seasonal/autumn.csv` ?")
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
Inversez la correspondance pour trouver toutes les lignes qui *ne* contiennent *pas* le mot `molar` dans `seasonal/spring.csv`, et indiquez leur numéro de ligne.
Rappelez-vous qu'il est de bon ton de placer tous les drapeaux *avant d'* autres valeurs telles que les noms de fichiers ou le terme de recherche "molar".

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
      has_code("grep", incorrect_msg = "Avez-vous appelé `grep` ?"),
      has_code("-v", incorrect_msg = "Avez-vous inversé la correspondance avec `-v` ?"),
      has_code("-n", incorrect_msg = "Avez-vous affiché les numéros de ligne avec `-n` ?"),
      has_code("molar", incorrect_msg = "Avez-vous recherché `molar` ?"),
      has_code("seasonal/spring.csv", incorrect_msg = "Avez-vous recherché le fichier `seasonal/spring.csv` ?")
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
Comptez le nombre de lignes contenant le mot `incisor` dans `autumn.csv` et `winter.csv` combinés.
(Là encore, exécutez une seule commande à partir de votre répertoire personnel).

`@hint`
N'oubliez pas d'utiliser `-c` avec `grep` pour compter les lignes.

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
      has_code("grep", incorrect_msg = "Avez-vous appelé `grep` ?"),
      has_code("-c", incorrect_msg = "Avez-vous obtenu les comptes avec `-c` ?"),
      has_code("incisor", incorrect_msg = "Avez-vous recherché `incisor` ?"),
      has_code("seasonal/autumn.csv", incorrect_msg = "Avez-vous recherché le fichier `seasonal/autumn.csv` ?"),
      has_code("seasonal/winter.csv", incorrect_msg = "Avez-vous recherché le fichier `seasonal/winter.csv` ?")
    )
  )
)
```

---

## Pourquoi n'est-il pas toujours sûr de traiter les données comme du texte ?

```yaml
type: MultipleChoiceExercise
key: 11914639fc
xp: 50
```

La section `SEE ALSO` de la page de manuel pour `cut` fait référence à une commande appelée `paste`
qui peut être utilisé pour combiner des fichiers de données au lieu de les découper.

<hr>

Lisez la page de manuel pour `paste`,
puis lancez `paste` pour combiner les fichiers de données de l'automne et de l'hiver dans un seul tableau
en utilisant une virgule comme séparateur.
Qu'est-ce qui ne va pas dans le résultat du point de vue de l'analyse des données ?

`@possible_answers`
- Les en-têtes de colonne sont répétés.
- Les dernières lignes n'ont pas le bon nombre de colonnes.
- Certaines données de `winter.csv` sont manquantes.

`@hint`
Si vous `cut` la sortie de `paste` en utilisant des virgules comme séparateur,
produirait-il la bonne réponse ?

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err1 = 'Vrai, mais ce n\'est pas nécessairement une erreur.'
correct2 = 'Correct : joindre les lignes avec des colonnes crée seulement une colonne vide au début, pas deux.'
err3 = 'Non, toutes les données d\'hiver sont là.'
Ex().has_chosen(2, [err1, correct2, err3])
```
