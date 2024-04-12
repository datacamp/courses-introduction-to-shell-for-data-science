---
title: Manipulación de datos
description: >-
  Los comandos que viste en el capítulo anterior te permitían mover cosas en el
  sistema de archivos. Este capítulo te mostrará cómo trabajar con los datos de
  esos archivos. Las herramientas que utilizaremos son bastante sencillas, pero
  constituyen sólidos bloques de construcción.
lessons:
  - nb_of_exercises: 12
    title: ¿Cómo puedo ver el contenido de un archivo?
---

## ¿Cómo puedo ver el contenido de un archivo?

```yaml
type: ConsoleExercise
key: 8acc09ede3
xp: 100
```

Antes de renombrar o borrar archivos,
quizá quieras echar un vistazo a su contenido.
La forma más sencilla de hacerlo es con `cat`,
que sólo imprime el contenido de los archivos en la pantalla.
(Su nombre es la abreviatura de "concatenar", que significa "unir cosas",
ya que imprimirá todos los archivos cuyos nombres le des, uno tras otro).

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
Imprime el contenido de `course.txt` en la pantalla.

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
    has_expr_output(incorrect_msg="Your command didn't generate the right output. Have you used `cat` followed by the name of the file, `course.txt`?")
)
Ex().success_msg("Nice! Let's look at other ways to view a file's contents.")
```

---

## ¿Cómo puedo ver el contenido de un archivo por partes?

```yaml
type: ConsoleExercise
key: d8a30a3f81
xp: 100
```

Puedes utilizar `cat` para imprimir archivos grandes y luego desplazarte por la salida,
pero suele ser más cómodo **paginar** la salida.
El comando original para hacer esto se llamaba `more`,
pero ha sido sustituido por un comando más potente llamado `less`.
(Este tipo de nomenclatura es lo que se considera humor en el mundo Unix).
Cuando `less` un archivo,
se muestra una página cada vez;
puedes pulsar la barra espaciadora para pasar página o escribir `q` para salir.

Si das a `less` los nombres de varios archivos,
puedes escribir `:n` (dos puntos y una "n" minúscula) para pasar al siguiente archivo,
`:p` para volver al anterior,
o `:q` para dejarlo.

Nota: Si ves las soluciones de los ejercicios que utilizan `less`,
verás un comando extra al final que desactiva *la paginación*.
para que podamos probar tus soluciones con eficacia.

`@instructions`
Utiliza `less seasonal/spring.csv seasonal/summer.csv` para ver esos dos archivos en ese orden.
Pulsa la barra espaciadora para pasar página, `:n` para ir al segundo archivo y `:q` para salir.

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
                 incorrect_msg='Use `less` and the filenames. Remember that `:n` moves you to the next file.'),
        has_code(r'\s*less\s+seasonal/summer\.csv\s+seasonal/spring\.csv\s*')
    )
)
```

---

## ¿Cómo puedo ver el inicio de un archivo?

```yaml
type: MultipleChoiceExercise
key: 82bdc9af65
lang: shell
xp: 50
skills:
  - 1
```

Lo primero que hacen la mayoría de los científicos de datos cuando reciben un nuevo conjunto de datos para analizar es
averiguar qué campos contiene y qué valores tienen esos campos.
Si el conjunto de datos se ha exportado desde una base de datos o una hoja de cálculo,
a menudo se almacenará como **valores separados por comas** (CSV).
Una forma rápida de averiguar qué contiene es mirar las primeras filas.

Podemos hacerlo en el shell utilizando un comando llamado `head`.
Como su nombre indica,
imprime las primeras líneas de un archivo
(donde "unos pocos" significa 10),
por lo que el comando:

```{shell}
head seasonal/summer.csv
```

pantallas:

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

¿Qué hace `head` si no hay 10 líneas en el archivo?
(Para averiguarlo, úsalo para mirar la parte superior de `people/agarwal.txt`.)

`@possible_answers`
- Imprime un mensaje de error porque el archivo es demasiado corto.
- Muestra tantas líneas como haya.
- Muestra suficientes líneas en blanco para que el total sea 10.

`@hint`
¿Qué es lo más útil que podría hacer?

`@pre_exercise_code`
```{python}

```

`@sct`
```{shell}
Ex().has_chosen(2, ["Incorrect: that isn't the most useful thing it could do.",
                    "Correct!",
                    "Incorrect: that would be impossible to distinguish from a file that ended with a bunch of blank lines."])
```

---

## ¿Cómo puedo escribir menos?

```yaml
type: BulletConsoleExercise
key: 0b7b8ca8f7
xp: 100
```

Una de las herramientas del shell es **completar pestañas**.
Si empiezas a escribir el nombre de un archivo y luego pulsas la tecla de tabulación,
el intérprete de comandos hará todo lo posible para autocompletar la ruta.
Por ejemplo,
si escribes `sea` y pulsas el tabulador,
rellenará el nombre del directorio `seasonal/` (con una barra al final).
Si a continuación escribes `a` y tabulador,
completará la ruta como `seasonal/autumn.csv`.

Si la ruta es ambigua,
como `seasonal/s`,
Si pulsas tabulador por segunda vez, aparecerá una lista de posibilidades.
Escribiendo uno o dos caracteres más para concretar tu trayectoria
y luego pulsar tabulador
rellenará el resto del nombre.

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
Ejecuta `head seasonal/autumn.csv` sin escribir el nombre completo del archivo.

`@hint`
Escribe toda la ruta que necesites, pulsa tabulador y repite.

`@solution`
```{shell}
head seasonal/autumn.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_expr_output(incorrect_msg="The checker couldn't find the right output in your command. Are you sure you called `head` on `seasonal/autumn.csv`?")
)

```

***

```yaml
type: ConsoleExercise
key: e249266733
xp: 50
```

`@instructions`
Ejecuta `head seasonal/spring.csv` sin escribir el nombre completo del archivo.

`@hint`
Escribe toda la ruta que necesites, pulsa tabulador y repite.

`@solution`
```{shell}
head seasonal/spring.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_expr_output(incorrect_msg="The checker couldn't find the right output in your command. Are you sure you called `head` on `seasonal/spring.csv`?")
)
Ex().success_msg("Good work! Once you get used to using tab completion, it will save you a lot of time!")

```

---

## ¿Cómo puedo controlar lo que hacen los comandos?

```yaml
type: ConsoleExercise
key: 9eb608f6c9
xp: 100
```

No siempre querrás mirar las 10 primeras líneas de un archivo,
para que el shell te permita cambiar el comportamiento de `head`
dándole una **bandera de línea de comandos** (o simplemente "bandera" para abreviar).
Si ejecutas el comando

```{shell}
head -n 3 seasonal/summer.csv
```

`head` sólo mostrará las tres primeras líneas del archivo.
Si ejecutas `head -n 100`,
mostrará los 100 primeros (suponiendo que haya tantos),
etc.

El nombre de una bandera suele indicar su finalidad
(por ejemplo, `-n` debe indicar "**n**número de líneas").
Las banderas de mando no tienen por qué ser un `-` seguido de una sola letra,
pero es una convención muy utilizada.

Nota: se considera de buen estilo poner todas las banderas *antes* de cualquier nombre de archivo,
así en este curso,
sólo aceptamos respuestas que lo hagan.

`@instructions`
Muestra las 5 primeras líneas de `winter.csv` en el directorio `seasonal`.

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
        has_expr_output(incorrect_msg="Are you sure you're calling `head` on the `seasonal/winter.csv` file?"),
        has_expr_output(strict=True, incorrect_msg="Are you sure you used the flag `-n 5`?")
    ),
    check_not(has_output("2017-02-17,incisor"), incorrect_msg = "Are you sure you used the flag `-n 5`?")
)
Ex().success_msg("Nice! With this technique, you can avoid your shell from blowing up if you want to have a look at larger text files.")
```

---

## ¿Cómo puedo listar todo lo que hay debajo de un directorio?

```yaml
type: ConsoleExercise
key: f830d46419
xp: 100
```

Para ver todo lo que hay debajo de un directorio,
por muy anidada que esté,
puedes dar a `ls` la bandera `-R`
(que significa "recursivo").
Si utilizas `ls -R` en tu directorio personal,
verás algo así

```
backup          course.txt      people          seasonal

./backup:

./people:
agarwal.txt

./seasonal:
autumn.csv      spring.csv      summer.csv      winter.csv
```

Muestra todos los archivos y directorios del nivel actual,
y luego todo lo que haya en cada subdirectorio,
etc.

`@instructions`
Para ayudarte a saber qué es qué,
`ls` tiene otra bandera `-F` que imprime un `/` después del nombre de cada directorio
y un `*` después del nombre de cada programa ejecutable.
Ejecuta `ls` con las dos banderas, `-R` y `-F`, y la ruta absoluta a tu directorio personal
para ver todo lo que contiene.
(El orden de las banderas no importa, pero el nombre del directorio debe ir en último lugar).

`@hint`
Puedes especificar tu directorio personal utilizando `~` o `.` o su ruta absoluta.

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
  has_expr_output(incorrect_msg='Use either `ls -R -F` or `ls -F -R` and the path `/home/repl`.'),
  has_expr_output(expr = "ls -R -F .", incorrect_msg='Use either `ls -R -F` or `ls -F -R` and the path `/home/repl`.')
)
Ex().success_msg("That's a pretty neat overview, isn't it?")
```

---

## ¿Cómo puedo obtener ayuda para un comando?

```yaml
type: BulletConsoleExercise
key: 7b90b8a7cd
xp: 100
```

Para saber qué hacen los comandos,
la gente solía utilizar el comando `man` 
(abreviatura de "manual").
Por ejemplo,
el comando `man head` muestra esta información:

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
     a header consisting of the string ``==> XXX &lt;=='' where ``XXX''
     is the name of the file.

SEE ALSO
     tail(1)
```

`man` invoca automáticamente `less`,
por lo que es posible que tengas que pulsar la barra espaciadora para hojear la información
y `:q` para dejarlo.

La descripción de una línea que aparece en `NAME` te explica brevemente lo que hace el comando,
y el resumen en `SYNOPSIS` enumera todas las banderas que entiende.
Todo lo que es opcional se muestra entre corchetes `[...]`,
las alternativas o/y están separadas por `|`,
y las cosas que pueden repetirse se muestran en `...`,
así que la página del manual de `head` te está diciendo que puedes *o bien* dar un recuento de líneas con `-n`
o un recuento de bytes con `-c`,
y que puedes darle cualquier número de nombres de archivo.

El problema del manual de Unix es que tienes que saber lo que buscas.
Si no lo haces
puedes buscar en [Stack Overflow](https://stackoverflow.com/),
haz una pregunta en los canales de Slack de DataCamp,
o consulta las secciones `SEE ALSO` de los comandos que ya conoces.

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
Lee la página del manual del comando `tail` para averiguarlo
lo que hace poner un signo `+` delante del número utilizado con la bandera `-n`.
(Recuerda pulsar la barra espaciadora para pasar página y/o escribir `q` para salir).

`@hint`
Recuerda: `man` es la abreviatura de "manual".

`@solution`
```{shell}
# Run the following command *without* '| cat':
man tail | cat

```

`@sct`
```{python}
Ex().has_code(r'\s*man\s+tail.*', incorrect_msg='Use `man` and the command name.')

```

***

```yaml
type: ConsoleExercise
key: 6a07958ae0
xp: 50
```

`@instructions`
Utiliza `tail` con la bandera `-n +7` para mostrar todas *menos* las seis primeras líneas de `seasonal/spring.csv`.

`@hint`
Utiliza el signo "+" delante del número de líneas que quieres que se muestren.

`@solution`
```{shell}
tail -n +7 seasonal/spring.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_output('2017-09-07,molar', incorrect_msg="Are you calling `tail` on `seasonal/spring.csv`?"),
    has_expr_output(strict=True, incorrect_msg="Are you share you used the flag `-n +7`?")
)

```

---

## ¿Cómo puedo seleccionar columnas de un fichero?

```yaml
type: MultipleChoiceExercise
key: 925e9d645a
xp: 50
```

`head` y `tail` te permiten seleccionar filas de un archivo de texto.
Si quieres seleccionar columnas,
puedes utilizar el comando `cut`.
Tiene varias opciones (utiliza `man cut` para explorarlas),
pero lo más habitual es algo así como

```{shell}
cut -f 2-5,8 -d , values.csv
```

lo que significa
" selecciona las columnas 2 a 5 y la columna 8,
utilizando la coma como separador".
`cut` utiliza `-f` (que significa "campos") para especificar columnas
y `-d` (que significa "delimitador") para especificar el separador.
Debes especificar esto último porque algunos archivos pueden utilizar espacios, tabuladores o dos puntos para separar las columnas.

<hr>

¿Qué comando seleccionará la primera columna (que contiene fechas) del archivo `spring.csv`?

`@possible_answers`
- `cut -d , -f 1 seasonal/spring.csv`
- `cut -d, -f1 seasonal/spring.csv`
- Cualquiera de las anteriores.
- Ninguna de las dos cosas, porque `-f` debe ir antes que `-d`.

`@hint`
El orden de las banderas no importa.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
Ex().has_chosen(3, ['Yes, but that is not all', 'Yes, but that is not all', 'Correct! Adding a space after the flag is good style, but not compulsory.', 'No, flag order doesn\'t matter'])
```

---

## ¿Qué no se puede cortar?

```yaml
type: MultipleChoiceExercise
key: b9bb10ae87
xp: 50
```

`cut` es una orden simple.
En particular,
no entiende las cadenas entrecomilladas.
Si, por ejemplo, tu archivo es

```
Name,Age
"Johel,Ranjit",28
"Sharma,Rupinder",26
```

entonces:

```{shell}
cut -f 2 -d , everyone.csv
```

producirá:

```
Age
Ranjit"
Rupinder"
```

en lugar de la edad de cada uno,
porque pensará que la coma entre el apellido y el nombre es un separador de columnas.

<hr>

Cuál es la salida de `cut -d : -f 2-4` en la línea:

```
first:second:third:
```

(Fíjate en los dos puntos finales).

`@possible_answers`
- `second`
- `second:third`
- `second:third:`
- Ninguna de las anteriores, porque no hay cuatro campos.

`@hint`
Presta atención a los dos puntos finales.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
Ex().has_chosen(3, ['No, there is more.', 'No, there is more.', 'Correct! The trailing colon creates an empty fourth field.', 'No, `cut` does the best it can.'])
```

---

## ¿Cómo puedo repetir órdenes?

```yaml
type: TabConsoleExercise
key: 32c0d30049
xp: 100
```

Una de las mayores ventajas de utilizar el shell es que
te facilita volver a hacer las cosas.
Si ejecutas algunos comandos,
puedes pulsar la tecla de flecha arriba para volver a recorrerlos.
También puedes utilizar las teclas de flecha izquierda y derecha y la tecla Supr para editarlas.
Si pulsas Retorno, se ejecutará el comando modificado.

Aún mejor, `history` imprimirá una lista de los comandos que has ejecutado recientemente.
Cada uno va precedido de un número de serie para facilitar la reejecución de determinados comandos:
escribe `!55` para volver a ejecutar el comando 55 de tu historial (si es que tienes tantos).
También puedes volver a ejecutar un comando escribiendo un signo de exclamación seguido del nombre del comando,
como `!head` o `!cut`,
que volverá a ejecutar el uso más reciente de ese comando.

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
Ejecuta `head summer.csv` en tu directorio personal (debería fallar).

`@hint`
El tabulador no funcionará si no hay un nombre de archivo que coincida.

`@solution`
```{shell}
head summer.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_code(r'\s*head\s+summer.csv\s*', incorrect_msg="Use `head` and a filename, `summer.csv`. Don't worry if it fails. It should.")
)

```

***

```yaml
type: ConsoleExercise
key: cba6bf99a5
xp: 20
```

`@instructions`
Cambia el directorio a `seasonal`.

`@hint`
Recuerda que `cd` significa "cambiar directorio".

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
key: 74f5c8d2fc
xp: 20
```

`@instructions`
Vuelve a ejecutar el comando `head` con `!head`.

`@hint`
No escribas ningún espacio entre `!` y lo que sigue.

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
                        incorrect_msg='Use `!head` to repeat the `head` command.'),
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
Utiliza `history` para ver lo que has hecho.

`@hint`
Observa que `history` muestra los comandos más recientes en último lugar, para que queden en tu pantalla cuando termine de ejecutarse.

`@solution`
```{shell}
history

```

`@sct`
```{python}
Ex().has_code(r'history', incorrect_msg='Use `history` without flags to get a list of previous commands.')

```

***

```yaml
type: ConsoleExercise
key: 0629b2adf3
xp: 20
```

`@instructions`
Vuelve a ejecutar `head` utilizando `!` seguido de un número de comando.

`@hint`
No escribas ningún espacio entre `!` y lo que sigue.

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
                        incorrect_msg='Have you used `!<a_number>` to rerun the last `head` from the history?'),
        # The head cmd should appear twice, at positions 1 and 3, though this will change 
        # if the student typed a wrong answer.
        # Since we're also checking output, this should be niche enough to ignore.
        has_code(r'!3'),
        has_code(r'!1') 
    )
)
Ex().success_msg("Well done! To the next one!")

```

---

## ¿Cómo puedo seleccionar líneas que contengan valores específicos?

```yaml
type: BulletConsoleExercise
key: adf1516acf
xp: 100
```

`head` y `tail` seleccionar filas,
`cut` selecciona columnas,
y `grep` selecciona las líneas según lo que contengan.
En su forma más simple,
`grep` toma un fragmento de texto seguido de uno o varios nombres de archivo
e imprime todas las líneas de esos archivos que contengan ese texto.
Por ejemplo,
`grep bicuspid seasonal/winter.csv`
imprime las líneas de `winter.csv` que contengan "bicúspide".

`grep` también puedes buscar patrones;
los exploraremos en el próximo curso.
Lo más importante en este momento son algunas de las banderas más comunes de `grep`:

- `-c`: imprime un recuento de las líneas coincidentes en lugar de las propias líneas
- `-h`: *no* imprimir los nombres de los archivos al buscar varios archivos
- `-i`: ignora mayúsculas y minúsculas (por ejemplo, trata "Regresión" y "regresión" como coincidencias)
- `-l`: imprime los nombres de los archivos que contienen coincidencias, no las coincidencias
- `-n`: imprime los números de línea de las líneas coincidentes
- `-v`: invierte la coincidencia, es decir, muestra sólo las líneas que *no* coinciden

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
Imprime el contenido de todas las líneas que contengan la palabra `molar` en `seasonal/autumn.csv`
ejecutando un único comando en tu directorio personal. No utilices ninguna bandera.

`@hint`
Utiliza `grep` con la palabra que buscas y el nombre del archivo o archivos en los que quieres buscar.

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
      has_code("grep", incorrect_msg = "Did you call `grep`?"),
      has_code("molar", incorrect_msg = "Did you search for `molar`?"),
      has_code("seasonal/autumn.csv", incorrect_msg = "Did you search the `seasonal/autumn.csv` file?")
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
Invierte la coincidencia para encontrar todas las líneas que *no* contienen la palabra `molar` en `seasonal/spring.csv`, y muestra sus números de línea.
Recuerda que se considera de buen estilo poner todas las banderas *antes* de otros valores como los nombres de archivo o el término de búsqueda "molar".

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
      has_code("grep", incorrect_msg = "Did you call `grep`?"),
      has_code("-v", incorrect_msg = "Did you invert the match with `-v`?"),
      has_code("-n", incorrect_msg = "Did you show line numbers with `-n`?"),
      has_code("molar", incorrect_msg = "Did you search for `molar`?"),
      has_code("seasonal/spring.csv", incorrect_msg = "Did you search the `seasonal/spring.csv` file?")
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
Cuenta cuántas líneas contienen la palabra `incisor` en `autumn.csv` y `winter.csv` juntas.
(De nuevo, ejecuta un único comando desde tu directorio personal).

`@hint`
Recuerda utilizar `-c` con `grep` para contar las líneas.

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
      has_code("grep", incorrect_msg = "Did you call `grep`?"),
      has_code("-c", incorrect_msg = "Did you get counts with `-c`?"),
      has_code("incisor", incorrect_msg = "Did you search for `incisor`?"),
      has_code("seasonal/autumn.csv", incorrect_msg = "Did you search the `seasonal/autumn.csv` file?"),
      has_code("seasonal/winter.csv", incorrect_msg = "Did you search the `seasonal/winter.csv` file?")
    )
  )
)

```

---

## ¿Por qué no siempre es seguro tratar los datos como texto?

```yaml
type: MultipleChoiceExercise
key: 11914639fc
xp: 50
```

La sección `SEE ALSO` de la página del manual de `cut` hace referencia a un comando llamado `paste`
que puede utilizarse para combinar archivos de datos en lugar de cortarlos.

<hr>

Lee la página del manual `paste`,
y luego ejecuta `paste` para combinar los archivos de datos de otoño e invierno en una única tabla
utilizando una coma como separador.
¿Qué tiene de malo el resultado desde el punto de vista del análisis de datos?

`@possible_answers`
- Los encabezados de columna se repiten.
- Las últimas filas tienen un número incorrecto de columnas.
- Faltan algunos datos de `winter.csv`.

`@hint`
Si `cut` la salida de `paste` utilizando comas como separador,
¿produciría la respuesta correcta?

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err1 = 'True, but it is not necessarily an error.'
correct2 = 'Correct: joining the lines with columns creates only one empty column at the start, not two.'
err3 = 'No, all of the winter data is there.'
Ex().has_chosen(2, [err1, correct2, err3])
```
