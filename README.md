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
git reset <meu-arquivo>
```

