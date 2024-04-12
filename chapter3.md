---
title: Combinar herramientas
description: >-
  El verdadero poder del shell Unix no reside en los comandos individuales, sino
  en lo fácil que es combinarlos para hacer cosas nuevas. Este capítulo te
  mostrará cómo utilizar este poder para seleccionar los datos que quieras, e
  introduce comandos para ordenar valores y eliminar duplicados.
lessons:
  - nb_of_exercises: 12
    title: ¿Cómo puedo almacenar la salida de un comando en un archivo?
---

## ¿Cómo puedo almacenar la salida de un comando en un archivo?

```yaml
type: ConsoleExercise
key: 07a427d50c
xp: 100
```

Todas las herramientas que has visto hasta ahora te permiten nombrar los archivos de entrada.
La mayoría no tienen la opción de dar un nombre al archivo de salida porque no lo necesitan.
En lugar de eso,
puedes utilizar **redirección** para guardar la salida de cualquier comando donde quieras.
Si ejecutas este comando:

```{shell}
head -n 5 seasonal/summer.csv
```

imprime en la pantalla las 5 primeras líneas de los datos del verano.
Si ejecutas este comando en su lugar:

```{shell}
head -n 5 seasonal/summer.csv > top.csv
```

no aparece nada en la pantalla.
En lugar de eso,
`head`se guarda en un nuevo archivo llamado `top.csv`.
Puedes echar un vistazo al contenido de ese archivo utilizando `cat`:

```{shell}
cat top.csv
```

El signo mayor que `>` indica al intérprete de comandos que redirija la salida de `head` a un archivo.
No forma parte del comando `head`;
en su lugar,
funciona con todos los comandos del shell que producen salida.

`@instructions`
Combina `tail` con la redirección para guardar las últimas 5 líneas de `seasonal/winter.csv` en un archivo llamado `last.csv`.

`@hint`
Utiliza `tail -n 5` para obtener las 5 últimas líneas.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
tail -n 5 seasonal/winter.csv > last.csv
```

`@sct`
```{python}
patt = "The line `%s` should be in the file `last.csv`, but it isn't. Redirect the output of `tail -n 5 seasonal/winter.csv` to `last.csv` with `>`."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/last.csv').multi(
        check_not(has_code('2017-07-01,incisor'), incorrect_msg='`last.csv` has too many lines. Did you use the flag `-n 5` with `tail`?'),
        has_code('2017-07-17,canine', incorrect_msg=patt%'2017-07-17,canine'),
        has_code('2017-08-13,canine', incorrect_msg=patt%'2017-08-13,canine')
    )
)
Ex().success_msg("Nice! Let's practice some more!")
```

---

## ¿Cómo puedo utilizar la salida de un comando como entrada?

```yaml
type: BulletConsoleExercise
key: f47d337593
xp: 100
```

Supón que quieres obtener líneas de la mitad de un fichero.
Más concretamente,
Supongamos que quieres obtener las líneas 3-5 de uno de nuestros archivos de datos.
Puedes empezar utilizando `head` para obtener las 5 primeras líneas
y redirigirlo a un archivo,
y luego utiliza `tail` para seleccionar los 3 últimos:

```{shell}
head -n 5 seasonal/winter.csv > top.csv
tail -n 3 top.csv
```

Una comprobación rápida confirma que se trata de las líneas 3-5 de nuestro archivo original,
porque son las 3 últimas líneas de las 5 primeras.

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
Selecciona las dos últimas líneas de `seasonal/winter.csv`
y guárdalos en un archivo llamado `bottom.csv`.

`@hint`
Utiliza `tail` para seleccionar líneas y `>` para redirigir la salida de `tail`.

`@solution`
```{shell}
tail -n 2 seasonal/winter.csv > bottom.csv

```

`@sct`
```{python}
patt="The line `%s` should be in the file `bottom.csv`, but it isn't. Redirect the output of `tail -n 2 seasonal/winter.csv` to `bottom.csv` with `>`."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/bottom.csv').multi(
        check_not(has_code('2017-08-11,bicuspid'), incorrect_msg = '`bottom.csv` has too many lines. Did you use the flag `-n 2` with `tail`?'),
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
Selecciona la primera línea de `bottom.csv`
para obtener la penúltima línea del archivo original.

`@hint`
Utiliza `head` para seleccionar la línea que quieras.

`@solution`
```{shell}
head -n 1 bottom.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/bottom.csv').has_code('2017-08-11,wisdom', incorrect_msg="There's something wrong with the `bottom.csv` file. Make sure you don't change it!"),
    has_expr_output(strict=True, incorrect_msg="Have you used `head` correctly on `bottom.csv`? Make sure to use the `-n` flag correctly.")
)

Ex().success_msg("Well done. Head over to the next exercise to find out about better ways to combine commands.")                             

```

---

## ¿Cuál es la mejor forma de combinar comandos?

```yaml
type: ConsoleExercise
key: b36aea9a1e
xp: 100
```

Utilizar la redirección para combinar comandos tiene dos inconvenientes:

1. Deja un montón de archivos intermedios por ahí (como `top.csv`).
2. Los comandos para producir tu resultado final están dispersos en varias líneas de la historia.

El shell proporciona otra herramienta que resuelve estos dos problemas a la vez, llamada **pipa**.
Una vez más,
empieza por ejecutar `head`:

```{shell}
head -n 5 seasonal/summer.csv
```

En lugar de enviar la salida de `head` a un archivo,
añade una barra vertical y el comando `tail` *sin* nombre de archivo:

```{shell}
head -n 5 seasonal/summer.csv | tail -n 3
```

El símbolo de la tubería indica al shell que utilice la salida del comando de la izquierda
como entrada para el comando de la derecha.

`@instructions`
Utiliza `cut` para seleccionar todos los nombres de los dientes de la columna 2 del archivo delimitado por comas `seasonal/summer.csv`, y luego pasa el resultado a `grep`, con una coincidencia invertida, para excluir la línea de cabecera que contiene la palabra "Diente". *`cut` y `grep` se trataron en detalle en el Capítulo 2, ejercicios 8 y 11 respectivamente.*

`@hint`
- La primera parte del comando tiene la forma `cut -d field_delimiter -f column_number filename`.
- La segunda parte del comando tiene la forma `grep -v thing_to_match`.

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
    has_expr_output(incorrect_msg = 'Have you piped the result of `cut -d , -f 2 seasonal/summer.csv` into `grep -v Tooth` with `|`?'),
    check_not(has_output("Tooth"), incorrect_msg = 'Did you exclude the `"Tooth"` header line using `grep`?')
)
Ex().success_msg("Perfect piping! This may be the first time you used `|`, but it's definitely not the last!")
```

---

## ¿Cómo puedo combinar varios comandos?

```yaml
type: ConsoleExercise
key: b8753881d6
xp: 100
```

Puedes encadenar cualquier número de comandos.
Por ejemplo,
este comando:

```{shell}
cut -d , -f 1 seasonal/spring.csv | grep -v Date | head -n 10
```

will:

1. Selecciona la primera columna de los datos del muelle;
2. elimina la línea de encabezamiento que contiene la palabra "Fecha"; y
3. Selecciona las 10 primeras líneas de datos reales.

`@instructions`
En el ejercicio anterior, utilizaste el siguiente comando para seleccionar todos los nombres de dientes de la columna 2 de `seasonal/summer.csv`:

```
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth
```

Amplía esta cadena con un comando `head` para seleccionar sólo el primer nombre de diente.

`@hint`
Copia y pega el código de las instrucciones, añade una tubería y, a continuación, llama a `head` con la bandera `-n`.

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
    has_output('^\s*canine\s*$', incorrect_msg = "Have you used `|` to extend the pipeline with a `head` command? Make sure to set the `-n` flag correctly."),
    # by coincidence, tail -n 1 returns the same as head -n 1, so check that head was called
    has_code("head", "Have you used `|` to extend the pipeline with a `head` command?")
)
Ex().success_msg("Cheerful chaining! By chaining several commands together, you can build powerful data manipulation pipelines.")
```

---

## ¿Cómo puedo contar los registros de un fichero?

```yaml
type: ConsoleExercise
key: ae6a48d6aa
xp: 100
```

El comando `wc` (abreviatura de "recuento de palabras") imprime el número de **caracteres, **palabras y **líneas de un archivo.
Puedes hacer que imprima sólo uno de ellos utilizando `-c`, `-w`, o `-l` respectivamente.

`@instructions`
Cuenta cuántos registros en `seasonal/spring.csv` tienen fechas en julio de 2017 (`2017-07`). 
- Para ello, utiliza `grep` con una fecha parcial para seleccionar las líneas y canaliza este resultado en `wc` con una bandera adecuada para contar las líneas.

`@hint`
- Utiliza `head seasonal/spring.csv` para recordar el formato de la fecha.
- La primera parte del comando tiene la forma `grep thing_to_match filename`.
- Después de la tubería, `|`, llama a `wc` con la bandera `-l`.

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
      has_code("grep", incorrect_msg = "Did you call `grep`?"),
      has_code("2017-07", incorrect_msg = "Did you search for `2017-07`?"),
      has_code("seasonal/spring.csv", incorrect_msg = "Did you search the `seasonal/spring.csv` file?"),
      has_code("|", incorrect_msg = "Did you pipe to `wc` using `|`?"),      
      has_code("wc", incorrect_msg = "Did you call `wc`?"),
      has_code("-l", incorrect_msg = "Did you count lines with `-l`?")
    )
  )
)
Ex().success_msg("Careful counting! Determining how much data you have is a great first step in any data analysis.")
```

---

## ¿Cómo puedo especificar muchos archivos a la vez?

```yaml
type: ConsoleExercise
key: 602d47e70c
xp: 100
```

La mayoría de los comandos del shell funcionarán en varios archivos si les das varios nombres de archivo.
Por ejemplo,
puedes obtener la primera columna de todos los archivos de datos estacionales a la vez de la siguiente manera:

```{shell}
cut -d , -f 1 seasonal/winter.csv seasonal/spring.csv seasonal/summer.csv seasonal/autumn.csv
```

Pero escribir los nombres de muchos archivos una y otra vez es una mala idea:
pierde el tiempo,
y tarde o temprano omitirás un archivo o repetirás el nombre de un archivo.
Para mejorar tu vida,
el intérprete de comandos te permite utilizar **cartas comodín** para especificar una lista de archivos con una sola expresión.
El comodín más común es `*`,
que significa "coincide con cero o más caracteres".
Utilízalo,
podemos acortar el comando `cut` anterior a esto:

```{shell}
cut -d , -f 1 seasonal/*
```

o:

```{shell}
cut -d , -f 1 seasonal/*.csv
```

`@instructions`
Escribe un único comando utilizando `head` para obtener las tres primeras líneas tanto de `seasonal/spring.csv` como de `seasonal/summer.csv`, un total de seis líneas de datos, pero *no* de los archivos de datos de otoño o invierno.
Utiliza un comodín en lugar de escribir el nombre completo de los archivos.

`@hint`
- El comando tiene la forma `head -n number_of_lines filename_pattern`.
- Podrías hacer coincidir archivos en el directorio `a`, empezando por `b`, utilizando `a/b*`, por ejemplo.

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
    has_expr_output(incorrect_msg = "You can use `seasonal/s*` to select `seasonal/spring.csv` and `seasonal/summer.csv`. Make sure to only include the first three lines of each file with the `-n` flag!"),
    check_not(has_output('==> seasonal/autumn.csv <=='), incorrect_msg = "Don't include the output for `seasonal/autumn.csv`. You can use `seasonal/s*` to select `seasonal/spring.csv` and `seasonal/summer.csv`"),
    check_not(has_output('==> seasonal/winter.csv <=='), incorrect_msg = "Don't include the output for `seasonal/winter.csv`. You can use `seasonal/s*` to select `seasonal/spring.csv` and `seasonal/summer.csv`")
)
Ex().success_msg("Wild wildcard work! This becomes even more important if your directory contains hundreds or thousands of files.")
```

---

## ¿Qué otros comodines puedo utilizar?

```yaml
type: PureMultipleChoiceExercise
key: f8feeacd8c
xp: 50
```

El intérprete de comandos también tiene otros comodines,
aunque su uso es menos frecuente:

- `?` coincide con un único carácter, por lo que `201?.txt` coincidirá con `2017.txt` o `2018.txt`, pero no con `2017-01.txt`.
- `[...]` coincide con cualquiera de los caracteres dentro de los corchetes, por lo que `201[78].txt` coincide con `2017.txt` o `2018.txt`, pero no con `2016.txt`.
- `{...}` coincide con cualquiera de los patrones separados por comas que hay dentro de las llaves, por lo que `{*.txt, *.csv}` coincide con cualquier archivo cuyo nombre termine en `.txt` o `.csv`, pero no con archivos cuyo nombre termine en `.pdf`.

<hr/>

¿Qué expresión coincidiría con `singh.pdf` y `johel.txt` pero *no* con `sandhu.pdf` o `sandhu.txt`?

`@hint`
Compara cada expresión con cada nombre de archivo sucesivamente.

`@possible_answers`
- `[sj]*.{.pdf, .txt}`
- `{s*.pdf, j*.txt}`
- `[singh,johel]{*.pdf, *.txt}`
- [`{singh.pdf, j*.txt}`]

`@feedback`
- No: `.pdf` y `.txt` no son nombres de archivo.
- No: esto coincidirá con `sandhu.pdf`.
- No: la expresión entre corchetes sólo coincide con un carácter, no con palabras enteras.
- Correcto.

---

## ¿Cómo puedo ordenar líneas de texto?

```yaml
type: ConsoleExercise
key: f06d9e310e
xp: 100
```

Como su nombre indica,
`sort` ordena los datos.
Por defecto lo hace en orden alfabético ascendente,
pero se pueden utilizar las banderas `-n` y `-r` para ordenar numéricamente e invertir el orden de su salida,
mientras que `-b` le dice que ignore los espacios en blanco a la izquierda
y `-f` le dice que distinga entre mayúsculas y minúsculas (es decir, que no distinga entre mayúsculas y minúsculas).
Las tuberías suelen utilizar `grep` para deshacerse de los registros no deseados
y luego `sort` para poner en orden los registros restantes.

`@instructions`
¿Recuerdas la combinación de `cut` y `grep` para seleccionar todos los nombres de dientes de la columna 2 de `seasonal/summer.csv`?

```
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth
```

A partir de esta receta, ordena los nombres de los dientes en `seasonal/winter.csv` (no `summer.csv`) en orden alfabético descendente. Para ello, amplía la tubería con un paso `sort`.

`@hint`
Copia y pega el comando en las instrucciones, cambia el nombre del archivo, añade una tubería y, a continuación, llama a `sort` con la bandera `-r`.

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
      has_code("cut", incorrect_msg = "Did you call `cut`?"),
      has_code("-d", incorrect_msg = "Did you specify a field delimiter with `-d`?"),
      has_code("seasonal/winter.csv", incorrect_msg = "Did you get data from the `seasonal/winter.csv` file?"),
      has_code("|", incorrect_msg = "Did you pipe from `cut` to `grep` to `sort` using `|`?"),      
      has_code("grep", incorrect_msg = "Did you call `grep`?"),
      has_code("-v", incorrect_msg = "Did you invert the match with `-v`?"),
      has_code("Tooth", incorrect_msg = "Did you search for `Tooth`?"),
      has_code("sort", incorrect_msg = "Did you call `sort`?"),
      has_code("-r", incorrect_msg = "Did you reverse the sort order with `-r`?")
    )
  )
)
Ex().success_msg("Sorted! `sort` has many uses. For example, piping `sort -n` to `head` shows you the largest values.")
```

---

## ¿Cómo puedo eliminar las líneas duplicadas?

```yaml
type: ConsoleExercise
key: ed77aed337
xp: 100
```

Otro comando que se utiliza a menudo con `sort` es `uniq`,
cuya función es eliminar las líneas duplicadas.
Más concretamente,
elimina las líneas duplicadas *adyacentes*.
Si un archivo contiene:

```
2017-07-03
2017-07-03
2017-08-03
2017-08-03
```

entonces `uniq` producirá:

```
2017-07-03
2017-08-03
```

pero si contiene

```
2017-07-03
2017-08-03
2017-07-03
2017-08-03
```

entonces `uniq` imprimirá las cuatro líneas.
La razón es que `uniq` está hecho para trabajar con archivos muy grandes.
Para eliminar líneas no adyacentes de un archivo,
tendría que mantener todo el archivo en memoria
(o al menos,
todas las líneas únicas vistas hasta ahora).
Eliminando sólo los duplicados adyacentes,
sólo tiene que mantener en memoria la línea única más reciente.

`@instructions`
Escribe un conducto para:

- obtén la segunda columna de `seasonal/winter.csv`,
- elimina la palabra "Diente" de la salida para que sólo se muestren los nombres de los dientes,
- ordenar la salida de modo que todas las apariciones de un nombre de diente concreto sean adyacentes; y
- mostrar cada nombre de diente una vez junto con un recuento de la frecuencia con la que se produce.

El inicio de tu canalización es el mismo que en el ejercicio anterior:

```
cut -d , -f 2 seasonal/winter.csv | grep -v Tooth
```

Amplíalo con un comando `sort`, y utiliza `uniq -c` para mostrar líneas únicas con un recuento de la frecuencia con que se produce cada una, en lugar de utilizar `uniq` y `wc`.

`@hint`
Copia y pega el comando en las instrucciones, pipea a `sort` sin banderas, y luego pipea de nuevo a `uniq` con una bandera `-c`.

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
                     incorrect_msg="You should start from this command: `cut -d , -f 2 seasonal/winter.csv | grep -v Tooth`. Now extend it!"),
            has_code('\|\s+sort', incorrect_msg="Have you extended the command with `| sort`?"),
            has_code('\|\s+uniq', incorrect_msg="Have you extended the command with `| uniq`?"),
            has_code('-c', incorrect_msg="Have you included counts with `-c`?")
        )
    )
)
Ex().success_msg("Great! After all of this work on a pipe, it would be nice if we could store the result, no?")
```

---

## ¿Cómo puedo guardar la salida de una tubería?

```yaml
type: MultipleChoiceExercise
key: 4115aa25b2
xp: 50
```

El intérprete de comandos nos permite redirigir la salida de una secuencia de comandos canalizados:

```{shell}
cut -d , -f 2 seasonal/*.csv | grep -v Tooth > teeth-only.txt
```

Sin embargo, `>` debe aparecer al final de la tubería:
si intentamos utilizarlo en el medio, así:

```{shell}
cut -d , -f 2 seasonal/*.csv > teeth-only.txt | grep -v Tooth
```

entonces toda la salida de `cut` se escribe en `teeth-only.txt`,
así que no queda nada para `grep`
y espera eternamente alguna entrada.

<hr>

¿Qué ocurre si ponemos la redirección al principio de una tubería como en:

```{shell}
> result.txt head -n 3 seasonal/winter.csv
```

`@possible_answers`
- [La salida del comando se redirige al archivo como de costumbre.]
- El intérprete de comandos lo notifica como error.
- El intérprete de comandos espera la entrada para siempre.

`@hint`
Pruébalo en la concha.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
Ex().has_chosen(1, ['Correct!', 'No; the shell can actually execute this.', 'No; the shell can actually execute this.'])
```

---

## ¿Cómo puedo detener un programa en ejecución?

```yaml
type: ConsoleExercise
key: d1694dbdcd
xp: 100
```

Los comandos y scripts que has ejecutado hasta ahora se han ejecutado rápidamente,
pero algunas tareas tardarán minutos, horas o incluso días en completarse.
También puedes equivocarte al poner la redirección en medio de una tubería,
haciendo que se cuelgue.
Si decides que no quieres que un programa siga ejecutándose,
puedes teclear `Ctrl` + `C` para finalizarlo.
A menudo se escribe `^C` en la documentación de Unix;
ten en cuenta que la "c" puede ser minúscula.

`@instructions`
Ejecuta el comando:

```{shell}
head
```

sin argumentos (para que espere una entrada que nunca llegará)
y luego detenlo escribiendo `Ctrl` + `C`.

`@hint`
Simplemente escribe head, pulsa Intro y sal del programa en ejecución con `Ctrl` + `C`.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
# Simply type head, hit Enter and exit the running program with `Ctrl` + `C`.
```

`@sct`
```{python}
Ex().has_code(r'\s*head\s*', fixed=False, incorrect_msg="Have you used `head`?")
```

---

## Para terminar

```yaml
type: BulletConsoleExercise
key: 659d3caa48
xp: 100
```

Para terminar,
construirás una tubería para averiguar cuántos registros hay en el más corto de los archivos de datos estacionales.

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
Utiliza `wc` con los parámetros adecuados para listar el número de líneas de todos los archivos de datos estacionales.
(Utiliza un comodín para los nombres de archivo en lugar de escribirlos todos a mano).

`@hint`
Utiliza `-l` para listar sólo las líneas y `*` para los nombres de archivo.

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
      has_code("wc", incorrect_msg = "Did you call `wc`?"),
      has_code("-l", incorrect_msg = "Did you count the number of lines with `-l`?"),
      has_code("seasonal/\*", incorrect_msg = "Did you get data from all `seasonal/*` files?")
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
Añade otro comando al anterior utilizando una tubería para eliminar la línea que contiene la palabra "total".

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
      has_code("wc", incorrect_msg = "Did you call `wc`?"),
      has_code("-l", incorrect_msg = "Did you count the number of lines with `-l`?"),
      has_code("seasonal/\*", incorrect_msg = "Did you get data from all `seasonal/*` files?"),
      has_code("|", incorrect_msg = "Did you pipe from `wc` to `grep` using `|`?"),      
      has_code("grep", incorrect_msg = "Did you call `grep`?"),
      has_code("-v", incorrect_msg = "Did you invert the match with `-v`?"),
      has_code("total", incorrect_msg = "Did you search for `total`?")
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
Añade dos etapas más a la cadena que utilicen `sort -n` y `head -n 1` para encontrar el archivo que contenga menos líneas.

`@hint`
- Utiliza la bandera `-n` de `sort` para ordenar numéricamente.
- Utiliza `head`'s `-n` flag para limitarte a mantener 1 línea.

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
      has_code("wc", incorrect_msg = "Did you call `wc`?"),
      has_code("-l", incorrect_msg = "Did you count the number of lines with `-l`?"),
      has_code("seasonal/\*", incorrect_msg = "Did you get data from all `seasonal/*` files?"),
      has_code("|", incorrect_msg = "Did you pipe from `wc` to `grep` to `sort` to `head` using `|`?"),      
      has_code("grep", incorrect_msg = "Did you call `grep`?"),
      has_code("-v", incorrect_msg = "Did you invert the match with `-v`?"),
      has_code("total", incorrect_msg = "Did you search for `total`?"),
      has_code("sort", incorrect_msg = "Did you call `sort`?"),
      has_code("-n", incorrect_msg = "Did you specify the number of lines to keep with `-n`?"),
      has_code("1", incorrect_msg = "Did you specify 1 line to keep with `-n 1`?")
    )
  )
)
Ex().success_msg("Great! It turns out `autumn.csv` is the file with the fewest lines. Rush over to chapter 4 to learn more about batch processing!")

```
