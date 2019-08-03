---
title: "Forçando o rebuild de sites Jekyll hospedados no GitHub"
layout: post
date: '2018-01-03 10:00:00'
image: "/images/colinha.png"
tags:
- colinha
- jekyll
- build
- rebuild
- github pages
- github
- português
comments: true
---

Seja por um CSS quebrado ou por uma mudança que parece não ter tido efeito, às vezes é necessário forçar o rebuild de um site [Jekyll](https://jekyllrb.com/) hospedado no GitHub. A colinha de hoje explica uma forma de fazer isso.

O GitHub possui um "serviço" para servir páginas a partir de repositórios chamado de [GitHub Pages](https://pages.github.com/). Esse serviço permite que você crie páginas quase que instantaneamente a partir do seu repositório e tudo que você precisa é de um arquivo markdown como o README de um projeto e um arquivo de configuração básica.

Isso é possível pois o GitHub Pages usa o Jekyll, um gerador de site estático open source, para fazer o build de sites. Entre outras facilidades que não vou falar hoje, um ponto extremamente positivo de usar essas duas ferramentas para colocar o seu site no ar é que você não precisa "subir" para o GitHub um build do seu site toda vez que quiser publicar uma mudança, o próprio GitHub se encarrega do build para você.

Porém algumas vezes é ncessário rodar o processo de build novamente e como esse processo acontece nos servidores do GitHub que nós não temos acesso, precisamos de outras formas de forçar o build.

Uma delas e a que eu uso hoje em dia é fazer um commit "vazio", ou seja, um commit que não carrega mudanças em arquivo algum do seu diretório.

Basta ir para o diretório do site (aqui vou usar o do meu site como exemplo):

~~~ console
$ cd jtemporal.github.io
~~~

e escrever um commit usando a opção `--allow-empty`:

~~~ console
~/jtemporal.github.io $ git commit --allow-empty -m "Forçando o rebuild"
~~~

ou se preferir editar a mensagem do commit no editor de texto:

~~~ console
~/jtemporal.github.io $ git commit --allow-empty
~~~

E é isso. Isso é o suficiente para forçar o processo do build novamente. Massa né? 😜

----
