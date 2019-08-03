---
layout: post
title: Uma história de Pair Programming a longa distância
date: 2017-03-11 00:00:00 +0000
tags:
- português
- tmux
- tmate
- vim
- pair programming
- parear
- código
comments: true
image: "/images/variados.png"

---
Pair programming, parear, escrever código em dupla. Quem escreve código, alguma
vez na vida já pareou, e se não o fez ainda, provavelmente irá fazer isso em algum
momento da vida. Para os estudantes da arte da programação que escrevem código em
grupo, pair é uma técnica tão normal que muitas vezes nem percebem que estão fazendo.

Escrever código em par é bom por uma série de motivos, dentre eles gosto de destacar
o aumento de produtividade e foco, oportunidade de aprendizado e também diversão.
Desenvolver um programa ou um script a quatro mãos, é uma das experiências mais
gostosas de ser programadora.

![pair programming](http://ljdchost.com/4k7kp0I.gif)

### Trabalho remoto e programação a longa distância

Muitos podem achar que para parear é necessário estar na mesma sala que o seu par.
Quem trabalha remotamente sabe o quanto isso não é necessáriamente verdade.

Para um time de desenvolvedores que trabalha junto, programar a longa distância
é mais uma das coisas fantásticas que programadores fazem e isso vem muitas vezes
de mãos dadas com o trabalho remoto. No time da Serenata, podemos contabilizar horas
e horas de telaselfselfs compartilhadas, sejam entre aqueles trabalhando na mesma cidade
([Irio Musskopf](https://twitter.com/irio) e [Ana Schwendler](https://twitter.com/anaschwendler)),
ou entre aquelas trabalhando em estados diferentes ([Eduardo Cuducos](https://twitter.com/cuducos)
e eu).

Nós usamos os olhos dos colegas de trabalho como forma de escrever um código melhor
e também como fonte milagrosa da dádiva do _desempacamento_. Usar a tecnologia à
nosso favor na hora de programar é uma arte.

Quem pareia tem suas formas preferidas, eu vou falar uma das minhas. Programo e
m Python, e no meu dia a dia uso:

* O Ubuntu: meu sistema operacional oficial desde 2015;
* O terminal do Ubuntu: meu companheiro para todas as horas;
* VIM: o editor de texto dos viciados em telas pretas e atalhos de teclado;
* tmux (e tmate): o multiplexador de terminais queridinho dos devs.

### VIM

[VIM](http://www.vim.org/) é um editor de texto altamente configurável e focado
em eficiência. Mas também é conhecido por não ser fácil de usar pois é uma ferr
amenta que precisa ser aprendida antes de ser utilizada. A frase “não deixo de
usar vim pois não sei como sair dele” é a piadinha mais comum de se ouvir pelas
pessoas de tech.

![VIM](http://i.imgur.com/B5rKp2z.png)
<center>
<i>Tela inicial do VIM</i>
</center>

Para usar o VIM você precisa saber algumas coisas básicas:

* Ele possui modos diferentes de uso: como o de inserção que serve para escrever
e o modo de comando que usamos para informar ao vim comandos que queremos executar;
* Os comandos do VIM são escritos, coisas como `set number`, para mostrar os núme
ros da linha, `set cc=80` (_color column_) mostra uma coluna no octagésimo caracter
de todas linhas e `wq` (_write and quit_) para salvar o arquivo e sair do editor.

O VIM foi feito para que tudo que você queira fazer no editor possa ser feito s
em remover as mãos do teclado.

### tmux e tmate
Quem desenvolve usando o terminal às vezes se pega tendo que abrir mais de uma
janela, por exemplo, se eu estiver rodando um web-app, eu provavelmente vou ter
um terminal rodando um server, um outro com acesso ao código que estou editando
, se forem tanto o html quanto o css isso já me dá três janelas abertas.

![exemplo de terminal com tmux](http://i.imgur.com/kzMTb12.png)
<center>
<i>sessão tmate com  vim, server, tree e ipython tudo rodando ao mesmo tempo</i>
</center>

Para eliminar essa quantidade excessiva de janelas abertas, o [tmux](https://tm
ux.github.io/) foi desenvolvido, ele é um multiplexador de terminais e permite
que você tenha todos os arquivos e processos rodando e mostrando resultados na
mesma tela. Com base no tmux, surgiu o [tmate](https://tmate.io/), que além de
usar todas as maravilhas que o tmux tem, ele ainda abre um canal de comunicação
seguro para que o seu _mate_ (amigo) possa se conectar por meio de uma sessão s
sh. E o melhor é que o seu mate não precisa nem estar num ambiente que provê ac
esso ssh, lhe basta um navegador.

Uma vez aberto o canal iniciado com um simples tmate no seu terminal, você pode
mandar o link de acesso para quem quiser e, caso o link enviado não seja o _read
-only_, a pessoa do outro lado da conexão poderá programar no seu computador.

Assim como o VIM, tmux /tmate possui modos e atalhos de teclado para sua utiliz
ação. E eu recomendo fortemente [essa palestra](https://gist.github.com/rougeth/db185fc21c376ece8fc6) do [Marco Rougeth](https://twitter.com/marcorougeth) par
a usar de referência com esses comandos.

### Canal por voz
Junto ao editor de texto, que para essa estrutura funcionar precisa ser um que
abra no terminal, e o terminal compartilhado, eu ainda uso algum canal por voz.

![mic drop](http://68.media.tumblr.com/7d98d3b163734d963c7629a495868009/tumblr_inline_nuqb7jzcCU1rfr6lu_500.gif)

Já usei: [Zoom](https://zoom.us/), [Hangouts](https://hangouts.google.com/), [a
ppear.in](https://appear.in/) e [Skype](https://www.skype.com/en/) e nesse caso
é muita questão de escolha pessoal, disponibilidade de banda e quanto cada um d
esses influencia na capacidade de processamento do seu computador.

Essa é uma alternativa ao famoso screen share trazida pelos grandes programas d
e video-conferência. No meu computador que traz uma capacidade de processamento
limitada, VIM + tmate + Hangouts, tem se mostrado uma forma graciosa de trabalh
ar a distância sem os tantos travamentos que já atrapalharam por diversas vezes.

Tendo à mão todas essas tecnologias e um pouquinho de treino, pair será um mome
nto legal de muito código escrito a quatro mãos.

![OMG](https://media4.giphy.com/media/TXJiSN8vCERuE/giphy.gif)

Obrigada ao grupy-rp por me apresentar e me treinar na arte do vim, tmux e pair
programming; ao Marco muito obrigada por me apresentar tmate, [tmuxp](https://t
muxp.git-pull.com/en/latest/) (um gerenciador de sessões do tmux) e outras mara
vilhas como essas; ao Otávio obrigada pelas dicas e feedback; e ao time do Sere
nata obrigada pelas oportunidades diárias de pair programming ❤ ❤ ❤