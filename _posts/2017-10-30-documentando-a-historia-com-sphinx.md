---
title: "Documentando a história com Sphinx"
layout: post
img: tutorial.png
date: '2017-10-30 10:00:00'
tags:
- tutorial
- python
- sphinx
- readthedocs
- read the docs
- documentação
- português
comments: true
---

Sabe aquelas documentações bonitas de bibliotecas que você encontra por aí? Por exemplo, a documentação do [Bottery](https://docs.bottery.io/en/latest/) ou a do [Flask](http://flask.pocoo.org/docs/0.12/)? Todas são construídas com uma biblioteca Python chamada [Sphinx](http://www.sphinx-doc.org/en/stable/#).

Sphinx foi criada para gerar a própria documentação do Python e hoje é muito utilizada por facilitar a construção automatizada de documentações de bibliotecas. Uma coisa legal que dá para fazer é utilizar Sphinx para construir um histórico de acontecimentos como é feito no [Manual do Big Kahuna da Python Brasil](http://manual-do-big-kahuna.readthedocs.io/en/latest/).

Com o objetivo de fazer o mesmo tipo de histórico com eventos regionais comecei um [repositório para a PythonSudeste](https://github.com/pythonsudeste/pythonsudeste_documentacao) e agora também um para PyConAmazônia que, graças ao Nilo Menezes, [possui um _post mortem_ da organização completo disponibilizado para a comunidade](https://www.dropbox.com/s/tr83g5j5amdkyxt/Pycon%20Amaz%C3%B4nia%202017%20-%20Memorial%20da%20Organiza%C3%A7%C3%A3o%20do%20Evento.pdf?dl=0).

Vou narrar aqui os passos que fiz na esperança que mais organizadores de eventos regionais possam disponibilizar o mesmo tipo de levantamento histórico de seus eventos.

## Tudo começa com repositório git

Comecei adicionando dois arquivos:
- `requirements.txt`: esse contém as bibliotecas que vamos usar para gerar a documentação
<center>
<script src="https://gist.github.com/jtemporal/7e6a99f4245407367dc07740b04f925e.js"></script>
<small>
<i>requirements.txt</i>
</small>
</center>
- `README.md`: esse contém informações de como rodar o projeto
O projeto Sphinx que vamos usar aqui roda num ambiente virtual com Python 3. Eu particularmente gosto de usar [`virtualenv`](https://virtualenv.pypa.io/en/stable/) para criar meu ambiente:

``` console
$ virtualenv .env
$ source .env/bin/activate # pode variar depedendo do seu sistema
```
E para instalar as dependencias:

``` console
(.env) $ pip install -r requirements.txt
```

Depois de instaladas, começamos rodando o quickstart do Sphinx:

``` console
(.env) $ sphinx-quickstart
```
Esse _quickstart_ vai te fazer inúmeras perguntas de como montar a sua documentação. Eu [copiei o _output_ em um Gist](https://gist.github.com/jtemporal/e30f156e6444ca20fe07f65e0c6215bf) para que você possa estudar o que responder para cada uma das perguntas e também para que veja as repostas que eu dei. Em sua maioria, eu segui com o padrão já oferecido pelo Sphinx. Alguns pontos importantes dessas perguntas:

- Rodando o `sphinx-quickstart` você precisará responder coisas como “Qual o nome do projeto?”, “Qual o nome do autor?” e “Qual a língua do conteúdo?”, então é importante responder com calma cada uma das perguntas ;)
- Em algum momento desse questionário é tomada a decisão sobre separar o _build_ e o _source_ e aqui a dica é: Se for colocar no [ReadTheDocs](https://readthedocs.org/) **não** precisa _commitar_ o _build_ \o/ (mas eu vou falar disso melhor num outro post)

Ao final desse longo questionário que acabamos de responder, você vai encontrar uma estrutura pronta para ser usada:

<center>
<script src="https://gist.github.com/jtemporal/e4ef18051ec0d627678ad658826dc362.js"></script>
</center>

### build/
Inicialmente vazia, mas quando rodarmos o comando de construção do site lá vai ficar cheio de coisa ;P

### source/
É lá que vamos colocar todos nossos arquivos que vão virar páginas do nosso projeto.

### conf.py
As respostas que demos durante o _quickstart_ ficam armazenadas dentro desse arquivo de configurações e é ele que o sphinx usa para gerar os arquivos `.html` a partir dos arquivos de texto. Aqui note que a extensão preferida do Sphinx é `.rst` de _reStructuredText_ e desconfio que escolheram `.rst` por ser uma forma de escrita baseada em identação. 👀

### index.rst
É a partir do `index.rst` que o Sphinx vai construir o `index.html` da sua documentação. Se você abrir o `index.rst` no GitHub você vai ver que ele é relativamente simples:

<center>
<script src="https://gist.github.com/jtemporal/39028b49f8c0b851b4bfccf2b4a149fc.js"></script>
</center>

Essa é a “versão de fábrica” do `index.rst` que é criada ao rodar o _quickstart_. Com ela já é possível rodar um _build_ inicial do site. Para fazer o _build_ usamos o `make`, ele se encarrega de buscar no diretório do projeto os arquivos `.rst` e “traduzi-los” para `.html`. Vejamos:

``` console
(.env) $ make html
```

se rodar sem erros, o resultado na tela deve ser parecido com isso:
<center>
<script src="https://gist.github.com/jtemporal/123389890312d764ec16bcea64e06178.js"></script>
</center>
_Mas Jessica, pra quê build se você mesma disse lá em cima que o ReadTheDocs não precisa dele?_ É verdade, mas o jeito mais fácil de verificar o resultado do seu trabalho é localmente e para isso você precisa ter as páginas `.html`.

Outra coisa que você vai precisar é uma forma de visualizar essas páginas, claro que você pode apenas abrir os arquivos `.html` no seu navegador favorito, mas outra opção é usar iniciar um _server_ (servidor). Servidores foram adicionados como parte [built-in do módulo `http` no Python 3](https://docs.python.org/3/library/http.server.html#module-http.server) e são muito úteis em casos como esse. Para iniciar um processo servidor basta rodar:

``` console
(.env) $ python -m "http.server"
```

E usando o navegador acessar o caminho `localhost:8000`. Rodando o processo a partir do root do projeto como fizemos você deve ver uma listagem de todos arquivos e diretórios no seu navegador:

<img src="https://i.imgur.com/cLzKN77.png" style="max-width: 60%;">

E aí é só seguir pelo caminho até a pasta onde estão os `.html`:

<img src="https://i.imgur.com/1XNPT8Q.png" style="max-width: 60%;">

Ao clicar em `html/`, por conter um arquivo `index.html`, seu navegador irá mostrar o resultado do `build` que fica mais ou menos assim usando o `index.rst` gerado de fábrica:


<center>
<img src="https://i.imgur.com/X0VyLbU.png">
<small>
<i>Resultado do primeiro build com o index.rst de fábrica</i>
</small>
</center>

## Introdução de conteúdo \o/

Todos esses passos até agora foram para preparar nosso projeto para chegar na parte que realmente queremos. Vamos começar criando uma página de conteúdo chamada i`pyconamazonia2017.rst` apenas com um título e criar a conexão entre ela e nosso `index.rst`, veja:

<center>
<script src="https://gist.github.com/jtemporal/8d6a0aea5efe3dd251e4787b876863df.js"></script>
</center>

<center>
<script src="https://gist.github.com/jtemporal/b604f5ea85b0240cf2466a91b3726e23.js"></script>
</center>

Ao rodar o _build_ teremos o seguinte resultado:
<center>
<img src="https://i.imgur.com/nA3IG1u.png">
<small>
<i>resultado do build para pyconamazonia.rst</i>
</small>
</center>

<center>
<img src="https://i.imgur.com/7ReRbwJ.png">
<small>
<i>resultado do build para index.rst</i>
</small>
</center>

Mudando um pouco o conteudo desse `index.rst` para colocar uma capa por exemplo temos:
<center>
<script src="https://gist.github.com/jtemporal/5d026f71e9bad58e1ce064551cf49615.js"></script>
<small>
<i>index.rst com link para imagem de capa</i>
</small>
</center>

Nota: a imagem não renderizou aqui pois o link só faz sentido dentro do projeto já que este possui uma pasta que contém a imagem. Depois do _build_ a página fica assim:

![renderizando o projeto com a capa](https://i.imgur.com/skq9ygN.png)

A partir daí é só continuar preenchendo e conectando novas páginas do jeito que achar mais interessante ;)

---

Depois de tudo isso você pode achar que o tema padrão para as páginas não tá legal e querer mudar. O Sphinx possui vários temas embutidos mas aqui vamos usar o tema do ReadTheDocs. Primeiro começamos adicionando ele ao `requirements.txt`:

<center>
<script src="https://gist.github.com/jtemporal/32648f3777c33ff2feb8961c49be9173.js"></script>
<small>
<i>requirements.txt com o tema do ReadTheDocs</i>
</small>
</center>

E depois instalando da seguinte forma:

``` console
(.env) $ pip install -r requirements.txt
```

E agora fica só faltando alterar o valor da variável que indica o tema no arquivo de configuração (`conf.py`) para o tema do ReadTheDocs:

``` console
html_theme = ‘sphinx_rtd_theme’
```
E ao fazer novo _build_, a sua página inicial vai ter essa carinha:

![projeto renderizado com o tema do Read The Docs](https://i.imgur.com/fVXB8YJ.png)

---
## Considerações

O projeto do memorial da PyCon Amazônia foi estruturado para receber **o maior número de contribuições possíveis**. Se quiser [corre lá no GitHub pra ver como tá sendo isso](https://github.com/pythonbrasil/pycon-amazonia-memorial) 🎉

E uma dica sobre `.rst` é [usar esse cheatsheet aqui](https://github.com/ralsina/rst-cheatsheet/blob/master/rst-cheatsheet.rst) quando não souber a forma de fazer algo em restructured text 🙃

Agora, a parte de deixar tudo isso online fica pro próximo post, por hoje é isso pessoal ;)

## Agradecimentos
Marco Rougeth e Silvia Benza pelas revisões.
