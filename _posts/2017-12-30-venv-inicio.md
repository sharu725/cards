---
title: 'Ambientes virtuais Python: venv'
layout: post
date: 2017-12-30T00:00:00.000+00:00
img: colinha.png
tags:
- colinha
- python
- python 3
- python3
- venv
- versoes
- ambiente virtual
- ambientes virtuais
- virtualenv
- português
comments: true
subtitle: Aprenda a criar ambientes virtuais Python usando o módulo venv
description: Aprenda a criar ambientes virtuais Python usando o módulo venv
image: "/images/colinha.png"

---
***

Author note: You [can read this post in English](https://jtemporal.com/python-virtual-environments-venv/).

***

Depois de aprender que [Python](https://www.python.org/) possui várias versões e [aprender a instalar mais de uma delas com o pyenv](http://jtemporal.com/pyenv-inicio/), a colinha de hoje mostra um próximo passo: utilizar o [venv](https://docs.python.org/3/library/venv.html) para criar ambientes virtuais com o Python 3.

No lançamento da versão 3.3 foi anunciado o venv como novo módulo da linguagem. E na versão 3.6 o venv se torna a escolha oficial para criação de ambientes virtuais Python com o script do [pyvenv se tornando _deprecated_](https://docs.python.org/dev/whatsnew/3.6.html#id8).

História do surgimento do venv a parte, vamos aos finalmentes. Basta ter qualquer versão do Python apartir da 3.3 que os códigos abaixo devem funcionar `¯\_(ツ)_/¯`.

Para criar um ambiente virtual:

``` console
$ python -m venv meuenv
```

E para ativar o ambiente:

``` console
$ source meuenv/bin/activate
```

Agora atenção para uma coisa: essa forma de criar ambientes virtuais não permite que você crie ambientes para outras versões que não aquela que você esteja usando.

``` console
$ python --version
# Python 3.3.0
```

Aqui eu estava usando o Python 3.3.0 então o _myenv_ vai ter essa versão. Daí é que fica legal ter o _pyenv_ instalado! Você só precisa trocar a versão para a que você quer e criar o ambiente. Veja:

``` console
$ python --version
# Python 3.3.0
$ pyenv local 3.6.4
$ python --version
# Python 3.6.4
$ python -m venv meuoutroenv
$ source meuoutroenv/bin/activate
```

Uma das vantagens de ter um módulo desses embutido na linguagem é não precisar instalar nada mais para conseguir criar ambientes virtuais. Pra galera evita instalar coisas extras no computador como eu isso é fantástico.

E agora só fazer `pip install` e começar com os códigos ☺️

***

## Links

* [Pontos altos do lançamento da versão 3.3 do Python](https://docs.python.org/dev/whatsnew/3.3.html#summary-release-highlights)