---
layout: post
title: Playing with Docker’s container listing
date: 2019-03-16 03:00:00 +0000
img: "/colinha.png"
comments: true
tags:
- colinha
- 'docker '
- containers

---
If you use Docker, probably the third command you learned was to list containers, but do you know that you can tailor the container listing to your needs? Well, the hint today is to show two tricks I use a lot:

1. List filtering;
2. List formatting.
3. 

Let's take a look at these two cases with care.

## Filtering the container list

I think the first "advanced" command I had to learn was filtering a set of containers. Soon after getting started with Docker it is very common to have a "dirty" environment, that is, to have many containers with exited status `exited` without being removed.

So I learned how to filter such containers by their status. Within the traditional container listing command (`docker ps`), there is a flag called `--filter` or `-f`. This flag allows you to indicate filters to be made, for example:

    docker ps -a -f status=exited

The result of this command will be a list of containers that have stopped running, but have not been removed. As I've said before, it's pretty easy to lose control and have lots of stale containers "dirtying" your environment. So listing those that aren't running can be a helpful hand i removing them more easily. I like to use a command like this:

    docker rm -v $(docker ps -a -q -f status=exited)

Passing the `-q` flag causes the `docker ps` result to only show the container IDs, putting this complete command inside `$()` results in us passing the list of IDs to the `docker rm` and thus removing all the containers stopped with only one command. And the flag `-v` there is just to have feedback of what is going on with the command, it will show the ID of each container being deleted.

## Formatando a lista de containers

Além de remover os containers que ficam sujando nosso ambiente, às vezes eu preciso de algumas informações sobre algum dos containers que estão rodando. Normalmente quando usamos o `docker ps` vemos informações como ID do container, o comando que você rodou para iniciar ele, a imagem que está sendo usada e outras coisas mais... Mas às vezes ver todas essas informações na tela pode ser uma sobrecarga de informações.

Aí que entra a mágica de formatar o resultado do `docker ps`. A flag `--format` que comanda o show dessa vez. Ela aceita um [template Go](https://golang.org/pkg/text/template/). Se você não conhece templates Go, aqui vai uma explicação super rápida e superficial: muito utilizados para criação de sites estáticos, um template é uma _string_ que é "preenchida" com informações sendo guiada por variáveis.

Por exemplo, o template:

<script src="https://gist.github.com/jtemporal/ba346fb6a05b6b5badb07a5928240d1c.js"></script>

poderia ser preenchido com qualquer nome que estivesse contido na variável `Name`. Para identificar variáveis em templates basta encontrar a palavra precedida por um _ponto_ e dentro de _chaves duplas_. Se quiser saber mais sobre [templates Go da uma olhada nesse artigo](https://gopher.pro.br/post/http-uso-de-templates/) do grupo de estudos que fala muito bem sobre o tema.

Então, pra começar cada uma das informações que aparecem na tela você pode chamar seguindo esse mapa:

* `CONTAINER ID`: ID
* `IMAGE`: Image
* `COMMAND`: Command
* `CREATED`: RunningFor
* `STATUS`: Status
* `PORTS`: Ports
* `NAMES`: Names

Como eu geralmente só quero ver o nome, a imagem e o ID dos containers que estão rodando, meu comando acaba sendo esse:

<script src="https://gist.github.com/jtemporal/6ba7e2a2ac369738bb8278ad58993161.js"></script>

Que me da um resultado assim:

        CONTAINER ID        IMAGE                          NAMES
        11b8af1aeb43        jupyter/datascience-notebook   relaxed_hypatia

***

Legal né? E aí, você já listou os seus containers hoje?