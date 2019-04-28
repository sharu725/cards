---
layout: post
title: Transformando um site feito em Jekyll em uma página de um site feito em Pelican
date: 2019-04-28 00:00:00 -0300
img: "/tutorial.png"
tags:
- tutorial
- jekyll
- pelican
- python
- ruby
- gerador de site estático
- static site generator
- github pages
- pyladies
- pyladies br conf
comments: true
subtitle: Comandos para colocar uma página dentro de um site Pelican

---
No post _“Movendo um site construído com Jekyll para dentro de um site construído com Pelican”_ eu contei a jornada que foi mover o site do PyLadies BR Conf para dentro do site do PyLadies Brasil. Aqui vai o passo a passo final com comandos que usei tudo isso.

***

**Nota da autora:** Aqui ensino o passo a passo para novas edições do PyLadies BR Conf, mas os comandos podem ser ajustados para fazer a mesma coisa com outros sites.

***

## Ingredientes

1. Um repositório do site conferência clonado e configurado
2. Um repositório do site oficial PyLadies Brasil clonado e configurado

## Preparativos

No repositório da conferência, ajuste o arquivo `_config.yml`:

* a variável `base_url` deve conter um valor seguindo o padrão `/conf-X` onde `X` indica a edição da conferência
* a variável `url` deve conter o domínio `https://brasil.pyladies.com`

Você pode encontrar um `_config.yml` [ajustado para a primeira edição da conferência aqui](https://github.com/pyladies-brazil/conf/blob/1eeb8e7ed0decbec5644677b7099fce9228b950b/_config.yml#L4-L5).

No repositório do site do PyLadies Brasil, ajuste o arquivo `pelicanconf.py`:

* acrescente o nome da pasta nova na variável `STATIC_PATHS` seguindo o padrão `/conf-X` onde `X` indica a edição da conferência
* acrescente a cofiguração de metadata da pasta nova na variável `EXTRA_PATH_METADATA` seguindo o padrão `'conf-X': {'path': 'conf-X'` onde `X` indica a edição da conferência
* Crie uma pasta vazia dentro da pasta `content/` seguindo o padrão `/conf-X` onde `X` indica a edição da conferência

Você pode encontrar um `pelicanconf.py` [ajustado para a primeira edição da conferência aqui](https://github.com/pyladies-brazil/br-pyladies-pelican/blob/3feaa42eb7096b3653d490494b3ef33f22887145/pelicanconf.py#L58-L72).

## Misturando ingredientes

A partir de agora todos os comandos vão ser referentes a pasta `conf-1/` então, lembre-se de ajustar os comandos para a edição da conferência que estiver criando. No repositório da conferência, rode o comando para fazer _build_ do site:

    $ jekyll build

Isso deve gerar a pasta `_site/`. Agora copie o conteúdo da pasta `_site/` para dentro da pasta da conferência que criamos:

    $ cp -r conf/_site/* br-pyladies-pelican/content/conf-1/

## Levando ao forno

Se tudo funcionou corretamente até aqui faça o _build_ do site do PyLadies Brasil:

    $ html serve

e confira se consegue acessar o site da conferência em: [https://localhost:8000/conf-1](https://localhost:8000/conf-1)

## Servindo

Faça os _commits_ das alterações e abra o _pull request_ 😉

***

Você pode ver o [_pull request_ que eu fiz para a primeira edição aqui](https://github.com/pyladies-brazil/br-pyladies-pelican/pull/237).

Xêro.