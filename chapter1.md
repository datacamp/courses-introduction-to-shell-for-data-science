---
title: Manipular archivos y directorios
description: >-
  Este capítulo es una breve introducción al shell Unix. Aprenderás por qué se
  sigue utilizando después de casi 50 años, cómo se compara con las herramientas
  gráficas con las que puedes estar más familiarizado, cómo moverte por el shell
  y cómo crear, modificar y eliminar archivos y carpetas.
free_preview: true
lessons:
  - nb_of_exercises: 12
    title: ¿Cómo se compara el shell con una interfaz de escritorio?
---

## ¿Cómo se compara el shell con una interfaz de escritorio?

```yaml
type: PureMultipleChoiceExercise
key: badd717ea4
xp: 50
```

Un sistema operativo como Windows, Linux o Mac OS es un tipo especial de programa.
Controla el procesador, el disco duro y la conexión de red del ordenador,
pero su función más importante es ejecutar otros programas.

Puesto que los seres humanos no son digitales,
necesitan una interfaz para interactuar con el sistema operativo.
El más común hoy en día es un explorador gráfico de archivos,
que traduce los clics y dobles clics en órdenes para abrir archivos y ejecutar programas.
Antes de que los ordenadores tuvieran pantallas gráficas,
sin embargo,
la gente tecleaba instrucciones en un programa llamado **command-line shell**.
Cada vez que se introduce un comando,
el shell ejecuta otros programas,
imprime su salida de forma legible para el ser humano,
y luego muestra un *prompt* para indicar que está preparado para aceptar la siguiente orden.
(Su nombre proviene de la idea de que es la "carcasa exterior" del ordenador).

Escribir comandos en lugar de hacer clic y arrastrar puede parecer torpe al principio,
pero como verás
una vez que empieces a explicar lo que quieres que haga el ordenador,
puedes combinar comandos antiguos para crear otros nuevos
y automatizar las operaciones repetitivas
con sólo pulsar unas teclas.

<hr>
¿Qué relación hay entre el explorador gráfico de archivos que utiliza la mayoría de la gente y el shell de línea de comandos?

`@hint`
Recuerda que un usuario sólo puede interactuar con un sistema operativo a través de un programa.

`@possible_answers`
- El explorador de archivos te permite ver y editar archivos, mientras que el intérprete de comandos te permite ejecutar programas.
- El explorador de archivos está construido sobre el shell.
- El intérprete de comandos forma parte del sistema operativo, mientras que el explorador de archivos es independiente.
- [Ambas son interfaces para emitir comandos al sistema operativo.]

`@feedback`
- Ambos te permiten ver y editar archivos y ejecutar programas.
- Tanto los exploradores gráficos de archivos como el shell llaman a las mismas funciones subyacentes del sistema operativo.
- Tanto el intérprete de comandos como el explorador de archivos son programas que traducen las órdenes del usuario (tecleadas o pulsadas) en llamadas al sistema operativo.
- Correcto. Ambos toman las órdenes del usuario (ya sean tecleadas o pulsadas) y las envían al sistema operativo.

---

## ¿Dónde estoy?

```yaml
type: MultipleChoiceExercise
key: 7c1481dbd3
xp: 50
```

El **sistema de archivos** gestiona archivos y directorios (o carpetas).
Cada uno se identifica por una **senda absoluta**.
que muestra cómo llegar a él desde el **directorio raíz** del sistema de archivos:
`/home/repl` es el directorio `repl` en el directorio `home`,
mientras que `/home/repl/course.txt` es un archivo `course.txt` en ese directorio,
y `/` por sí solo es el directorio raíz.

Para saber en qué parte del sistema de archivos te encuentras,
ejecuta el comando `pwd`
(abreviatura de "**p**rint **w**orking **d**irectory").
Imprime la ruta absoluta de tu **directorio de trabajo actual**,
que es donde el shell ejecuta comandos y busca archivos por defecto.

<hr>
Ejecuta `pwd`.
¿Dónde estás ahora?

`@possible_answers`
- `/home`
- `/repl`
- `/home/repl`

`@hint`
Los sistemas Unix suelen colocar los directorios personales de todos los usuarios debajo de `/home`.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err = "That is not the correct path."
correct = "Correct - you are in `/home/repl`."

Ex().has_chosen(3, [err, err, correct])
```

---

## ¿Cómo puedo identificar archivos y directorios?

```yaml
type: MultipleChoiceExercise
key: f5b0499835
xp: 50
```

`pwd` te dice dónde estás.
Para saber lo que hay,
escribe `ls` (que es la abreviatura de "**l**i**s**ting") y pulsa la tecla intro.
Por sí sola,
`ls` lista el contenido de tu directorio actual
(el que aparece en `pwd`).
Si añades los nombres de algunos archivos,
`ls` los enumerará,
y si añades los nombres de los directorios,
te mostrará su contenido.
Por ejemplo,
`ls /home/repl` te muestra lo que hay en tu directorio inicial
(normalmente llamado tu **directorio de inicio**).

<hr>
Utiliza `ls` con un argumento adecuado para listar los archivos del directorio `/home/repl/seasonal`
(que contiene información sobre consultas dentales por fecha, desglosada por temporada).
¿Cuál de estos archivos *no* está en ese directorio?

`@possible_answers`
- `autumn.csv`
- `fall.csv`
- `spring.csv`
- `winter.csv`

`@hint`
Si le das a `ls` una ruta, te muestra lo que hay en esa ruta.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err = "That file is in the `seasonal` directory."
correct = "Correct - that file is *not* in the `seasonal` directory."

Ex().has_chosen(2, [err, correct, err, err])
```

---

## ¿De qué otra forma puedo identificar los archivos y directorios?

```yaml
type: BulletConsoleExercise
key: a766184b59
xp: 100
```

Una trayectoria absoluta es como una latitud y una longitud: tiene el mismo valor estés donde estés. Una **senda relativa**, en cambio, especifica una ubicación a partir de donde te encuentras: es como decir "20 kilómetros al norte".

Como ejemplos:
- Si te encuentras en el directorio `/home/repl`, la **ruta relativa** `seasonal` especifica el mismo directorio que la **ruta absoluta** `/home/repl/seasonal`. 
- Si te encuentras en el directorio `/home/repl/seasonal`, la **ruta relativa** `winter.csv` especifica el mismo archivo que la **ruta absoluta** `/home/repl/seasonal/winter.csv`.

El shell decide si una ruta es absoluta o relativa fijándose en su primer carácter: Si empieza por `/`, es absoluto. Si *no* empieza por `/`, es relativo.

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
Estás en `/home/repl`. Utiliza `ls` con una **ruta relativa** para listar el archivo cuya ruta absoluta sea `/home/repl/course.txt` (y sólo ese archivo).

`@hint`
A menudo puedes construir la ruta relativa a un archivo o directorio por debajo de tu ubicación actual
restando la ruta absoluta de tu ubicación actual
de la ruta absoluta de lo que quieras.

`@solution`
```{shell}
ls course.txt

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/home/repl"),
    has_code("ls", incorrect_msg = "You didn't call `ls` to generate the file listing."), # to prevent `echo "course.txt"`
    check_correct(
      has_expr_output(strict=True),
      has_code("ls +course.txt", incorrect_msg = "Your command didn't generate the correct file listing. Use `ls` followed by a relative path to `/home/repl/course.txt`.")
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
Estás en `/home/repl`.
Utiliza `ls` con una ruta **relativa**.
para listar el archivo `/home/repl/seasonal/summer.csv` (y sólo ese archivo).

`@hint`
Las rutas relativas *no* empiezan con una "/" inicial.

`@solution`
```{shell}
ls seasonal/summer.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/home/repl"),
    has_code("ls", incorrect_msg = "You didn't call `ls` to generate the file listing."), 
    check_correct(
      has_expr_output(strict=True),
      has_code("ls +seasonal/summer.csv", incorrect_msg = "Your command didn't generate the correct file listing. Use `ls` followed by a relative path to `/home/repl/seasonal/summer.csv`.")
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
Estás en `/home/repl`.
Utiliza `ls` con una ruta **relativa**.
para listar el contenido del directorio `/home/repl/people`.

`@hint`
Las rutas relativas no empiezan con una "/" inicial.

`@solution`
```{shell}
ls people

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/home/repl"),
    has_code("ls", incorrect_msg = "You didn't call `ls` to generate the file listing."), 
    check_correct(
      has_expr_output(strict=True),
      has_code("ls +people", incorrect_msg = "Your command didn't generate the correct file listing. Use `ls` followed by a relative path to `/home/repl/people`.")
    )
)
Ex().success_msg("Well done. Now that you know about listing files and directories, let's see how you can move around the filesystem!")

```

---

## ¿Cómo puedo moverme a otro directorio?

```yaml
type: BulletConsoleExercise
key: dbdaec5610
xp: 100
```

Igual que puedes moverte por un explorador de archivos haciendo doble clic en las carpetas,
puedes moverte por el sistema de archivos utilizando el comando `cd`
(que significa "cambiar directorio").

Si escribes `cd seasonal` y luego escribes `pwd`,
el caparazón te dirá que ahora estás en `/home/repl/seasonal`.
Si luego ejecutas `ls` solo,
te muestra el contenido de `/home/repl/seasonal`,
porque ahí es donde estás tú.
Si quieres volver a tu directorio de inicio `/home/repl`,
puedes utilizar el comando `cd /home/repl`.

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
Estás en `/home/repl`/.
Cambia el directorio a `/home/repl/seasonal` utilizando una ruta relativa.

`@hint`
Recuerda que `cd` significa "cambiar de directorio" y que las rutas relativas no empiezan por un "/" inicial.

`@solution`
```{shell}
cd seasonal

```

`@sct`
```{python}
Ex().check_correct(
  has_cwd('/home/repl/seasonal'),
  has_code('cd +seasonal', incorrect_msg="If your current working directory (find out with `pwd`) is `/home/repl`, you can move to the `seasonal` folder with `cd seasonal`.")
)

```

***

```yaml
type: ConsoleExercise
key: e69c8eac15
xp: 35
```

`@instructions`
Utiliza `pwd` para comprobar que estás allí.

`@hint`
Recuerda pulsar "intro" o "retorno" después de introducir el comando.

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
Utiliza `ls` sin ninguna ruta para ver lo que hay en ese directorio.

`@hint`
Recuerda pulsar "intro" o "retorno" después del comando.

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
      has_code('ls', incorrect_msg="Your command did not generate the correct output. Have you used `ls` with no paths to show the contents of the current directory?")
    )
)

Ex().success_msg("Neat! This was about navigating down to subdirectories. What about moving up? Let's find out!")

```

---

## ¿Cómo puedo ascender en un directorio?

```yaml
type: PureMultipleChoiceExercise
key: 09c717ef76
xp: 50
```

El **padre** de un directorio es el directorio que está por encima de él.
Por ejemplo, `/home` es el padre de `/home/repl`,
y `/home/repl` es el padre de `/home/repl/seasonal`.
Siempre puedes dar la ruta absoluta de tu directorio padre a comandos como `cd` y `ls`.
Más a menudo,
sin embargo,
aprovecharás el hecho de que la vía especial `..`
(dos puntos sin espacios) significa "el directorio situado encima del directorio en el que estoy actualmente".
Si estás en `/home/repl/seasonal`,
entonces `cd ..` te desplaza a `/home/repl`.
Si vuelves a utilizar `cd ..`,
te pone en `/home`.
Una más `cd ..` te sitúa en el *directorio raíz* `/`,
que es la parte superior del sistema de archivos.
(Recuerda poner un espacio entre `cd` y `..`: es un comando y una ruta, no un único comando de cuatro letras).

Un punto solo, `.`, siempre significa "el directorio actual",
así que `ls` por sí solo y `ls .` hacen lo mismo,
mientras que `cd .` no tiene ningún efecto
(porque te traslada al directorio en el que estás actualmente).

Una última ruta especial es `~` (el carácter tilde),
que significa "tu directorio personal",
como `/home/repl`.
No importa dónde estés,
`ls ~` siempre mostrará el contenido de tu directorio personal,
y `cd ~` siempre te llevará a casa.

<hr>
Si estás en `/home/repl/seasonal`,
¿adónde te lleva `cd ~/../.`?

`@hint`
Traza la ruta directorio a directorio.

`@possible_answers`
- `/home/repl`
- [`/home`]
- `/home/repl/seasonal`
- `/` (el directorio raíz)

`@feedback`
- No, pero `~` o `..` por sí solos te llevarían hasta allí.
- Correcto. La ruta significa "directorio principal", "subir un nivel", "aquí".
- No, pero `.` por sí solo lo haría.
- No, la parte final del camino es `.` (que significa "aquí") y no `..` (que significa "arriba").

---

## ¿Cómo puedo copiar archivos?

```yaml
type: BulletConsoleExercise
key: 832de9e74c
xp: 100
```

A menudo querrás copiar archivos,
muévelos a otros directorios para organizarlos,
o cámbiales el nombre.
Un comando para hacerlo es `cp`, que es la abreviatura de "copiar".
Si `original.txt` es un archivo existente,
entonces:

```{shell}
cp original.txt duplicate.txt
```

crea una copia de `original.txt` llamada `duplicate.txt`.
Si ya existía un archivo llamado `duplicate.txt`,
se sobrescribe.
Si el último parámetro a `cp` es un directorio existente,
entonces un comando como

```{shell}
cp seasonal/autumn.csv seasonal/winter.csv backup
```

copia *todos* los archivos en ese directorio.

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
Haz una copia de `seasonal/summer.csv` en el directorio `backup` (que también está en `/home/repl`),
llamando al nuevo archivo `summer.bck`.

`@hint`
Combina el nombre del directorio de destino y el nombre del archivo copiado
para crear una ruta relativa para el nuevo archivo.

`@solution`
```{shell}
cp seasonal/summer.csv backup/summer.bck

```

`@sct`
```{python}
Ex().check_correct(
    check_file('/home/repl/backup/summer.bck', missing_msg="`summer.bck` doesn't appear to exist in the `backup` directory. Provide two paths to `cp`: the existing file (`seasonal/summer.csv`) and the destination file (`backup/summer.bck`)."),
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
Copia `spring.csv` y `summer.csv` del directorio `seasonal` al directorio `backup` 
*sin* cambiar tu directorio de trabajo actual (`/home/repl`).

`@hint`
Utiliza `cp` con los nombres de los archivos que quieres copiar
y *luego* el nombre del directorio donde copiarlos.

`@solution`
```{shell}
cp seasonal/spring.csv seasonal/summer.csv backup

```

`@sct`
```{python}
patt = "`%s` doesn't appear to have been copied into the `backup` directory. Provide two filenames and a directory name to `cp`."
Ex().multi(
    has_cwd('/home/repl', incorrect_msg="Make sure to copy the files while in `{{dir}}`! Use `cd {{dir}}` to navigate back there."),
    check_file('/home/repl/backup/spring.csv', missing_msg=patt%'spring.csv'),
    check_file('/home/repl/backup/summer.csv', missing_msg=patt%'summer.csv')
)
Ex().success_msg("Good job. Other than copying, we should also be able to move files from one directory to another. Learn about it in the next exercise!")
```

---

## ¿Cómo puedo mover un archivo?

```yaml
type: ConsoleExercise
key: 663a083a3c
xp: 100
```

Mientras que `cp` copia un archivo,
`mv` lo mueve de un directorio a otro,
como si lo hubieras arrastrado en un explorador gráfico de archivos.
Maneja sus parámetros del mismo modo que `cp`,
por lo que el comando:

```{shell}
mv autumn.csv winter.csv ..
```

mueve los archivos `autumn.csv` y `winter.csv` del directorio de trabajo actual
sube un nivel hasta su directorio padre
(porque `..` siempre se refiere al directorio situado por encima de tu ubicación actual).

`@instructions`
Estás en `/home/repl`, que tiene los subdirectorios `seasonal` y `backup`.
Con un solo comando, mueve `spring.csv` y `summer.csv` de `seasonal` a `backup`.

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
backup_patt="The file `%s` is not in the `backup` directory. Have you used `mv` correctly? Use two filenames and a directory as parameters to `mv`."
seasonal_patt="The file `%s` is still in the `seasonal` directory. Make sure to move the files with `mv` rather than copying them with `cp`!"
Ex().multi(
    check_file('/home/repl/backup/spring.csv', missing_msg=backup_patt%'spring.csv'),
    check_file('/home/repl/backup/summer.csv', missing_msg=backup_patt%'summer.csv'),
    check_not(check_file('/home/repl/seasonal/spring.csv'), incorrect_msg=seasonal_patt%'spring.csv'),
    check_not(check_file('/home/repl/seasonal/summer.csv'), incorrect_msg=seasonal_patt%'summer.csv')
)
Ex().success_msg("Well done, let's keep this shell train going!")
```

---

## ¿Cómo puedo cambiar el nombre de los archivos?

```yaml
type: BulletConsoleExercise
key: 001801a652
xp: 100
```

`mv` también se puede utilizar para renombrar archivos. Si te presentas:

```{shell}
mv course.txt old-course.txt
```

entonces el archivo `course.txt` del directorio de trabajo actual se "mueve" al archivo `old-course.txt`.
Esto es diferente de cómo funcionan los exploradores de archivos,
pero a menudo es útil.

Una advertencia:
igual que `cp`,
`mv` sobrescribirá los archivos existentes.
Si,
por ejemplo,
ya tienes un archivo llamado `old-course.txt`,
entonces el comando mostrado arriba lo sustituirá por lo que haya en `course.txt`.

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
Entra en el directorio `seasonal`.

`@hint`
Recuerda que `cd` significa "cambiar de directorio" y que las rutas relativas no empiezan por un "/" inicial.

`@solution`
```{shell}
cd seasonal

```

`@sct`
```{python}
Ex().check_correct(
  has_cwd('/home/repl/seasonal'),
  has_code('cd +seasonal', incorrect_msg="If your current working directory (find out with `pwd`) is `/home/repl`, you can move to the `seasonal` folder with `cd seasonal`.")
)

```

***

```yaml
type: ConsoleExercise
key: ed5fe1df23
xp: 35
```

`@instructions`
Cambia el nombre del archivo `winter.csv` por `winter.csv.bck`.

`@hint`
Utiliza `mv` con el nombre actual del archivo y el nombre que quieras que tenga en ese orden.

`@solution`
```{shell}
mv winter.csv winter.csv.bck

```

`@sct`
```{python}
hint = " Use `mv` with two arguments: the file you want to rename (`winter.csv`) and the new name for the file (`winter.csv.bck`)."
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    multi(
        check_file('/home/repl/seasonal/winter.csv.bck', missing_msg="We expected to find `winter.csv.bck` in the directory." + hint),
        check_not(check_file('/home/repl/seasonal/winter.csv'), incorrect_msg="We were no longer expecting `winter.csv` to be in the directory." + hint)
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
Ejecuta `ls` para comprobar que todo ha funcionado.

`@hint`
Recuerda pulsar "intro" o "retorno" para ejecutar el comando.

`@solution`
```{shell}
ls

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    has_expr_output(incorrect_msg="Have you used `ls` to list the contents of your current working directory?")
)
Ex().multi(
    has_cwd("/home/repl/seasonal"),
    check_correct(
      has_expr_output(strict=True),
      has_code("ls", incorrect_msg = "Your command didn't generate the correct file listing. Use `ls` without arguments to list the contents of your current working directory.")
    )
)
Ex().success_msg("Copying, moving, renaming, you've all got it figured out! Next up: deleting files.")

```

---

## ¿Cómo puedo borrar archivos?

```yaml
type: BulletConsoleExercise
key: '2734680614'
xp: 100
```

Podemos copiar archivos y moverlos;
para borrarlos,
utilizamos `rm`,
que significa "eliminar".
Como en `cp` y `mv`,
puedes dar a `rm` los nombres de tantos archivos como quieras, así:

```{shell}
rm thesis.txt backup/thesis-2017-08.txt
```

elimina tanto `thesis.txt` como `backup/thesis-2017-08.txt`

`rm` hace exactamente lo que dice su nombre,
y lo hace enseguida:
a diferencia de los navegadores gráficos de archivos,
la concha no tiene papelera,
de modo que cuando escribas el comando anterior
tu tesis ha desaparecido para siempre.

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
Estás en `/home/repl`.
Entra en el directorio `seasonal`.

`@hint`
Recuerda que `cd` significa "cambiar de directorio" y que una ruta relativa no empieza por '/'.

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
Elimina `autumn.csv`.

`@hint`
Recuerda que `rm` significa "eliminar".

`@solution`
```{shell}
rm autumn.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    check_not(check_file('/home/repl/seasonal/autumn.csv'), incorrect_msg="We weren't expecting `autumn.csv` to still be in the `seasonal` directory. Use `rm` with the path to the file you want to remove."),
    has_code('rm', incorrect_msg = 'Use `rm` to remove the file, rather than moving it.')
)

```

***

```yaml
type: ConsoleExercise
key: 09f2d105cd
xp: 25
```

`@instructions`
Vuelve a tu directorio personal.

`@hint`
Si utilizas `cd` sin ningún camino, te lleva a casa.

`@solution`
```{shell}
cd

```

`@sct`
```{python}
Ex().has_cwd('/home/repl', incorrect_msg="Use `cd ..` or `cd ~` to return to the home directory.")

```

***

```yaml
type: ConsoleExercise
key: 9eaf49744c
xp: 25
```

`@instructions`
Elimina `seasonal/summer.csv` sin volver a cambiar de directorio.

`@hint`
Recuerda que `rm` significa "eliminar".

`@solution`
```{shell}
rm seasonal/summer.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_not(check_file('/home/repl/seasonal/summer.csv'), incorrect_msg="We weren't expecting `summer.csv` to still be in the `seasonal` directory. Use `rm` with the path to the file you want to remove."),
    has_code('rm', incorrect_msg = 'Use `rm` to remove the file, rather than moving it.')
)
Ex().success_msg("Impressive stuff! Off to the next one!")

```

---

## ¿Cómo puedo crear y eliminar directorios?

```yaml
type: BulletConsoleExercise
key: 63e8fbd0c2
xp: 100
```

`mv` trata los directorios de la misma forma que los archivos:
si estás en tu directorio personal y ejecutas `mv seasonal by-season`,
por ejemplo,
`mv` cambia el nombre del directorio `seasonal` a `by-season`.
Sin embargo,
`rm` funciona de forma diferente.

Si intentas `rm` un directorio,
el intérprete de comandos imprime un mensaje de error diciéndote que no puede hacerlo,
principalmente para evitar que borres accidentalmente un directorio entero lleno de trabajo.
En lugar de eso,
puedes utilizar un comando independiente llamado `rmdir`.
Para mayor seguridad,
sólo funciona cuando el directorio está vacío,
por lo que debes borrar los archivos de un directorio *antes* de borrar el directorio.
(Los usuarios experimentados pueden utilizar la opción `-r` en `rm` para conseguir el mismo efecto;
hablaremos de las opciones de los comandos en el próximo capítulo).

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
Sin cambiar de directorio,
borra el archivo `agarwal.txt` del directorio `people`.

`@hint`
Recuerda que `rm` significa "eliminar" y que una ruta relativa no empieza por "/".

`@solution`
```{shell}
rm people/agarwal.txt

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_not(check_file('/home/repl/people/agarwal.txt'), incorrect_msg="`agarwal.txt` should no longer be in `/home/repl/people`. Have you used `rm` correctly?"),
    has_expr_output(expr = 'ls people', output = '', incorrect_msg = 'There are still files in the `people` directory. If you simply moved `agarwal.txt`, or created new files, delete them all.')
)

```

***

```yaml
type: ConsoleExercise
key: 661633e531
xp: 25
```

`@instructions`
Ahora que el directorio `people` está vacío,
utiliza un único comando para eliminarlo.

`@hint`
Recuerda que `rm` sólo funciona con archivos.

`@solution`
```{shell}
rmdir people

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_not(has_dir('/home/repl/people'),
              incorrect_msg = "The 'people' directory should no longer be in your home directory. Use `rmdir` to remove it!")
)

```

***

```yaml
type: ConsoleExercise
key: 89f7ffc1da
xp: 25
```

`@instructions`
Puesto que un directorio no es un archivo,
debes utilizar el comando `mkdir directory_name`
para crear un nuevo directorio (vacío).
Utiliza este comando para crear un nuevo directorio llamado `yearly` debajo de tu directorio personal.

`@hint`
Ejecuta `mkdir` con el nombre del directorio que quieras crear.

`@solution`
```{shell}
mkdir yearly

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_dir('/home/repl/yearly', msg="There is no `yearly` directory in your home directory. Use `mkdir yearly` to make one!")
)

```

***

```yaml
type: ConsoleExercise
key: 013a5ff2dc
xp: 25
```

`@instructions`
Ahora que `yearly` existe,
crea dentro otro directorio llamado `2017` 
*sin* salir de tu directorio personal.

`@hint`
Utiliza una ruta relativa para el subdirectorio que quieras crear.

`@solution`
```{shell}
mkdir yearly/2017

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_dir('/home/repl/yearly/2017',
            msg="Cannot find a '2017' directory in '/home/repl/yearly'. You can make this directory using the relative path `yearly/2017`.")
)
Ex().success_msg("Cool! Let's wrap up this chapter with an exercise that repeats some of its concepts!")

```

---

## Para terminar

```yaml
type: BulletConsoleExercise
key: b1990e9a42
xp: 100
```

A menudo crearás archivos intermedios cuando analices datos.
En lugar de almacenarlos en tu directorio personal,
puedes ponerlos en `/tmp`,
que es donde la gente y los programas suelen guardar archivos que sólo necesitan brevemente.
(Ten en cuenta que `/tmp` está inmediatamente debajo del directorio raíz `/`,
*no* debajo de tu directorio personal).
Este ejercicio de recapitulación te mostrará cómo hacerlo.

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
Utiliza `cd` para entrar en `/tmp`.

`@hint`
Recuerda que `cd` significa "cambiar de directorio" y que una ruta absoluta empieza por '/'.

`@solution`
```{shell}
cd /tmp

```

`@sct`
```{python}
Ex().check_correct(
  has_cwd('/tmp'),
  has_code('cd +/tmp', incorrect_msg = 'You are in the wrong directory. Use `cd` to change directory to `/tmp`.')
)

```

***

```yaml
type: ConsoleExercise
key: 7e6ada440d
xp: 25
```

`@instructions`
Lista el contenido de `/tmp` *sin* escribir el nombre de un directorio.

`@hint`
Si no le dices a `ls` qué debe listar, te muestra lo que hay en tu directorio actual.

`@solution`
```{shell}
ls

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/tmp"),
    has_code("ls", incorrect_msg = "You didn't call `ls` to generate the file listing."),
    check_correct(
      has_expr_output(strict=True),
      has_code("^\s*ls\s*$", incorrect_msg = "Your command didn't generate the correct file listing. Use `ls` without`.")
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
Crea un nuevo directorio dentro de `/tmp` llamado `scratch`.

`@hint`
Utiliza `mkdir` para crear directorios.

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
      has_code('mkdir +scratch', incorrect_msg="Cannot find a 'scratch' directory under '/tmp'. Make sure to use `mkdir` correctly.")
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
Mueve `/home/repl/people/agarwal.txt` a `/tmp/scratch`.
Te sugerimos que utilices el acceso directo `~` para tu directorio principal y una ruta relativa para el segundo, en lugar de la ruta absoluta.

`@hint`


`@solution`
```{shell}
mv ~/people/agarwal.txt scratch

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/tmp'),
    check_file('/tmp/scratch/agarwal.txt', missing_msg="Cannot find 'agarwal.txt' in '/tmp/scratch'. Use `mv` with `~/people/agarwal.txt` as the first parameter and `scratch` as the second.")
)
Ex().success_msg("This concludes Chapter 1 of Introduction to Shell! Rush over to the next chapter to learn more about manipulating data!")

```
