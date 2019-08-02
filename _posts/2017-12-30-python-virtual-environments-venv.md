---
title: 'Python virtual environments: venv'
layout: post
date: 2017-12-30 00:00:00
img: "/pro_tip.png"
tags:
- english
- virtual environment
- virtual environments
- versions
- protip
- pro tip
- python
- python 3
- python3
- venv
- virtualenv
comments: true
subtitle: Learn how to create virtual environments using Python's module venv

---
***

Nota da autora: Se preferir leia [esse texto em Português](https://jtemporal.com/venv-inicio/).

***

After learning that [Python](https://www.python.org/) has several versions, and after learning [how to install any of them using pyenv](https://jtemporal.com/python-and-it-s-versions/), today's tip teaches you the next step: how to use [venv](https://docs.python.org/3/library/venv.html) to create virtual environments with Python 3.

At the launch of Python 3.3 venv was announced as a new language module, and in version 3.6, venv became the official choice for creating Python virtual environments with the [deprecation of the pyenv script](https://docs.python.org/dev/whatsnew/3.6.html#id8).

History of how venv came to life aside, let's go to what matters. You can use any version equal to or higher than Python's 3.3 that the commands below should work  `¯\_(ツ)_/¯`.

To create a virtual environment:

``` console
$ python -m venv myenv
```

To activate the environment:

``` console
$ source myenv/bin/activate
```

Now pay attention to one thing: this way of creating virtual environments doesn't allow you to create environments to other Python versions than the one you are already using.

``` console
$ python --version
# Python 3.3.0
```

Here I was using Python 3.3.0, so my virtual env has that version. That's when having _pyenv_ installed is a good thing! You only have to use pyenv and switch to the version you want then create the environment. Take a look:

``` console
$ python --version
# Python 3.3.0
$ pyenv local 3.6.4
$ python --version
# Python 3.6.4
$ python -m venv meuoutroenv
$ source meuoutroenv/bin/activate
```

One of the advantages of having one module of this kind built-in the language is that you don't need to install anything else to create your virtual environments. To the people that avoid installing things on the computer like myself, this is fantastic!

Now that you have your virtual environment created you can go ahead, `pip install` all things and start coding. ☺️

***

## Links

* [Highlights of Python 3.3 version release](https://docs.python.org/dev/whatsnew/3.3.html#summary-release-highlights "Highlights of Python 3.3 version release")