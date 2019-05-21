---
title: 'Zelfstudie: Voorbereiden van gegevens met het trainen van een Voorspellend model R'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: In deel één van deze driedelige reeks zelfstudies, zult u de gegevens uit een Azure SQL-database met het trainen van een Voorspellend model R met Machine Learning Services (preview) van Azure SQL Database voorbereiden.
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
ms.openlocfilehash: aa9c41ee34a50ab9b1409357bfe7d123166601bf
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978731"
---
# <a name="tutorial-prepare-data-to-train-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Zelfstudie: Gegevens met het trainen van een Voorspellend model R met Machine Learning Services (preview) van Azure SQL Database voorbereiden

In deel één van deze driedelige reeks zelfstudies, zult u de gegevens uit een Azure SQL-database met het trainen van een Voorspellend model R met Machine Learning Services (preview) van Azure SQL Database voorbereiden.

Voor deze reeks zelfstudies Stel dat u een ski verhuur bedrijf de eigenaar bent en u wilt het aantal fietsen verhuurd die u op een datum in de toekomst hebt te voorspellen. Deze informatie helpt u bereid u voor uw voorraad, personeel en faciliteiten.

In dit artikel leert u hoe u:

> [!div class="checklist"]
> * Een voorbeelddatabase importeren in een Azure SQL database
> * De gegevens uit de Azure SQL database laden in een gegevensframe met R
> * De gegevens door te identificeren van sommige kolommen als categorische voorbereiden

In [deel twee](sql-database-tutorial-predictive-model-build-compare.md), leert u hoe u maken en meerdere modellen trainen en vervolgens kiest u de meest nauwkeurige.

In [deel drie](sql-database-tutorial-predictive-model-deploy.md), leert u hoe u het model opslaan in een database en maak vervolgens een opgeslagen procedure die voorspellingen op basis van nieuwe gegevens kunt maken.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: als u een Azure-abonnement geen [maken van een account](https://azure.microsoft.com/free/) voordat u begint.

* Azure SQL Database-Server met Machine Learning-Services ingeschakeld - tijdens de openbare preview, onboarding Microsoft zal u en machine-learning inschakelen voor uw bestaande of nieuwe databases. Volg de stappen in [Meld u aan voor de preview-versie](sql-database-machine-learning-services-overview.md#signup).

* RevoScaleR package - Zie [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler?view=sql-server-2017#versions-and-platforms) voor opties om dit pakket lokaal te installeren.

* Deze zelfstudie wordt gebruikgemaakt van R IDE - [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

* Hulpprogramma voor SQL-query - in deze zelfstudie wordt ervan uitgegaan dat u [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) of [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS).

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="import-the-sample-database"></a>De voorbeelddatabase importeren

De voorbeeldgegevensset gebruikt in deze zelfstudie is opgeslagen in een **.bacpac** back-upbestand van de database voor u om te downloaden en gebruiken.

1. Download het bestand [TutorialDB.bacpac](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bacpac).

1. Volg de aanwijzingen in [een BACPAC-bestand voor het maken van een Azure SQL-database importeren](https://docs.microsoft.com/azure/sql-database/sql-database-import), met behulp van deze gegevens:

   * Importeren uit de **TutorialDB.bacpac** dat u hebt gedownload
   * Tijdens de openbare preview kunnen ervoor kiezen de **Gen5/vCore** configuratie voor de nieuwe database
   * Naam van de nieuwe database "TutorialDB"

## <a name="load-the-data-into-a-data-frame"></a>De gegevens in een gegevensframe laden

Voor het gebruik van de gegevens in R, zult u de gegevens laden in Azure SQL-database in een gegevensframe (`rentaldata`).

Maak een nieuwe RScript-bestand in RStudio en voer het volgende script. Vervang **Server**, **UID**, en **PWD** met uw eigen verbindingsgegevens.

```r
#Define the connection string to connect to the TutorialDB database
connStr <- paste("Driver=SQL Server",
               "; Server=", "<Azure SQL Database Server>",
               "; Database=TutorialDB",
               "; UID=", "<user>",
               "; PWD=", "<password>",
                  sep = "");

#Get the data from the table
SQL_rentaldata <- RxSqlServerData(table = "dbo.rental_data", connectionString = connStr, returnDataFrame = TRUE);

#Import the data into a data frame
rentaldata <- rxImport(SQL_rentaldata);

#Take a look at the structure of the data and the top rows
head(rentaldata);
str(rentaldata);
```

U ziet het resultaat is vergelijkbaar met de volgende.

```results
   Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
1  2014    1     20      445         2        1      0
2  2014    2     13       40         5        0      0
3  2013    3     10      456         1        0      0
4  2014    3     31       38         2        0      0
5  2014    4     24       23         5        0      0
6  2015    2     11       42         4        0      0
'data.frame':       453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : num  2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : int  1 0 0 0 0 0 0 0 0 0 ...
$ Snow       : num  0 0 0 0 0 0 0 0 0 0 ...
```

## <a name="prepare-the-data"></a>De gegevens voorbereiden

In deze database, de meeste van de voorbereiding van het al is uitgevoerd, maar, doet u hier een meer voorbereiding.
De volgende R-script gebruiken om te identificeren van drie kolommen als *categorieën* door het veranderen van de gegevens die u wilt *factor*.

```r
#Changing the three factor columns to factor types
rentaldata$Holiday <- factor(rentaldata$Holiday);
rentaldata$Snow    <- factor(rentaldata$Snow);
rentaldata$WeekDay <- factor(rentaldata$WeekDay);

#Visualize the dataset after the change
str(rentaldata);
```

U ziet het resultaat is vergelijkbaar met de volgende.

```results
data.frame':      453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : Factor w/ 7 levels "1","2","3","4",..: 2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : Factor w/ 2 levels "0","1": 2 1 1 1 1 1 1 1 1 1 ...
$ Snow       : Factor w/ 2 levels "0","1": 1 1 1 1 1 1 1 1 1 1 ...
```

De gegevens is nu klaar voor de training.

## <a name="clean-up-resources"></a>Resources opschonen

Als u niet verder om door te gaan met deze zelfstudie, moet u de TutorialDB-database verwijderen uit uw Azure SQL Database-server.

Voer de volgende stappen uit vanuit de Azure-portal:

1. Selecteer in het menu links in Azure portal **alle resources** of **SQL-databases**.
1. In de **filteren op naam...**  veld **TutorialDB**, en selecteer uw abonnement.
1. Selecteer uw database TutorialDB.
1. Selecteer **Verwijderen** op de pagina **Overzicht**.

## <a name="next-steps"></a>Volgende stappen

In deel één van deze zelfstudie, moet u deze stappen voltooid:

* Een back-upbestand van de database importeren in een Azure SQL database
* De gegevens uit de Azure SQL database laden in een gegevensframe met R
* De gegevens voorbereiden met het vaststellen van sommige kolommen als categorieën

Ga als volgt deel twee van deze serie zelfstudies voor het maken van een machine learning-model dat gebruikmaakt van gegevens uit de database TutorialDB:

> [!div class="nextstepaction"]
> [Zelfstudie: Maken van een Voorspellend model in R met Azure SQL Database Machine Learning Services (preview)](sql-database-tutorial-predictive-model-build-compare.md)
