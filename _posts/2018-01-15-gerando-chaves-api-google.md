---
title: "Gerando chaves das APIs do Google"
layout: post
date: '2018-01-15 01:00:00'
img: tutorial.png
tags:
- tutorial
- google api
- apis
- api
- português
comments: true
---

Hoje em dia é sempre API isso ou API aquilo. APIs nada mais são do que uma forma de acessar dados e/ou serviços por meio de um programa. Mas pra acessar APIs como as do Google você vai precisar de uma chave. Esse tutorial de hoje vai ensinar você a criar uma chave do Google e ativar uma das APIs do Google para usá-la. Lembrando que os passo a seguir são para criação de chaves para uso próprio, chaves para aplicações web e outras aplicações pode ter passos diferentes ;)

![let's do this](https://media.giphy.com/media/zaezT79s3Ng7C/giphy.gif)
<center>
<i>Vamos lá!</i>
</center>


## Começando
Pra começar acesse o [painel de desenvolvimento do Google](https://console.developers.google.com/apis/dashboard). Ele deve ter uma carinha parecida com essa aqui:

![painel de desenvolvimento do google](https://i.imgur.com/52hRqLe.png)

## Criando um projeto
Para criar uma ou mais chaves, você precisa ter um projeto. Toda e qualquer chave de API do Google precisa de um projeto para existis. Então vamos começar criando um desses. Clique em `Criar projeto`:

![criar projetos](https://i.imgur.com/3cTDEam.png)

Depois em criar:

![criar](https://i.imgur.com/nEtPSFp.png)

E então escolha um nome para o seu projeto e clique em criar:

![selecionar nome e criar](https://i.imgur.com/E2QJ67X.png)

Ao clicar em criar você voltará para o painel:

![painel depois da criação de um projeto](https://i.imgur.com/f4oXDLm.png)

Notou como não tem mais o aviso de "está faltando projetos"?

## Criando uma chave

Agora no menu a esquerda da tela clique em `Credenciais`

![menu a esquerda: Credenciais](https://i.imgur.com/OD6mafF.png)

para acessar o painel de chaves.

![painel de credenciais](https://i.imgur.com/SYlKCwK.png)

É nessa parte que no futuro vão ser listadas todas as chaves que você gerou para aquele projeto. Agora clique no botão `Criar credenciais` bem no centro da tela:

![botao criar credenciais](https://i.imgur.com/hf30xl2.png)

Esse botão vai mostrar algumas opções, dentre elas a que nós queremos se chama "Chave de API":

![opções de criar credenciais](https://i.imgur.com/EVNdvzv.png)

Ao clicar nela irá abrir uma janelinha com a sua chave (aqui editada por motivos de segurança):

![sua chave de API](https://i.imgur.com/vkQOqpR.png)

Uma coisa que você vai querer fazer aqui é clicar no botão de copiar que aparece no final da chave de API (afinal você provavelmente ta criando a chave para usá-la). Depois é clicar em fechar para voltar para o painel de credenciais que agora tem uma lista de um item yaaayy \o/ e deve ser algo mais ou menos assim:

![painel com uma chave](https://i.imgur.com/09LaYVn.png)

## Ativando uma API
Agora o próximo passo é escolher uma API na biblioteca das APIs disponíveis. Comece escolhendo a opção `Bibliotecas` naquele menu lateral esquerdo:

![menu a esquerda: bibliotecas](https://i.imgur.com/WxgwfN8.png)

Você pode passear pelas bibliotecas disponíveis nesse painel que irá aparecer:

![painel de bibliotecas](https://i.imgur.com/NotZpSU.png)

Ou procurar por uma API específica, nesse caso a do YouTube:

![procurando a API do YouTube](https://i.imgur.com/KAoj2Wf.png)

Eu escolhi aqui a API de dados do youtube (a primeira da lista acima), e cliquei nela, o que me levou para a seguinte página:

![YouTube data API](https://i.imgur.com/Ej1gxu7.png)

Nessa página encontramos algumas informações sobre essa API, incluindo links para tutoriais de como utilizá-la. Mas o que nos interessa de verdade nessa página é clicar no botão de ativar:

![clicar em ativar](https://i.imgur.com/P9LMurv.png)

Ao clicar em ativar, somos levados para o painel da API, é nesse painel que você acompanha várias métricas do uso dessa API.

![painel de uso da API](https://i.imgur.com/7fboIJQ.png)

## The end

Sim, cabou já da pra usar :P

---
## Perguntas

1. **Posso usar uma chave só para várias APIs?**
Pode! Não vai dar problema nenhum. Afinal quem decide qual API usar é a sua aplicação/ seu programa ;)

1. **Se eu posso usar a mesma chave pra várias APIs pra que eu vou criar outras?**
Vamos supor que você tem vários scripts/projetos/apps que fazem requisições para APIs do google, facilita um pouco a vida se você conseguir apagar uma chave de um app que você não quer que tenha acesso a API do google não é mesmo?

1. **Posso mandar minha chave de API pra um coleguinha?**
Poder você pode, mas não deve né? Importante cuidar da segurança, já imaginou se esse coleguinha começa a fazer requisições sem controle pra API do google? A conta do cartão vai chegar alta no fim do mês.

1. **Eu não preciso me cadastrar em nada?**
Não, acessando o console estando logado na sua conta do Gmail você já tem acesso a parte free das APIs.

----
## Agradecimentos
Ao querido Cássio Botaro que deu essa ideia de tutorial <3
