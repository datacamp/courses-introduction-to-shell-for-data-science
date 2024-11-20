---
title: Manipulation de fichiers et de répertoires
description: >-
  Ce chapitre est une brève introduction au shell Unix. Vous apprendrez pourquoi
  il est toujours utilisé après presque 50 ans, comment il se compare aux outils
  graphiques qui vous sont peut-être plus familiers, comment vous déplacer dans
  le shell et comment créer, modifier et supprimer des fichiers et des dossiers.
free_preview: true
lessons:
  - nb_of_exercises: 12
    title: Comment le shell se compare-t-il à une interface de bureau ?
---

## Comment le shell se compare-t-il à une interface de bureau ?

```yaml
type: PureMultipleChoiceExercise
key: badd717ea4
xp: 50
```

Un système d'exploitation tel que Windows, Linux ou Mac OS est un type de programme particulier.
Il contrôle le processeur, le disque dur et la connexion réseau de l'ordinateur,
mais sa tâche la plus importante est d'exécuter d'autres programmes.

Les êtres humains n'étant pas numériques,
ils ont besoin d'une interface pour interagir avec le système d'exploitation.
Le plus courant aujourd'hui est l'explorateur de fichiers graphique,
qui traduit les clics et les double-clics en commandes permettant d'ouvrir des fichiers et d'exécuter des programmes.
Avant que les ordinateurs n'aient des écrans graphiques,
mais..,
les personnes tapaient des instructions dans un programme appelé **shell de ligne de commande**.
Chaque fois qu'une commande est introduite,
le shell exécute d'autres programmes,
imprime leurs résultats sous une forme lisible par l'homme,
puis affiche une *invite pour* signaler qu'il est prêt à accepter la commande suivante.
(Son nom vient du fait qu'il s'agit de la "shell" de l'ordinateur).

Taper des commandes au lieu de cliquer et de faire glisser peut sembler maladroit au début,
mais comme vous le verrez,
une fois que vous commencez à préciser ce que vous voulez que l'ordinateur fasse,
vous pouvez combiner d'anciennes commandes pour en créer de nouvelles
et automatiser les opérations répétitives
en quelques clics.

<hr>
Quelle est la relation entre l'explorateur de fichiers graphique que la plupart des gens utilisent et le shell de ligne de commande ?

`@hint`
Rappelez-vous qu'un utilisateur ne peut interagir avec un système d'exploitation que par l'intermédiaire d'un programme.

`@possible_answers`
- L'explorateur de fichiers vous permet de visualiser et de modifier des fichiers, tandis que le shell vous permet d'exécuter des programmes.
- L'explorateur de fichiers est construit au-dessus du shell.
- Le shell fait partie du système d'exploitation, tandis que l'explorateur de fichiers en est séparé.
- [Il s'agit de deux interfaces permettant d'envoyer des commandes au système d'exploitation.]

`@feedback`
- Tous deux vous permettent d'afficher et de modifier des fichiers et d'exécuter des programmes.
- Les explorateurs de fichiers graphiques et le shell font tous deux appel aux mêmes fonctions sous-jacentes du système d'exploitation.
- Le shell et l'explorateur de fichiers sont tous deux des programmes qui traduisent les commandes de l'utilisateur (tapées ou cliquées) en appels au système d'exploitation.
- C'est exact ! Les deux prennent les commandes de l'utilisateur (qu'elles soient tapées ou cliquées) et les envoient au système d'exploitation.

---

## Où suis-je ?

```yaml
type: MultipleChoiceExercise
key: 7c1481dbd3
xp: 50
```

Le **système de fichiers** gère les fichiers et les répertoires (ou dossiers).
Chacun est identifié par un **chemin absolu**
qui montre comment l'atteindre à partir du **répertoire racine** du système de fichiers :
`/home/repl` est le répertoire `repl` dans le répertoire `home`,
tandis que `/home/repl/course.txt` est un fichier `course.txt` dans ce répertoire,
et `/` est le répertoire racine.

Pour savoir où vous vous trouvez dans le système de fichiers,
exécutez la commande `pwd`
(abréviation de "**print** **working** **directory**").
Cette commande affiche le chemin absolu de votre **répertoire de travail actuel**,
qui est l'endroit où le shell exécute les commandes et recherche les fichiers par défaut.

<hr>
Run `pwd`.
Où êtes-vous en ce moment ?

`@possible_answers`
- `/home`
- `/repl`
- `/home/repl`

`@hint`
Les systèmes Unix placent généralement les répertoires personnels de tous les utilisateurs sous `/home`.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err = "Ce n'est pas le bon chemin."
correct = "Correct - vous êtes dans `/home/repl`."

Ex().has_chosen(3, [err, err, correct])
```

---

## Comment identifier les fichiers et les répertoires ?

```yaml
type: MultipleChoiceExercise
key: f5b0499835
xp: 50
```

`pwd` vous indique où vous vous trouvez.
Pour découvrir ce qui s'y trouve,
tapez `ls` (abréviation de "**listing**") et appuyez sur la touche entrée.
En soi,
`ls` liste le contenu de votre répertoire actuel
(celui affiché par `pwd`).
Si vous ajoutez les noms de certains fichiers,
`ls` en dressera la liste,
et si vous ajoutez les noms des répertoires,
la liste de leur contenu s'affichera.
Par exemple :
`ls /home/repl` vous indique ce qui se trouve dans votre répertoire de départ
(généralement appelé votre **répertoire personnel**).

<hr>
Utilisez `ls` avec un argument approprié pour lister les fichiers dans le répertoire `/home/repl/seasonal`
(qui contient des informations sur les cabinets dentaires par date, ventilées par saison).
Lequel de ces fichiers ne se trouve pas dans ce répertoire ?

`@possible_answers`
- `autumn.csv`
- `fall.csv`
- `spring.csv`
- `winter.csv`

`@hint`
Si vous donnez un chemin à `ls`, il indique ce qui se trouve sur ce chemin.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err = "Ce fichier se trouve dans le répertoire `seasonal`."
correct = "Correct - ce fichier ne se trouve *pas* dans le répertoire `seasonal`."

Ex().has_chosen(2, [err, correct, err, err])
```

---

## Comment puis-je identifier les fichiers et les répertoires ?

```yaml
type: BulletConsoleExercise
key: a766184b59
xp: 100
```

Un chemin absolu est comme une latitude et une longitude : il a la même valeur quel que soit l'endroit où vous vous trouvez. Un **chemin relatif**, en revanche, spécifie un emplacement à partir de l'endroit où vous vous trouvez : c'est comme dire "20 kilomètres au nord".

A titre d'exemple :
- Si vous vous trouvez dans le répertoire `/home/repl`, le chemin **relatif** `seasonal` indique le même répertoire que le chemin **absolu** `/home/repl/seasonal`. 
- Si vous vous trouvez dans le répertoire `/home/repl/seasonal`, le chemin **relatif** `winter.csv` spécifie le même fichier que le chemin **absolu** `/home/repl/seasonal/winter.csv`.

Le shell décide si un chemin est absolu ou relatif en regardant son premier caractère : S'il commence par `/`, il est absolu. S'il ne commence *pas* par `/`, il est relatif.

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
Vous êtes sur `/home/repl`. Utilisez `ls` avec un **chemin d'accès relatif** pour répertorier le fichier dont le chemin d'accès absolu est `/home/repl/course.txt` (et uniquement ce fichier).

`@hint`
Vous pouvez souvent construire le chemin relatif d'un fichier ou d'un répertoire situé en dessous de votre emplacement actuel
en soustrayant le chemin absolu de votre position actuelle
à partir du chemin absolu de la chose que vous voulez.

`@solution`
```{shell}
ls course.txt

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/home/repl"),
    has_code("ls", incorrect_msg = "Vous n'avez pas appelé `ls` pour générer la liste des fichiers."), # to prevent `echo "course.txt"`
    check_correct(
      has_expr_output(strict=True),
      has_code("ls +course.txt", incorrect_msg = "Votre commande n'a pas généré la liste de fichiers correcte. Utilisez `ls` suivi d'un chemin relatif vers `/home/repl/course.txt`.")
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
Vous êtes sur `/home/repl`.
Utilisez `ls` avec un chemin d'accès **relatif** 
pour répertorier le fichier `/home/repl/seasonal/summer.csv` (et uniquement ce fichier).

`@hint`
Les chemins relatifs ne commencent *pas* par un "/".

`@solution`
```{shell}
ls seasonal/summer.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/home/repl"),
    has_code("ls", incorrect_msg = "Vous n'avez pas appelé `ls` pour générer la liste des fichiers."), 
    check_correct(
      has_expr_output(strict=True),
      has_code("ls +seasonal/summer.csv", incorrect_msg = "Votre commande n'a pas généré la liste de fichiers correcte. Utilisez `ls` suivi d'un chemin relatif vers `/home/repl/seasonal/summer.csv`.")
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
Vous êtes sur `/home/repl`.
Utilisez `ls` avec un chemin d'accès **relatif** 
pour lister le contenu du répertoire `/home/repl/people`.

`@hint`
Les chemins relatifs ne commencent pas par un "/".

`@solution`
```{shell}
ls people

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/home/repl"),
    has_code("ls", incorrect_msg = "Vous n'avez pas appelé `ls` pour générer la liste des fichiers."), 
    check_correct(
      has_expr_output(strict=True),
      has_code("ls +people", incorrect_msg = "Votre commande n'a pas généré la liste correcte des fichiers. Utilisez `ls` suivi d'un chemin relatif vers `/home/repl/people`.")
    )
)
Ex().success_msg("Bien joué. Maintenant que vous savez comment lister les fichiers et les répertoires, voyons comment vous pouvez vous déplacer dans le système de fichiers !")
```

---

## Comment puis-je changer de répertoire ?

```yaml
type: BulletConsoleExercise
key: dbdaec5610
xp: 100
```

Tout comme vous pouvez vous déplacer dans un navigateur de fichiers en double-cliquant sur les dossiers,
vous pouvez vous déplacer dans le système de fichiers à l'aide de la commande `cd`
(qui signifie "changer de répertoire").

Si vous tapez `cd seasonal` puis `pwd`,
le shell vous indiquera que vous êtes maintenant sur `/home/repl/seasonal`.
Si vous exécutez ensuite `ls` de manière autonome,
il vous montre le contenu de `/home/repl/seasonal`,
car c'est là que vous êtes.
Si vous souhaitez revenir à votre répertoire d'origine `/home/repl`,
vous pouvez utiliser la commande `cd /home/repl`.

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
Vous êtes sur `/home/repl`/.
Changez de répertoire pour `/home/repl/seasonal` en utilisant un chemin d'accès relatif.

`@hint`
N'oubliez pas que `cd` signifie "changer de répertoire" et que les chemins relatifs ne commencent pas par un "/".

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
key: e69c8eac15
xp: 35
```

`@instructions`
Utilisez `pwd` pour vérifier que vous êtes bien là.

`@hint`
N'oubliez pas d'appuyer sur "enter" ou "return" après avoir saisi la commande.

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
Utilisez `ls` sans chemin d'accès pour voir ce qui se trouve dans ce répertoire.

`@hint`
N'oubliez pas d'appuyer sur "enter" ou "return" après la commande.

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
      has_code('ls', incorrect_msg="Votre commande n'a pas généré la sortie correcte. Avez-vous utilisé `ls` sans chemins pour afficher le contenu du répertoire courant ?")
    )
)

Ex().success_msg("Super ! Il s'agissait de naviguer vers les sous-répertoires. Qu'en est-il de remonter ? Découvrons-le !")
```

---

## Comment puis-je monter dans un répertoire ?

```yaml
type: PureMultipleChoiceExercise
key: 09c717ef76
xp: 50
```

Le **parent** d'un répertoire est le répertoire situé au-dessus de lui.
Par exemple, `/home` est le parent de `/home/repl`,
et `/home/repl` est le parent de `/home/repl/seasonal`.
Vous pouvez toujours donner le chemin absolu de votre répertoire parent à des commandes telles que `cd` et `ls`.
Plus souvent,
mais..,
vous profiterez du fait que le chemin spécial `..`
(deux points sans espace) signifie "le répertoire au-dessus de celui dans lequel je me trouve actuellement".
Si vous êtes sur `/home/repl/seasonal`,
Ensuite, `cd ..` vous fait passer à `/home/repl`.
Si vous utilisez à nouveau `cd ..`,
Il vous place sur le site `/home`.
Un autre `cd ..` vous place dans le *répertoire racine* `/`,
qui est le sommet du système de fichiers.
(N'oubliez pas de mettre un espace entre `cd` et `..` - il s'agit d'une commande et d'un chemin, et non d'une seule commande de quatre lettres).

Un point seul, `.`, signifie toujours "le répertoire actuel",
Ainsi, `ls` et `ls .` font la même chose,
tandis que `cd .` n'a aucun effet
(parce qu'il vous déplace dans le répertoire dans lequel vous vous trouvez actuellement).

Un dernier chemin spécial est `~` (le caractère tilde),
ce qui signifie "votre répertoire personnel",
comme `/home/repl`.
Où que vous soyez,
`ls ~` affichera toujours le contenu de votre répertoire personnel,
et `cd ~` vous ramènera toujours à la maison.

<hr>
Si vous êtes dans `/home/repl/seasonal`,
où vous mène `cd ~/../.` ?

`@hint`
Tracez le chemin un répertoire à la fois.

`@possible_answers`
- `/home/repl`
- [`/home`]
- `/home/repl/seasonal`
- `/` (le répertoire racine)

`@feedback`
- Non, mais l'un ou l'autre des sites `~` ou `..`, pris isolément, vous permettrait d'atteindre cet objectif.
- C'est exact ! Le chemin d'accès signifie "répertoire personnel", "à un niveau supérieur", "ici".
- Non, mais `.` le ferait à lui seul.
- Non, la dernière partie du chemin est `.` (signifiant "ici") et non `..` (signifiant "en haut").

---

## Comment puis-je copier des fichiers ?

```yaml
type: BulletConsoleExercise
key: 832de9e74c
xp: 100
```

Vous souhaiterez souvent copier des fichiers,
les déplacer dans d'autres répertoires pour les organiser,
ou les renommer.
L'une des commandes permettant d'effectuer cette opération est `cp`, qui est l'abréviation de "copy" (copier).
Si `original.txt` est un fichier existant,
ensuite :

```{shell}
cp original.txt duplicate.txt
```

crée une copie de `original.txt` appelée `duplicate.txt`.
S'il existe déjà un fichier appelé `duplicate.txt`,
il est écrasé.
Si le dernier paramètre de `cp` est un répertoire existant,
puis une commande comme :

```{shell}
cp seasonal/autumn.csv seasonal/winter.csv backup
```

copie *tous les* fichiers dans ce répertoire.

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
Faites une copie de `seasonal/summer.csv` dans le répertoire `backup` (qui se trouve également dans `/home/repl`),
appeler le nouveau fichier `summer.bck`.

`@hint`
Combinez le nom du répertoire de destination et le nom du fichier copié.
pour créer un chemin relatif pour le nouveau fichier.

`@solution`
```{shell}
cp seasonal/summer.csv backup/summer.bck

```

`@sct`
```{python}
Ex().check_correct(
    check_file('/home/repl/backup/summer.bck', missing_msg="`summer.bck` ne semble pas exister dans le répertoire `backup`. Fournissez deux chemins à `cp` : le fichier existant (`seasonal/summer.csv`) et le fichier de destination (`backup/summer.bck`)."),
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
Copiez `spring.csv` et `summer.csv` du répertoire `seasonal` dans le répertoire `backup`.
*sans* modifier votre répertoire de travail actuel (`/home/repl`).

`@hint`
Utilisez `cp` avec les noms des fichiers que vous souhaitez copier
*puis* le nom du répertoire dans lequel les copier.

`@solution`
```{shell}
cp seasonal/spring.csv seasonal/summer.csv backup

```

`@sct`
```{python}
patt = "`%s` ne semble pas avoir été copié dans le répertoire `backup`. Fournissez deux noms de fichiers et un nom de répertoire à `cp`."
Ex().multi(
    has_cwd('/home/repl', incorrect_msg="Assurez-vous de copier les fichiers tout en étant dans `{{dir}}` ! Utilisez `cd {{dir}}` pour y revenir."),
    check_file('/home/repl/backup/spring.csv', missing_msg=patt%'spring.csv'),
    check_file('/home/repl/backup/summer.csv', missing_msg=patt%'summer.csv')
)
Ex().success_msg("Bon travail. En plus de copier, nous devrions également être capables de déplacer des fichiers d'un répertoire à un autre. Apprenez-en plus dans l'exercice suivant !")
```

---

## Comment déplacer un fichier ?

```yaml
type: ConsoleExercise
key: 663a083a3c
xp: 100
```

Tandis que `cp` copie un fichier,
`mv` le déplace d'un répertoire à un autre,
comme si vous l'aviez fait glisser dans un navigateur de fichiers graphiques.
Il traite ses paramètres de la même manière que `cp`,
donc la commande :

```{shell}
mv autumn.csv winter.csv ..
```

déplace les fichiers `autumn.csv` et `winter.csv` du répertoire de travail actuel
remonter d'un niveau vers son répertoire parent
(car `..` fait toujours référence au répertoire situé au-dessus de votre emplacement actuel).

`@instructions`
Vous êtes dans le répertoire `/home/repl`, qui contient les sous-répertoires `seasonal` et `backup`.
En utilisant une seule commande, déplacez `spring.csv` et `summer.csv` de `seasonal` à `backup`.

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
backup_patt="Le fichier `%s` n'est pas dans le répertoire `backup`. Avez-vous utilisé `mv` correctement ? Utilisez deux noms de fichiers et un répertoire comme paramètres pour `mv`."
seasonal_patt="Le fichier `%s` est toujours dans le répertoire `seasonal`. Assurez-vous de déplacer les fichiers avec `mv` plutôt que de les copier avec `cp` !"
Ex().multi(
    check_file('/home/repl/backup/spring.csv', missing_msg=backup_patt%'spring.csv'),
    check_file('/home/repl/backup/summer.csv', missing_msg=backup_patt%'summer.csv'),
    check_not(check_file('/home/repl/seasonal/spring.csv'), incorrect_msg=seasonal_patt%'spring.csv'),
    check_not(check_file('/home/repl/seasonal/summer.csv'), incorrect_msg=seasonal_patt%'summer.csv')
)
Ex().success_msg("Bien joué, continuons sur cette lancée avec le shell !")
```

---

## Comment renommer des fichiers ?

```yaml
type: BulletConsoleExercise
key: 001801a652
xp: 100
```

`mv` peut également être utilisé pour renommer des fichiers. Si vous courez :

```{shell}
mv course.txt old-course.txt
```

le fichier `course.txt` du répertoire de travail actuel est "déplacé" vers le fichier `old-course.txt`.
Ce fonctionnement est différent de celui des navigateurs de fichiers,
mais elle est souvent pratique.

Un avertissement :
tout comme `cp`,
`mv` écrasera les fichiers existants.
Si,
par exemple,
vous avez déjà un fichier appelé `old-course.txt`,
alors la commande ci-dessus le remplacera par ce qui se trouve dans `course.txt`.

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
Allez dans le répertoire `seasonal`.

`@hint`
N'oubliez pas que `cd` signifie "changer de répertoire" et que les chemins relatifs ne commencent pas par un "/".

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
key: ed5fe1df23
xp: 35
```

`@instructions`
Renommez le fichier `winter.csv` en `winter.csv.bck`.

`@hint`
Utilisez `mv` avec le nom actuel du fichier et le nom que vous souhaitez lui donner dans cet ordre.

`@solution`
```{shell}
mv winter.csv winter.csv.bck

```

`@sct`
```{python}
hint = " Utilisez `mv` avec deux arguments : le fichier que vous souhaitez renommer (`winter.csv`) et le nouveau nom pour le fichier (`winter.csv.bck`)."
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    multi(
        check_file('/home/repl/seasonal/winter.csv.bck', missing_msg="Nous nous attendions à trouver `winter.csv.bck` dans le répertoire." + hint),
        check_not(check_file('/home/repl/seasonal/winter.csv'), incorrect_msg="Nous ne nous attendions plus à ce que `winter.csv` soit dans le répertoire." + hint)
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
Lancez le site `ls` pour vérifier que tout a fonctionné.

`@hint`
N'oubliez pas d'appuyer sur "enter" ou "return" pour exécuter la commande.

`@solution`
```{shell}
ls

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    has_expr_output(incorrect_msg="Avez-vous utilisé `ls` pour lister le contenu de votre répertoire de travail actuel ?")
)
Ex().multi(
    has_cwd("/home/repl/seasonal"),
    check_correct(
      has_expr_output(strict=True),
      has_code("ls", incorrect_msg = "Votre commande n'a pas généré la liste de fichiers correcte. Utilisez `ls` sans arguments pour lister le contenu de votre répertoire de travail actuel.")
    )
)
Ex().success_msg("Copier, déplacer, renommer, vous avez tout compris ! Ensuite : supprimer des fichiers.")
```

---

## Comment supprimer des fichiers ?

```yaml
type: BulletConsoleExercise
key: '2734680614'
xp: 100
```

Nous pouvons copier des fichiers et les déplacer ;
pour les supprimer,
Nous utilisons `rm`,
qui signifie "supprimer".
Comme pour `cp` et `mv`,
vous pouvez donner à `rm` les noms d'autant de fichiers que vous le souhaitez :

```{shell}
rm thesis.txt backup/thesis-2017-08.txt
```

supprime à la fois `thesis.txt` et `backup/thesis-2017-08.txt`

`rm` fait exactement ce que son nom indique,
et il le fait immédiatement :
contrairement aux navigateurs de fichiers graphiques,
le shell n'a pas de poubelle,
Ainsi, lorsque vous tapez la commande ci-dessus,
votre thèse a disparu pour de bon.

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
Vous êtes sur `/home/repl`.
Allez dans le répertoire `seasonal`.

`@hint`
N'oubliez pas que `cd` signifie "changer de répertoire" et qu'un chemin relatif ne commence pas par un "/".

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
Retirez `autumn.csv`.

`@hint`
N'oubliez pas que `rm` est l'abréviation de "remove" (supprimer).

`@solution`
```{shell}
rm autumn.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    check_not(check_file('/home/repl/seasonal/autumn.csv'), incorrect_msg="Nous ne nous attendions pas à ce que `autumn.csv` soit encore dans le répertoire `seasonal`. Utilisez `rm` avec le chemin du fichier que vous souhaitez supprimer."),
    has_code('rm', incorrect_msg = 'Utilisez `rm` pour supprimer le fichier, plutôt que de le déplacer.')
)
```

***

```yaml
type: ConsoleExercise
key: 09f2d105cd
xp: 25
```

`@instructions`
Retournez dans votre répertoire personnel.

`@hint`
Si vous utilisez `cd` sans aucun chemin, il vous ramène à la maison.

`@solution`
```{shell}
cd

```

`@sct`
```{python}
Ex().has_cwd('/home/repl', incorrect_msg="Utilisez `cd ..` ou `cd ~` pour revenir au répertoire personnel.")
```

***

```yaml
type: ConsoleExercise
key: 9eaf49744c
xp: 25
```

`@instructions`
Supprimez `seasonal/summer.csv` sans changer à nouveau de répertoire.

`@hint`
N'oubliez pas que `rm` est l'abréviation de "remove" (supprimer).

`@solution`
```{shell}
rm seasonal/summer.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_not(check_file('/home/repl/seasonal/summer.csv'), incorrect_msg="Nous ne nous attendions pas à ce que `summer.csv` soit encore dans le répertoire `seasonal`. Utilisez `rm` avec le chemin vers le fichier que vous souhaitez supprimer."),
    has_code('rm', incorrect_msg = 'Utilisez `rm` pour supprimer le fichier, plutôt que de le déplacer.')
)
Ex().success_msg("Impressionnant ! Passons au suivant !")
```

---

## Comment puis-je créer et supprimer des répertoires ?

```yaml
type: BulletConsoleExercise
key: 63e8fbd0c2
xp: 100
```

`mv` traite les répertoires de la même manière que les fichiers :
si vous êtes dans votre répertoire personnel et que vous exécutez `mv seasonal by-season`,
par exemple,
`mv` change le nom du répertoire `seasonal` en `by-season`.
Cependant,
`rm` fonctionne différemment.

Si vous essayez de `rm` un répertoire,
le shell affiche un message d'erreur vous indiquant qu'il ne peut pas le faire,
principalement pour vous éviter d'effacer accidentellement un répertoire entier de travaux.
Au lieu de cela,
vous pouvez utiliser une commande distincte appelée `rmdir`.
Pour plus de sécurité,
cela ne fonctionne que lorsque le répertoire est vide,
vous devez donc supprimer les fichiers d'un répertoire *avant de* supprimer le répertoire.
(Les utilisateurs expérimentés peuvent utiliser l'option `-r` pour `rm` afin d'obtenir le même effet ;
nous aborderons les options de commande dans le chapitre suivant).

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
Sans changer de répertoire,
supprimez le fichier `agarwal.txt` dans le répertoire `people`.

`@hint`
N'oubliez pas que `rm` signifie "supprimer" et qu'un chemin relatif ne commence pas par un "/".

`@solution`
```{shell}
rm people/agarwal.txt

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_not(check_file('/home/repl/people/agarwal.txt'), incorrect_msg="`agarwal.txt` ne devrait plus être dans `/home/repl/people`. Avez-vous utilisé `rm` correctement ?"),
    has_expr_output(expr = 'ls people', output = '', incorrect_msg = 'Il y a encore des fichiers dans le répertoire `people`. Si vous avez simplement déplacé `agarwal.txt`, ou créé de nouveaux fichiers, supprimez-les tous.')
)
```

***

```yaml
type: ConsoleExercise
key: 661633e531
xp: 25
```

`@instructions`
Maintenant que le répertoire `people` est vide,
utilisez une seule commande pour le supprimer.

`@hint`
N'oubliez pas que `rm` ne fonctionne que sur les fichiers.

`@solution`
```{shell}
rmdir people

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_not(has_dir('/home/repl/people'),
              incorrect_msg = "Le répertoire 'people' ne devrait plus être dans votre répertoire personnel. Utilisez `rmdir` pour le supprimer !")
)
```

***

```yaml
type: ConsoleExercise
key: 89f7ffc1da
xp: 25
```

`@instructions`
Un répertoire n'est pas un fichier,
vous devez utiliser la commande `mkdir directory_name`
pour créer un nouveau répertoire (vide).
Utilisez cette commande pour créer un nouveau répertoire appelé `yearly` sous votre répertoire personnel.

`@hint`
Exécutez `mkdir` avec le nom du répertoire que vous souhaitez créer.

`@solution`
```{shell}
mkdir yearly

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_dir('/home/repl/yearly', msg="Il n'y a pas de répertoire `yearly` dans votre répertoire personnel. Utilisez `mkdir yearly` pour en créer un !")
)
```

***

```yaml
type: ConsoleExercise
key: 013a5ff2dc
xp: 25
```

`@instructions`
Maintenant que `yearly` existe,
créez un autre répertoire appelé `2017` à l'intérieur de celui-ci
*sans* quitter votre répertoire personnel.

`@hint`
Utilisez un chemin relatif pour le sous-répertoire que vous souhaitez créer.

`@solution`
```{shell}
mkdir yearly/2017

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_dir('/home/repl/yearly/2017',
            msg="Impossible de trouver un répertoire '2017' dans '/home/repl/yearly'. Vous pouvez créer ce répertoire en utilisant le chemin relatif `yearly/2017`.")
)
Ex().success_msg("Cool ! Terminons ce chapitre avec un exercice qui répète certains de ses concepts !")
```

---

## Conclusion

```yaml
type: BulletConsoleExercise
key: b1990e9a42
xp: 100
```

Vous créerez souvent des fichiers intermédiaires lors de l'analyse des données.
Plutôt que de les stocker dans votre répertoire personnel,
vous pouvez les mettre dans `/tmp`,
qui est l'endroit où les personnes et les programmes conservent souvent les fichiers dont ils n'ont besoin que brièvement.
(Notez que `/tmp` se trouve immédiatement sous le répertoire racine `/`,
*pas* en dessous de votre répertoire personnel).
Cet exercice de synthèse vous montrera comment procéder.

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
Utilisez `cd` pour accéder à `/tmp`.

`@hint`
N'oubliez pas que `cd` signifie "changer de répertoire" et qu'un chemin absolu commence par un "/".

`@solution`
```{shell}
cd /tmp

```

`@sct`
```{python}
Ex().check_correct(
  has_cwd('/tmp'),
  has_code('cd +/tmp', incorrect_msg = 'Vous êtes dans le mauvais répertoire. Utilisez `cd` pour changer de répertoire vers `/tmp`.')
)
```

***

```yaml
type: ConsoleExercise
key: 7e6ada440d
xp: 25
```

`@instructions`
Lister le contenu de `/tmp` *sans* taper le nom d'un répertoire.

`@hint`
Si vous ne dites pas à `ls` ce qu'il doit lister, il vous montrera ce qui se trouve dans votre répertoire actuel.

`@solution`
```{shell}
ls

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/tmp"),
    has_code("ls", incorrect_msg = "Vous n'avez pas appelé `ls` pour générer la liste des fichiers."),
    check_correct(
      has_expr_output(strict=True),
      has_code("^\s*ls\s*$", incorrect_msg = "Votre commande n'a pas généré la liste correcte des fichiers. Utilisez `ls` sans argument.")
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
Créez un nouveau répertoire à l'intérieur de `/tmp` appelé `scratch`.

`@hint`
Utilisez `mkdir` pour créer des répertoires.

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
      has_code('mkdir +scratch', incorrect_msg="Impossible de trouver un répertoire 'scratch' sous '/tmp'. Assurez-vous d'utiliser `mkdir` correctement.")
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
Déplacez `/home/repl/people/agarwal.txt` dans `/tmp/scratch`.
Nous vous suggérons d'utiliser le raccourci `~` pour votre répertoire personnel et un chemin relatif pour le second plutôt qu'un chemin absolu.

`@hint`


`@solution`
```{shell}
mv ~/people/agarwal.txt scratch

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/tmp'),
    check_file('/tmp/scratch/agarwal.txt', missing_msg="Impossible de trouver 'agarwal.txt' dans '/tmp/scratch'. Utilisez `mv` avec `~/people/agarwal.txt` comme premier paramètre et `scratch` comme second.")
)
Ex().success_msg("Cela conclut le Chapitre 1 de l'Introduction au Shell ! Passez au chapitre suivant pour en savoir plus sur la manipulation des données !")
```
