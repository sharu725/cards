---
title: "Brincando com o PostgreSQL"
layout: post
date: '2018-04-08 01:00:00'
img: tutorial.png
tags:
- tutorial
- banco de dados
- ferramenta
- docker
- docker-compose
- pgweb
- sql
- dados
- português
comments: true
---

## O que você vai encontrar nesse post

- Como rodar um banco usando docker e docker-compose
- Como rodar migrações nesse banco
- Como interagir com esse banco por uma interface

## Aviso inicial

Todo o código e estrutura que vou mostrar aqui está [nesse repositório do GitHub](https://github.com/jtemporal/tadpgweb/).

## Rodando o PostgreSQL no Docker

Pra começar você pode [instalar o PostgreSQL]() na sua máquina seguindo as instruções do site oficial porém aqui vou mostrar como usar o Postgres dentro de um container Docker usando o Docker-Compose.
Pra começar (se ainda não o fez) instale o Docker e o Docker-Compose TK na sua máquina.
Depois disso, vamos precisar de um arquivo chamado `docker-compose.yml` contendo as seguintes linhas:

~~~ yml
version: "3"
services:
  tad:
    image: postgres:9.6
    container_name: "postgres"
    environment:
      - POSTGRES_DB=tadpgweb
      - POSTGRES_USER=postgres
      - TZ=GMT
    volumes:
      - "./data/postgres:/var/lib/postgresql/data"
    ports:
      - 5432:5432
~~~

Vamos ver as configurações que esse arquivo traz:

1. **Diretório de dados**: quando esse container rodar vai existir um diretório `data/` na sua pasta que irá ser mapeada para dentro do container e conterá todos os dados do PostgreSQL;
1. **O nome do serviço**: aqui chamado de `tad`;
1. **A porta aberta**: para que a gente acesse o banco precisamos mapear uma porta do container para a nossa máquina `host`. Por padrão vamos escolher a mesma porta que o PostgreSQL usaria se não estivesse rodando dentro do container;
1. **O nome do banco**: Aqui eu chamei o banco de `tadpgweb`;
1. **Usuário do banco**: no arquivo definimos o usuário do banco como sendo o `postgres` mesmo.

E agora para colocar nosso banco “de pé”, o seguinte comando num terminal bastará:

```console
docker-compose up tad
```

Na primeira vez que rodamos esse comando, algumas coisas vão acontecer a começar pelo download da imagem do PostgreSQL na versão 9.6, a criação do diretório `data/` para conter os dados do postgres e depois disso, a criação de um banco com as configurações do `docker-compose.yml`.

## Migrações: de comer ou de passar no cabelo

Criar e apagar tabelas e colunas, entre outras alterações podem ser feitas por meio de migrações. Migrações não passam de arquivos que executam comandos no banco. Esses comandos podem ser de alteração nas estruturas do banco como criação e deleção de tabelas e colunas ou comandos de preenchimento de dados conhecidos como migrações de dados ou _data migrations_.

Aqui eu escrevi 4 migrações que vamos rodar e vou explicar uma a uma, então preparem-se para o curso curto e intensivo do básico de SQL ;P

A primeira migração, chamada de `001_create_table_up.sql` e vai criar uma tabela `clientes`, com 3 colunas, a coluna `id` que é um sequencial que começa em um, a coluna `nome` que pode ter tamanho máximo de 250 caracteres e por último a coluna `idade` que aceita números, além disso essa migração também define que as colunas `id` e `nome` não podem ser nulas e a coluna `id` é chave primária da tabela `clientes`:

```sql
CREATE TABLE clientes (id serial NOT NULL, nome VARCHAR(250) NOT NULL, idade INTEGER, PRIMARY KEY (id));
```

Agora vamos supor que a tabela que criamos não precise de uma coluna idade, então criamos a migração `002_alter_table_up.sql` para nos livrarmos dela:

```sql
ALTER TABLE clientes DROP COLUMN idade;
```

Além de não precisar da coluna nós vamos querer uma outra coluna chamada de `endereco` também com tamanho máximo de 250 caracteres, e foi o que fizemos na migração `003_alter_table_up.sql`:

```sql
ALTER TABLE clientes ADD COLUMN endereco VARCHAR(250) NOT NULL;
```

E agora que temos uma estrutura pronta para armazenar dados vamos fazer umas inserções nessa linda tabela com a migração `004_data_migration_up.sql`, essa migração cria três registros na tabela `clientes` fazendo `INSERTS`:

```sql
INSERT INTO clientes (nome, endereco) VALUES ('Umbrella Corporation', '545 S Birdneck RD STE 202B Virginia Beach, VA 23451');
INSERT INTO clientes (nome, endereco) VALUES ('OCP Omni Consumer Products', 'Delta City (formerly Detroit)');
INSERT INTO clientes (nome, endereco) VALUES ('Weyland-Yutani Corporation', 'Weyland-Yutani Corporation HQ, Tokyo');
```

Note que os nomes de todas as migrações terminam em `up`, isso por que, cada uma das migrações que você viu aqui, tem uma migração irmã que _desfaz_ o que essa migração fez. Isso é uma boa prática de software ;)

Mas agora que nós temos essas migrações como aplicá-las ao nosso banco?

Primeiro vamos copiar as migrações pro lugar certo e depois “rodá-las” dentro do container:

```console
sudo cp migrations/*up.sql data/postgres/
docker-compose exec tad psql -U postgres -d tadpgweb -1 -f /var/lib/postgresql/data/001_create_table_up.sql
docker-compose exec tad psql -U postgres -d tadpgweb -1 -f /var/lib/postgresql/data/002_alter_table_up.sql
docker-compose exec tad psql -U postgres -d tadpgweb -1 -f /var/lib/postgresql/data/003_alter_table_up.sql
docker-compose exec tad psql -U postgres -d tadpgweb -1 -f /var/lib/postgresql/data/004_data_migration_up.sql
```

A sintaxe SQL não é muito misteriosa, mas se você estiver um pouco enferrujada(o) existem vários materiais disponíveis por aí, dentre eles o material em português do [Criar Web](http://www.criarweb.com/sql/), o [curso de SQL do codecademy](https://www.codecademy.com/learn/learn-sql) e o tutorial de [SQL do GeekForGeeks](https://www.geeksforgeeks.org/sql-tutorial/) ambos em inglês.

## Um cliente para interação com o banco

Existem muitos clientes/aplicações que podemos usar para interagir com um banco como o [pgAdmin](https://www.pgadmin.org/) e a CLI [psql](https://www.postgresql.org/docs/current/static/app-psql.html). Quando rodamos nossas migrações usamos a PSQL para aplicar Além dessas, hoje vamos aprender a usar um cliente web chamado [pgWeb](http://sosedoff.github.io/pgweb/).

Escrito em [Go](http://golang.org/) o pgweb é mais leve que as demais opções e permite fazer as operações mais básicas como aquelas das nossas 4 migrações. Primeiro [instale da forma que preferir](https://github.com/sosedoff/pgweb#installation). Como eu tenho Go configurado no meu computador fui de `go get` mesmo:

```console
go get github.com/sosedoff/pgweb
```

Agora para rodar basta pegar um terminal e rodar o seguinte comando:

```console
pgweb
```

E o resultado será o seguinte:

![foto do terminal rodando o pgweb](https://i.imgur.com/hxIsA4W.png)

Agora é só ir no seu navegador favorito e acessar `http://localhost:8081/` e ver a telinha para conectar no banco:

![página de conexão com o banco do pgweb](https://i.imgur.com/CMjOnVS.png)

Então é só preencher as lacunas:

![página de conexão com o banco do pgweb com os dados preenchidos](https://i.imgur.com/Wifauax.png)

e clicar em `Connect` para ser levado para a página:

![página inicial do pgweb após conexão](https://i.imgur.com/k4DvoLB.png)

## Minhas coisas favoritas do pgweb

Apesar de não ser tão completo como um pgAdmin da vida, o pgweb traz todas as funções para um uso diário e rápido.

### Execução de queries: `Query`

Com o pgweb dá até pra criar tabelas, mas dá também para fazer consultas mais simples como um `SELECT` e ver o resultado das consultas logo abaixo:

![execução de uma query](https://i.imgur.com/8ifppem.png)

Além disso, a parte mais legal é inspecionar os detalhes de uma query clicando no botão `Explain Query`:

![detalhes sobre uma query](https://i.imgur.com/xLUxGMk.png)

### Inspeção de registros: `Rows`

Quer ver as linhas contidas em cada tabela? Clique na tabela no menu lateral esquerdo e clique na aba de `Rows` para mostrar os registros daquela tabela:

![linhas da tabela clientes](https://i.imgur.com/IASRc0X.png)

### Uso de filtros: `Rows`

Ainda na aba `Rows` conseguimos ver e executar filtros simples como selecionar todas linhas que possuam `id` maior que `1` por exemplo:

![filtro de id maior que dois](https://i.imgur.com/lkkcKD7.png)

### Inspeção da estrutura da tabela: `Structure`

Para ver uma listagem completa da estrutura de cada tabela, novamente clique na tabela escolhida no menu à esquerda da tela e clique na aba `Structure`:

![aba “structure” da tabela clientes](https://i.imgur.com/ltQrIi8.png)

### Histórico: `History`

Além disso, todas as ações que fizemos ficam registradas na aba de histórico `History`:

![histórico de consultas](https://i.imgur.com/jSFTc7m.png)

---

Legal né? Seja você iniciante em banco de dados procurando uma forma de colocar em prática os estudos ou alguém mais experiente fazendo testes pequenos e rápidos, taí uma forma alternativa de interagir com o banco ;)

## Links

- [Docker](https://www.docker.com/)
- [Docker-Compose](https://docs.docker.com/compose/)
