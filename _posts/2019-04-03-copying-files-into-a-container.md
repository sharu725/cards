---
layout: post
title: Copying files into a container
date: 2019-04-03 11:00:00 -0300
img: "/pro_tip.png"
comments: true
tags:
- pro tip
- docker
- container
- containers
- pro tip
- listing
- image
subtitle: Let's learn to copy files into a container without using volumes

---
***

Se preferir [leia esse texto em Português](https://jtemporal.com/copiando-arquivos-para-dentro-do-container/).

***

Sometimes volumes do not work and you have to copy things into the container. Really! You must be wondering, _"how does a technology that everyone uses, doesn’t work?!”_.

Okay, okay, I know this is looking like those stories of "works on my machine" inverted. But let me explain, earlier this year I was working with a temporary computer. Unfortunately, I did not have administrator powers of that computer, which prevented me from doing certain things, including giving Docker permission to share volumes with the Windows file system.

Is worth mentioning that I only discovered how to copy files to a running container because windows have these permission issues.

So, let's go!

## Materials

For this little tutorial, you’ll need:

* A running Docker container
* A file to copy to said Docker containerr
* Basic Docker knowledge

## Finding out the container

Depending on how you started your container you won't know its name, so let's first check it out. Since I work with science and data analysis, it is very common to find myself with a Jupyter container running, this is the one I will use here. To get the name of the container we need to list the containers that are up:

<script src="https://gist.github.com/jtemporal/6ba7e2a2ac369738bb8278ad58993161.js"></script>

In my case this command shows a result like this:

    CONTAINER ID        IMAGE                          NAMES
    11b8af1aeb43        jupyter/datascience-notebook   relaxed_hypatia

I explained how to assemble this command that only shows the ID of the container, the image being used and the name of the container in [this other pro tip](https://jtemporal.com/brincando-com-a-listagem-de-containers-docker/). With the result above we know that the container I'm interested in is called `relaxed_hypatia`

## Finding out where to send the file

Well if, like me, you usually map volumes to share the data with your container you probably already know where to send the data. However, if you don't do this or it is a new image that you are using for the first time, your mission is to find out where to send the files. Usually, this information is in the documentation of the image.

For Jupyter Project images, there is a traditional folder to map the volumes that this: `/home/jovyan/work`. I'll send the file there if you are using a different image remember to replace that path, to the path of your container.

## Finally copying the file

Now that you've figured out where to send the data file and the name of the container, it's time to finally copy the file to the container. If you like play copying files back and forth through the terminal, you should have the custom of using the `cp` command. But if you don't have the habit, `cp` (from _copy_) is Ctrl+C Ctrl+V from the terminal, it makes a copy of a file from somewhere to another place. For example, let's assume I have the following situation:

    folder1/                        folder2/
    └── file_A.txt		

And that I want to copy `file_A.txt` to `folder2` using the terminal. I could do the following:

    cp folder1/file_A.txt folder2

And the result would be as follows:

    folder1/                        folder2/
    └── file_A.txt                  └── file_A.txt

And the same can be done with containers. Docker has its own `cp` version called `docker cp` that works in a way analogous to the terminal `cp`. With the small difference that the destination path is formed like this: `container_name:destination/path`. So let's copy the `dados.csv` file into my `relaxed_hypatia`:

    docker cp dados.csv relaxed_hypatia:/home/jovyan/work/dados.csv

Now I can see the data inside my container. As in my case I'm running a Jupyter container I can see this file there in the Jupyter interface:

<center>
<img src="/images/dados_docker_cp.png"/>
</center>

Cool huh? You can follow the same philosophy to pull data from inside the container to your local machine, just reverse the order of the paths.

## Final considerations

One very important thing to remember, containers are meant to be ephemeral, so make sure you keep a backup of your data. After all, after removing a container, you no longer have the ability to retrieve the data that was inside.

Another reason a friend recently taught me in favor of copying the data into the container is that keeping volumes up-to-date when your container does a lot of reading and writing is extremely costly. So if you have a large file that does not change, like a data file, or a process that does a lot of reading and writing, like a rails server, it's worth considering whether to copy the files into the container or even make an image already with these files. Always remember to weigh the points in favor and points against volumes the next time you use containers.

***

That's all folks! xoxo