---
layout: post
title: Playing with Docker’s container listing
date: 2019-03-16 03:00:00 +0000
img: "/pro_tip.png"
comments: true
tags:
- 'docker '
- container
- listing
- protip
- pro tip
- docker ps
- containers
subtitle: Learn to list and filter containers from the list

---
If you use Docker, probably the third command you learned was to list containers, but do you know that you can tailor the container listing to your needs? Well, the hint today is to show two tricks I use a lot:

1. List filtering;
2. List formatting.

Let's take a look at these two cases with care.

## Filtering the container list

I think the first "advanced" command I had to learn was filtering a set of containers. Soon after getting started with Docker it is very common to have a "dirty" environment, that is, to have many containers with exited status `exited` without being removed.

So I learned how to filter such containers by their status. Within the traditional container listing command (`docker ps`), there is a flag called `--filter` or `-f`. This flag allows you to indicate filters to be made, for example:

    docker ps -a -f status=exited

The result of this command will be a list of containers that have stopped running, but have not been removed. As I've said before, it's pretty easy to lose control and have lots of stale containers "dirtying" your environment. So listing those that aren't running can be a helpful hand i removing them more easily. I like to use a command like this:

    docker rm -v $(docker ps -a -q -f status=exited)

Passing the `-q` flag causes the `docker ps` result to only show the container IDs, putting this complete command inside `$()` results in us passing the list of IDs to the `docker rm` and thus removing all the containers stopped with only one command. And the flag `-v` there is just to have feedback of what is going on with the command, it will show the ID of each container being deleted.

## Formating the containers list

In addition to removing the containers that are dirting up our environment, sometimes I need some information about some of the containers that are running. Normally when we use the `docker ps` we see information like container ID, the command you ran to start the container, the image being used and a lot of other things... But sometimes seeing all that information on the screen all at once can be an information overload.

That's when the magic of formatting the `docker ps` result comes in handy. The flag `--format` is the show this time, it accepts a [template Go](https://golang.org/pkg/text/template/) . If you do not know Go templates, here's a super quick and superficial explanation: used for creating static sites, a template go is a string that is "filled" with information that is stored in variables.

For example, the template:

<script src="https://gist.github.com/jtemporal/13ca6f547d5ab2f86b4f3b019fb26c43.js"></script>

That template could be filled with any name that was stored in the variable `Name`. To identify variables in templates you'll need to find the word that is preceded by a dot and is inside double curly brackets. If you are interested in learning more about Go templates, I'd recommend [you check out this article in the official documentation](https://golang.org/pkg/text/template/) for more information.

To start our formatting we need to know where the information that shows up on the screen is stored and you can use the table bellow for that:

| Information | Variable |
| --- | --- |
| CONTAINER ID | ID |
| IMAGE | Image |
| COMMAND | Command |
| CREATED | RunningFor |
| Status | Status |
| PORTS | Ports |
| NAMES | Names |

In my case I usually look for the name, the image and the container ID's so my command ends up being the following:

<script src="https://gist.github.com/jtemporal/6ba7e2a2ac369738bb8278ad58993161.js"></script>

Will give me a result like this:

        CONTAINER ID        IMAGE                          NAMES
        11b8af1aeb43        jupyter/datascience-notebook   relaxed_hypatia

***

Cool Huh? So have you listed your containers today?