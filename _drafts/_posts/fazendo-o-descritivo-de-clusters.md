---
layout: post
title: Fazendo o descritivo de clusters
date: 2019-02-21 03:00:00 +0000
img: "/images/colinha.png"
comments: true
tags:
- colinha
published: false

---
lorem

    data_grouped.describe()
    filtro = data_grouped.describe().columns.get_level_values(1).isin(['mean', 'std', 'count'])
    data_grouped.describe().iloc[:, filtro]

lorem