---
title: "Comandos Docker"
layout: post
date: '2018-01-14 08:00:00'
image: "/images/colinha.png"
tags:
- colinha
- cheatsheet
- docker
- português
- container
- containers
- conteiner
- conteiners
comments: true
---

Se você lida com Docker diariamente é necessário saber alguns comandos. Se você for feito eu que esquece essas coisas, aqui vai uma colinha pra ajudar ;)

## Sumário
<!--toc-->
1. [Listando](#listando)
1. [Removendo](#removendo)
1. [Matando](#matando)
1. [Alguns opções válidas de conhecer](#opcoes)
<!--end toc-->

---

<h2 id="listando">Listando</h2>

### Imagens na sua máquina
Lista todas as imagens da sua máquina inclusive as penduradas (dangling)

``` console
$ docker images -a
```

### Containers em execução

``` console
$ docker ps
```

### Todos containers
E suas informações

``` console
$ docker ps -a
```

### Apenas os IDs de todos os containers

``` console
$ docker ps -qa
```

---

<h2 id="removendo">Removendo</h2>

### Um container que foi parado

``` console
$ docker rm ID
```

### Todos  containers que foram parados

``` console
$ docker rm $(docker ps -qa)
```

### Uma imagem do sistema
Se a imagem nao estiver em uso, você pode removê-la usando o comando abaixo e a ID da imagem

``` console
$ docker rmi ID
```

### Todas imagens do sistema
Remove **TODAS** as imagens Docker que não estiverem em uso da sua máquina. Use com cuidado!

Irá mostrar um erro caso alguma imagem esteja em uso.

``` console
$ docker rmi $(docker images -qa)
```

### Todas imagens do sistema
Remove **TODAS** as imagens Docker que não estiverem em uso da sua máquina. Use com cuidado!

Irá ignorar imagens em uso uso.

``` console
$ docker system prune --all
```
![resultado da remoção](https://i.imgur.com/BCPzKXW.png)

---

<h2 id="matando">Matando</h2>

### Um ou mais containers
Basta passar uma lista de IDs para matar mais do que um container

``` console
$ docker kill ID
```

---

<h2 id="opcoes">Alguns opções válidas de conhecer</h2>

### -a
Maioria, se não todos, os comandos e subcomandos do Docker possuem uma opção `-a` que vem de `all` todos.

### --rm
Mesma coisa coisa acontece com a opção `--rm`. Quando usada ela indica que o container deverá ser removido após sua execução

### -q
Maioria, se não todos, os comandos e subcomandos do Docker possuem uma opção `-q` que lista apenas os IDs
