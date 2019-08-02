---
title: 'Python e suas versões: pyenv parte 2'
layout: post
date: 2018-01-02 10:00:00
img: colinha.png
tags:
- colinha
- python
- pyenv
- versoes
- versões
- português
comments: true

---
A colinha de hoje é inspirada nesse tweet aqui:

<center>
<blockquote class="twitter-tweet" data-lang="pt"><p lang="pt" dir="ltr">como ele funciona a respeito dos pacotes instalados via pip? tem um pip pra cada versão? os pacotes ficam disponíveis pra múltiplas versões?</p>— luciano ratamero (@lucianoratamero) <a href="https://twitter.com/lucianoratamero/status/947291620109639680?ref_src=twsrc%5Etfw">31 de dezembro de 2017</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
</center>

Então se você já instalou o [pyenv](https://github.com/pyenv/pyenv) e agora quer saber um pouqinho mais sobre como ele funciona assim como o Luciano, aqui vai...

## O pip

Pra quem não sabe o que é o [pip](https://pip.pypa.io/en/stable/) ele é a ferramenta recomendada para fazer a instalação de pacotes Python.

## Onde vão as versões do pyenv

Dentro do diretório/pasta do pyenv temos um diretório chamado `versions` e é lá que moram as versões que você instala usando o comando `pyenv install`, na minha máquina eu tenho três versões do Python instaladas:

``` console
versions
├── 2.7.12
├── 3.3.0
└── 3.6.4
```

## O pyenv e o pip

Cada nova versão do Python que você instala usando o pyenv vem com o seu próprio pip. Veja:

<center>
<img src="https://i.imgur.com/HoWFDf8.png"/>
<br>
<i>Listagem de pacotes instalados em cada versão</i>
</center>
Agora, se você setar uma versão do Python usando pyenv e instalar um pacote usando o pip o que acontece? Esse pacote fica disponível para as demais versões?

Resposta rápida: Não, um pacote instalado numa versão não fica disponível noutra versão.

Vejamos, vamos usar de exemplo o pacote do [Caipyra](https://github.com/jtemporal/caipyra). Antes de instalá-lo temos:

<center>
<img src="https://i.imgur.com/VxQK3Hn.png"/>
<br>
<i>Listagem de pacotes instalados em cada versão</i>
</center>

Instalando o caipyra na versão `3.3.0`:

<center>
<img src="https://i.imgur.com/YV5bJD6.png"/>
<br>
<i>Instalação do pacote Caipyra na versão 3.3.0</i>
</center>

Listando novamente os pacotes instalados em cada versão:

<center>
<img src="https://i.imgur.com/xBOnYD1.png"/>
<br>
<i>Listagem de pacotes instalados em cada versão</i>
</center>

E finalmente o teste para ver se o pacote fica disponível ou não para outras versões:

<center>
<img src="https://i.imgur.com/SmUqbsm.png"/>
<br>
<i>Rodando o import caipyra em todas as versões</i>
</center>

Tanto na versão `2.7.12` quando na versão `3.6.4` Python grita `ModuleNotFoundErrror`, esse erro é característico quando o pacote não está instalado.

Massa né? Preparar, apontar, instalar pacotes diferentes em cada versão do Python 😜

***

## Links

* A [primeira parte dessa colinha pode ser encontrada aqui](https://jtemporal.com/pyenv-inicio/) e mostra como instalar o pyenv.
* Para entender como pyenv funciona por debaixo dos panos da uma lidinha na seção [How it Works no README do projeto](https://github.com/pyenv/pyenv#how-it-works)

## Agradecimentos

Obrigada Luciano Ratamero pelas dúvidas que me deram a ideia desse outro post 😉