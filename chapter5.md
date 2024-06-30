---
title: Criação de novas ferramentas
description: >-
  O histórico permite que você repita coisas com apenas alguns toques no
  teclado, e os pipes permitem que você combine comandos existentes para criar
  novos. Neste capítulo, você verá como dar um passo adiante e criar seus
  próprios comandos.
lessons:
  - nb_of_exercises: 9
    title: Como posso editar um arquivo?
---

## Como posso editar um arquivo?

```yaml
type: ConsoleExercise
key: 39eee3cfc0
xp: 100
```

O Unix tem uma variedade impressionante de editores de texto.
Para este curso,
usaremos um simples chamado Nano.
Se você digitar `nano filename`,
Você abrirá o site `filename` para edição.
(ou criá-lo se ele ainda não existir).
Você pode se movimentar com as teclas de seta,
excluir caracteres usando backspace,
e fazer outras operações com combinações de teclas de controle:

- `Ctrl` + `K`: excluir uma linha.
- `Ctrl` + `U`: remover a exclusão de uma linha.
- `Ctrl` + `O`: salva o arquivo ('O' significa 'output'). _Você também precisará pressionar Enter para confirmar o nome do arquivo!_
- `Ctrl` + `X`: sair do editor.

`@instructions`
Execute `nano names.txt` para editar um novo arquivo em seu diretório pessoal
e digite as quatro linhas a seguir:

```
Lovelace
Hopper
Johnson
Wilson
```

Para salvar o que você escreveu,
Digite `Ctrl` + `O` para gravar o arquivo,
e, em seguida, tecle Enter para confirmar o nome do arquivo,
e, em seguida, `Ctrl` + `X` para sair do editor.

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
patt = "Você incluiu a linha `%s` no arquivo `names.txt`? Use `nano names.txt` novamente para atualizar seu arquivo. Use `Ctrl` + `O` para salvar e `Ctrl` + `X` para sair."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/names.txt').multi(
        has_code(r'Lovelace', incorrect_msg=patt%'Lovelace'),
        has_code(r'Hopper', incorrect_msg=patt%'Hopper'),
        has_code(r'Johnson', incorrect_msg=patt%'Johnson'),
        has_code(r'Wilson', incorrect_msg=patt%'Wilson')
    )
)
Ex().success_msg("Muito bem! Vamos para a próxima!")
```

---

## Como posso registrar o que acabei de fazer?

```yaml
type: BulletConsoleExercise
key: 80c3532985
xp: 100
```

Quando você estiver fazendo uma análise complexa,
você frequentemente desejará manter um registro dos comandos que usou.
Você pode fazer isso com as ferramentas que já viu:

1. Execute `history`.
2. Canalize sua saída para `tail -n 10` (ou para quantas etapas recentes você quiser salvar).
3. Redirecione isso para um arquivo chamado algo como `figure-5.history`.

Isso é melhor do que anotar as coisas em um caderno de laboratório
porque é garantido que você não perderá nenhuma etapa.
Ela também ilustra a ideia central da concha:
ferramentas simples que produzem e consomem linhas de texto
podem ser combinados de várias maneiras
para resolver uma ampla gama de problemas.

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
Copie os arquivos `seasonal/spring.csv` e `seasonal/summer.csv` para seu diretório pessoal.

`@hint`
Use `cp` para copiar e `~` como atalho para o caminho até seu diretório pessoal.

`@solution`
```{shell}
cp seasonal/s* ~

```

`@sct`
```{python}
msg="Você usou `cp seasonal/s* ~` para copiar os arquivos necessários para o seu diretório home?"
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/spring.csv', missing_msg=msg).\
        has_code(r'2017-01-25,wisdom', incorrect_msg=msg),
    check_file('/home/repl/summer.csv', missing_msg=msg).\
        has_code(r'2017-01-11,canine', incorrect_msg=msg)
)
Ex().success_msg("Registro notável! Se você digitou algum comando errado, você sempre pode usar `nano` para limpar o arquivo de histórico de salvamentos depois.")
```

***

```yaml
type: ConsoleExercise
key: 09a432e4df
xp: 35
```

`@instructions`
Use o `grep` com o sinalizador `-h` (para impedir que ele imprima nomes de arquivos)
e `-v Tooth` (para selecionar linhas que *não* correspondem à linha do cabeçalho)
para selecionar os registros de dados em `spring.csv` e `summer.csv`, nessa ordem
e redirecionar a saída para `temp.csv`.

`@hint`
Coloque os sinalizadores antes dos nomes dos arquivos.

`@solution`
```{shell}
grep -h -v Tooth spring.csv summer.csv > temp.csv

```

`@sct`
```{python}
msg1 = "Certifique-se de redirecionar a saída do comando `grep` para `temp.csv` com `>`!"
msg2 = "Você usou `grep -h -v ___ ___ ___` (preencha os espaços) para preencher `temp.csv`?"
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
Passe o tubo `history` em `tail -n 3`
e redirecionar a saída para `steps.txt`
para salvar os três últimos comandos em um arquivo.
(Você precisa salvar três em vez de apenas dois
porque o próprio comando `history` estará na lista).

`@hint`
Lembre-se de que o redirecionamento com `>` vem no final da sequência de comandos canalizados.

`@solution`
```{shell}
history | tail -n 3 > steps.txt

```

`@sct`
```{python}
msg1="Certifique-se de redirecionar a saída do seu comando para `steps.txt`."
msg2="Você usou `history | tail ___ ___` (preencha os espaços) para preencher `steps.txt`?"
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
Ex().success_msg("Muito bem! Vamos aumentar o nível!")
```

---

## Como posso salvar os comandos para re-executar mais tarde?

```yaml
type: BulletConsoleExercise
key: 4507a0dbd8
xp: 100
```

Até agora, você tem usado o shell de forma interativa.
Mas como os comandos que você digita são apenas texto,
você pode armazená-los em arquivos para que o shell os execute repetidamente.
Para começar a explorar esse recurso avançado, você pode usar o recurso de gerenciamento de dados,
Coloque o seguinte comando em um arquivo chamado `headers.sh`:

```{shell}
head -n 1 seasonal/*.csv
```

Esse comando seleciona a primeira linha de cada um dos arquivos CSV no diretório `seasonal`.
Quando você tiver criado esse arquivo,
você pode executá-lo digitando:

```{shell}
bash headers.sh
```

Isso informa ao shell (que é apenas um programa chamado `bash`)
para executar os comandos contidos no arquivo `headers.sh`,
que produz o mesmo resultado que a execução direta dos comandos.

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
Use o endereço `nano dates.sh` para criar um arquivo chamado `dates.sh`
que contém esse comando:

```{shell}
cut -d , -f 1 seasonal/*.csv
```

para extrair a primeira coluna de todos os arquivos CSV em `seasonal`.

`@hint`
Coloque os comandos mostrados no arquivo sem linhas em branco ou espaços extras.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/dates.sh ~

```

`@sct`
```{python}
msg = "Você incluiu a linha `cut -d , -f 1 seasonal/*.csv` no arquivo `dates.sh`? Use `nano dates.sh` novamente para atualizar seu arquivo. Use `Ctrl` + `O` para salvar e `Ctrl` + `X` para sair."
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
Use `bash` para executar o arquivo `dates.sh`.

`@hint`
Use `bash filename` para executar o arquivo.

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
      has_code("bash", incorrect_msg = 'Você chamou `bash`?'),
      has_code("dates.sh", incorrect_msg = 'Você especificou o arquivo `dates.sh`?')
    )
  )
)
```

---

## Como posso reutilizar os tubos?

```yaml
type: BulletConsoleExercise
key: da13667750
xp: 100
```

Um arquivo cheio de comandos do shell é chamado de \*shell**script**,
ou, às vezes, apenas um "script". Os scripts não precisam ter nomes que terminem em `.sh`,
mas esta lição usará essa convenção
para ajudar você a controlar quais arquivos são scripts.

Os scripts também podem conter pipes.
Por exemplo,
se `all-dates.sh` contiver essa linha:

```{shell}
cut -d , -f 1 seasonal/*.csv | grep -v Date | sort | uniq
```

então:

```{shell}
bash all-dates.sh > dates.out
```

extrairá as datas exclusivas dos arquivos de dados sazonais
e salve-os em `dates.out`.

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
Um arquivo `teeth.sh` no seu diretório pessoal foi preparado para você, mas contém alguns espaços em branco.
Use o Nano para editar o arquivo e substituir os dois espaços reservados `____` 
com `seasonal/*.csv` e `-c`, de modo que esse script imprima uma contagem dos dados que você recebeu.
número de vezes que cada nome de dente aparece nos arquivos CSV no diretório `seasonal`.

`@hint`
Use `nano teeth.sh` para editar o arquivo.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/teeth.sh ~

```

`@sct`
```{python}
msg="Você substituiu os espaços em branco corretamente para que o comando em `teeth.sh` leia `cut -d , -f 2 seasonal/*.csv | grep -v Tooth | sort | uniq -c`? Use `nano teeth.sh` novamente para fazer as alterações necessárias."
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
Use `bash` para executar `teeth.sh` e `>` para redirecionar sua saída para `teeth.out`.

`@hint`
Lembre-se de que `> teeth.out` deve vir *após* o comando que está produzindo a saída.

`@solution`
```{shell}
# We need to use 'cp' below to satisfy our automated tests.
# You should only use the last line that runs 'bash'.
cp /solutions/teeth.sh .
bash teeth.sh > teeth.out

```

`@sct`
```{python}
msg="Você redirecionou corretamente o resultado de `bash teeth.sh` para `teeth.out` com o `>`?"
Ex().multi(
  has_cwd('/home/repl'),
  check_correct(
    check_file('/home/repl/teeth.out').multi(
      has_code(r'31 canine', incorrect_msg=msg),
      has_code(r'17 wisdom', incorrect_msg=msg)
    ),
    multi(
      has_code("bash", incorrect_msg = 'Você chamou `bash`?'),
      has_code("bash\s+teeth.sh", incorrect_msg = 'Você executou o arquivo `teeth.sh`?'),
      has_code(">\s+teeth.out", incorrect_msg = 'Você redirecionou para o arquivo `teeth.out`?')
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
Execute `cat teeth.out` para inspecionar seus resultados.

`@hint`
Lembre-se de que você pode digitar os primeiros caracteres de um nome de arquivo e, em seguida, pressionar a tecla Tab para completar automaticamente.

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
      has_code("cat", incorrect_msg = 'Você chamou `cat`?'),
      has_code("teeth.out", incorrect_msg = 'Você especificou o arquivo `teeth.out`?')
    )
  )
)
Ex().success_msg("Legal! Tudo isso pode parecer forçado no início, mas o bom é que você está automatizando partes do seu fluxo de trabalho passo a passo. Algo que é muito útil como cientista de dados!")

```

---

## Como posso passar nomes de arquivos para scripts?

```yaml
type: BulletConsoleExercise
key: c2623b9c14
xp: 100
```

Um script que processa arquivos específicos é útil como um registro do que você fez, mas um que permite processar qualquer arquivo que você queira é mais útil.
Para apoiar isso,
você pode usar a expressão especial `$@` (cifrão imediatamente seguido pelo sinal de arroba)
para significar "todos os parâmetros da linha de comando fornecidos ao script".

Por exemplo, se `unique-lines.sh` contiver `sort $@ | uniq`, quando você executar:

```{shell}
bash unique-lines.sh seasonal/summer.csv
```

o shell substitui `$@` por `seasonal/summer.csv` e processa um arquivo. Se você executar isso:

```{shell}
bash unique-lines.sh seasonal/summer.csv seasonal/autumn.csv
```

ele processa dois arquivos de dados, e assim por diante.

_Como lembrete, para salvar o que você escreveu no Nano, digite `Ctrl` + `O` para gravar o arquivo, depois Enter para confirmar o nome do arquivo e, em seguida, `Ctrl` + `X` para sair do editor._

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
Edite o script `count-records.sh` com o Nano e preencha os dois espaços reservados `____` 
com `$@` e `-l` _(a letra_), respectivamente, para que você conte o número de linhas em um ou mais arquivos,
excluindo a primeira linha de cada um.

`@hint`
* Use `nano count-records.sh` para editar o nome do arquivo.
* Certifique-se de que você está especificando a _letra_ `-l`, e não o número um.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/count-records.sh ~

```

`@sct`
```{python}
msg="Você substituiu os espaços em branco corretamente para que o comando em `count-records.sh` leia `tail -q -n +2 $@ | wc -l`? Use `nano count-records.sh` novamente para fazer as alterações necessárias."
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
Execute `count-records.sh` em `seasonal/*.csv`
e redirecione a saída para `num-records.out` usando `>`.

`@hint`
Use `>` para redirecionar a saída.

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
      has_code("bash", incorrect_msg = 'Você chamou `bash`?'),
      has_code("bash\s+count-records.sh", incorrect_msg = 'Você executou o arquivo `count-records.sh`?'),
      has_code("seasonal/\*", incorrect_msg = 'Você especificou os arquivos para processar com `seasonal/*`?'),
      has_code(">\s+num-records.out", incorrect_msg = 'Você redirecionou para o arquivo `num-records.out`?')
    )
  )
)
Ex().success_msg("Trabalho bem feito! Seu poder no shell está em constante expansão!")
```

---

## Como posso processar um único argumento?

```yaml
type: PureMultipleChoiceExercise
key: 4092cb4cda
xp: 50
```

Assim como `$@`,
o shell permite que você use `$1`, `$2` e assim por diante para se referir a parâmetros específicos da linha de comando.
Você pode usar isso para escrever comandos que pareçam mais simples ou mais naturais do que os do shell.
Por exemplo,
Você pode criar um script chamado `column.sh` que seleciona uma única coluna de um arquivo CSV 
quando o usuário fornece o nome do arquivo como o primeiro parâmetro e a coluna como o segundo:

```{shell}
cut -d , -f $2 $1
```

e, em seguida, execute-o usando:

```{shell}
bash column.sh seasonal/autumn.csv 1
```

Observe como o script usa os dois parâmetros na ordem inversa.

<hr>

O script `get-field.sh` deve receber um nome de arquivo,
o número da linha a ser selecionada,
o número da coluna a ser selecionada,
e imprimir apenas esse campo de um arquivo CSV.
Por exemplo:

```
bash get-field.sh seasonal/summer.csv 4 2
```

Você deve selecionar o segundo campo da linha 4 de `seasonal/summer.csv`.
Qual dos seguintes comandos deve ser colocado em `get-field.sh` para que você faça isso?

`@hint`
Lembre-se de que os parâmetros da linha de comando são numerados da esquerda para a direita.

`@possible_answers`
- `head -n $1 $2 | tail -n 1 | cut -d , -f $3`
- [`head -n $2 $1 | tail -n 1 | cut -d , -f $3`]
- `head -n $3 $1 | tail -n 1 | cut -d , -f $2`
- `head -n $2 $3 | tail -n 1 | cut -d , -f $1`

`@feedback`
- Não: isso tentará usar o nome do arquivo como o número de linhas a serem selecionadas com `head`.
- Correto!
- Não: isso tentará usar o número da coluna como o número da linha e vice-versa.
- Não: isso usará o número do campo como o nome do arquivo e vice-versa.

---

## Como um script de shell pode fazer muitas coisas?

```yaml
type: TabConsoleExercise
key: 846bc70e9d
xp: 100
```

Até agora, nossos scripts de shells tinham um único comando ou pipe, mas um script pode conter muitas linhas de comandos. Por exemplo, você pode criar um que informe quantos registros estão no menor e no maior dos seus arquivos de dados, ou seja, o intervalo dos comprimentos dos seus conjuntos de dados.

Observe que, no Nano, "copiar e colar" é feito navegando até a linha que você deseja copiar, pressionando `CTRL` + `K` para cortar a linha e, em seguida, `CTRL` + `U` duas vezes para colar duas cópias dela.

_Como lembrete, para salvar o que você escreveu no Nano, digite `Ctrl` + `O` para gravar o arquivo, depois Enter para confirmar o nome do arquivo e, em seguida, `Ctrl` + `X` para sair do editor._

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
Use o Nano para editar o script `range.sh`
e substitua os dois espaços reservados `____` 
com `$@` e `-v`
de modo que ele liste os nomes e o número de linhas em todos os arquivos fornecidos na linha de comando
*sem* mostrar o número total de linhas em todos os arquivos.
(Não tente subtrair as linhas do cabeçalho da coluna dos arquivos).

`@hint`
Use `wc -l $@` para contar as linhas em todos os arquivos fornecidos na linha de comando.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/range-1.sh range.sh

```

`@sct`
```{python}
msg="Você substituiu os espaços em branco corretamente para que o comando em `range.sh` leia `wc -l $@ | grep -v total`? Use `nano range.sh` novamente para fazer as alterações necessárias."
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
Use o Nano novamente para adicionar `sort -n` e `head -n 1` nessa ordem
para o pipeline em `range.sh`
para exibir o nome e a contagem de linhas do arquivo mais curto fornecido a ele.

`@hint`


`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/range-2.sh range.sh

```

`@sct`
```{python}
msg="Você adicionou `sort -n` e `head -n 1` com pipes ao arquivo `range.sh`? Use `nano range.sh` novamente para fazer as alterações necessárias."
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
Novamente usando o Nano, adicione uma segunda linha a `range.sh` para imprimir o nome e a contagem de registros de
o arquivo *mais longo* do diretório *, bem* como o mais curto.
Essa linha deve ser uma duplicata da linha que você já escreveu,
mas com `sort -n -r` em vez de `sort -n`.

`@hint`
Copie a primeira linha e modifique a ordem de classificação.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/range-3.sh range.sh

```

`@sct`
```{python}
msg1="Mantenha a primeira linha no arquivo `range.sh`: `wc -l $@ | grep -v total | sort -n | head -n 1`"
msg2="Você duplicou a primeira linha em `range.sh` e fez uma pequena mudança? `sort -n -r` em vez de `sort -n`!"
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
Execute o script nos arquivos do diretório `seasonal` 
Usando `seasonal/*.csv` para corresponder a todos os arquivos
e redirecionar a saída usando `>`
em um arquivo chamado `range.out` em seu diretório pessoal.

`@hint`
Use `bash range.sh` para executar o script, `seasonal/*.csv` para especificar arquivos e `> range.out` para redirecionar a saída.

`@solution`
```{shell}
bash range.sh seasonal/*.csv > range.out

```

`@sct`
```{python}
msg="Você redirecionou corretamente o resultado de `bash range.sh seasonal/*.csv` para `range.out` com o `>`?"
Ex().multi(
has_cwd('/home/repl'),
multi(
has_code("bash", incorrect_msg = 'Você chamou `bash`?'),
has_code("bash\s+range.sh", incorrect_msg = 'Você executou o arquivo `range.sh`?'),
has_code("seasonal/\*", incorrect_msg = 'Você especificou os arquivos a serem processados com `seasonal/*`?'),
has_code(">\s+range.out", incorrect_msg = 'Você redirecionou para o arquivo `range.out`?')
)
)

Ex().success_msg("Está indo bem. Vá para o próximo exercício para aprender sobre como escrever loops!")

```

---

## Como posso escrever loops em um script de shell?

```yaml
type: BulletConsoleExercise
key: 6be8ca6009
xp: 100
```

Os scripts de shell também podem conter loops. Você pode escrevê-los usando ponto e vírgula ou dividi-los em linhas sem ponto e vírgula para torná-los mais legíveis:

```{shell}
# Print the first and last data records of each file.
for filename in $@
do

    head -n 2 $filename | tail -n 1

    tail -n 1 $filename

done
```

(Você não precisa recuar os comandos dentro do loop, mas isso torna as coisas mais claras).

A primeira linha desse script é um **comentário** para informar aos leitores o que o script faz. Os comentários começam com o caractere `#` e vão até o final da linha. Seu futuro eu agradecerá por adicionar breves explicações como a mostrada aqui a cada script que você escrever.

_Como lembrete, para salvar o que você escreveu no Nano, digite `Ctrl` + `O` para gravar o arquivo, depois Enter para confirmar o nome do arquivo e, em seguida, `Ctrl` + `X` para sair do editor._

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
Preencha os espaços reservados no script `date-range.sh`
com `$filename` (duas vezes), `head` e `tail`
para que ele imprima a primeira e a última data de um ou mais arquivos.

`@hint`
Lembre-se de usar `$filename` para obter o valor atual da variável do loop.

`@solution`
```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/date-range.sh date-range.sh

```

`@sct`
```{python}
msgpatt="No `date-range.sh`, você alterou a linha %s no loop para ser `%s`? Use `nano date-range.sh` para fazer as alterações."
cmdpatt = 'cut -d , -f 1 $filename | grep -v Date | sort | %s -n 1'
msg1=msgpatt%('primeira', cmdpatt%'head')
msg2=msgpatt%('segunda', cmdpatt%'tail')
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
Execute o site `date-range.sh` em todos os quatro arquivos de dados sazonais
usando `seasonal/*.csv` para combinar com seus nomes.

`@hint`
A expressão curinga deve começar com o nome do diretório.

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
      has_code("bash", incorrect_msg = 'Você chamou `bash`?'),
      has_code("bash\s+date-range.sh", incorrect_msg = 'Você executou o arquivo `date-range.sh`?'),
      has_code("seasonal/\*", incorrect_msg = 'Você especificou os arquivos a serem processados com `seasonal/*`?')
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
Execute `date-range.sh` em todos os quatro arquivos de dados sazonais usando `seasonal/*.csv` para corresponder aos seus nomes,
e envie sua saída para `sort` para que você veja que seus scripts podem ser usados como os comandos internos do Unix.

`@hint`
Use a mesma expressão curinga que você usou anteriormente.

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
      has_code("bash", incorrect_msg = 'Você chamou `bash`?'),
      has_code("bash\s+date-range.sh", incorrect_msg = 'Você executou o arquivo `date-range.sh`?'),
      has_code("seasonal/\*", incorrect_msg = 'Você especificou os arquivos a serem processados com `seasonal/*`?'),
      has_code("|", incorrect_msg = 'Você canalizou a saída do script para `sort`?'),
      has_code("sort", incorrect_msg = 'Você chamou `sort`?')
    )
  )
)
Ex().success_msg("Mágico! Note como todas as coisas que aprendemos são componíveis.")
```

---

## O que acontece quando não forneço nomes de arquivos?

```yaml
type: MultipleChoiceExercise
key: 8a162c4d54
xp: 50
```

Um erro comum em scripts de shell (e comandos interativos) é colocar nomes de arquivos no lugar errado.
Se você digitar:

```{shell}
tail -n 3
```

então, já que `tail` não recebeu nenhum nome de arquivo,
ele espera para ler a entrada do seu teclado.
Isso significa que se você digitar:

```{shell}
head -n 5 | tail -n 3 somefile.txt
```

Em seguida, `tail` imprime as três últimas linhas de `somefile.txt`,
mas o `head` espera eternamente pela entrada do teclado,
já que não recebeu um nome de arquivo e não há nada antes dele no pipeline.

<hr>

Suponha que você digite acidentalmente:

```{shell}
head -n 5 | tail -n 3 somefile.txt
```

O que você deve fazer em seguida?

`@possible_answers`
- Aguarde 10 segundos para que o site `head` atinja o tempo limite.
- Digite `somefile.txt` e pressione Enter para dar a `head` alguma entrada.
- Use `Ctrl` + `C` para interromper a execução do programa `head`.

`@hint`
O que o site `head` faz se você não tiver um nome de arquivo e não houver nada no upstream dele?

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
a1 = 'Não, os comandos não vão expirar.'
a2 = 'Não, isso dará ao `head` o texto `somefile.txt` para processar, mas então ele ficará esperando por mais entrada.'
a3 = "Sim! Você deve usar `Ctrl` + `C` para parar um programa em execução. Isso conclui este curso introdutório! Se você estiver interessado em aprender mais sobre ferramentas de linha de comando, recomendamos fortemente fazer nosso curso introdutório gratuito de Git!"
Ex().has_chosen(3, [a1, a2, a3])
```
