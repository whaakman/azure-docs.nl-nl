---
title: 'Zelfstudie: Trainen en te vergelijken van voorspellende modellen in R'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: In deel twee van deze driedelige reeks zelfstudies hebt u twee voorspellende modellen maken in R met Machine Learning Services (preview) van Azure SQL Database en selecteer vervolgens de meest nauwkeurige model.
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
ms.date: 05/02/2019
ms.openlocfilehash: 3d336d6a53b6d234048c56d8492d278bef6fed64
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957608"
---
# <a name="tutorial-create-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Zelfstudie: Maken van een Voorspellend model in R met Azure SQL Database Machine Learning Services (preview)

In deel twee van deze driedelige reeks zelfstudies hebt u twee voorspellende modellen maken in R met Machine Learning Services (preview) van Azure SQL Database en selecteer vervolgens de meest nauwkeurige model.

In dit artikel leert u hoe u:

> [!div class="checklist"]
> * Twee machine learning-modellen trainen
> * Voorspellingen op basis van beide modellen maken
> * Vergelijk de resultaten voor de meest nauwkeurige model kiezen

In [deel één](sql-database-tutorial-predictive-model-prepare-data.md), hebt u geleerd hoe u een voorbeelddatabase importeren in een Azure SQL database en vervolgens de gegevens moet worden gebruikt voor het trainen van een Voorspellend model R. voorbereiden

In [deel drie](sql-database-tutorial-predictive-model-deploy.md), leert u hoe u het model opslaan in een database en maak vervolgens een opgeslagen procedure die voorspellingen op basis van nieuwe gegevens kunt maken.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Vereisten

* Deel twee van deze zelfstudie wordt ervan uitgegaan dat u hebt [ **deel één** ](sql-database-tutorial-predictive-model-prepare-data.md) en de vereisten.

## <a name="train-two-models"></a>Twee modellen trainen

Als u het beste model voor de ski-fietsverhuurgegevens zoekt, maken van twee verschillende modellen (lineaire regressie en beslissingsstructuur) en zien welke nauwkeuriger is voorspellen. U gebruikt de gegevensframe `rentaldata` dat u hebt gemaakt in deel één van deze serie.

```r
#First, split the dataset into two different sets:
# one for training the model and the other for validating it
train_data = rentaldata[rentaldata$Year < 2015,];
test_data  = rentaldata[rentaldata$Year == 2015,];

#Use the RentalCount column to check the quality of the prediction against actual values
actual_counts <- test_data$RentalCount;

#Model 1: Use rxLinMod to create a linear regression model, trained with the training data set
model_linmod <- rxLinMod(RentalCount ~  Month + Day + WeekDay + Snow + Holiday, data = train_data);

#Model 2: Use rxDTree to create a decision tree model, trained with the training data set
model_dtree  <- rxDTree(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = train_data);
```

## <a name="make-predictions-from-both-models"></a>Voorspellingen op basis van beide modellen maken

Gebruik een functie predict predict die de verhuur telt elke getrainde model gebruiken.

```r
#Use both models to make predictions using the test data set.
predict_linmod <- rxPredict(model_linmod, test_data, writeModelVars = TRUE, extraVarsToWrite = c("Year"));

predict_dtree  <- rxPredict(model_dtree,  test_data, writeModelVars = TRUE, extraVarsToWrite = c("Year"));

#To verify it worked, look at the top rows of the two prediction data sets.
head(predict_linmod);
head(predict_dtree);
```

```results
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1         27.45858          42       2     11     4      0       0
2        387.29344         360       3     29     1      0       0
3         16.37349          20       4     22     4      0       0
4         31.07058          42       3      6     6      0       0
5        463.97263         405       2     28     7      1       0
6        102.21695          38       1     12     2      1       0
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1          40.0000          42       2     11     4      0       0
2         332.5714         360       3     29     1      0       0
3          27.7500          20       4     22     4      0       0
4          34.2500          42       3      6     6      0       0
5         645.7059         405       2     28     7      1       0
6          40.0000          38       1     12     2      1       0
```

## <a name="compare-the-results"></a>Vergelijk de resultaten

Nu wilt u zien welke van de modellen biedt de beste voorspellingen. Een snelle en eenvoudige manier om dit te doen is het gebruik van een basisfunctie die door plotting om het verschil tussen de werkelijke waarden in uw trainingsgegevens en de voorspelde waarden weer te geven.

```r
#Use the plotting functionality in R to visualize the results from the predictions
par(mfrow = c(2, 1));
plot(predict_linmod$RentalCount_Pred - predict_linmod$RentalCount, main = "Difference between actual and predicted. rxLinmod");
plot(predict_dtree$RentalCount_Pred  - predict_dtree$RentalCount,  main = "Difference between actual and predicted. rxDTree");
```

![Vergelijking van de twee modellen](./media/sql-database-tutorial-predictive-model-build-compare/compare-models.png)

Het lijkt erop dat de boomstructuur van de beslissing van de twee modellen de nauwkeuriger is.

## <a name="clean-up-resources"></a>Resources opschonen

Als u niet verder om door te gaan met deze zelfstudie, moet u de TutorialDB-database verwijderen uit uw Azure SQL Database-server.

Voer de volgende stappen uit vanuit de Azure-portal:

1. Selecteer in het menu links in Azure portal **alle resources** of **SQL-databases**.
1. In de **filteren op naam...**  veld **TutorialDB**, en selecteer uw abonnement.
1. Selecteer uw database TutorialDB.
1. Selecteer **Verwijderen** op de pagina **Overzicht**.

## <a name="next-steps"></a>Volgende stappen

In deel twee van deze zelfstudie, moet u deze stappen voltooid:

* Twee machine learning-modellen trainen
* Voorspellingen op basis van beide modellen maken
* Vergelijk de resultaten voor de meest nauwkeurige model kiezen

Volg deel drie van deze serie zelfstudies voor het implementeren van de machine learning-model dat u hebt gemaakt:

> [!div class="nextstepaction"]
> [Zelfstudie: Een Voorspellend model R met Machine Learning Services (preview) van Azure SQL Database implementeren](sql-database-tutorial-predictive-model-deploy.md)
