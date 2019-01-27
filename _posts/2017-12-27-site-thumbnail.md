---
title: "Adicione cores ao compartilhar seu site"
layout: post
date: '2017-12-27 10:00:00'
img: colinha.png
tags:
- colinha
- web
- web dev
- desenvolvimento web
- html
- meta tags
- share
- compartilhamentos
- português
comments: true
---

Você já notou que ao compartilhar um link na sua rede social favorita esses links carregam imagens para mostrar? Uma espécie de thumbnail do site?

Pois bem, hoje é amplamente utilizada em várias redes sociais, mas quem começou essa brincadeira foi o Facebook que implementou um negócio chamado The [Open Graph protocol](http://ogp.me/). Os objetos dentro do face, são colocados num grafo social (social graph) e são conhecidos como "objetos ricos" (rich objects). Isso que dizer, em poucas palavras, que cada objeto tem uma coleção de características que acrescentam informções a esses objetos.

E o Facebook queria que as páginas compartilhadas dentro dele pudessem carregar a mesma quantidade de informação e aí que entram meta tags. A ideia é usar meta tags para representar ricamente cada página da internet dentro de um grafo social.

Existem várias meta tags que podem ser utilizadas, mas na colinha de hoje vamos falar da `og:image`.

## O site do Pizza De Dados

Esse semana passada, eu procurei deixar o site do [Pizza de Dados](http://podcast.datascience.pizza/) mais amigável aos olhos e uma das coisas que eu quis mudar foi acrescentar a meta tag com a nossa logo 🍕

Antes ao compartilhar o site do Pizza tinhamos algo assim:

![foto do compartilhamento do pizza no facebook antes da adição da meta tag](/images/og-image/antes-tag.png)

Nota como fica muito sem graça? Às vezes na tentativa de preencher as informações sobre a págica a rede social ainda pode acabar escolhendo uma imagem qualquer que exista na página e usá-la como "thumbnail".

Então para alterar esse comportamento e adicionar um pouco de cor às nossas postagens seguindo o site oficial da OGP acrescentamos uma linha parecida com essa:

~~~ html
<meta property="og:image" content="http://ia.media-imdb.com/images/rock.jpg" />
~~~

no head do site. E 🌈automagicamene🌈 ao compartilhar o site do Pizza temos:

![foto do compartilhamento do pizza no facebook antes da adição da meta tag](/images/og-image/depois-tag.png)

Tranquilo né? Agora é só usar no seu site tbm 😉

----
## Links
- O site oficial da [Open Graph protocol](http://ogp.me/)
