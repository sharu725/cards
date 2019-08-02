---
title: 'Python and it''s versions: pyenv'
layout: post
date: 2017-12-29 10:00:00
img: "/pro_tip.png"
tags:
- english
- versions
- protip
- python
- pyenv
comments: true
subtitle: Learn how to install manage Python versions using pyenv

---
One of the first things we learn in [Python](https://www.python.org/) is that there is more than one version of the same language working side by side. That can bring a few problems and the inevitable question: _"Which version should I choose?"_. Today's tip teaches you a way to install and manage many Python versions on your machine using [pyenv](https://github.com/pyenv/pyenv) 😉.

Amongst other things, the two best features of pyenv in my humble opinion are:

1. install new Python versions easily;
2. choose the global Python version.

![sombrancelhas gif](https://media.giphy.com/media/10lqVdCCc9812M/giphy.gif)

To start you'll need to install pyenv, here I'll demonstrate one of several installation methods, but in the project's _readme_ file [you can find other possibilities](https://github.com/pyenv/pyenv#installation) (and also operational system related details):

``` console
$ git clone https://github.com/pyenv/pyenv.git ~/.pyenv
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
$ exec "$SHELL"
```

After these easy steps, other versions of Python are an install away:

``` console
$ pyenv install 3.6.4
```

When in doubt of which version you can install, just use the install flag `-l` that it lists all versions available:Se estiver em dúvida quais versões você pode instalar só usar a flag `-l` do _install_ que ele vai listar todas as disponíveis:

``` console
$ pyenv install -l
```

Cool huh? Now you can install version like crazy 😂

***