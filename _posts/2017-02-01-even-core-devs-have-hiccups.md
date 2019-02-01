---
title: "Even core devs have hiccups ¯\\_(ツ)_/¯"
layout: post
date: '2017-02-01 00:00:00'
img: click-2.png
tags:
- postgres
- python
- serenata
- serenata de amor
- banco de dados
- inglês
comments: true
---

After a few months working on a open-source project sometimes I still find myself having trouble with some things. And that is okay! This time was with PostgreSQL.

As some of you know I work on a project called Serenata de Amor Operation. One of the tools we developed is called Jarbas. It is a web application with a Django backend using PostgreSQL for database. So the last time I configured and ran Jarbas was last year on a Linux computer it also was the first time I contributed for the project. Not much mystery there, but now I'm on a MacOS machine and things are a bit different.

---

So I started by installing PostgreSQL with brew (which I highly recommend). Homebrew makes the installation process pretty straightforward. The command line for it is:

```console
$ brew install postgresql
```

You may think that's it. Easy peasy. Then just go to Jarbas cloned repository and the first thing to do would be creating a `.env` file and then running the traditional Django command `python manage.py migrate`. And that's where I got the **first error**:

```
$ python manage.py migrate
Traceback (most recent call last):
File “/Users/temporal/anaconda3/envs/jarbas/lib/python3.5/site-packages/django/db/backends/base/base.py”, line 213, in ensure_connection
self.connect()
File “/Users/temporal/anaconda3/envs/jarbas/lib/python3.5/site-packages/django/db/backends/base/base.py”, line 189, in connect
self.connection = self.get_new_connection(conn_params)
File “/Users/temporal/anaconda3/envs/jarbas/lib/python3.5/site-packages/django/db/backends/postgresql/base.py”, line 176, in get_new_connection
connection = Database.connect(**conn_params)
File “/Users/temporal/anaconda3/envs/jarbas/lib/python3.5/site-packages/psycopg2/__init__.py”, line 130, in connect
conn = _connect(dsn, connection_factory=connection_factory, **kwasync)
psycopg2.OperationalError: could not connect to server: Connection refused
Is the server running on host “localhost” (::1) and accepting
TCP/IP connections on port 5432?
could not connect to server: Connection refused
Is the server running on host “localhost” (127.0.0.1) and accepting
TCP/IP connections on port 5432?
```

That's when you ask yourself: _Wait, what? What kind of error is that_?

<center>
![travolta perdido gif](https://media.giphy.com/media/6uGhT1O4sxpi8/giphy.gif)
</center>

After a few minutes searching I came across this [wonderful Stackoverflow thread](https://stackoverflow.com/a/28249245). I'll break it down for you: After you install PostgreSQL probably (like me), you'll have to start the service **and** initialize a database for you to use. I won't copy here the steps described on the thread but there you'll see it gives you two choices at a given moment:

 - Either load the launch agent;
 - Start the agent.

I chose to start it since it was my first time doing this it was logical that this was a safe choice to go on.

After starting it, I needed to check whether it worked or not. _You never now util you test it right?_ And to do so, we try to connect into PostgreSQL by doing this:

```console
$ psql -d postgres
```

That only gave me a running Postgres client I still needed a database.

Jarbas app expect a database called `jarbas`. So that's the name we give when creating the new database by doing:

```console
$ createdb jarbas
```

As you might expect, that wasn't enough once more. Tried again to run the migrate command and this happened: `django.db.utils.OperationalError: FATAL: role "Jarbas" does not exist.`

_Don't you just love dev life?!_

Let's create a Role then:

```
$ psql -d postgres
=> CREATE ROLE jarbas;
```

And once again got an error: `django.db.utils.OperationalError: FATAL: role "jarbas" is not permitted to log in`. Now we need to give the role permissions, why that didn't cross my mind before?

```
$ psql -d postgres
=> ALTER ROLE jarbas LOGIN;
```

Again with the migrate command and voilà:

<center>
![all ok](https://i.imgur.com/2nhYU2P.png)
</center>

And then you get to the point where you might think: _How the hell am I supposed to know if everything is working properly?_ Well, Jarbas has test suites so you might try to run tests and they will fail:

```
$ python manage.py test                                         
Creating test database for alias 'default'...
Got an error creating the test database: permission denied to create database

Type 'yes' if you would like to try deleting the test database 'test_jarbas', or 'no' to cancel: no
Tests cancelled.
```

You'll need to make another modification on jarbas role to give it permissions to create a database:

After that, tests beautifully pass:

<center>
![jarbas tests passing gif](https://media.giphy.com/media/xUA7bjcqhnBpgOvHig/giphy.gif)
</center>
