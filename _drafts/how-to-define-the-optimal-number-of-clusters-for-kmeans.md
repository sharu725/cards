---
layout: post
title: How to define the optimal number of clusters for KMeans
date: 2019-04-10 00:00:00 -0300
img: "/images/tutorial.png"
comments: true
tags:
- tutorial
- travis
- " docker "
- container
- containers
- ci
- ci cd
- cd
- continuous integration
- integração continua
- continous delivery
- entrega continua
- docker hub

---
One of the most famous methods to find clusters in data in an unsupervised way is using KMeans. But what do we do when we have absolutely no idea how many clusters the data is going to form? We can't just guess.

Discovering the number of clusters is a challenge especially when we are dealing with unsupervised machine learning and clustering algorithms. To solve the issue of "how many clusters should I choose" there's a method known as the Elbow Method.

The idea is pretty basic: define the optimal amount of clusters that can be found even though we don't know the answer in advance. Seems like magic, doesn't it? But I promise you it isn't.

***

The code that I'm going to use from now on [can be found in this GitHub repository](https://github.com/jtemporal/kmeans_e_cotovelo).

***

So to begin, we need data! We will use the Iris dataset. There's a genre of flowers called Iris, it is a group of around 300 flower species with different petal and sepal sizes. Biological curiosity aside, the dataset is used to demonstrate how machine learning  and clustering algorithms work a lot. I mean A LOT!

This dataset holds 150 samples of three Iris species (Iris setosa, Iris virginica and Iris versicolor) that even though are very similar,  are distinguishable using a model developed by the biologist and statistician Ronald Fisher.

This dataset is so widely used that data science libraries usually have it built-in so it can be used in demonstrations and examples. Let's take a peek at the first lines of our dataset, to start we need to import it, I prefer the version that comes in the seaborn library: