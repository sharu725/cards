---
title: "Pegando o começo de uma string com Go ou Python"
layout: post
image: "/images/colinha.png"
date: '2018-05-16 05:00:00'
tags:
- colinha
- go
- gophers
- golang
- strings
- string
- python
- português
comments: true
---

A colinha de hoje mostra como pegar o começo de uma string tanto em Go como em Python sem usar expressões regulares 🎉

Vamos supor que você tem a seguinte string: `"Gopher"` e você quer saber se ela começa com `"Go"` e também se começa com `"C"`. Em Go:

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.HasPrefix("Gopher", "Go"))
	fmt.Println(strings.HasPrefix("Gopher", "C"))
}
```

e em Python:

```python
print('Gopher'.startswith('Go'))
print('Gopher'.startswith('C'))
```

Os dois exemplos vão printar "true" para a primeira e "false" para segunda em ambas linguagens.

Curiosamente, se você quiser saber se a sua string começa com `""` (string vazia) ambas linguagens vão retornar "true" também.

Em go:

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.HasPrefix("Gopher", ""))
}
```

Em Python:

```python
print('Gopher'.startswith(''))
```

Importante saber disso para evitar ou cair propositalmente em casos de borda.

Massa né? Partiu aprender outras linguagens  😜

---

## Links
- [Python str.startswith](https://docs.python.org/3/library/stdtypes.html#str.startswith)
- [Go strings.HasPrefix](https://golang.org/pkg/strings/#HasPrefix)
- [Implementação do strings.HasPrefix](https://github.com/golang/go/blob/4102e6ff56eee8fd6a1689f4bcf9d5a92cc44a6c/src/strings/strings.go#L450)
- [Se quiser ver o Cesar explicando tudo isso em vídeo dá uma olhada nesse post do grupo de estudos de Go](https://gopher.pro.br/post/hangout-hasprefix/)

## Agradecimentos
Principalmente ao Cesar que pacientemente tem me ensinado Go.
