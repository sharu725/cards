---
layout: post
title: How to define the optimal number of clusters for KMeans
date: 2019-04-13 03:00:00 +0000
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

![](https://cdn-images-1.medium.com/max/800/1*rLqNbyrHGZs7NpE6dBl3qg.png "dataset.head") <center><br><i>First 5 rows of the Iris dataset, the result of the `iris.head()` command</i></center>

Okay, yes! We have the answers in our dataset but for the purpose of this tutorial, we are going to forget that we already know the species of our samples and we are going to try to group them using KMeans.

## KMeans

If you aren't familiarized with KMeans yet, you must know that to use this method you'll need to inform an initial amount of clusters before you start. Thinking of the version implemented in scikit-learn in particular, if you don't inform an initial number of clusters by default it will try to find 8 distinct groups.

When we don't know how many clusters our samples are going to form we need a way to validate what is being encountered instead of just guessing.

## The elbow method

And that's where the Elbow method comes into action. The idea is to run KMeans for many different amounts of clusters and say which one of those amounts is the **optimal number of clusters**. What usually happens is that as we increase the quantities of clusters the differences between clusters gets smaller while the differences between samples inside clusters increase as well. So the goal is to find a balance point in which the samples in a group are the most homogeneous possible and the clusters are the most different from one another.

Since KMeans calculates the distances between samples and the center of the cluster from which sample belongs, the ideal is that this distance is the smallest possible. Mathematically speaking we are searching for a number of groups that the within clusters sum of squares (`wcss`) is closest to `0`, being zero the optimal result.

## Using scikit-learn

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

When we use the function written above to calculate the within clusters sum-of-squares for our Iris dataset and plot the result, we find a plot like this:

![](https://cdn-images-1.medium.com/max/800/1*BeBON5cT5jXuTvXRJ8GhTw.png)

<center><i><small>on the x axis: the number of clusters used in the KMeans, and on the y axis: the within clusters sum-of-squares</small></i></center>

Each orange point is a cluster quantity, note that we start in two and go up to 20 clusters. And there where we might get the first doubt: _Which of these points is exactly the optimal cluster amount? Is it the point a2 or a3 or even the a4?_

## May math save us all

What if I told you that there's a mathematical formula to helps us out?

<center>
<img src="https://cdn-images-1.medium.com/max/800/1*1qNRC20LzjzP5C6MNsfiVQ.gif" />
</center>

It turns out that the point that indicates the balance between greater homogeneity within the cluster and the greater difference between clusters is the point in the curve that is most distant of a line drawn between points `a0` and `a18`. And guess what! There is a formula that calculates the distance between a point and a line! And it is this one below:

<center> <br><img src="https://cdn-images-1.medium.com/max/800/1*9J7Wnh5L0eIcHXBeWlzvNA.png"/> <small><br><i>Formula that calculates the distance between a point and a line that passes through P0 and P1</i></small> </center>

Não se assuste, no nosso caso _P0_ é o nosso _a0_ e o _P1_ é o nosso _a18_, veja:

![](https://cdn-images-1.medium.com/max/800/1*5VTBI6T5c7De-GtKTBaNNA.png)

e o par `(x,y)` representa as coordenadas de qualquer ponto que a gente queira calcular a distância. Vamos olhar mais uma vez para o nosso gráfico de cotovelo:

![](https://cdn-images-1.medium.com/max/800/1*5AILcLRFN7UzhLCKw6MaYQ.png)

Suponha que queremos calcular a distância entre o ponto `a1` e a reta que passa por `a0`e `a18`, aqui as informações que precisamos:

![](https://cdn-images-1.medium.com/max/800/1*ifUwiwpotxsqPaRp8q8E3w.png)

Substituindo os valores acima na fórmula do cálculo da distância temos o seguinte:

![](https://cdn-images-1.medium.com/max/1200/1*DYmZL126BCy2xJNPVtoTbg.png)

![](https://cdn-images-1.medium.com/max/800/1*MhxkB7f42ajRkgJfTa7TAw.png)

#### Matemática e Python, o casalzão que todo mundo ama

Imagino que você concorda comigo quando eu digo que _“Ninguém merece ficar fazendo essas continhas na mão”._ Então vamos usar um método pra isso, em resumo é só transcrever a fórmula do cálculo da distância entre um ponto e uma reta para código, ficou assim:

Esse método `optimal_number_of_clusters()` recebe uma lista contendo as somas dos quadrados para cada quantidade de clusters que calculamos usando `calculate_wcss()` e, como resultado, retorna a quantidade ótima de clusters. Agora que sabemos como calcular a quantidade ótima de clusters podemos finalmente usar o KMeans com tranquilidade:

#### Comparando antes e depois

Depois de _clusterizar_ os dados devemos dar uma olhada nos descritivos estatísticos de cada cluster:

![](https://cdn-images-1.medium.com/max/1200/1*6b4p4aw1EMAp00xlEsFpfA.png)

Descritivo estatístico agrupado por cluster

Nada mal se compararmos com o descritivos dos dados originais que coloquei aqui em baixo né?

![](https://cdn-images-1.medium.com/max/1200/1*6LLgj9Jgz44Z7rZegS9V_w.png)

Descritivo estatístico agrupado por espécie

#### Conclusão

Óbvio que nenhuma clusterização vai ter 100% de acurácia principalmente quando nós temos clusters possuem características que são bem similares umas das outras. Eu particularmente gosto bastante da versão visual da clusterização. Vamos colocar as duas _features_ de pétala no gráfico e colorir de acordo com os clusters — e também de acordo com as espécies:

![](https://cdn-images-1.medium.com/max/800/1*Rnz35jPaqoARWQYZir_5iQ.png)

Conseguimos ver no gráfico que aquelas 12 observações que “mudaram” de classificação poderia fazer facilmente parte de qualquer um dos clusters.

Por fim, ainda vale salientar que o método do cotovelo não é a única forma de dizer a quantidade ótima de clusters. Existe ainda uma forma usando, por exemplo, o [_coeficiente de silhueta_](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.silhouette_score.html). Mas hoje, ficamos por aqui.

Xêro e boa _clusterização_.