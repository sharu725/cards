---
layout: post
title: Diferenciando json.loads de json.load e uma pitade de BigQuery
date: 2019-03-15 00:00:00 -0300
img: "/images/colinha.png"
comments: true
tags:
- colinha
- python
- bigquery
- sql
- json
- api

---
Se você nunca entendeu a diferença entre `json.loads()` e `json.load()` chegou a hora de entender! Na colinha de hoje vamos aprender a diferenciar esse métodos deliciosos.

## JSON

Se você chegou até aqui, provavelmente já sabe o que é o JSON. Mas caso não saiba, bora recapitular: [O JSON (JavaScript Object Notation ou Notação de Objetos JavaScript)](http://json.org/json-pt.html) é um formato leve de troca de dados. É fácil de ler e escrever, o que ajuda a ser usado por humanos além de também ser fácil de ser criado e interpretado por máquinas.

Se você já usou APIs provavelmente trocou dados com elas usando este formato. A maioria das linguagens de programação moderna oferece suporte de alguma forma a JSON. No caso do Python, existe uma estrutura que é quase a mesma coisa que um objeto JSON: o dicionário.

## Módulo JSON no Python

Além de uma estrutura de dados, Python também traz de fábrica [um módulo para manipulação de JSONs](https://docs.python.org/3.5/library/json.html). Para utilizar esse módulo basta fazer o seguinte:

    import json

Dentre os vários métodos, existem dois que são o foco dessa colinha de hoje `.loads()` e `.load()` que apesar de muito semelhantes em resultado, são diferentes em modo de funcionamento.

## Dados

Mas antes de começar, vamos pegar alguns dados. Esses dias eu tava dando uma olhada no [BigQuery](https://cloud.google.com/bigquery/), que entre muitas coisas legais que a ferramenta permite fazer, também da acesso a uma enormidade de dados públicos. Dentre eles dados do GitHub. Muitos desses dados estão disponíveis via API do próprio GitHub, mas se quiser aprender a mexer no BigQuery, eles já disponibilizam os dados lá dentro pra gente:

![](/bq-github.png)

Muito fofo né? Dá pra ver pela imagem acima que o conjunto de dados `github_repos` contém 9 tabelas. Dentre elas uma tabela chamada `languages`:

![](/bq-github-languages.png)

Essa tabela embora grande tem poucas colunas, apenas 4 para ser mais exata:

![](/bq-github-languages-schema.png)

Com o BigQuery eh possivel exportar dados para a sua conta do GoogleDrive. E foi isso que eu fiz. Ao exportar esses dados o BigQuery cria uma pasta no seu Drive:

![](/2019-03-15 19.16.34.png)

E dentro dessa pasta um arquivo com o mesmo nome de extensao `.json`.

Eu escolhi esssa tabela por trazer dados de um dominio conhecido por mim. Sabendo disso, eu escolhi a tabela de O que eu fiz foi baixar uma amostrinha desses dados com apenas 100 registros, no formato JSON. 

    SELECT * FROM `bigquery-public-data.github_repos.languages` LIMIT 100

### .load()

### .loads()