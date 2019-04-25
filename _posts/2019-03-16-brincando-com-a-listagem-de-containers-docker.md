---
layout: post
title: Brincando com a listagem de containers Docker
date: 2019-03-16 03:00:00 +0000
img: "/colinha.png"
comments: true
tags:
- colinha
- 'docker '
- containers
subtitle: Aprenda a filtrar e formatar a listagem de containers Docker

---
Se você usa Docker, provavelmente o terceiro comando que aprendeu foi listar eles, mas você sabe que dá pra tunar a listagem de containers?

Pois bem, a colinha de hoje é para mostrar dois truques que eu uso muito:

1. filtragem da lista;
2. formatação da lista.

Vamos ver esses dois casos com carinho.

## Filtrando a lista de containers

Acho que o primeiro comando "mais avançado" que eu tive que fazer foi filtrar um conjunto de containers. Logo ao começar a mexer com Docker é muito comum ficar com o ambiente "sujo", ou seja, ter muitos containers com _status_ de `exited` parados sem serem removidos.

Por isso, eu aprendi a filtrar tais containers pelo _status_. Dentro do comando tradicional de listar containers (`docker ps`), existe uma _flag_ chamada de `--filter` ou `-f`. Essa _flag_ permite que você indique filtros a serem feitos, por exemplo:

    docker ps -a -f status=exited

O resultado desse comando vai ser uma lista de containers que deixaram de executar, mas não foram removidos. Como eu falei antes, é bem fácil perder o controle e ter muitos containers parados "sujando" o seu ambiente. Então listar aqueles que não estão em execução pode ser uma mão na roda para remover eles mais facilmente. Eu gosto usar esse comando assim:

    docker rm -v $(docker ps -a -q -f status=exited)

Passando a _flag_ `-q` faz com que, o resultado do `docker ps` mostre apenas os IDs dos containers, colocando esse comando completo dentro do `$()` faz com que passemos a lista de IDs para o `docker rm` e assim removendo todos os containers parados com apenas um comando. Ah a _flag_ `-v` ali é só pra ter um _feedback_ do que está rolando com o comando, ele vai mostrar o ID de cada container que estiver sendo excluído.

## Formatando a lista de containers

Além de remover os containers que ficam sujando nosso ambiente, às vezes eu preciso de algumas informações sobre algum dos containers que estão rodando. Normalmente quando usamos o `docker ps` vemos informações como ID do container, o comando que você rodou para iniciar ele, a imagem que está sendo usada e outras coisas mais... Mas às vezes ver todas essas informações na tela pode ser uma sobrecarga de informações.

Aí que entra a mágica de formatar o resultado do `docker ps`. A flag `--format` que comanda o show dessa vez. Ela aceita um [template Go](https://golang.org/pkg/text/template/). Se você não conhece templates Go, aqui vai uma explicação super rápida e superficial: muito utilizados para criação de sites estáticos, um template é uma _string_ que é "preenchida" com informações sendo guiada por variáveis.

Por exemplo, o template:

<script src="https://gist.github.com/jtemporal/ba346fb6a05b6b5badb07a5928240d1c.js"></script>

poderia ser preenchido com qualquer nome que estivesse contido na variável `Name`. Para identificar variáveis em templates basta encontrar a palavra precedida por um _ponto_ e dentro de _chaves duplas_. Se quiser saber mais sobre [templates Go da uma olhada nesse artigo](https://gopher.pro.br/post/http-uso-de-templates/) do grupo de estudos que fala muito bem sobre o tema.

Então, pra começar cada uma das informações que aparecem na tela você pode chamar seguindo esse mapa:

|    Information   |  Variable  |
|------------------|------------|
|  `CONTAINER ID`  |      ID    |
|     `IMAGE`      |    Image   |
|    `COMMAND`     |   Command  |
|    `CREATED`     | RunningFor |
|    `Status`      |   Status   |
|     `PORTS`      |   Ports    |
|     `NAMES`      |   Names    |

Como eu geralmente só quero ver o nome, a imagem e o ID dos containers que estão rodando, meu comando acaba sendo esse:

<script src="https://gist.github.com/jtemporal/6ba7e2a2ac369738bb8278ad58993161.js"></script>

Que me da um resultado assim:

        CONTAINER ID        IMAGE                          NAMES
        11b8af1aeb43        jupyter/datascience-notebook   relaxed_hypatia

***

Legal né? E aí, você já listou os seus containers hoje?