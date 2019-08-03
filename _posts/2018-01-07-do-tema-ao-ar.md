---
title: "Colocando um site no ar com Jekyll: usando o terminal"
layout: post
image: "/images/tutorial.png"
date: '2018-01-07 10:00:00'
tags:
- tutorial
- jekyll
- ruby
- terminal
- site
- github pages
- github
- github
- português
comments: true
---

Numa [colinha eu falei sobre como usar Jekyll](http://jtemporal.com/temas-jekyll/) é uma boa ideia para ter o seu site no ar vamos ao exemplo prático usando o terminal. O objetivo aqui é ter um site no ar, então vamos lá.

## Escolhendo um tema

Vá até o [Jekyll Themes](http://jekyllthemes.org/) e escolha um tema. Eu escolhi pra hoje o [Fresh](http://jekyllthemes.org/themes/fresh/).
![fresh no jekyll themes](https://i.imgur.com/P7Yq4hP.png)

Fresh é tema de um blog, com algumas páginas extras como "Sobre" e "Contato". Além disso ele é responsivo, considerando que muito conteudo hoje é consumido pelos smartphones e tablets por aí essa é uma capacidade que queremos. Dá uma olhada na Demo do Fresh:
![fresh demo](http://artemsheludko.pw/fresh/assets/img/fresh.gif)

## Preparando o ambiente

Precisaremos instalar (caso ainda não tenha) a seguinte listinha:
- Ruby
- Jekyll
- Git
- Gem

Os passos a seguir são para o meu sistema operacional (Elementary OS), tenha isso em mente quando for reproduzir os passos aqui, pois pode ser que tenha variações para o seu OS.

Vamos as intalações.
``` console
$ sudo apt install git
$ sudo apt install ruby-full
$ sudo apt install rubygems       # ou rubygems-integration
$ gem install jekyll bundler
```

Pra que tudo isso?

1. **git**: vamos precisar colocar o site no [GitHub](http://github.com/) para servi-lo;
1. **ruby-full**: uma versão antiga mais estável do Ruby. Como Jekyll foi feita nessa lingaguem precisamos dela instalada no seu computador;
1. **rubygems**: Assim como o pip para Python e o npm para o mundo Node, existe o gem para ruby. Ele é um gerenciador de pacotes e ele que precisaremos para instalar jekyll e os demais pacotes para o Fresh;
1. **jekyll**: O pacote do momento;
1. **bundler**: No melhor esquema Inception o bundler é um pacote que controla outros pacotes, ele controla versões dos pacotes e suas dependências nos projetos.

## Baixando o tema
Pra quem já tá acostumado com Git acho que essa parte vai ser tranquila. O [link para o repositório é esse aqui](https://github.com/artemsheludko/fresh). Pra quem não está acostumado os passos são esses:

``` console
$ git clone https://github.com/artemsheludko/fresh.git
```

Nota: Aqui eu preferi fazer um _clone_ propositalmente, porém para aqueles mais íntimos do Git e de seus processos pode fazer um _fork_ mesmo. Nesse caso escolhi o clone pois já vi (e já aconteceu comigo também) que ao tentar usar os bons hábitos de abrir _pull requests_ na sua própria rotina de publicação, mandar o PR para o repositório original ao invés de fazer no seu mesmo.

## Rodando o projeto

Depois de clonar é hora de colocar isso pra rodar né mesmo?! Então vamos lá, os passos abaixo vão deixar o seu site sendo servido localmente. Vamos a eles:

``` console
$ cd fresh
$ bundle install
$ bundle exec jekyll serve
```
![Foto do terminal servindo o fresh](https://i.imgur.com/Cxh1nNO.png)

_Observação_: se por acaso você não possui uma Gemfile no seu tema, os passos acima não vão funcionar, [da uma lida aqui nessa colinha sobre a Gemfile](http://jtemporal.com/gemfile/) pra entender um pouquinho mais sobre isso ;)

Agora vá até o seu navegador favorito e acesse `http://localhost:4000/fresh/`. Massa né?!

Muito bem, vamos entender o que acabamos de fazer:

1. **bundle install**: O bundle é um comando que vai procurar na sua Gemfile e a Gemfile.lock os nomes e versões dos pacotes para instalar as dependências necessárias para rodar projeto;
1. **bundle exec jekyll serve**: Esse aqui é o que faz o trabalho de ajustar o ambiente para rodar o servidor Jekyll. Além disso, toda vez que você roda esse comando é feito o build do site.

Para parar o servidor basta apertar `Ctrl + c`, você vai precisar disso para os próximos passos.

## Arquivo de configuração
Vamos começar pela configuração pois, é no arquivo `_config.yml` que moram as informações como título do site, autor e por aí vai.

Abra o arquivo `_config.yml` no seu editor favorito e vamos alterar e entender ele aos pouquinhos. Para ver as alterações no site, sempre que você mudar o `_config.yml` você precisará parar e reiniciar o servidor do Jekyll.

### Informações de perfil
Nessa parte vão as informações iniciais:
- O título (`title`): que o nome que aparece quando você abre uma aba no navegador;
- O nome (`name`): geralmente algo descritivo do que o seu blog vai abordar;
- A descrição (`description`): que vai dentro do HTML e quando alguém compartilhar o seu blog é essa descrição que aparece, isso também ajuda a colocar o seu site nos resultados de buscas como Google e o DuckDuckGo;
- A URL de base (`baseurl`): a partir de que link o seu site é servido, o caminho para a sua "home"
- A URL (`url`): aqui é onde vai o domínio seja ele aquele que o GitHub Pages disponibiliza (username.github.io) ou um que você venha a comprar (como é o meu caso aqui).

Edite esse primeiro bloco para algo parecido com isso:

``` yml
# Profile information
name: Contos
title: Um blog com Jekyll
description: >
    Uma coleção de contos
permalink: ':title/'
baseurl: "/blogfresh" # the subpath of your site, e.g. /blog
url: "http://jtemporal.com" # the base hostname & protocol for your site, e.g. http://example.com
```

Execute novamente o comando do servidor `bundle exec jekyll serve`. Dessa vez o site que você precisa acessar mudou pois mudamos o `baseurl` para `/blogfresh`. Agora acesse `http://localhost:4000/blogfresh/` para ver as mudanças. Com essas alterações no `_config.yml` o site deve estar parecido com isso:

![blog alterado profile info](https://i.imgur.com/kut6tWL.png)

### Social
É aqui que vem os links para suas redes sociais. As que você não quiser deixar disponível basta não preencher. Eu vou ensinar como esconder os botões para as redes sociais que não tiverem um valor aqui em outro blog post, por enquanto vamos colocar apenas uma delas:

``` yml
# Social
facebook: #Add your Facebook
twitter: #Add your Twitter
google-plus: #Add your Google+
github: @jtemporal
```

Infelizmente esse tema não "esconde" os botões para as redes sociais que não possuem um usuário/link válido.

### Formulário de contato
Se você quiser que as pessoas entrem em contato com você pelo formulário de contato do site é só remover o comentário e colocar o seu e-mail no lugar de `your-email@domain.com`. Como eu não curto receber e-mail deixei comentado mesmo `¯\_(ツ)_/¯`.

``` yml
# Contact form
email: #your-email@domain.com
```

### Comentários
Essa uma parte legal, comentários nos seus posts! Se você não conhece o [Disqus](http://disqus.com/) ainda, ele nada mais é do que uma plataforma que ajuda a aumentar o envolvimento nos seus sites. Ele permite de forma relativamente fácil que pessoas comentem no seu blog. Crie uma conta e coloque o seu identficador aqui:

``` yml
# Comments
discus-identifier: jtemporal
```

Após reiniciar o servidor para que as mudanças do arquivo de configuração façam efeito, você deve ter uma área paracida com a da imagem abaixo no fim de um post:

![area de comentarios disqus](https://i.imgur.com/3Lc0O9q.png)

### Paginação
É nessa seção que você define quantos posts aparecem por página no seu site:
- Posts por página (`paginate`): quantos blog posts aparecem em cada página;
- URL de uma página (`paginate_path`): essa variável define como vão ser geradas as URLs de uma página específica, por exemplo, `blog/page2/index.html`.

``` yml
# Paginate
paginate: 5
paginate_path: /page:num/
```

### Configurações de build
Aqui você não vai precisar mudar nada, mas é bom entender o que tá rolando né?!

- Renderizador de markdown (`markdown`): aqui você pode escolher que renderizador de Markdown usar. O padrão é o `kramdown`;
- Gems necessárias para esse tema (`gems`): aqui vai uma lista de gems que precisam ser instaladas;
- Diretórios e arquivos para exclusão (`exclude`): esses são arquivos e diretórios para serem desconsiderados na hora de gerar as páginas do site;
- Diretórios e arquivos para inclusão (`include`): esses são arquivos e diretórios para serem considerados na hora de gerar as páginas do site, se você quiser servir algum arquivo a partir do seu site você deve incluir ele nessa lista.

``` yml
# Build settings
markdown: kramdown
gems:
  - jekyll-feed
  - jekyll-paginate
exclude:
  - Gemfile
  - Gemfile.lock
include: [_pages]
```

Ao terminar de arrumar o `_config.yml` lembre-se de commitar as mudanças! Vou assumir que você fez isso a cada passo daqui pra frente ;)

## Posts
O Fresh, assim como todos os outros temas disponíveis trazem exemplos de posts. A primeira coisa eu você vai notar ao abrir um deles no editor de texto é que ele tem uma espécie de cabeçalho conhecido como [Front Matter](https://jekyllrb.com/docs/frontmatter/). Lá são definidos coisas como o layout do post, o título, a data de publicação e por aí vai...

### Entendendo o Front Matter

- `layout`: O Jekyll enxerga todas as páginas do blog como um blog post que ele precisa renderizar, inclusive as páginas _About_, _Contact_ e _Home_ então a tag layout é utilizada para diferenciar a renderização, as opções são: post ou default;
- `title`: O título da postagem;
- `date`: A data e hora da publicação no formato `AAAA-MM-DD HH:MM:SS` ainda é possível passar o fuso horário da publicação caso queira usando esse campo;
- `description`: Cada blogpost pode ter um paragrafo de sinopse na listagem de posts. É aqui que essa sinopse vai.

#### Primeiro post

Vamos criar um post novo do zero e colocar no site. Crie um arquivo dentro do diretório `_posts`:

``` console
$ touch _posts/2018-01-07-ola-mundo.md
```

É costumeiro usar o padrão `AAAA-MM-DD-nome-do-post.md` nos nomes dos arquivos. Lembre-se que é apartir do nome do arquivo que vão ser geradas as URLs para cada post.

Abra o arquivo que acabamos de criar, cole o seguinte Front Matter e conteudo:

``` plaintext
---
layout: post
title:  "Olá mundo"
date:   2018-01-07 00:00:00
description: Primeiro blogpost
---

Olá mundo!
```

Se o servidor Jekyll não estiver rodando, inicie ele. Se já estiver rodando ao criar e salvar o arquivo dê um tempinho para que ele possa gerar o HTML desse novo post. Vá até o navegador e recarregue a página. Et voilà!

![Primeiro blogpost](https://i.imgur.com/z528dqZ.png)

Commite esse arquivo.

## Publicando o site
Bom, como lá no começo fizemos um `git clone` todas as ligações desse repositório estão com o repositório inicial. Como assim? Os repositrórios Git fazem comunicação com o GitHub (ou qualquer outra rede) por meio do que chamamos de `remote`. Não vou entrar em detalhes do funcionamento disso nesse post mas você pode ler mais sobre isso [na documentação oficial do Git](https://git-scm.com/docs/git-remote).

Aqui nós vamos fazer alguns passos, alguns deles pelo site do GitHub, outros no terminal mesmo.

### Terminal: segunda fase
Os comandos do terminal acontecem em duas fases, a primeira é essa aqui que consiste em renomear o `remote` do repositório original para `upstream` isso é útil pois você pode utilizar o `upstream` para atualizar o código do seu tema.

``` console
$ git remote rename origin upstream
```

Depois disso vamos ao GitHub.

### GitHub
Aqui, assumindo que você já possui uma conta no GitHub, [caso não tenha é só criar, é rapidinho](https://github.com/join?source=header-home). Para os passos a seguir eu vou mostrar duas versões, uma para quem **já tem um site** publicado (como eu) e uma para quem está publicando um site pela **primeira vez**.

#### Meu primeiro site

1. [Criar um repositório novo](https://github.com/new) como esse é o seu primeiro site, esse repositório precisa ter um nome especial seguindo o padrão `meu-nome-de-usuario.github.io`:
![foto do repo username.github.io](https://i.imgur.com/2vOk9OJ.png)
1. Copiar as instruções que vem na área `...or push an existing repository from the command line`:
![instruçoes de remote primeiro site](https://i.imgur.com/me5fQgM.png)

#### Já tenho um site
Pra quem já tem um site no GitHub, esse novo site vai ser uma do seu site atual. Por exemplo, eu tenho esse site aqui `http://jtemporal.com` e esse blog novo vai virar `http://jtemporal.com/blog`. O mesmo padrão se repete se você não tiver um domínio personalizado, seu site ficará algo tipo `https://jtemporal/github.io/blogfresh`. Vamos lá:

1. [Criar um repositório novo](https://github.com/new): só seguir a imagem abaixo, você não precisa configurar mais nada, e lembrete que aqui eu dei o mesmo nome que está no meu `url` lá no `_config.yml`:
![criando novo repo no gihub](https://i.imgur.com/EX0HGFq.png)
1. Copiar as instruções que vem na área `...or push an existing repository from the command line`:
![instruçoes de remote](https://i.imgur.com/kcFTVrk.png)

### Terminal: segunda fase
A segunda fase do terminal consiste em mandar o nosso código para o GitHub e colocar o site no ar de fato. Vamos lá que tá quase:

``` console
$ git remote add origin git@github.com:jtemporal/blogfresh.git
# ou: git remote add origin git@github.com:jtemporal/jtemporal.github.io.git
$ git push -u origin master
```
### GitHub: o retorno
Depois de mandar o código para o GitHub agora precisamos configurar o site, simbora!
Vamos começar indo para aba de `Settings` do repositório:
![aba de configurações](https://i.imgur.com/f3rxngC.png)

Descendo nessa página de configurações encontramos a seção referente ao GitHub Pages. É essa seção que acaba publicando o site:
![seção GitHub Pages](https://i.imgur.com/7brruPu.png)

Quando selecionado `None` na área de Source, o GitHub Pages está desativado. Então vamos selecionar um ramo para publicar nosso site a partir dele, nesse caso o ramo será o `master` mesmo:
![selecionando o ramo de publicacao](https://i.imgur.com/fFh4CN0.png)

E agora clicar em `Save`:
![clique em save](https://i.imgur.com/60Li2Ww.png)

E o resultado será algo parecido com:
![site publicado](https://i.imgur.com/BRM01sH.png)

É só acessar o link do site agora que deve estar lá bonitão ;)

E agora? Bem agora você pode começar a escrever outros posts e ir commitando eles. Todo commit/pull request para a master vai automagicamente executar os passos de build do site e deploy. Happy blogging!

----
## Links e considerações
- Existem outras formas de instalar o Ruby na sua máquina, encontre elas estão listadas na [documentação do Ruby em português](https://www.ruby-lang.org/pt/documentation/installation/).
- [Bundler](http://bundler.io/).
- Eu usei os comandos do Git para chaves SSH, escolha pessoal minha, se preferir use os mesmos comandos utilizando o acesso via HTTPS. Caso queira utilizar o SSH assim como eu você vai precisar de chaves, [esses tutoriais do GitHub em inglês ensina como gerá-las e utilizá-las](https://help.github.com/articles/connecting-to-github-with-ssh/).
- Prometo fazer um post explicando como alterar o layout e outras coisas no tema em breve.
