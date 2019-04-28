---
layout: post
title: Movendo um site construído com Jekyll para dentro de um site construído com Pelican
date: 2019-04-28 00:00:00 -0300
img: "/variados.png"
tags:
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
subtitle: Como foi ajustar um site Pelican para servir uma página pronta feita em
  Jekyll

---
#### Ano passado aconteceu a primeira edição do PyLadies BR Conf lá em Natal no Rio Grande do Norte. A edição desse ano vai acontecer na cidade de São Paulo e, com o início dos preparativos para a segunda edição, precisamos mover o site da edição passada para um novo lugar. Aqui vou contar a história de como isso aconteceu.

***

**TK Nota da autora:** Se você não tiver interesse na jornada que culminou em ter o site no ar, a receita de bolo com os comandos para chegar nesse resultado pode ser encontrada aqui. 😉

***

#### Contexto

Para começar os trabalhos você precisa saber que [o site oficial do PyLadies](http://brasil.pyladies.com/) Brasil é construído com um gerador de sites estáticos escrito em Python chamado [Pelican](https://docs.getpelican.com/en/stable/). Eu particularmente não sou fã do Pelican, mas escolhas pessoais à parte, ele tem funcionado muito bem como o site da PyLadies até o momento.

Já [o site do PyLadies BR Conf](https://pyladies-brazil.github.io/conf/), foi feito usando um gerador de sites estáticos diferente: [o Jekyll,](https://jekyllrb.com/) que é feito em Ruby. Embora não seja feito em Python ele foi escolhido por vários motivos, um deles sendo a ampla [biblioteca de templates prontos](http://jekyllthemes.org/) para uso o que, por sua vez, facilitou o site da conferência de ser customizado e colocado no ar.

No entanto, agora que estamos organizando a segunda edição do evento, nos deparamos com a seguinte situação:

> Queremos colocar o site da edição nova no ar aproveitando a versão antiga e ao mesmo tempo mantendo também a versão anterior no ar por motivos históricos.

Existia várias formas de tornar isso possível, cada uma delas com seus lados positivo e negativo, por exemplo, poderíamos criar um novo repositório para o site desse ano. Mas daqui alguns anos com várias edições do evento tendo acontecido teríamos uma quantidade infindável de repositórios sem atualizações. Com isso uma solução interessante seria colocar o site da primeira edição com uma página do site oficial do PyLadies, assim manteríamos o histórico do evento vivo.

#### Preparando o Pelican

No Pelican é costumeiro encontrar todos os arquivos de conteúdo dentro da pasta `content/`, é nela que encontramos um diretório chamado `extra/` que contém arquivos estáticos como o `robots.txt`.

Esses arquivos estáticos e essa pasta `extra/`, assim como a pasta `images/`, precisam ser mapeados dentro de duas variáveis no `pelicanconf.py`. Essa variáveis fazem o controle de arquivos estáticos para que o Pelican, ao fazer o _build_ do site, possa copiar esses arquivos para pasta de _build_. Inicialmente elas estavam assim:

<script src="[https://gist.github.com/jtemporal/33c16fbd43e7d4c1ed6e7b1fc2b8a4aa.js](https://gist.github.com/jtemporal/33c16fbd43e7d4c1ed6e7b1fc2b8a4aa.js "https://gist.github.com/jtemporal/33c16fbd43e7d4c1ed6e7b1fc2b8a4aa.js")"></script>

<center>[Fonte](https://github.com/pyladies-brazil/br-pyladies-pelican/pull/237/files#diff-bee76e83181b4a5548a4ffecd1bea88d)</center>

Então ao olhar para isso inferi que se eu colocasse um arquivo `teste.html` dentro da pasta `content/extra/` e mapeasse ele dentro dessas variáveis, eu encontraria o arquivo `teste.html` ele estaria no site buildado. Então alterei as variáveis de controle acrescentando as configurações no mesmo padrão para o arquivo `teste.html`:

Normalmente, o Pelican tenta interpretar todos os arquivos dentro da pasta `content/` e gerar um arquivo `HTML` de resultado. Por isso, tentei fazer o _build_ do site com essas alterações encontrei o **primeiro erro**:

    ERROR: Skipping teste.html: could not find information about 'NameError: title'

Algumas horas depois de muitas tentativas falhas e de vasculhar a internet para achar o que poderia resolver esse erro, eu encontrei [essa issue no GitHub do Pelican](https://github.com/getpelican/pelican/issues/1157) onde a pessoa tinha tido o mesmo problema e encontrado a solução. O que me faltava para resolver o problema era adicionar a seguinte linha ao `pelicanconf.py`:

    READERS = {'html': None}

A variável `READERS` serve para indicar _parseadores_ de arquivos, ao criá-la com o valor acima, estamos dizendo para o Pelican não fazer o _parse_ de arquivos `HTML`. Ufa, problema resolvido, tudo funcionando, o Pelican serviu o arquivo perfeitamente: consegui acessar `localhost:8000/teste` e ver o arquivo de _teste_.

#### Gerando o site da conferência localmente

Primeiro teste feito, chegou a hora de gerar o site localmente. Como eu falei ali em cima, o site da conferência foi feito em Jekyll, um dos motivos para isso era que o próprio GitHub se encarrega de fazer o _build_ do site para colocar ele no ar. Isso é ótimo porém nos impede de ter acesso ao site _buildado_.

Então para termos os site _buildado_ e colocar ele como uma página no site oficial eu rodei o _build_ localmente. Eu fiz um [tutorial bem detalhado](https://jtemporal.com/do-tema-ao-ar/) de como rodar e colocar um site no ar com Jekyll, então não vou entrar detalhes da explicação de cada comando aqui, mas recomendo você passar lá pra ler depois caso queira entender melhor.

Pra começar eu já tinha o repositório do site da conferência clonado no meu computador e também já tinha instalado todas as bibliotecas que precisava pra construir o site, então eu alterei o arquivo `_config.yml` trocando os valores das chaves `base_url` e `url` assim:

    baseurl: "/conf-1"url: "https://brasil.pyladies.com"

Com essa alteração o site _buildado_ vai estar configurado para ser servido a partir de `[https://brasil.pyladies.com/conf-1](https://brasil.pyladies.com/conf-1 "https://brasil.pyladies.com/conf-1")`. Então, para fazer o build é só seguir o tradicional:

    $ jekyll build 

O comando `build` gera o site da conferência dentro da pasta `_site` e ficamos com uma estrutura assim:

Uma subpasta chamada `assets/` com vários arquivos de _script_ do site CSS, JavaScript, imagens e um arquivo `index.html`. É todo esse conteúdo que vamos copiar lá para o site oficial.

#### Movendo o site da conferência para dentro do site oficial

Agora que já temos o site da conferência eu voltei para a pasta do site oficial e criei dentro da pasta `content/extra/` uma pasta chamada `conf-1/` e copiei o conteúdo da pasta `_site/` para `conf-1/` assim:

Depois disso, atualizei o `pelicanconf.py`:

Ao tentar rodar o build do site oficial me deparei com o **segundo erro**: olhando para essas configurações assumi que se eu colocasse a pasta `conf-1/` dentro de `extra/` e adicionasse as configurações no `STATIC_PATHS` e no `EXTRA_PATH_METADATA` seguindo o mesmo padrão que já tinha visto, seria o suficiente apara acessar `localhost:8000/conf-1` e ver o site da conferência, mas ledo engano meu.

Foi preciso mais algumas horas e outras tantas tentativas frustradas para descobrir, na base da tentativa e erro, que o Pelican não consegue lidar com subpastas da forma que eu esperava. Eu esperava que, com as configurações a estrutura de pastas acima, o Pelican criasse uma pasta `conf-1/` no site oficial o que me mostraria o site da coferência em `localhost:8000/conf-1`, mas o que aconteceu foi que o site da conferência foi mostrado em `localhost:8000/extra/conf-1`.

Lendo esse post você pode até achar que é óbvio o caminho para resolver esse segundo erro, inclusive eu mesma achei que poderia ter resolvido mais rápido agora que estou escrevendo aqui, mas, para mim, o momento eureka aconteceu ao olhar com mais calma para o conteúdo da pasta `output/` — pasta em que o Pelican normalmente faz o build de sites.

Com as configurações que eu fiz, dentro da pasta `output/` eu encontrei uma pasta `extra/` e essa pasta que continha a pasta `conf-1/`. Antes das minhas alterações essa pasta `extra/` não era gerada no site _buildado_. Então, decidi colocar a pasta `conf-1/` dentro de `content/` e no mesmo nível que a pasta `extra/` assim:

E também alterando o arquivo `pelicanconf.py` da seguinte forma:

Agora ao fazer o _build_ do site oficial do PyLadies Brasil conseguimos ver a página da conferência como esperado. 🎉 🎉 EBAAA!!

### Moral da história

Persista nas suas tentativas e mantenha a calma. Mas, ainda mais importante que isso, converse com amigos e amigas sobre o seu problema, isso pode te ajudar a enxergar novas possibilidades. Foi o que eu fiz e me ajudou a exergar a saída para o segundo erro. 😉

Agora que você já leu tudo recomendo ler também o post _“Transformando um site feito em Jekyll em uma página de um site feito em Pelican”_ para ver o passo a passo sem erros para reproduzir o que contei aqui.

Xêro.