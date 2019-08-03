---
title: "Até core devs tem atrapalhos ¯\\_(ツ)_/¯"
layout: post
date: '2017-02-01 00:00:00'
image: "/images/tutorial.png"
tags:
- postgres
- python
- serenata
- serenata de amor
- banco de dados
- inglês
comments: true
---

Depois de alguns meses trabalhando em projeto open-source eu ainda me encontro problemas com algumas coisas. E tudo bem! Dessa vez vou contar o causo do PostgreSQL.

Como alguns de vocês sabem, eu trabalho na [Operação Serenata de Amor](https://serenata.ai). Uma das ferramentas que nós desenvolvemos é chamada de [Jarbas](https://jarbas.serenata.ai/dashboard/chamber_of_deputies/reimbursement/). Ele é uma aplicação web com _backend_ em Django usando PostgreSQL para banco de dados. A última vez que eu configurei e rodei o Jarbas foi o ano passado e eu estava usando um computador com Linux. Sem muitos mistérios, essa também foi a primeira vez que contribui para o projeto. Mas agora, eu estou usando uma máquina MacOS e as coisas são um pouquinho diferente.

---

Eu comecei instalando o PostgreSQL usando `brew` (recomendo veementemente). Homebrew torna o processo de instalação relativamente simples. O comando para isso foi:

```console
$ brew install postgresql
```

Você pode pensar que é apenas isso. Mamão com açúcar. Então é só ir para o repositório do Jarbas que você clonou e criar o arquivo de configuração de ambiente - `.env` - e rodar o tradicional comando do Django `python manage.py migrate`. E foi é aí que encontramos o **primeiro** erro:

```
$ python manage.py migrate
Traceback (most recent call last):
File “/Users/temporal/anaconda3/envs/jarbas/lib/python3.5/site-packages/django/db/backends/base/base.py”, line 213, in ensure_connection
self.connect()
File “/Users/temporal/anaconda3/envs/jarbas/lib/python3.5/site-packages/django/db/backends/base/base.py”, line 189, in connect
self.connection = self.get_new_connection(conn_params)
File “/Users/temporal/anaconda3/envs/jarbas/lib/python3.5/site-packages/django/db/backends/postgresql/base.py”, line 176, in get_new_connection
connection = Database.connect(**conn_params)
File “/Users/temporal/anaconda3/envs/jarbas/lib/python3.5/site-packages/psycopg2/__init__.py”, line 130, in connect
conn = _connect(dsn, connection_factory=connection_factory, **kwasync)
psycopg2.OperationalError: could not connect to server: Connection refused
Is the server running on host “localhost” (::1) and accepting
TCP/IP connections on port 5432?
could not connect to server: Connection refused
Is the server running on host “localhost” (127.0.0.1) and accepting
TCP/IP connections on port 5432?
```

Esse é o momento em que você se pergunta: _Pera, o quê? Que tipo de erro é esse?_


![travolta perdido gif](https://media.giphy.com/media/6uGhT1O4sxpi8/giphy.gif)

Depois de alguns minutos procurando, eu encontrei essa [maravilhosa conversa no Stackoverflow](https://stackoverflow.com/a/28249245). Vou simplificar aqui: Depois de instalar o PostgreSQL você muito provavelmente (como eu) terá que iniciar o serviço e uma base de dados para você utilizar. Eu não vou repassar os passos descritos no stackoverflow aqui mas chega um ponto onde você terá duas opções:

 - Carregar o agente de inicialização;
 - Inicializar de fato o agente.

Como era minha primeira vez fazendo isso, eu achei que era um caminho mais lógico inicializar um novo agente.

Depois da inicialização, eu preciso checar se está funcionando ou não. _Nunca se sabe até testar, num é mesmo?_ E para fazer isso, nós podemos conectar ao PostgreSQL:

```console
$ psql -d postgres
```

Isso me deu apenas o Postgres rodando, eu ainda preciso de uma base de dados.

O app do Jarbas espera uma base chamada `jarbas`. Então vamos dar esse nome para nossa nova base de dados fazendo o seguinte:

```console
$ createdb jarbas
```

Como você pode esperar, isso não foi o suficiente. Eu tentei rodar novamente o comando de migração e aí isso aconteceu: `django.db.utils.OperationalError: FATAL: role "Jarbas" does not exist.`

_Aah como não amar a vida de dev?!_

Então vamos criar um papel:

```
$ psql -d postgres
=> CREATE ROLE jarbas;
```

E de novo estourou um erro: `django.db.utils.OperationalError: FATAL: role "jarbas" is not permitted to log in`. O que falta agora é dar permissões para o papel , não sei como não pensei nisso antes…

```
$ psql -d postgres
=> ALTER ROLE jarbas LOGIN;
```

Agora rodando mais uma vez o comando de migração e voilà:

![all ok](https://i.imgur.com/2nhYU2P.png)

E aí chegamos no ponto onde podemos pensar: _Como diabos vou saber se tá tudo funcionando como deveria?_ Bem, Jarbas tem suites de teste que você pode tentar rodar, aí elas vão falhar:

```
$ python manage.py test                                         
Creating test database for alias 'default'...
Got an error creating the test database: permission denied to create database

Type 'yes' if you would like to try deleting the test database 'test_jarbas', or 'no' to cancel: no
Tests cancelled.
```

Vamos precisar fazer outra modificação no papel jarbas e dar para esse papel a permissão para criar novos bancos de dados:

```
$ psql -d postgres
=> ALTER ROLE jarbas CREATEDB;
```

E aí, finalmente os testes passam lindamente:

![jarbas tests passing gif](https://media.giphy.com/media/xUA7bjcqhnBpgOvHig/giphy.gif)
