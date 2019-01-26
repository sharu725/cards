---
title: "O último da lista com Python"
layout: post
date: '2018-01-01 10:00:00'
tags:
- colinha
- python
- lista
- listas
- manipulação de listas
- português
comments: true
---

[Python](https://www.python.org/) é conhecida por facilitar a escrita de códigos bonitos e pequenos. A colinha de hoje é sobre como pegar o último elemento de uma lista usando essa linguagem \o/

Vindo de outras linguagens como Java ou C para o Python é normal trazer um pouco de sotaque para os seus códigos e querer fazer algo mais ou menos assim para pegar o último elemento de uma lista:

~~~ python
lista = [1, 2, 3, 4, 5]
indice = len(lista) - 1
print(indice)
# 4
ultimo = lista[indice]
print(ultimo)
# 5
~~~

Usar a função de tamanho `len()` para pegar o comprimeto da lista e subtrair `1` para obter o índice do último elemento daquela lista. E tudo bem! Isso funciona. Porém Python apresenta um jeito mais elegante de fazer isso, veja:

~~~ python
ultimo = lista[-1]
print(ultimo)
# 5
~~~

![magic gif](https://media.giphy.com/media/12NUbkX6p4xOO4/giphy.gif)

Apesar de parecer isso não é mágica não! Da [documentação do Python](https://docs.python.org/3/library/stdtypes.html#common-sequence-operations):

> If i or j is negative, the index is relative to the end of sequence _s_: `len(s) + i` or `len(s) + j` is substituted. But note that `-0` is still `0`.

De uma forma mais simples o [Raymond Hettinger](https://twitter.com/raymondh) explica nesse tweet com código:
<center>
<blockquote class="twitter-tweet" data-lang="pt"><p lang="en" dir="ltr"><a href="https://twitter.com/hashtag/python?src=hash&amp;ref_src=twsrc%5Etfw">#python</a> tip:  How to support negative indexing:<br><br>def __getitem__(self, i):<br>    if i &lt; 0:<br>        i += len(self)<br>    if i &lt; 0 or i &gt;= len(self):<br>        raise IndexError<br>    ...</p>&mdash; Raymond Hettinger (@raymondh) <a href="https://twitter.com/raymondh/status/943615980650971136?ref_src=twsrc%5Etfw">20 de dezembro de 2017</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
</center>

A implementação de fato é mais complexa que essa, mas o que acontece em linhas gerais: o método `__getitem__` é invocado quando fazemos a chamada `lista[-1]` recebendo a própria lista (`self`) e o índice negativo (`i`) então soma esse índice com o tamanho da lista e retornando o valor do indice atualizado. Curiosamente é o mesmo que fiz no primeiro exemplo porém já vem implementado de fábrica.

E detalhe, o mesmo pode ser feito para strings!

~~~ python
palavra = 'opaopa'
print(palavra[-1])
# a
~~~

Massa né? Agora é só usar índice negativo nos seus códigos também  😉

----
## Links
- Para mais detalhes do slice de strings olhe essa [Introdução informal de Python em inglês](https://docs.python.org/3.6/tutorial/introduction.html)
- Referência ao caso especial de índices negativos [no método `__getitem__` na documentação do Python](ttps://docs.python.org/3/reference/datamodel.html#object.__getitem__)

## Agradecimentos
Mário Sérgio, Diego Ponciano e Paulo Haddad por alguns dos links nesse post!
