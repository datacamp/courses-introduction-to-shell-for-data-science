---
title: Combinação de ferramentas
description: >-
  O verdadeiro poder do shell Unix não está nos comandos individuais, mas na
  facilidade com que eles podem ser combinados para fazer coisas novas. Este
  capítulo mostrará como usar esse poder para selecionar os dados que você
  deseja e apresentará comandos para classificar valores e remover duplicatas.
lessons:
  - nb_of_exercises: 12
    title: Como posso armazenar a saída de um comando em um arquivo?
---

## Como posso armazenar a saída de um comando em um arquivo?

```yaml
type: ConsoleExercise
key: 07a427d50c
xp: 100
```

Todas as ferramentas que você viu até agora permitem que você nomeie os arquivos de entrada.
A maioria não tem uma opção para nomear um arquivo de saída porque não precisa de um.
Em vez disso,
você pode usar o **redirecionamento** para salvar a saída de qualquer comando em qualquer lugar que desejar.
Se você executar esse comando:

```{shell}
head -n 5 seasonal/summer.csv
```

ele imprime as primeiras 5 linhas dos dados de verão na tela.
Se, em vez disso, você executar esse comando:

```{shell}
head -n 5 seasonal/summer.csv > top.csv
```

nada aparece na tela.
Em vez disso,
`head`é colocada em um novo arquivo chamado `top.csv`.
Você pode dar uma olhada no conteúdo desse arquivo usando `cat`:

```{shell}
cat top.csv
```

O sinal de maior que `>` informa ao shell para redirecionar a saída de `head` para um arquivo.
Isso não faz parte do comando `head`;
Em vez disso,
ele funciona com todos os comandos do shell que produzem saída.

`@instructions`
Combine `tail` com redirecionamento para salvar as últimas 5 linhas de `seasonal/winter.csv` em um arquivo chamado `last.csv`.

`@hint`
Use `tail -n 5` para obter as últimas 5 linhas.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
tail -n 5 seasonal/winter.csv > last.csv
```

`@sct`
```{python}
patt = "A linha `%s` deveria estar no arquivo `last.csv`, mas não está. Redirecione a saída de `tail -n 5 seasonal/winter.csv` para `last.csv` com `>`."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/last.csv').multi(
        check_not(has_code('2017-07-01,incisor'), incorrect_msg='`last.csv` tem muitas linhas. Você usou a flag `-n 5` com `tail`?'),
        has_code('2017-07-17,canine', incorrect_msg=patt%'2017-07-17,canine'),
        has_code('2017-08-13,canine', incorrect_msg=patt%'2017-08-13,canine')
    )
)
Ex().success_msg("Legal! Vamos praticar mais um pouco!")

```

---

## Como posso usar a saída de um comando como entrada?

```yaml
type: BulletConsoleExercise
key: f47d337593
xp: 100
```

Suponha que você queira obter linhas do meio de um arquivo.
Mais especificamente,
Suponha que você queira obter as linhas 3-5 de um dos nossos arquivos de dados.
Você pode começar usando o site `head` para obter as primeiras 5 linhas
e redirecioná-lo para um arquivo,
e, em seguida, use `tail` para selecionar os últimos 3:

```{shell}
head -n 5 seasonal/winter.csv > top.csv
tail -n 3 top.csv
```

Uma verificação rápida confirma que essas são as linhas 3-5 do nosso arquivo original,
porque são as últimas 3 linhas das 5 primeiras.

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
Selecione as duas últimas linhas de `seasonal/winter.csv`
e salve-os em um arquivo chamado `bottom.csv`.

`@hint`
Use `tail` para selecionar linhas e `>` para redirecionar a saída de `tail`.

`@solution`
```{shell}
tail -n 2 seasonal/winter.csv > bottom.csv

```

`@sct`
```{python}
patt="A linha `%s` deveria estar no arquivo `bottom.csv`, mas não está. Redirecione a saída de `tail -n 2 seasonal/winter.csv` para `bottom.csv` com `>`."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/bottom.csv').multi(
        check_not(has_code('2017-08-11,bicuspid'), incorrect_msg = '`bottom.csv` tem muitas linhas. Você usou a flag `-n 2` com `tail`?'),
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
Selecione a primeira linha de `bottom.csv`
para que você obtenha a penúltima linha do arquivo original.

`@hint`
Use `head` para selecionar a linha que você deseja.

`@solution`
```{shell}
head -n 1 bottom.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/bottom.csv').has_code('2017-08-11,wisdom', incorrect_msg="Há algo errado com o arquivo `bottom.csv`. Certifique-se de não alterá-lo!"),
    has_expr_output(strict=True, incorrect_msg="Você usou `head` corretamente no `bottom.csv`? Certifique-se de usar a flag `-n` corretamente.")
)

Ex().success_msg("Muito bem. Vá para o próximo exercício para descobrir maneiras melhores de combinar comandos.")                             
```

---

## Qual é a melhor maneira de combinar comandos?

```yaml
type: ConsoleExercise
key: b36aea9a1e
xp: 100
```

O uso do redirecionamento para combinar comandos tem duas desvantagens:

1. Ele deixa muitos arquivos intermediários espalhados (como `top.csv`).
2. Os comandos para produzir o resultado final estão espalhados por várias linhas do histórico.

O shell fornece outra ferramenta que resolve esses dois problemas ao mesmo tempo, chamada de **pipe**.
Mais uma vez,
Comece executando `head`:

```{shell}
head -n 5 seasonal/summer.csv
```

Em vez de enviar a saída do `head` para um arquivo,
adicione uma barra vertical e o comando `tail` *sem* um nome de arquivo:

```{shell}
head -n 5 seasonal/summer.csv | tail -n 3
```

O símbolo de pipe diz ao shell para usar a saída do comando à esquerda
como a entrada para o comando à direita.

`@instructions`
Use `cut` para selecionar todos os nomes de dentes da coluna 2 do arquivo delimitado por vírgula `seasonal/summer.csv` e, em seguida, envie o resultado para `grep`, com uma correspondência invertida, para excluir a linha de cabeçalho que contém a palavra "Tooth". *`cut` e `grep` foram abordados em detalhes no Capítulo 2, exercícios 8 e 11, respectivamente.*

`@hint`
- A primeira parte do comando tem o formato `cut -d field_delimiter -f column_number filename`.
- A segunda parte do comando tem o formato `grep -v thing_to_match`.

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
    has_expr_output(incorrect_msg = 'Você canalizou o resultado de `cut -d , -f 2 seasonal/summer.csv` em `grep -v Tooth` com `|`?'),
    check_not(has_output("Tooth"), incorrect_msg = 'Você excluiu a linha de cabeçalho `"Tooth"` usando `grep`?')
)
Ex().success_msg("Canalização perfeita! Esta pode ser a primeira vez que você usou `|`, mas definitivamente não será a última!")
```

---

## Como posso combinar vários comandos?

```yaml
type: ConsoleExercise
key: b8753881d6
xp: 100
```

Você pode encadear qualquer número de comandos.
Por exemplo,
esse comando:

```{shell}
cut -d , -f 1 seasonal/spring.csv | grep -v Date | head -n 10
```

vontade:

1. selecione a primeira coluna dos dados de primavera;
2. remover a linha de cabeçalho que contém a palavra "Data"; e
3. selecione as primeiras 10 linhas de dados reais.

`@instructions`
No exercício anterior, você usou o seguinte comando para selecionar todos os nomes de dentes da coluna 2 de `seasonal/summer.csv`:

```
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth
```

Amplie esse pipeline com um comando `head` para selecionar apenas o primeiro nome de dente.

`@hint`
Copie e cole o código nas instruções, acrescente um pipe e, em seguida, chame `head` com o sinalizador `-n`.

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
    has_output('^\s*canine\s*$', incorrect_msg = "Você usou `|` para estender o pipeline com um comando `head`? Certifique-se de definir corretamente a flag `-n`."),
    # by coincidence, tail -n 1 returns the same as head -n 1, so check that head was called
    has_code("head", "Você usou `|` para estender o pipeline com um comando `head`?")
)
Ex().success_msg("Encadeamento alegre! Ao encadear vários comandos juntos, você pode construir pipelines poderosos de manipulação de dados.")

```

---

## Como posso contar os registros em um arquivo?

```yaml
type: ConsoleExercise
key: ae6a48d6aa
xp: 100
```

O comando `wc` (abreviação de "word count", contagem de palavras) imprime o número de **caracteres**, **palavras**e **linhas**em um arquivo.
Você pode fazer com que ele imprima apenas um deles usando `-c`, `-w` ou `-l`, respectivamente.

`@instructions`
Conte quantos registros em `seasonal/spring.csv` têm datas em julho de 2017 (`2017-07`). 
- Para fazer isso, use `grep` com uma data parcial para selecionar as linhas e envie esse resultado para `wc` com um sinalizador apropriado para contar as linhas.

`@hint`
- Use o site `head seasonal/spring.csv` para lembrar a você mesmo o formato da data.
- A primeira parte do comando tem o formato `grep thing_to_match filename`.
- Após o pipe, `|`, chame `wc` com o sinalizador `-l`.

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
      has_code("grep", incorrect_msg = "Você chamou `grep`?"),
      has_code("2017-07", incorrect_msg = "Você procurou por `2017-07`?"),
      has_code("seasonal/spring.csv", incorrect_msg = "Você procurou no arquivo `seasonal/spring.csv`?"),
      has_code("|", incorrect_msg = "Você usou `|` para canalizar para `wc`?"),      
      has_code("wc", incorrect_msg = "Você chamou `wc`?"),
      has_code("-l", incorrect_msg = "Você contou as linhas com `-l`?")
    )
  )
)
Ex().success_msg("Contagem cuidadosa! Determinar quanto de dados você tem é um ótimo primeiro passo em qualquer análise de dados.")

```

---

## Como posso especificar vários arquivos de uma vez?

```yaml
type: ConsoleExercise
key: 602d47e70c
xp: 100
```

A maioria dos comandos do shell funcionará em vários arquivos se você der a eles vários nomes de arquivos.
Por exemplo,
você pode obter a primeira coluna de todos os arquivos de dados sazonais de uma só vez, assim:

```{shell}
cut -d , -f 1 seasonal/winter.csv seasonal/spring.csv seasonal/summer.csv seasonal/autumn.csv
```

Mas digitar os nomes de muitos arquivos repetidamente é uma má ideia:
você perde tempo,
e, mais cedo ou mais tarde, você deixará um arquivo de fora ou repetirá o nome de um arquivo.
Para tornar sua vida melhor,
o shell permite que você use **curingas** para especificar uma lista de arquivos com uma única expressão.
O curinga mais comum é `*`,
que significa "corresponder a zero ou mais caracteres".
Usando-o,
podemos encurtar o comando `cut` acima para isso:

```{shell}
cut -d , -f 1 seasonal/*
```

ou:

```{shell}
cut -d , -f 1 seasonal/*.csv
```

`@instructions`
Escreva um único comando usando `head` para obter as três primeiras linhas de `seasonal/spring.csv` e `seasonal/summer.csv`, um total de seis linhas de dados, mas *não* dos arquivos de dados de outono ou inverno.
Use um curinga em vez de escrever os nomes dos arquivos por extenso.

`@hint`
- O comando tem o formato `head -n number_of_lines filename_pattern`.
- Você poderia combinar arquivos no diretório `a`, começando com `b`, usando `a/b*`, por exemplo.

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
    has_expr_output(incorrect_msg = "Você pode usar `seasonal/s*` para selecionar `seasonal/spring.csv` e `seasonal/summer.csv`. Certifique-se de incluir apenas as três primeiras linhas de cada arquivo com a flag `-n`!"),
    check_not(has_output('==> seasonal/autumn.csv <=='), incorrect_msg = "Não inclua a saída para `seasonal/autumn.csv`. Você pode usar `seasonal/s*` para selecionar `seasonal/spring.csv` e `seasonal/summer.csv`"),
    check_not(has_output('==> seasonal/winter.csv <=='), incorrect_msg = "Não inclua a saída para `seasonal/winter.csv`. Você pode usar `seasonal/s*` para selecionar `seasonal/spring.csv` e `seasonal/summer.csv`")
)
Ex().success_msg("Trabalho incrível com curingas! Isso se torna ainda mais importante se o seu diretório contiver centenas ou milhares de arquivos.")

```

---

## Que outros curingas posso usar?

```yaml
type: PureMultipleChoiceExercise
key: f8feeacd8c
xp: 50
```

O shell também tem outros curingas,
embora sejam menos usados:

- `?` corresponde a um único caractere, portanto, `201?.txt` corresponderá a `2017.txt` ou `2018.txt`, mas não a `2017-01.txt`.
- `[...]` corresponde a qualquer um dos caracteres dentro dos colchetes, portanto, `201[78].txt` corresponde a `2017.txt` ou `2018.txt`, mas não a `2016.txt`.
- `{...}` corresponde a qualquer um dos padrões separados por vírgulas dentro dos colchetes, de modo que `{*.txt, *.csv}` corresponde a qualquer arquivo cujo nome termine com `.txt` ou `.csv`, mas não a arquivos cujos nomes terminem com `.pdf`.

<hr/>

Qual expressão corresponderia a `singh.pdf` e `johel.txt`, mas *não* a `sandhu.pdf` ou `sandhu.txt`?

`@hint`
Faça a correspondência de cada expressão com cada nome de arquivo.

`@possible_answers`
- `[sj]*.{.pdf, .txt}`
- `{s*.pdf, j*.txt}`
- `[singh,johel]{*.pdf, *.txt}`
- [`{singh.pdf, j*.txt}`]

`@feedback`
- Não: `.pdf` e `.txt` não são nomes de arquivos.
- Não: você encontrará o endereço `sandhu.pdf`.
- Não: a expressão entre colchetes corresponde a apenas um caractere, não a palavras inteiras.
- Correto!

---

## Como posso classificar linhas de texto?

```yaml
type: ConsoleExercise
key: f06d9e310e
xp: 100
```

Como o próprio nome sugere,
`sort` coloca os dados em ordem.
Por padrão, ele faz isso em ordem alfabética crescente,
mas os sinalizadores `-n` e `-r` podem ser usados para classificar numericamente e inverter a ordem da saída,
enquanto `-b` diz a você para ignorar os espaços em branco à esquerda
e `-f` diz a ele para **dobrar**as maiúsculas e minúsculas (ou seja, não diferenciar maiúsculas de minúsculas).
Os pipelines geralmente usam o site `grep` para se livrar de registros indesejados
e, em seguida, `sort` para colocar os registros restantes em ordem.

`@instructions`
Você se lembra da combinação de `cut` e `grep` para selecionar todos os nomes de dentes da coluna 2 de `seasonal/summer.csv`?

```
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth
```

A partir dessa receita, classifique os nomes dos dentes em `seasonal/winter.csv` (não em `summer.csv`) em ordem alfabética decrescente. Para fazer isso, estenda o pipeline com uma etapa `sort`.

`@hint`
Copie e cole o comando nas instruções, altere o nome do arquivo, acrescente um pipe e, em seguida, chame `sort` com o sinalizador `-r`.

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
      has_code("cut", incorrect_msg = "Você chamou `cut`?"),
      has_code("-d", incorrect_msg = "Você especificou um delimitador de campo com `-d`?"),
      has_code("seasonal/winter.csv", incorrect_msg = "Você obteve dados do arquivo `seasonal/winter.csv`?"),
      has_code("|", incorrect_msg = "Você usou `|` para canalizar de `cut` para `grep` para `sort`?"),      
      has_code("grep", incorrect_msg = "Você chamou `grep`?"),
      has_code("-v", incorrect_msg = "Você inverteu a correspondência com `-v`?"),
      has_code("Tooth", incorrect_msg = "Você procurou por `Tooth`?"),
      has_code("sort", incorrect_msg = "Você chamou `sort`?"),
      has_code("-r", incorrect_msg = "Você inverteu a ordem de classificação com `-r`?")
    )
  )
)
Ex().success_msg("Ordenado! `sort` tem muitos usos. Por exemplo, canalizar `sort -n` para `head` mostra os maiores valores.")

```

---

## Como posso remover linhas duplicadas?

```yaml
type: ConsoleExercise
key: ed77aed337
xp: 100
```

Outro comando que é frequentemente usado com `sort` é `uniq`,
cuja função é remover linhas duplicadas.
Mais especificamente,
ele remove as linhas duplicadas *adjacentes*.
Se um arquivo contiver:

```
2017-07-03
2017-07-03
2017-08-03
2017-08-03
```

então `uniq` produzirá:

```
2017-07-03
2017-08-03
```

mas se ele contiver:

```
2017-07-03
2017-08-03
2017-07-03
2017-08-03
```

então `uniq` imprimirá todas as quatro linhas.
O motivo é que o `uniq` foi criado para trabalhar com arquivos muito grandes.
Para remover linhas não adjacentes de um arquivo,
ele teria que manter o arquivo inteiro na memória
(ou pelo menos,
todas as linhas exclusivas vistas até agora).
Removendo apenas as duplicatas adjacentes,
ele só precisa manter na memória a linha exclusiva mais recente.

`@instructions`
Escreva um pipeline para:

- Obtenha a segunda coluna de `seasonal/winter.csv`,
- remova a palavra "Tooth" da saída para que apenas os nomes dos dentes sejam exibidos,
- classificar a saída de modo que todas as ocorrências de um determinado nome de dente fiquem adjacentes; e
- exibe o nome de cada dente uma vez, juntamente com uma contagem da frequência com que ele ocorre.

O início do seu pipeline é o mesmo do exercício anterior:

```
cut -d , -f 2 seasonal/winter.csv | grep -v Tooth
```

Amplie-o com um comando `sort` e use `uniq -c` para exibir linhas exclusivas com uma contagem da frequência com que cada uma ocorre, em vez de usar `uniq` e `wc`.

`@hint`
Copie e cole o comando nas instruções, canalize para `sort` sem sinalizadores e, em seguida, canalize novamente para `uniq` com um sinalizador `-c`.

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
                     incorrect_msg="Você deve começar com este comando: `cut -d , -f 2 seasonal/winter.csv | grep -v Tooth`. Agora estenda-o!"),
            has_code('\|\s+sort', incorrect_msg="Você estendeu o comando com `| sort`?"),
            has_code('\|\s+uniq', incorrect_msg="Você estendeu o comando com `| uniq`?"),
            has_code('-c', incorrect_msg="Você incluiu contagens com `-c`?")
        )
    )
)
Ex().success_msg("Ótimo! Depois de todo esse trabalho em um pipe, seria bom se pudéssemos armazenar o resultado, não?")

```

---

## Como posso salvar a saída de um pipe?

```yaml
type: MultipleChoiceExercise
key: 4115aa25b2
xp: 50
```

O shell nos permite redirecionar a saída de uma sequência de comandos canalizados:

```{shell}
cut -d , -f 2 seasonal/*.csv | grep -v Tooth > teeth-only.txt
```

No entanto, `>` deve aparecer no final do pipeline:
se tentarmos usá-lo no meio, assim:

```{shell}
cut -d , -f 2 seasonal/*.csv > teeth-only.txt | grep -v Tooth
```

Então, toda a saída de `cut` é gravada em `teeth-only.txt`,
portanto, não há mais nada para você `grep`
e ele espera eternamente por alguma entrada.

<hr>

O que acontece se colocarmos o redirecionamento na frente de um pipeline, como em:

```{shell}
> result.txt head -n 3 seasonal/winter.csv
```

`@possible_answers`
- [A saída do comando é redirecionada para o arquivo como de costume.]
- O shell informa isso como um erro.
- O shell aguarda a entrada para sempre.

`@hint`
Experimente na concha.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
Ex().has_chosen(1, ['Correto!', 'Não; o shell pode realmente executar isso.', 'Não; o shell pode realmente executar isso.'])
```

---

## Como posso interromper um programa em execução?

```yaml
type: ConsoleExercise
key: d1694dbdcd
xp: 100
```

Os comandos e scripts que você executou até agora foram todos executados rapidamente,
mas algumas tarefas levarão minutos, horas ou até dias para serem concluídas.
Você também pode, por engano, colocar o redirecionamento no meio de um pipeline,
fazendo com que ele desligue.
Se você decidir que não quer que um programa continue em execução,
você pode digitar `Ctrl` + `C` para encerrá-lo.
Isso geralmente é escrito `^C` na documentação do Unix;
Observe que o "c" pode ser minúsculo.

`@instructions`
Execute o comando:

```{shell}
head
```

sem argumentos (para que você espere por uma entrada que nunca virá)
e, em seguida, interrompa-o digitando `Ctrl` + `C`.

`@hint`
Basta digitar head, pressionar Enter e sair do programa em execução com `Ctrl` + `C`.

`@pre_exercise_code`
```{python}

```

`@solution`
```{shell}
# Simply type head, hit Enter and exit the running program with `Ctrl` + `C`.
```

`@sct`
```{python}
Ex().has_code(r'\s*head\s*', fixed=False, incorrect_msg="Você usou `head`?")
```

---

## Concluindo

```yaml
type: BulletConsoleExercise
key: 659d3caa48
xp: 100
```

Para concluir,
você criará um pipeline para descobrir quantos registros estão no menor dos arquivos de dados sazonais.

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
Use `wc` com os parâmetros apropriados para listar o número de linhas em todos os arquivos de dados sazonais.
(Use um curinga para os nomes de arquivos em vez de digitá-los todos à mão).

`@hint`
Use `-l` para listar apenas as linhas e `*` para corresponder aos nomes de arquivos.

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
      has_code("wc", incorrect_msg = "Você chamou `wc`?"),
      has_code("-l", incorrect_msg = "Você contou o número de linhas com `-l`?"),
      has_code("seasonal/\*", incorrect_msg = "Você obteve dados de todos os arquivos `seasonal/*`?")
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
Adicione outro comando ao anterior usando um pipe para remover a linha que contém a palavra "total".

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
      has_code("wc", incorrect_msg = "Você chamou `wc`?"),
      has_code("-l", incorrect_msg = "Você contou o número de linhas com `-l`?"),
      has_code("seasonal/\*", incorrect_msg = "Você obteve dados de todos os arquivos `seasonal/*`?"),
      has_code("|", incorrect_msg = "Você usou pipe de `wc` para `grep` usando `|`?"),      
      has_code("grep", incorrect_msg = "Você chamou `grep`?"),
      has_code("-v", incorrect_msg = "Você inverteu a correspondência com `-v`?"),
      has_code("total", incorrect_msg = "Você procurou por `total`?")
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
Adicione mais dois estágios ao pipeline que usam `sort -n` e `head -n 1` para localizar o arquivo que contém o menor número de linhas.

`@hint`
- Use o sinalizador `sort`'s `-n` para classificar numericamente.
- Use o sinalizador `head`'s `-n` para limitar a manutenção de 1 linha.

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
      has_code("wc", incorrect_msg = "Você chamou `wc`?"),
      has_code("-l", incorrect_msg = "Você contou o número de linhas com `-l`?"),
      has_code("seasonal/\*", incorrect_msg = "Você obteve dados de todos os arquivos `seasonal/*`?"),
      has_code("|", incorrect_msg = "Você usou `|` para canalizar de `wc` para `grep` para `sort` para `head`?"),      
      has_code("grep", incorrect_msg = "Você chamou `grep`?"),
      has_code("-v", incorrect_msg = "Você inverteu a correspondência com `-v`?"),
      has_code("total", incorrect_msg = "Você procurou por `total`?"),
      has_code("sort", incorrect_msg = "Você chamou `sort`?"),
      has_code("-n", incorrect_msg = "Você especificou o número de linhas a manter com `-n`?"),
      has_code("1", incorrect_msg = "Você especificou 1 linha a manter com `-n 1`?")
    )
  )
)
Ex().success_msg("Ótimo! Acontece que `autumn.csv` é o arquivo com menos linhas. Corra para o capítulo 4 para aprender mais sobre processamento em lote!")

```
