---
title: Manipulação de arquivos e diretórios
description: >-
  Este capítulo é uma breve introdução ao shell Unix. Você aprenderá por que ele
  ainda é usado depois de quase 50 anos, como ele se compara às ferramentas
  gráficas com as quais você talvez esteja mais familiarizado, como se
  movimentar no shell e como criar, modificar e excluir arquivos e pastas.
free_preview: true
lessons:
  - nb_of_exercises: 12
    title: Como o shell se compara a uma interface de desktop?
---

## Como o shell se compara a uma interface de desktop?

```yaml
type: PureMultipleChoiceExercise
key: badd717ea4
xp: 50
```

Um sistema operacional como Windows, Linux ou Mac OS é um tipo especial de programa.
Ele controla o processador, o disco rígido e a conexão de rede do computador,
mas sua função mais importante é executar outros programas.

Já que os seres humanos não são digitais,
eles precisam de uma interface para interagir com o sistema operacional.
O mais comum hoje em dia é um explorador de arquivos gráfico,
que converte cliques e cliques duplos em comandos para abrir arquivos e executar programas.
Antes de os computadores terem telas gráficas,
No entanto,
as pessoas digitavam instruções em um programa chamado **shell de linha de comando**.
Cada vez que um comando é inserido,
o shell executa alguns outros programas,
imprime sua saída em formato legível para humanos,
e, em seguida, exibe um *prompt* para sinalizar que está pronto para aceitar o próximo comando.
(Seu nome vem da noção de que ele é o "invólucro externo" do computador).

Digitar comandos em vez de clicar e arrastar pode parecer desajeitado no início,
mas como você verá,
quando você começar a explicar o que deseja que o computador faça,
você pode combinar comandos antigos para criar novos comandos
e automatizar operações repetitivas
com apenas alguns toques no teclado.

<hr>
Qual é a relação entre o explorador de arquivos gráfico que a maioria das pessoas usa e o shell de linha de comando?

`@hint`
Lembre-se de que um usuário só pode interagir com um sistema operacional por meio de um programa.

`@possible_answers`
- O explorador de arquivos permite que você visualize e edite arquivos, enquanto o shell permite que você execute programas.
- O explorador de arquivos é construído sobre o shell.
- O shell faz parte do sistema operacional, enquanto o explorador de arquivos é separado.
- [Ambas são interfaces para a emissão de comandos para o sistema operacional.]

`@feedback`
- Ambos permitem que você visualize e edite arquivos e execute programas.
- Os exploradores de arquivos gráficos e o shell chamam as mesmas funções subjacentes do sistema operacional.
- O shell e o explorador de arquivos são programas que traduzem os comandos do usuário (digitados ou clicados) em chamadas para o sistema operacional.
- Correto! Ambos recebem os comandos do usuário (digitados ou clicados) e os enviam para o sistema operacional.

---

## Onde estou?

```yaml
type: MultipleChoiceExercise
key: 7c1481dbd3
xp: 50
```

O **sistema de arquivos** gerencia arquivos e diretórios (ou pastas).
Cada um é identificado por um **caminho absoluto**
que mostra como você pode acessá-lo a partir do **diretório raiz** do sistema de arquivos:
`/home/repl` é o diretório `repl` no diretório `home`,
enquanto `/home/repl/course.txt` é um arquivo `course.txt` nesse diretório,
e `/`, por si só, é o diretório raiz.

Para descobrir onde você está no sistema de arquivos,
executar o comando `pwd`
(abreviação de "**print** **working** **directory**").
Isso imprime o caminho absoluto do seu **diretório de trabalho atual**,
que é onde o shell executa comandos e procura arquivos por padrão.

<hr>
Execute `pwd`.
Onde você está agora?

`@possible_answers`
- `/home`
- `/repl`
- `/home/repl`

`@hint`
Em geral, os sistemas Unix colocam os diretórios pessoais de todos os usuários em `/home`.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err = "Esse não é o caminho correto."
correct = "Correto - você está em `/home/repl`."

Ex().has_chosen(3, [err, err, correct])
```

---

## Como posso identificar arquivos e diretórios?

```yaml
type: MultipleChoiceExercise
key: f5b0499835
xp: 50
```

`pwd` informa onde você está.
Para descobrir o que há lá,
digite `ls` (que é a abreviação de "**listing**") e pressione a tecla Enter.
Por si só,
`ls` lista o conteúdo do seu diretório atual
(aquele exibido por `pwd`).
Se você adicionar os nomes de alguns arquivos,
`ls` você os listará,
e se você adicionar os nomes dos diretórios,
ele listará seu conteúdo.
Por exemplo,
`ls /home/repl` mostra a você o que está em seu diretório inicial
(geralmente chamado de **diretório pessoal**).

<hr>
Use `ls` com um argumento apropriado para listar os arquivos no diretório `/home/repl/seasonal`
(que contém informações sobre cirurgias odontológicas por data, divididas por estação).
Qual desses arquivos *não* está nesse diretório?

`@possible_answers`
- `autumn.csv`
- `fall.csv`
- `spring.csv`
- `winter.csv`

`@hint`
Se você fornecer um caminho ao `ls`, ele mostrará o que está nesse caminho.

`@pre_exercise_code`
```{python}

```

`@sct`
```{python}
err = "Esse arquivo está no diretório `seasonal`."
correct = "Correto - esse arquivo *não* está no diretório `seasonal`."

Ex().has_chosen(2, [err, correct, err, err])
```

---

## De que outra forma posso identificar arquivos e diretórios?

```yaml
type: BulletConsoleExercise
key: a766184b59
xp: 100
```

Um caminho absoluto é como uma latitude e longitude: tem o mesmo valor, não importa onde você esteja. Um **caminho relativo**, por outro lado, especifica um local a partir de onde você está: é como dizer "20 quilômetros ao norte".

Como exemplos:
- Se você estiver no diretório `/home/repl`, o caminho **relativo** `seasonal` especifica o mesmo diretório que o caminho **absoluto** `/home/repl/seasonal`. 
- Se você estiver no diretório `/home/repl/seasonal`, o caminho **relativo** `winter.csv` especifica o mesmo arquivo que o caminho **absoluto** `/home/repl/seasonal/winter.csv`.

O shell decide se um caminho é absoluto ou relativo observando seu primeiro caractere: Se começar com `/`, é absoluto. Se *não* começar com `/`, é relativo.

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
Você está em `/home/repl`. Use `ls` com um **caminho relativo** para listar o arquivo que tem um caminho absoluto de `/home/repl/course.txt` (e somente esse arquivo).

`@hint`
Muitas vezes, você pode construir o caminho relativo para um arquivo ou diretório abaixo do local atual
subtraindo o caminho absoluto de sua localização atual
do caminho absoluto do item que você deseja.

`@solution`
```{shell}
ls course.txt

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/home/repl"),
    has_code("ls", incorrect_msg = "Você não chamou `ls` para gerar a listagem de arquivos."), # to prevent `echo "course.txt"`
    check_correct(
      has_expr_output(strict=True),
      has_code("ls +course.txt", incorrect_msg = "Seu comando não gerou a listagem de arquivos correta. Use `ls` seguido de um caminho relativo para `/home/repl/course.txt`.")
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
Você está em `/home/repl`.
Use `ls` com um caminho **relativo** 
para listar o arquivo `/home/repl/seasonal/summer.csv` (e somente esse arquivo).

`@hint`
Os caminhos relativos *não* começam com um '/' inicial.

`@solution`
```{shell}
ls seasonal/summer.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/home/repl"),
    has_code("ls", incorrect_msg = "Você não chamou `ls` para gerar a listagem de arquivos."), 
    check_correct(
      has_expr_output(strict=True),
      has_code("ls +seasonal/summer.csv", incorrect_msg = "Seu comando não gerou a listagem de arquivos correta. Use `ls` seguido de um caminho relativo para `/home/repl/seasonal/summer.csv`.")
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
Você está em `/home/repl`.
Use `ls` com um caminho **relativo** 
para listar o conteúdo do diretório `/home/repl/people`.

`@hint`
Os caminhos relativos não começam com um '/' inicial.

`@solution`
```{shell}
ls people

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/home/repl"),
    has_code("ls", incorrect_msg = "Você não chamou `ls` para gerar a listagem de arquivos."), 
    check_correct(
      has_expr_output(strict=True),
      has_code("ls +people", incorrect_msg = "Seu comando não gerou a listagem correta de arquivos. Use `ls` seguido de um caminho relativo para `/home/repl/people`.")
    )
)
Ex().success_msg("Muito bem. Agora que você sabe como listar arquivos e diretórios, vamos ver como você pode se mover pelo sistema de arquivos!")
```

---

## Como posso mudar para outro diretório?

```yaml
type: BulletConsoleExercise
key: dbdaec5610
xp: 100
```

Assim como você pode se movimentar em um navegador de arquivos clicando duas vezes nas pastas,
você pode se movimentar no sistema de arquivos usando o comando `cd`
(que significa "alterar diretório").

Se você digitar `cd seasonal` e depois `pwd`,
O shell informará que você está agora em `/home/repl/seasonal`.
Se você executar o site `ls` por conta própria,
Ele mostra a você o conteúdo de `/home/repl/seasonal`,
porque é onde você está.
Se você quiser voltar ao seu diretório inicial, `/home/repl`,
Você pode usar o comando `cd /home/repl`.

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
Você está em `/home/repl`/.
Altere o diretório para `/home/repl/seasonal` usando um caminho relativo.

`@hint`
Lembre-se de que `cd` significa "change directory" (alterar diretório) e que os caminhos relativos não começam com um '/' inicial.

`@solution`
```{shell}
cd seasonal

```

`@sct`
```{python}
Ex().check_correct(
  has_cwd('/home/repl/seasonal'),
  has_code('cd +seasonal', incorrect_msg="Se o seu diretório de trabalho atual (descubra com `pwd`) é `/home/repl`, você pode mover para a pasta `seasonal` com `cd seasonal`.")
)
```

***

```yaml
type: ConsoleExercise
key: e69c8eac15
xp: 35
```

`@instructions`
Use o site `pwd` para verificar se você está lá.

`@hint`
Lembre-se de pressionar "enter" ou "return" depois de digitar o comando.

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
Use `ls` sem nenhum caminho para ver o que há nesse diretório.

`@hint`
Lembre-se de pressionar "enter" ou "return" após o comando.

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
      has_code('ls', incorrect_msg="Seu comando não gerou a saída correta. Você usou `ls` sem caminhos para mostrar o conteúdo do diretório atual?")
    )
)

Ex().success_msg("Legal! Isso foi sobre navegar para subdiretórios. E quanto a subir? Vamos descobrir!")
```

---

## Como posso subir em um diretório?

```yaml
type: PureMultipleChoiceExercise
key: 09c717ef76
xp: 50
```

O **pai** de um diretório é o diretório acima dele.
Por exemplo, `/home` é o pai de `/home/repl`,
e `/home/repl` é o pai de `/home/repl/seasonal`.
Você sempre pode fornecer o caminho absoluto do diretório pai para comandos como `cd` e `ls`.
Com mais frequência,
No entanto,
você aproveitará o fato de que o caminho especial `..`
(dois pontos sem espaços) significa "o diretório acima daquele em que estou atualmente".
Se você estiver em `/home/repl/seasonal`,
Em seguida, `cd ..` leva você para `/home/repl`.
Se você usar o `cd ..` mais uma vez,
Isso coloca você em `/home`.
Mais um `cd ..` coloca você no *diretório raiz* `/`,
que é o topo do sistema de arquivos.
(Lembre-se de colocar um espaço entre `cd` e `..` \- é um comando e um caminho, não um único comando de quatro letras).

Um único ponto sozinho, `.`, sempre significa "o diretório atual",
Portanto, o `ls` sozinho e o `ls .` fazem a mesma coisa,
Enquanto `cd .` não tem efeito
(porque ele o move para o diretório em que você está atualmente).

Um último caminho especial é `~` (o caractere til),
que significa "seu diretório pessoal",
como `/home/repl`.
Não importa onde você esteja,
`ls ~` sempre listará o conteúdo do seu diretório pessoal,
e o `cd ~` sempre levará você para casa.

<hr>
Se você estiver em `/home/repl/seasonal`,
Para onde o `cd ~/.../.` leva você?

`@hint`
Rastreie o caminho um diretório de cada vez.

`@possible_answers`
- `/home/repl`
- [`/home`]
- `/home/repl/seasonal`
- `/` (o diretório raiz)

`@feedback`
- Não, mas o `~` ou o `..` por si só levariam você até lá.
- Correto! O caminho significa "diretório inicial", "um nível acima", "aqui".
- Não, mas o site `.` por si só faria isso.
- Não, a parte final do caminho é `.` (que significa "aqui") em vez de `..` (que significa "para cima").

---

## Como posso copiar arquivos?

```yaml
type: BulletConsoleExercise
key: 832de9e74c
xp: 100
```

Você frequentemente desejará copiar arquivos,
mova-os para outros diretórios para organizá-los,
ou renomeá-los.
Um comando para fazer isso é `cp`, que é a abreviação de "copy" (copiar).
Se `original.txt` for um arquivo existente,
então:

```{shell}
cp original.txt duplicate.txt
```

cria uma cópia de `original.txt` chamada `duplicate.txt`.
Se você já tiver um arquivo chamado `duplicate.txt`,
ele é sobrescrito.
Se o último parâmetro para `cp` for um diretório existente, você poderá usar o mesmo,
então um comando como:

```{shell}
cp seasonal/autumn.csv seasonal/winter.csv backup
```

copia *todos os* arquivos para esse diretório.

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
Faça uma cópia de `seasonal/summer.csv` no diretório `backup` (que também está em `/home/repl`),
chamando o novo arquivo `summer.bck`.

`@hint`
Combine o nome do diretório de destino e o nome do arquivo copiado
para criar um caminho relativo para o novo arquivo.

`@solution`
```{shell}
cp seasonal/summer.csv backup/summer.bck

```

`@sct`
```{python}
Ex().check_correct(
    check_file('/home/repl/backup/summer.bck', missing_msg="`summer.bck` não parece existir no diretório `backup`. Forneça dois caminhos para `cp`: o arquivo existente (`seasonal/summer.csv`) e o arquivo de destino (`backup/summer.bck`)."),
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
Copie `spring.csv` e `summer.csv` do diretório `seasonal` para o diretório `backup` 
*sem* alterar seu diretório de trabalho atual (`/home/repl`).

`@hint`
Use `cp` com os nomes dos arquivos que você deseja copiar
e *, em seguida,* o nome do diretório para o qual você deseja copiá-los.

`@solution`
```{shell}
cp seasonal/spring.csv seasonal/summer.csv backup

```

`@sct`
```{python}
patt = "`%s` não parece ter sido copiado para o diretório `backup`. Forneça dois nomes de arquivos e um nome de diretório para `cp`."
Ex().multi(
    has_cwd('/home/repl', incorrect_msg="Certifique-se de copiar os arquivos enquanto estiver em `{{dir}}`! Use `cd {{dir}}` para navegar de volta para lá."),
    check_file('/home/repl/backup/spring.csv', missing_msg=patt%'spring.csv'),
    check_file('/home/repl/backup/summer.csv', missing_msg=patt%'summer.csv')
)
Ex().success_msg("Bom trabalho. Além de copiar, também devemos ser capazes de mover arquivos de um diretório para outro. Aprenda sobre isso no próximo exercício!")
```

---

## Como posso mover um arquivo?

```yaml
type: ConsoleExercise
key: 663a083a3c
xp: 100
```

Enquanto o `cp` copia um arquivo,
`mv` move-o de um diretório para outro,
como se você o tivesse arrastado em um navegador de arquivos gráfico.
Ele trata seus parâmetros da mesma forma que `cp`,
portanto, o comando:

```{shell}
mv autumn.csv winter.csv ..
```

Move os arquivos `autumn.csv` e `winter.csv` do diretório de trabalho atual.
subir um nível até seu diretório pai
(porque `..` sempre se refere ao diretório acima da sua localização atual).

`@instructions`
Você está em `/home/repl`, que tem subdiretórios `seasonal` e `backup`.
Usando um único comando, mova `spring.csv` e `summer.csv` de `seasonal` para `backup`.

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
backup_patt="O arquivo `%s` não está no diretório `backup`. Você usou `mv` corretamente? Use dois nomes de arquivos e um diretório como parâmetros para `mv`."
seasonal_patt="O arquivo `%s` ainda está no diretório `seasonal`. Certifique-se de mover os arquivos com `mv` em vez de copiá-los com `cp`!"
Ex().multi(
    check_file('/home/repl/backup/spring.csv', missing_msg=backup_patt%'spring.csv'),
    check_file('/home/repl/backup/summer.csv', missing_msg=backup_patt%'summer.csv'),
    check_not(check_file('/home/repl/seasonal/spring.csv'), incorrect_msg=seasonal_patt%'spring.csv'),
    check_not(check_file('/home/repl/seasonal/summer.csv'), incorrect_msg=seasonal_patt%'summer.csv')
)
Ex().success_msg("Muito bem, vamos continuar com este trem de shell!")
```

---

## Como posso renomear arquivos?

```yaml
type: BulletConsoleExercise
key: 001801a652
xp: 100
```

`mv` também pode ser usado para renomear arquivos. Se você correr:

```{shell}
mv course.txt old-course.txt
```

então o arquivo `course.txt` no diretório de trabalho atual é "movido" para o arquivo `old-course.txt`.
Isso é diferente da forma como os navegadores de arquivos funcionam,
mas é sempre útil.

Um aviso:
Assim como `cp`,
`mv` sobrescreverá os arquivos existentes.
Se,
Por exemplo,
Você já tem um arquivo chamado `old-course.txt`,
Então, o comando mostrado acima o substituirá pelo que estiver em `course.txt`.

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
Vá para o diretório `seasonal`.

`@hint`
Lembre-se de que `cd` significa "change directory" (alterar diretório) e que os caminhos relativos não começam com um '/' inicial.

`@solution`
```{shell}
cd seasonal

```

`@sct`
```{python}
Ex().check_correct(
  has_cwd('/home/repl/seasonal'),
  has_code('cd +seasonal', incorrect_msg="Se o seu diretório de trabalho atual (descubra com `pwd`) é `/home/repl`, você pode mover para a pasta `seasonal` com `cd seasonal`.")
)
```

***

```yaml
type: ConsoleExercise
key: ed5fe1df23
xp: 35
```

`@instructions`
Renomeie o arquivo `winter.csv` para `winter.csv.bck`.

`@hint`
Use `mv` com o nome atual do arquivo e o nome que você deseja que ele tenha, nessa ordem.

`@solution`
```{shell}
mv winter.csv winter.csv.bck

```

`@sct`
```{python}
hint = " Use `mv` com dois argumentos: o arquivo que você quer renomear (`winter.csv`) e o novo nome para o arquivo (`winter.csv.bck`)."
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    multi(
        check_file('/home/repl/seasonal/winter.csv.bck', missing_msg="Esperávamos encontrar `winter.csv.bck` no diretório." + hint),
        check_not(check_file('/home/repl/seasonal/winter.csv'), incorrect_msg="Não esperávamos mais que `winter.csv` estivesse no diretório." + hint)
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
Execute `ls` para verificar se tudo funcionou.

`@hint`
Lembre-se de pressionar "enter" ou "return" para executar o comando.

`@solution`
```{shell}
ls

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    has_expr_output(incorrect_msg="Você usou `ls` para listar o conteúdo do seu diretório de trabalho atual?")
)
Ex().multi(
    has_cwd("/home/repl/seasonal"),
    check_correct(
      has_expr_output(strict=True),
      has_code("ls", incorrect_msg = "Seu comando não gerou a listagem correta de arquivos. Use `ls` sem argumentos para listar o conteúdo do seu diretório de trabalho atual.")
    )
)
Ex().success_msg("Copiar, mover, renomear, você entendeu tudo! A seguir: deletar arquivos.")

```

---

## Como posso excluir arquivos?

```yaml
type: BulletConsoleExercise
key: '2734680614'
xp: 100
```

Podemos copiar arquivos e movê-los;
para excluí-los,
Usamos o site `rm`,
que significa "remover".
Assim como em `cp` e `mv`,
você pode fornecer a `rm` os nomes de quantos arquivos quiser, portanto:

```{shell}
rm thesis.txt backup/thesis-2017-08.txt
```

remove tanto `thesis.txt` quanto `backup/thesis-2017-08.txt`

`rm` faz exatamente o que seu nome diz,
e ele faz isso imediatamente:
ao contrário dos navegadores de arquivos gráficos,
a concha não tem uma lata de lixo,
portanto, quando você digitar o comando acima,
sua tese desapareceu para sempre.

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
Você está em `/home/repl`.
Vá para o diretório `seasonal`.

`@hint`
Lembre-se de que `cd` significa "change directory" (alterar diretório) e que um caminho relativo não começa com um '/' inicial.

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
Remova `autumn.csv`.

`@hint`
Lembre-se de que `rm` significa "remover".

`@solution`
```{shell}
rm autumn.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl/seasonal'),
    check_not(check_file('/home/repl/seasonal/autumn.csv'), incorrect_msg="Não esperávamos que `autumn.csv` ainda estivesse no diretório `seasonal`. Use `rm` com o caminho para o arquivo que você deseja remover."),
    has_code('rm', incorrect_msg = 'Use `rm` para remover o arquivo, em vez de movê-lo.')
)
```

***

```yaml
type: ConsoleExercise
key: 09f2d105cd
xp: 25
```

`@instructions`
Volte para seu diretório pessoal.

`@hint`
Se você usar o site `cd` sem nenhum caminho, ele o levará para casa.

`@solution`
```{shell}
cd

```

`@sct`
```{python}
Ex().has_cwd('/home/repl', incorrect_msg="Use `cd ..` ou `cd ~` para retornar ao diretório home.")
```

***

```yaml
type: ConsoleExercise
key: 9eaf49744c
xp: 25
```

`@instructions`
Remova o endereço `seasonal/summer.csv` sem alterar os diretórios novamente.

`@hint`
Lembre-se de que `rm` significa "remover".

`@solution`
```{shell}
rm seasonal/summer.csv

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_not(check_file('/home/repl/seasonal/summer.csv'), incorrect_msg="Não esperávamos que `summer.csv` ainda estivesse no diretório `seasonal`. Use `rm` com o caminho para o arquivo que você deseja remover."),
    has_code('rm', incorrect_msg = 'Use `rm` para remover o arquivo, em vez de movê-lo.')
)
Ex().success_msg("Impressionante! Vamos para o próximo!")
```

---

## Como posso criar e excluir diretórios?

```yaml
type: BulletConsoleExercise
key: 63e8fbd0c2
xp: 100
```

`mv` trata os diretórios da mesma forma que trata os arquivos:
Se você estiver em seu diretório pessoal e executar `mv seasonal by-season`,
Por exemplo,
`mv` altera o nome do diretório `seasonal` para `by-season`.
No entanto,
`rm` funciona de forma diferente.

Se você tentar acessar `rm` um diretório,
o shell imprime uma mensagem de erro informando que você não pode fazer isso,
principalmente para impedir que você exclua acidentalmente um diretório inteiro cheio de trabalho.
Em vez disso,
Você pode usar um comando separado chamado `rmdir`.
Para maior segurança,
ele só funciona quando o diretório está vazio,
portanto, você deve excluir os arquivos em um diretório *antes de* excluir o diretório.
(Usuários experientes podem usar a opção `-r` para `rm` para obter o mesmo efeito;
discutiremos as opções de comando no próximo capítulo).

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
Sem alterar os diretórios,
excluir o arquivo `agarwal.txt` no diretório `people`.

`@hint`
Lembre-se de que `rm` significa "remover" e que um caminho relativo não começa com um '/' inicial.

`@solution`
```{shell}
rm people/agarwal.txt

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_not(check_file('/home/repl/people/agarwal.txt'), incorrect_msg="`agarwal.txt` não deve mais estar em `/home/repl/people`. Você usou `rm` corretamente?"),
    has_expr_output(expr = 'ls people', output = '', incorrect_msg = 'Ainda há arquivos no diretório `people`. Se você simplesmente moveu `agarwal.txt`, ou criou novos arquivos, delete todos eles.')
)
```

***

```yaml
type: ConsoleExercise
key: 661633e531
xp: 25
```

`@instructions`
Agora que o diretório `people` está vazio, você pode usar o link para acessar o diretório,
usar um único comando para excluí-lo.

`@hint`
Lembre-se de que o `rm` só funciona em arquivos.

`@solution`
```{shell}
rmdir people

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_not(has_dir('/home/repl/people'),
              incorrect_msg = "O diretório 'people' não deve mais estar no seu diretório home. Use `rmdir` para removê-lo!")
)
```

***

```yaml
type: ConsoleExercise
key: 89f7ffc1da
xp: 25
```

`@instructions`
Como um diretório não é um arquivo,
você deve usar o comando `mkdir directory_name`
para criar um novo diretório (vazio).
Use esse comando para criar um novo diretório chamado `yearly` abaixo do seu diretório pessoal.

`@hint`
Execute `mkdir` com o nome do diretório que você deseja criar.

`@solution`
```{shell}
mkdir yearly

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_dir('/home/repl/yearly', msg="Não há um diretório `yearly` no seu diretório home. Use `mkdir yearly` para criar um!")
)
```

***

```yaml
type: ConsoleExercise
key: 013a5ff2dc
xp: 25
```

`@instructions`
Agora que `yearly` existe,
Crie outro diretório chamado `2017` dentro dele
*sem* sair do seu diretório pessoal.

`@hint`
Use um caminho relativo para o subdiretório que você deseja criar.

`@solution`
```{shell}
mkdir yearly/2017

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_dir('/home/repl/yearly/2017',
            msg="Não foi possível encontrar um diretório '2017' em '/home/repl/yearly'. Você pode criar este diretório usando o caminho relativo `yearly/2017`.")
)
Ex().success_msg("Legal! Vamos encerrar este capítulo com um exercício que repete alguns de seus conceitos!")
```

---

## Concluindo

```yaml
type: BulletConsoleExercise
key: b1990e9a42
xp: 100
```

Você geralmente cria arquivos intermediários ao analisar dados.
Em vez de armazená-los em seu diretório pessoal,
Você pode colocá-los em `/tmp`,
que é onde as pessoas e os programas costumam manter arquivos dos quais só precisam por pouco tempo.
(Observe que `/tmp` está imediatamente abaixo do diretório raiz `/`,
*não* abaixo de seu diretório pessoal).
Este exercício de encerramento mostrará a você como fazer isso.

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
Use `cd` para acessar `/tmp`.

`@hint`
Lembre-se de que `cd` significa "change directory" (mudar de diretório) e que um caminho absoluto começa com '/'.

`@solution`
```{shell}
cd /tmp

```

`@sct`
```{python}
Ex().check_correct(
  has_cwd('/tmp'),
  has_code('cd +/tmp', incorrect_msg = 'Você está no diretório errado. Use `cd` para mudar o diretório para `/tmp`.')
)
```

***

```yaml
type: ConsoleExercise
key: 7e6ada440d
xp: 25
```

`@instructions`
Liste o conteúdo de `/tmp` *sem* digitar o nome do diretório.

`@hint`
Se você não informar ao `ls` o que listar, ele mostrará o que está no diretório atual.

`@solution`
```{shell}
ls

```

`@sct`
```{python}
Ex().multi(
    has_cwd("/tmp"),
    has_code("ls", incorrect_msg = "Você não chamou `ls` para gerar a listagem de arquivos."),
    check_correct(
      has_expr_output(strict=True),
      has_code("^\s*ls\s*$", incorrect_msg = "Seu comando não gerou a listagem de arquivos correta. Use `ls` sem nada.")
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
Crie um novo diretório dentro de `/tmp` chamado `scratch`.

`@hint`
Use `mkdir` para criar diretórios.

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
      has_code('mkdir +scratch', incorrect_msg="Não foi possível encontrar um diretório 'scratch' em '/tmp'. Certifique-se de usar `mkdir` corretamente.")
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
Mova `/home/repl/people/agarwal.txt` para `/tmp/scratch`.
Sugerimos que você use o atalho `~` para o seu diretório pessoal e um caminho relativo para o segundo, em vez do caminho absoluto.

`@hint`


`@solution`
```{shell}
mv ~/people/agarwal.txt scratch

```

`@sct`
```{python}
Ex().multi(
    has_cwd('/tmp'),
    check_file('/tmp/scratch/agarwal.txt', missing_msg="Não é possível encontrar 'agarwal.txt' em '/tmp/scratch'. Use `mv` com `~/people/agarwal.txt` como o primeiro parâmetro e `scratch` como o segundo.")
)
Ex().success_msg("Isso conclui o Capítulo 1 da Introdução ao Shell! Corra para o próximo capítulo para aprender mais sobre manipulação de dados!")
```
