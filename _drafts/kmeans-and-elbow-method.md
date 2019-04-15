---
layout: post
title: How to define the optimal number of clusters for KMeans
date: 2019-04-13 00:00:00 -0300
img: "/tutorial.png"
comments: true
tags:
- tutorial
- data science
- ciencia de dados
- kmeans
- cluster
- clusters
- clusterizacao
- clustering
- clusterization
- unsupervised learning
- unsupervised
- unsupervised clustering
- analysis
- nao supervisionado

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

![](https://cdn-images-1.medium.com/max/800/1*rLqNbyrHGZs7NpE6dBl3qg.png "dataset.head") <center><i>First 5 rows of the Iris dataset, the result of the `iris.head()` command</i></center>

Okay, yes! We have the answers in our dataset but for the purpose of this tutorial, we are going to forget that we already know the species of our samples and we are going to try to group them using KMeans.

## KMeans

If you aren't familiarized with KMeans yet, you must know that to use this method you'll need to inform an initial amount of clusters before you start. Thinking of the version implemented in scikit-learn in particular, if you don't inform an initial number of clusters by default it will try to find 8 distinct groups.

When we don't know how many clusters our samples are going to form we need a way to validate what is being encountered instead of just guessing.

## The elbow method

And that's where the Elbow method comes into action. The idea is to run KMeans for many different amounts of clusters and say which one of those amounts is the **optimal number of clusters**. What usually happens is that as we increase the quantities of clusters the differences between clusters gets smaller while the differences between samples inside clusters increase as well. So the goal is to find a balance point in which the samples in a group are the most homogeneous possible and the clusters are the most different from one another.

Since KMeans calculates the distances between samples and the center of the cluster from which sample belongs, the ideal is that this distance is the smallest possible. Mathematically speaking we are searching for a number of groups that the within clusters sum of squares (`wcss`) is closest to `0`, being zero the optimal result.

## Usando o scikit-learn

Scikit-learn's KMeans already calculates the `wcss` and its named `inertia`. There are two negative points to be considered when we talk about inertia: 

1. Inertia is a metric that assumes that your clusters are convex and isotropic, which means that if your clusters have alongated or irregular shapes this is a bad metric;
2. Also, the inertia isn't normalized, so if you have space with many dimensions you'll probably face the "dimensionality curse" since the distances tend to [get inflated in multidimensional spaces](https://scikit-learn.org/stable/modules/clustering.html#k-means).

Now that you are aware of all that, let's look at our data: we have only 4 dimensions - length and width of petals and sepals, let's use the elbow with our data, shall we? I wrote a function that receives a dataset as input and calculates the KMeans for 19 different amounts of clusters ranging from 2 to 20 possible groups and finally returns a list with our `wcss`:

    def calculate_wcss(data):
        wcss = []
        for n in range(2, 21):
            kmeans = KMeans(n_clusters=n)
            kmeans.fit(X=data)
            wcss.append(kmeans.inertia_)
    
    return wcss

When we use the function written above to calculate the within clusters sum-of-squares for our Iris data and plot the result, we find a plot like this:

![](https://cdn-images-1.medium.com/max/800/1*BeBON5cT5jXuTvXRJ8GhTw.png)

Cada ponto laranja é uma quantidade de clusters, note que começamos em 2 e vamos até 20 clusters. E aí pode vir a primeira dúvida: _Qual desses pontos é o que simboliza de fato a quantidade ótima de clusters? Será que é o a2, ou a3 ou até mesmo o a4?_

#### 