# Git basics

## Criando um novo repositório

Primeiramente, crie uma pasta, entre nela e inicialize o repositório.

```
mkdir git-basics
cd git-basics
git init
```
Crie uma arquivo *markdown* para *readme*.

`touch README.md`

## Se identificando

Indique seu nome e e-mail. O parâmetro `--global` é opicional, para se identificar em todos os repositórios, ao invés de apenas no atual.

```
git config --global user.name "Ygor Canalli"
git config --global user.email "ygor.canalli@gmail.com"
```
## *Staging area*

O staging area é a área de preparação de um *commit*. Utilizando seu editor de texto favorito, adicione algum conteúdo ao arquivo `README.md`. Para um exemplo de uso da linguagem *markdown*, o fonte deste tutorial.

### Verificando o *staging area*

Você poderá verificar o que foi alterado, o que será *commitado* e o que não será. Para isso, utilize o comando

```
git status
```

### Adicionando ao *staging area*

Verificando o *status* você perceberá que o *git* detecta a criação do arquivo `README.md`, porém consta como um alteração que não será *commitada*. Para que o arquivo seja incluído no próximo *commit*, utilize o comando

```
git add README.md
```

### Removendo do *staging area*

Para remover um arquivo do *staging area*, ou seja, não incluí-lo no próximo *commit*, utilize o comando

```
git checkout -- <meu-arquivo>
```

## *Commitando* alterações

Um *commit* é uma operação irreversível de atualização do repositório. Sendo assim, não é possível desfazer um commit, embora seja possível reverter o repositório para a versão anterior e prosseguir a partir dela, porém o *commit* em questão jamais deixará de fazer parte do histórico. Por isso, existe um estado intermediário, o *stating area*. É uma boa prática que cada *commit* seja uma alteração atômica, que diga respeito a somente uma coisa, para que seje possível rastrear cada modificação que o repositório passou. Para um exemplo, verifique o exemplo deste tutorial. É sempre aconselhável verificar o *staging area* antes de realizar um *commit*. Para *commitar* as alterações indicadas utilizamos o comando

```
git commit -m 'minha mensagem'
```

Também é possível realizar um commit sem indicar uma mensagem, neste caso será aberto um editor (em geral o `vi`) onde você deverá deixar uma mensagem que indique o que foi alterado no commit em questão. Após salvar e sair do editor, o git procederá à criação do *commit*.

## Repositórios remotos

O git permite que você tenha um repositório remoto, para onde você pode enviar alterações e recuperá-las novamente no futuro. Em geral utilizamos como repositórios remotos serviços de nuvem como GitHub, GitLab e BitBucket.

### Adicionando um repositório remoto

Para adicionar um repositório remoto, precisamos dar um nome a ele, que usualmente é `origin` e a URL remota. O exemplo abaixo adiciona um repositório remoto com o endereço deste tutorial. Você poderá fazer com qualquer repositório remoto que deseje. Caso ainda não possua um, sugiro criar um novo repositório no GitHub.

```
git remote add origin https://github.com/ygorcanalli/git-basics.git

```

### Enviando alterações com o comando _push_

O comando _push_ envia seus commits para o repositório remoto. A primeira vez que utilizamos este comando, precisamos indicar para onde serão enviadas as alterações. Utilizamos o comando abaixo para enviar para o remote chamado `origin` no branch `master` (falaremos sobre branches mais adiante):

```
git push -u origin master
```

Tendo configurado o destino do _remote_, nas próximas vezes basta utilizar o comando `git push`, e seus commits serão enviados. Você pode alterar o destino novamente utilizando o comando anterior.


### Recebendo alterações com o comando _pull_

Para receber alterações do repositório remoto utilizamos o comando

```
git pull
```

É importante ressaltar que antes que você utilize o comando em questão, é necessário comitar suas alterações, pois é possível que você tenha alterado algum arquivo que será baixado, e neste caso as duas versões precisarão ser unidas com um _merge_, que veremos em breve.

### Clonando um repositório remoto

Quando você deseja trabalhar sobre um repositório remoto pré-existente, é necessário que você realize um _clone_, pois por padrão não é possível baixar atualizações de um outro repositório caso você crie um novo repositório com `git init`. O exemplo abaixo mostra como podemos realizar a operação, que irá criar um pasta com o nome do repositório e baixar todos os arquivos para ela.

```
git clone https://github.com/ygorcanalli/git-basics.git
```

## Trabalhando com branches

A palavra branch significa galho, ou ramo. Utilizamos branches para viabilizar o tabalho paralelo em diferentes modificações. A ideia é isolar do código principal as modificações que são desenvolvidas, até que estejam prontas, para então levá-las para o código principal. Este código principal, como chamamos, na verdade é um branch chamado `master`. Se não especificamos onde o commit será feito, ele vai para o branch `master`.

Para exercitarmos os branches, primeiramente crie um arquivo em branco `force.md` e comite inserindo o título 'Force'.

Criaremos dois branches, um para o light side e outro para o dark side. Desta forma, podemos dividir o trabalho em duas frentes, ambas adicionando conteúdo ao mesmo arquivo, sem que haja interferências. Ao final, cada branch será unido de volta ao `master`.

Para criar um novo branch utilize o comando

```
git branch lightside
```

Para utilizar este branch utilize

```
git checkout lightside
```

A partir de agora todas os comits irão para o branch `lightside` ao invés do `master`. Adicione o conteúdo abaixo ao arquivo `force.md` e comite.

```
# The Force

## Light Side of The Force

>A Jedi uses the Force for knowledge and defense...never for attack
>Master Yoda
```
Agora iremos fazer um push para o repositório remoto, criando um novo branch remoto simultaneamente. Repare que o branch estava apenas em seu repositório local, não no remoto.

```
git push -u origin lightside
```

Agora, vá ao site do seu repositório remoto para visualizar seu novo branch. Quando você cria um novo branch, ele começa exatamente igual ao branch atual. Então, para criar um branch independente do atual, voltaremos ao branch `master`:


```
git checkout master
```

Agora criaremos um novo branch para o lado sombrio. É possível criar um branch e passar a utilizá-lo de uma única vez, com o comando

```
git checkout -b darkside
```

Agora criamos e estamos trabalhando no branch `darkside`. Repare que nele não há o conteúdo que adicionamos no outro branch. Adicione o conteúdo abaixo e comite.

```
# The Force

## Dark Side of The Force

>Power! Unlimited power!
>Darth Sidious
```

 Agora vamos fazer um push do nosso branch `darkside` para o repositório remoto.

```
git push -u origin darkside
```

A opção `-u` do comando `git push` não apenas cria o branch remoto, mas especifica que toda vez que for feito um push no branch local, ele irá para o branch remoto definido. Assim, nas próximas vezes que estiver fazendo um push a partir do branch local, não será necessário especificar o branch remoto, pois cada branch fica pré-configurado com seu branch remoto.

## Merges e pull requests

A operação _merge_, que significa mesclar, se trata de mesclar as alterações de um branch para outro, ou seja, juntar o código que foi feito em branches diferentes. Frequentemente utilizamos esta operação para unir o código de um determinado branch ao `master`.

Em nosso tutorial, realizaremos os merges através da funcionalidade `pull requests` do GitHub, que possui equivalentes em seus concorrentes. Um pull request é uma espécie de pedido de merge, que precisa ser aprovado para que as alterações entrem em vigor. Quando se trabalha em equipe, o ideia é que se faça toda vez que alguém for unir o código de um branch com suas atualizações ao `master`, para que o outro membro da equipe ou o responsável pelo código confira se a alteração é apropriada.

Para criar um pull request, vá na aba 'Pull requests' no site do GitHub e utilize o botão 'New pull request'. Lá você deverá indicar de onde partem as alterações, e para onde eles serão levadas. Repare na direção da seta para saber de onde para onde o código será mesclado. Crie um Pull request do branch `lightside` para o `master`. Tendo feito o pull request, clique no botão `Merge` para efetivar a operação.
