---
title: 'Zelfstudie: Clustering-model in R bouwen'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: In deel twee van deze driedelige zelfstudie bouwt u een model K-Means om uit te voeren clustering in R met Machine Learning Services (preview) van Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 05/17/2019
ms.openlocfilehash: 12738b63be92420c5f3afea6c133522cbd97f849
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66419760"
---
# <a name="tutorial-build-a-clustering-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Zelfstudie: Clustering-model in R met Machine Learning Services (preview) van Azure SQL Database maken

In deel twee van deze driedelige zelfstudie bouwt u een model K-Means om uit te voeren clustering in R met Machine Learning Services (preview) van Azure SQL Database.

In dit artikel leert u hoe u:

> [!div class="checklist"]
> * Het aantal clusters voor een algoritme K-Means definiëren
> * Clustering uitvoeren
> * Analyseer de resultaten

In [deel één](sql-database-tutorial-clustering-model-prepare-data.md), hebt u geleerd hoe u het voorbereiden van de gegevens van een Azure SQL-database om uit te voeren clustering in R.

In [deel drie](sql-database-tutorial-clustering-model-deploy.md), leert u hoe u een opgeslagen procedure maken in een Azure SQL-database die clustering op basis van nieuwe gegevens kunt uitvoeren.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Vereisten

* Deel twee van deze zelfstudie wordt ervan uitgegaan dat u hebt [ **deel één** ](sql-database-tutorial-clustering-model-prepare-data.md) en de vereisten.

## <a name="define-the-number-of-clusters"></a>Het aantal clusters definiëren

Als u wilt clusteren uw klantgegevens, gebruikt u de **K-Means** clustering-algoritme, de eenvoudigste en meest bekende manieren gegevens te groeperen.
U kunt meer lezen over K-Means in [een volledige handleiding voor het K-means clustering-algoritme](https://www.kdnuggets.com/2019/05/guide-k-means-clustering-algorithm.html).

Het algoritme accepteert twee invoeren: De gegevens zelf, en een vooraf gedefinieerd aantal '*k*' het aantal clusters te genereren.
De uitvoer is *k* clusters met de ingevoerde gegevens tussen de clusters gepartitioneerd.

Om te bepalen het aantal clusters voor de algoritme te gebruiken, gebruikt u een diagram van de binnen groepen som van de kwadraten, door het aantal clusters die zijn geëxtraheerd. Het juiste aantal clusters te gebruiken is bij de buigen of 'hoek' van de grafiek.

```r
# Determine number of clusters by using a plot of the within groups sum of squares,
# by number of clusters extracted. 
wss <- (nrow(customer_data) - 1) * sum(apply(customer_data, 2, var))
for (i in 2:20)
    wss[i] <- sum(kmeans(customer_data, centers = i)$withinss)
plot(1:20, wss, type = "b", xlab = "Number of Clusters", ylab = "Within groups sum of squares")
```

![Hoek graph](./media/sql-database-tutorial-clustering-model-build/elbow-graph.png)

Op basis van de grafiek, het ziet eruit als *k = 4* zou zijn een goede waarde om te proberen. Dat *k* waarde de klanten in vier clusters worden gegroepeerd.

## <a name="perform-clustering"></a>Clustering uitvoeren

In het volgende R-script, gebruikt u de functie **rxKmeans**, dit is de functie K-Means in het pakket RevoScaleR.

```r
# Output table to hold the customer group mappings.
# This is a table where the cluster mappings will be saved in the database.
return_cluster = RxSqlServerData(table = "return_cluster", connectionString = connStr);
# Set the seed for the random number generator for predictability
set.seed(10);
# Generate clusters using rxKmeans and output key / cluster to a table in SQL database
# called return_cluster
clust <- rxKmeans( ~ orderRatio + itemsRatio + monetaryRatio + frequency,
                   customer_returns,
                   numClusters=4,
                   outFile=return_cluster,
                   outColName="cluster",
                   extraVarsToWrite=c("customer"),
                   overwrite=TRUE);

# Read the customer returns cluster table from the database
customer_cluster <- rxDataStep(return_cluster);
```

## <a name="analyze-the-results"></a>Analyseer de resultaten

Nu dat u hebt gedaan de K-Means met clustering, wordt de volgende stap is het resultaat analyseren en kijk of u geen bruikbare informatie kunt vinden.

De **clust** -object bevat de resultaten van de K-Means clustering.

```r
#Look at the clustering details to analyze results
clust
```

```results
Call:
rxKmeans(formula = ~orderRatio + itemsRatio + monetaryRatio + 
    frequency, data = customer_returns, outFile = return_cluster, 
    outColName = "cluster", extraVarsToWrite = c("customer"), 
    overwrite = TRUE, numClusters = 4)
Data: customer_returns
Number of valid observations: 37336
Number of missing observations: 0 
Clustering algorithm:  

K-means clustering with 4 clusters of sizes 31675, 671, 2851, 2139
Cluster means:
   orderRatio   itemsRatio monetaryRatio frequency
1 0.000000000 0.0000000000    0.00000000  0.000000
2 0.007451565 0.0000000000    0.04449653  4.271237
3 1.008067345 0.2707821817    0.49515232  1.031568
4 0.000000000 0.0004675082    0.10858272  1.186068
Within cluster sum of squares by cluster:
         1          2          3          4
    0.0000  1329.0160 18561.3157   363.2188
```

De middelen vier cluster zijn opgegeven met behulp van de variabelen gedefinieerd in [deel één](sql-database-tutorial-clustering-model-prepare-data.md#separate-customers):

* *orderRatio* geretourneerde volgorde verhouding (totaal aantal orders volledig of gedeeltelijk geretourneerd ten opzichte van het totale aantal orders) =
* *itemsRatio* = geretourneerde item verhouding (totaal aantal items geretourneerd ten opzichte van het aantal items die zijn gekocht)
* *monetaryRatio* = retourbedrag verhouding (totale monetaire hoeveelheid geretourneerde ten opzichte van de hoeveelheid aangeschafte items)
* *frequentie* terugkeerfrequentie =

Gegevensanalyse met behulp van de K-Means vaak vereist verdere analyse van de resultaten en verdere stappen voor meer informatie over elk cluster, maar enkele goede leads kan bieden.
Hier volgen een aantal manieren u deze resultaten kan interpreteren:

* Cluster 1 (de grootste cluster) lijkt te zijn van een groep klanten die niet actief (alle waarden zijn nul).
* Cluster 3 lijkt te zijn van een groep die in termen van geretourneerde gedrag opvallen.

## <a name="clean-up-resources"></a>Resources opschonen

***Als u niet verder te gaan met deze zelfstudie om***, de database tpcxbb_1gb verwijderen uit uw Azure SQL Database-server.

Voer de volgende stappen uit vanuit de Azure-portal:

1. Selecteer in het menu links in Azure portal **alle resources** of **SQL-databases**.
1. In de **filteren op naam...**  veld **tpcxbb_1gb**, en selecteer uw abonnement.
1. Selecteer uw **tpcxbb_1gb** database.
1. Selecteer **Verwijderen** op de pagina **Overzicht**.

## <a name="next-steps"></a>Volgende stappen

In deel twee van deze zelfstudie, moet u deze stappen voltooid:

* Het aantal clusters voor een algoritme K-Means definiëren
* Clustering uitvoeren
* Analyseer de resultaten

Volg deel drie van deze serie zelfstudies voor het implementeren van de machine learning-model dat u hebt gemaakt:

> [!div class="nextstepaction"]
> [Zelfstudie: Clustering-model in R met Machine Learning Services (preview) van Azure SQL Database implementeren](sql-database-tutorial-clustering-model-deploy.md)