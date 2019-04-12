---
layout: post
title: How to define the optimal number of clusters for KMeans
date: 2019-04-10 00:00:00 -0300
img: "/tutorial.png"
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

So to begin, we need data! We will use [the Iris dataset](https://en.wikipedia.org/wiki/Iris_flower_data_set). There's a genre of flowers called Iris, it is a group of around 300 flower species with different petal and sepal sizes. Biological curiosity aside, the dataset is used to demonstrate how machine learning  and clustering algorithms work a lot. I mean A LOT!

This dataset holds 150 samples of three Iris species ([_Iris setosa_](https://en.wikipedia.org/wiki/Iris_setosa "Iris setosa"), [_Iris virginica_](https://en.wikipedia.org/wiki/Iris_virginica "Iris virginica") and [_Iris versicolor_](https://en.wikipedia.org/wiki/Iris_versicolor "Iris versicolor")) that even though are very similar,  are distinguishable using a model developed by the biologist and statistician Ronald Fisher.

This dataset is so widely used that data science libraries usually have it built-in so it can be used in demonstrations and examples. Let's take a peek at the first lines of our dataset, to start we need to import it, I prefer the version that comes in [the seaborn library](https://seaborn.pydata.org/):

    import seaborn as sns
    iris = sns.load_dataset('iris')

Glancing at the first 5 rows of our dataset you might ask me: _"But Jess, the answers were are seeking is right there in the_ `_species_` _column, weren't we going to do unsupervised clustering?"_

![](https://cdn-images-1.medium.com/max/800/1*rLqNbyrHGZs7NpE6dBl3qg.png "dataset.head")

First 5 rows of the Iris dataset, the result of the `iris.head()` command

Okay, yes! We have the answers in our dataset but for the purpose of this tutorial, we are going to forget that we already know the species of our samples and we are going to try to group them using KMeans.

## KMeans

If you aren't familiarized with KMeans yet, you must know that to use this method you'll need to inform an initial amount of clusters before you start. Thinking of the version implemented in scikit-learn in particular, if you don't inform an initial number of clusters by default it will try to find 8 distinct groups.

When we don't know how many clusters our samples are going to form we need a way to validate what is being encountered instead of just guessing.

## The elbow method

And that's where the Elbow method comes into action. The idea is to run KMeans for many different amounts of clusters and say which one of those amounts is the optimal number of clusters. What usually happens is that as we increase the quantities of clusters the differences between clusters gets smaller while the differences between samples inside clusters increase as well. So the goal is to find a balance point in which the samples in a group are the most homogeneous possible and the clusters are the most different from one another.