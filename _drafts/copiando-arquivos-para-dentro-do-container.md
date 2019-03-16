---
layout: post
title: Copiando arquivos para dentro do container
date: 2019-03-15 03:00:00 +0000
img: "/images/colinha.png"
comments: true
tags:
- colinha

---
Às vezes volumes não funcionam e a gente precisa copiar coisas para dentro do container. É sério! Você deve estar se perguntando, "_como uma tecnologia que todo mundo usa, não funciona?!"_

Tá, tá... Eu sei que isso tá parecendo aquelas histórias de "funciona na minha máquina" invertido. Mas vou explicar, no começo do ano eu estava trabalhando com um computador provisório. Infelizmente, eu não tinha poderes de administrador desse computador, o que me impedia de fazer certas coisas, inclusive de dar permissão ao Docker para compartilhar volumes com o sistema de arquivos do Windows.

Ah sim, talvez seja importante mencionar que eu só descobri como copiar arquivos para dentro de container que tá rodando, pois o Windows tem todas essas paradas de permissão.

Então bora lá!

## Materiais

Para essa colinha você vai precisar de:

* Um container Docker rodando
* Um arquivo que você quer copiar para dentro do Docker
* Conhecimentos básicos de Docker

### Descobrindo o container

Dependendo de como você inicie o seu container você não vai saber o nome dele, então vamos primeiro conferir isso. Como trabalho com ciência e análise de dados, é muito comum me encontrar com um container do Jupyter rodando, é esse que vou usar aqui. Para pegar o nome do container precisamos listar os containers que estão de pé:

    docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Name}}"

Aqui no meu caso, temos um resultado assim:

    CONTAINER ID        IMAGE                          NAMES
    11b8af1aeb43        jupyter/datascience-notebook   relaxed_hypatia

Eu expliquei como montar esse comando que só mostra o ID do container, a imagem sendo usada e o nome do container [nessa colinha aqui](https://jtemporal.com/brincando-com-a-listagem-de-containers-docker/). Com isso sabemos que o meu container de interesse se chama `relaxed_hypatia`.

docker cp dados.csv containernome:caminho/para/o/arquivo/dados.csv

[https://stackoverflow.com/a/31971697](https://stackoverflow.com/a/31971697 "https://stackoverflow.com/a/31971697")