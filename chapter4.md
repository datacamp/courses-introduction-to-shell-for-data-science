---
title: Processamento em lote
description: >-
  A maioria dos comandos do shell processa vários arquivos de uma vez. Este
  capítulo mostra a você como criar seus próprios pipelines para fazer isso. Ao
  longo do caminho, você verá como o shell usa variáveis para armazenar
  informações.
lessons:
  - nb_of_exercises: 10
    title: Como o shell armazena informações?
---

## Como o shell armazena informações?

```yaml
type: MultipleChoiceExercise
key: e4d5f4adea
xp: 50
```

Como outros programas, o shell armazena informações em variáveis.
Alguns deles,
chamados de **variáveis de ambiente**,
estão disponíveis o tempo todo.
Os nomes das variáveis de ambiente são convencionalmente escritos em letras maiúsculas,
e alguns dos mais comumente usados são mostrados abaixo.

| Variável | Finalidade                           | Valor                 |
|----------|-----------------------------------|-----------------------|
| `HOME`   | Diretório pessoal do usuário             | `/home/repl`          |
| `PWD `   | Diretório de trabalho atual         | Igual ao comando `pwd`  |
| `SHELL`  | Qual programa shell está sendo usado | `/bin/bash`           |
| `USER`   | Usuário ID                         | `repl`                |

Para obter uma lista completa (que é bem longa),
você pode digitar `set` no shell.

<hr>

Use `set` e `grep` com um pipe para exibir o valor de `HISTFILESIZE`,
que determina quantos comandos antigos são armazenados em seu histórico de comandos.
Qual é o seu valor?

`@possible_answers`
- 10
- 500
- [2000]
- A variável não está lá.

`@hint`
Use `set | grep HISTFILESIZE` para obter a linha que você precisa.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err1 = "Não: o shell registra mais histórico do que isso."
err2 = "Não: o shell registra mais histórico do que isso."
correct3 = "Correto: o shell salva 2000 comandos antigos por padrão neste sistema."
err4 = "Não: a variável `HISTFILESIZE` está presente."
Ex().has_chosen(3, [err1, err2, correct3, err4])
```

---

## Como posso imprimir o valor de uma variável?

```yaml
type: ConsoleExercise
key: afae0f33a7
xp: 100
```

Uma maneira mais simples de encontrar o valor de uma variável é usar um comando chamado `echo`, que imprime seus argumentos. Digitação

```{shell}
echo hello DataCamp!
```

Impressões

```
hello DataCamp!
```

Se você tentar usá-lo para imprimir o valor de uma variável como esta:

```{shell}
echo USER
```

você imprimirá o nome da variável, `USER`.

Para obter o valor da variável, você deve colocar um cifrão `$` na frente dela. Digitação 

```{shell}
echo $USER
```

Impressões

```
repl
```

Isso é verdade em todos os lugares:
para obter o valor de uma variável chamada `X`,
Você deve escrever `$X`.
(Isso é para que o shell possa dizer se você quer dizer "um arquivo chamado X"
ou "o valor de uma variável chamada X").

`@instructions`
A variável `OSTYPE` contém o nome do tipo de sistema operacional que você está usando.
Exiba seu valor usando `echo`.

`@hint`
Chame `echo` com a variável `OSTYPE` precedida por `$`.

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
            has_code('echo', incorrect_msg="Você chamou `echo`?"),
            has_code('OSTYPE', incorrect_msg="Você imprimiu a variável de ambiente `OSTYPE`?"),
            has_code(r'\$OSTYPE', incorrect_msg="Certifique-se de prefixar `OSTYPE` com um `$`.")
        )
    )
)
Ex().success_msg("Excelente ecoar de variáveis de ambiente! Você começou bem. Vamos continuar!")
```

---

## De que outra forma o shell armazena informações?

```yaml
type: BulletConsoleExercise
key: e925da48e4
xp: 100
```

O outro tipo de variável é chamado de **variável de shell**,
que é como uma variável local em uma linguagem de programação.

Para criar uma variável de shell,
você simplesmente atribui um valor a um nome:

```{shell}
training=seasonal/summer.csv
```

*sem* nenhum espaço antes ou depois do sinal `=`.
Quando você tiver feito isso,
você pode verificar o valor da variável com:

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
Defina uma variável chamada `testing` com o valor `seasonal/winter.csv`.

`@hint`
*Não* deve haver espaços entre o nome da variável e seu valor.

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
        has_code('testing', incorrect_msg='Você definiu uma variável de shell chamada `testing`?'),
        has_code('testing=', incorrect_msg='Você escreveu `=` diretamente após testing, sem espaços?'),
        has_code('=seasonal/winter\.csv', incorrect_msg='Você definiu o valor de `testing` como `seasonal/winter.csv`?')
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
Use `head -n 1 SOMETHING` para obter a primeira linha de `seasonal/winter.csv`
usando o valor da variável `testing` em vez do nome do arquivo.

`@hint`
Lembre-se de usar `$testing` em vez de apenas `testing`
(o `$` é necessário para que você obtenha o valor da variável).

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
    has_code(r'\$testing', incorrect_msg="Você referenciou a variável de shell usando `$testing`?"),
    check_correct(
        has_output('^Date,Tooth\s*$'),
        multi(
            has_code('head', incorrect_msg="Você chamou `head`?"),
            has_code('-n', incorrect_msg="Você limitou o número de linhas com `-n`?"),
            has_code(r'-n\s+1', incorrect_msg="Você escolheu manter 1 linha com `-n 1`?")     
        )
    )
)
Ex().success_msg("Estelar! Vamos ver como você pode repetir comandos facilmente.")
```

---

## Como posso repetir um comando várias vezes?

```yaml
type: ConsoleExercise
key: 920d1887e3
xp: 100
```

As variáveis de shell também são usadas em **loops**,
que repetem os comandos várias vezes.
Se você executar esse comando:

```{shell}
for filetype in gif jpg png; do echo $filetype; done
```

que ela produz:

```
gif
jpg
png
```

Observe esses aspectos do loop:

1. A estrutura é `for`...variable... `in`...list... `; do`...body... `; done`
2. A lista de itens que o loop deve processar (no nosso caso, as palavras `gif`, `jpg` e `png`).
3. A variável que mantém o controle de qual coisa o loop está processando no momento (no nosso caso, `filetype`).
4. O corpo do loop que faz o processamento (no nosso caso, `echo $filetype`).

Observe que o corpo usa `$filetype` para obter o valor da variável em vez de apenas `filetype`,
da mesma forma que você faz com qualquer outra variável do shell.
Observe também onde estão os pontos e vírgulas:
O primeiro vem entre a lista e a palavra-chave `do`,
e a segunda vem entre o corpo e a palavra-chave `done`.

`@instructions`
Modifique o loop para que ele seja impresso:

```
docx
odt
pdf
```

Use `filetype` como o nome da variável de loop.

`@hint`
Use a estrutura de código no texto introdutório, trocando os tipos de arquivo de imagem por tipos de arquivo de documento.

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
      has_code('for', incorrect_msg='Você chamou `for`?'),
      has_code('filetype', incorrect_msg='Você usou `filetype` como a variável do loop?'),
      has_code('in', incorrect_msg='Você usou `in` antes da lista de tipos de arquivo?'),
      has_code('docx odt pdf', incorrect_msg='Você iterou sobre `docx`, `odt` e `pdf` nessa ordem?'),
      has_code(r'pdf\s*;', incorrect_msg='Você colocou um ponto e vírgula após o último elemento do loop?'),
      has_code(r';\s*do', incorrect_msg='Você usou `do` após o primeiro ponto e vírgula?'),
      has_code('echo', incorrect_msg='Você chamou `echo`?'),
      has_code(r'\$filetype', incorrect_msg='Você usou `echo` com `$filetype`?'),
      has_code(r'filetype\s*;', incorrect_msg='Você colocou um ponto e vírgula após o corpo do loop?'),
      has_code('; done', incorrect_msg='Você finalizou com `done`?')
    )
  )
)
Ex().success_msg("Excelente para laços! Laços são brilhantes se você quiser fazer a mesma coisa centenas ou milhares de vezes.")

```

---

## Como posso repetir um comando uma vez para cada arquivo?

```yaml
type: ConsoleExercise
key: 8468b70a71
xp: 100
```

Você sempre pode digitar os nomes dos arquivos que deseja processar ao escrever o loop,
mas geralmente é melhor usar curingas.
Tente executar esse loop no console:

```{shell}
for filename in seasonal/*.csv; do echo $filename; done
```

Ele imprime:

```
seasonal/autumn.csv
seasonal/spring.csv
seasonal/summer.csv
seasonal/winter.csv
```

porque o shell expande `seasonal/*.csv` para ser uma lista de quatro nomes de arquivos
antes de executar o loop.

`@instructions`
Modifique a expressão curinga para `people/*`
para que o loop imprima os nomes dos arquivos no diretório `people` 
independentemente do sufixo que eles têm ou não têm.
Use `filename` como o nome da variável do loop.

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
      has_code('for', incorrect_msg='Você chamou `for`?'),
      has_code('filename', incorrect_msg='Você usou `filename` como a variável do loop?'),
      has_code('in', incorrect_msg='Você usou `in` antes da lista de tipos de arquivos?'),
      has_code('people/\*', incorrect_msg='Você especificou uma lista de arquivos com `people/*`?'),
      has_code(r'people/\*\s*;', incorrect_msg='Você colocou um ponto e vírgula após a lista de arquivos?'),
      has_code(r';\s*do', incorrect_msg='Você usou `do` após o primeiro ponto e vírgula?'),
      has_code('echo', incorrect_msg='Você chamou `echo`?'),
      has_code(r'\$filename', incorrect_msg='Você fez echo de `$filename`?'),
      has_code(r'filename\s*;', incorrect_msg='Você colocou um ponto e vírgula após o corpo do loop?'),
      has_code('; done', incorrect_msg='Você terminou com `done`?')
    )
  )
)
Ex().success_msg("Loopy looping! Wildcards and loops make a powerful combination.")

```

---

## Como posso registrar os nomes de um conjunto de arquivos?

```yaml
type: MultipleChoiceExercise
key: 153ca10317
xp: 50
```

As pessoas geralmente definem uma variável usando uma expressão curinga para registrar uma lista de nomes de arquivos.
Por exemplo,
se você definir `datasets` da seguinte forma:

```{shell}
datasets=seasonal/*.csv
```

você pode exibir os nomes dos arquivos posteriormente usando:

```{shell}
for filename in $datasets; do echo $filename; done
```

Isso economiza digitação e diminui a probabilidade de erros.

<hr>

Se você executar esses dois comandos em seu diretório pessoal,
Quantas linhas de saída serão impressas?

```{shell}
files=seasonal/*.csv
for f in $files; do echo $f; done
```

`@possible_answers`
- None: como `files` é definido em uma linha separada, ele não tem valor na segunda linha.
- Um: a palavra "arquivos".
- Quatro: os nomes de todos os quatro arquivos de dados sazonais.

`@hint`
Lembre-se de que `X` por si só é apenas "X", enquanto `$X` é o valor da variável `X`.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err1 = "Não: você não precisa definir uma variável na mesma linha em que a usa."
err2 = "Não: este exemplo define e usa a variável `files` no mesmo shell."
correct3 = "Correto. O comando é equivalente a `for f in seasonal/*.csv; do echo $f; done`."
Ex().has_chosen(3, [err1, err2, correct3])
```

---

## O nome de uma variável versus seu valor

```yaml
type: PureMultipleChoiceExercise
key: 4fcfb63c4f
xp: 50
```

Um erro comum é você esquecer de usar `$` antes do nome de uma variável.
Quando você faz isso,
o shell usa o nome que você digitou
em vez do valor dessa variável.

Um erro mais comum para usuários experientes é digitar incorretamente o nome da variável.
Por exemplo,
se você definir `datasets` da seguinte forma:

```{shell}
datasets=seasonal/*.csv
```

e, em seguida, digite:

```{shell}
echo $datsets
```

o shell não imprime nada,
porque `datsets` (sem o segundo "a") não está definido.

<hr>

Se você executasse esses dois comandos em seu diretório pessoal,
Que saída seria impressa?

```{shell}
files=seasonal/*.csv
for f in files; do echo $f; done
```

(Leia a primeira parte do loop com atenção antes de responder).

`@hint`
Lembre-se de que `X` por si só é apenas "X", enquanto `$X` é o valor da variável `X`.

`@possible_answers`
- [Uma linha: a palavra "arquivos".]
- Quatro linhas: os nomes de todos os quatro arquivos de dados sazonais.
- Quatro linhas em branco: a variável `f` não tem um valor atribuído.

`@feedback`
- Correto: o loop usa `files` em vez de `$files`, portanto, a lista consiste na palavra "files".
- Não: o loop usa `files` em vez de `$files`, portanto, a lista consiste na palavra "files" em vez da expansão de `files`.
- Não: a variável `f` é definida automaticamente pelo loop `for`.

---

## Como posso executar vários comandos em um único loop?

```yaml
type: ConsoleExercise
key: 39b5dcf81a
xp: 100
```

A impressão de nomes de arquivos é útil para depuração,
mas o verdadeiro objetivo dos loops é fazer coisas com vários arquivos.
Esse loop imprime a segunda linha de cada arquivo de dados:

```{shell}
for file in seasonal/*.csv; do head -n 2 $file | tail -n 1; done
```

Ele tem a mesma estrutura que os outros loops que você já viu:
tudo o que é diferente é que seu corpo é um pipeline de dois comandos em vez de um único comando.

`@instructions`
Escreva um loop que imprima a última entrada de julho de 2017 (`2017-07`) em cada arquivo sazonal. Ele deve produzir um resultado semelhante ao de você:

```{shell}
grep 2017-07 seasonal/winter.csv | tail -n 1
```

mas para **_cada_** arquivo sazonal separadamente. Use `file` como o nome da variável de loop e lembre-se de percorrer a lista de arquivos `seasonal/*.csv` _(em vez de 'seasonal/winter.csv', como no exemplo_).

`@hint`
O corpo do loop é o comando grep mostrado nas instruções, com `seasonal/winter.csv` substituído por `$file`.

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
  has_code('for', incorrect_msg='Você usou `for`?'),
  check_correct(
    has_expr_output(),
    multi(
      has_code('file', incorrect_msg='Você usou `file` como a variável do loop?'),
      has_code('in', incorrect_msg='Você usou `in` antes da lista de arquivos?'),
      has_code('seasonal/\*', incorrect_msg='Você especificou uma lista de arquivos com `seasonal/*`?'),
      has_code(r'seasonal\/\*\.csv\s*;', incorrect_msg='Você colocou um ponto e vírgula após a lista de arquivos?'),
      has_code(r';\s*do', incorrect_msg='Você usou `do` após o primeiro ponto e vírgula?'),
      has_code('grep', incorrect_msg='Você chamou `grep`?'),
      has_code('2017-07', incorrect_msg='Você fez a correspondência com `2017-07`?'),
      has_code(r'\$file', incorrect_msg='Você usou `$file` como o nome da variável do loop?'),
      has_code(r'file\s*|', incorrect_msg='Você usou um pipe para conectar seu segundo comando?'),
      has_code(r'tail\s*-n\s*1', incorrect_msg='Você usou `tail -n 1` para imprimir a última entrada de cada busca no seu segundo comando?'),
      has_code('; done', incorrect_msg='Você terminou com `done`?')
    )
  )
)

Ex().success_msg("Looping em loop! Curingas e loops fazem uma combinação poderosa.")

```

---

## Por que não devo usar espaços nos nomes de arquivos?

```yaml
type: PureMultipleChoiceExercise
key: b974b7f45a
xp: 50
```

É fácil e sensato dar aos arquivos nomes com várias palavras, como `July 2017.csv`
quando você estiver usando um explorador de arquivos gráfico.
No entanto,
isso causa problemas quando você está trabalhando no shell.
Por exemplo,
Suponha que você queira renomear `July 2017.csv` para `2017 July data.csv`.
Você não pode digitar:

```{shell}
mv July 2017.csv 2017 July data.csv
```

porque, para o shell, parece que você está tentando se mover
quatro arquivos chamados `July`, `2017.csv`, `2017`, e `July` (novamente)
em um diretório chamado `data.csv`.
Em vez disso,
você precisa colocar os nomes dos arquivos entre aspas
para que o shell trate cada um deles como um único parâmetro:

```{shell}
mv 'July 2017.csv' '2017 July data.csv'
```

<hr>

Se você tiver dois arquivos chamados `current.csv` e `last year.csv`
(com um espaço em seu nome)
e você digita:

```{shell}
rm current.csv last year.csv
```

o que acontecerá:

`@hint`
O que você acha que aconteceria se alguém lhe mostrasse o comando e você não soubesse quais arquivos existiam?

`@possible_answers`
- O shell imprimirá uma mensagem de erro porque `last` e `year.csv` não existem.
- O shell excluirá `current.csv`.
- [Ambas as opções acima.]
- Nada.

`@feedback`
- Sim, mas isso não é tudo.
- Sim, mas isso não é tudo.
- Correto. Você pode usar aspas simples, `'`, ou aspas duplas, `"`, ao redor dos nomes dos arquivos.
- Infelizmente, não.

---

## Como posso fazer várias coisas em um único loop?

```yaml
type: MultipleChoiceExercise
key: f6d0530991
xp: 50
```

Todos os loops que você viu até agora têm um único comando ou pipeline em seu corpo,
mas um loop pode conter qualquer número de comandos.
Para dizer à concha onde uma termina e a outra começa,
você deve separá-los com ponto e vírgula:

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

Suponha que você tenha esquecido o ponto e vírgula entre os comandos `echo` e `head` no loop anterior,
para que você peça para o shell ser executado:

```{shell}
for f in seasonal/*.csv; do echo $f head -n 2 $f | tail -n 1; done
```

O que a concha fará?

`@possible_answers`
- Imprimir uma mensagem de erro.
- Imprima uma linha para cada um dos quatro arquivos.
- Imprima uma linha para `autumn.csv` (o primeiro arquivo).
- Imprime a última linha de cada arquivo.

`@hint`
Você pode canalizar a saída de `echo` para `tail`.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err1 = "Não: o loop será executado, mas não fará algo sensato."
correct2 = "Sim: `echo` produz uma linha que inclui o nome do arquivo duas vezes, que `tail` então copia."
err3 = "Não: o loop é executado uma vez para cada um dos quatro nomes de arquivos."
err4 = "Não: a entrada de `tail` é a saída de `echo` para cada nome de arquivo."
Ex().has_chosen(2, [err1, correct2, err3, err4])
```
