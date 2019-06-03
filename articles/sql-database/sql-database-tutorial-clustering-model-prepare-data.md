---
title: 'Zelfstudie: Gegevens uit te voeren in R clustering voorbereiden'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: In deel één van deze driedelige reeks zelfstudies, zult u de gegevens uit een Azure SQL-database om uit te voeren clustering in R met Machine Learning Services (preview) van Azure SQL Database voorbereiden.
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
ms.openlocfilehash: 83ef25f04012933c2665e63e4617d480eb336f7b
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66419459"
---
# <a name="tutorial-prepare-data-to-perform-clustering-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Zelfstudie: Gegevens uit te voeren clustering in R met Machine Learning Services (preview) van Azure SQL Database voorbereiden

In deel één van deze driedelige reeks zelfstudies, zult u de gegevens uit een Azure SQL-database om uit te voeren clustering in R met Machine Learning Services (preview) van Azure SQL Database voorbereiden.

*Clustering* als gegevens in groepen waar leden van een groep op een bepaalde manier lijken ordenen kan worden verklaard.
U gebruikt de **K-Means** algoritme voor het uitvoeren van de clustering van klanten in een gegevensset van het product aanschaft en retourneert. Door clustering klanten, u zich kunt richten uw marketingactiviteiten effectiever door die zijn gericht op specifieke groepen.
K-Means clustering is een *leren zonder supervisie* algoritme dat zoekt naar patronen in gegevens op basis van overeenkomsten.

In dit artikel leert u hoe u:

> [!div class="checklist"]
> * Een voorbeelddatabase importeren in een Azure SQL database
> * Afzonderlijke klanten langs verschillende dimensies
> * De gegevens uit de Azure SQL database laden in een gegevensframe met R

In [deel twee](sql-database-tutorial-clustering-model-build.md), leert u hoe u voor het maken en een K-Means clustering-model te trainen.

In [deel drie](sql-database-tutorial-clustering-model-deploy.md), leert u hoe u een opgeslagen procedure maken in een Azure SQL-database die clustering op basis van nieuwe gegevens kunt uitvoeren.

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

De voorbeeldgegevensset gebruikt in deze zelfstudie is opgeslagen in een **.bacpac** back-upbestand van de database voor u om te downloaden en gebruiken. Deze gegevensset is afgeleid van de [tpcx-bb](http://www.tpc.org/tpcx-bb/default.asp) gegevensset geleverd door de [transactie verwerken prestaties Raad (TPC)](http://www.tpc.org/default.asp).

1. Download het bestand [tpcxbb_1gb.bacpac](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bacpac).

1. Volg de aanwijzingen in [een BACPAC-bestand voor het maken van een Azure SQL-database importeren](https://docs.microsoft.com/azure/sql-database/sql-database-import), met behulp van deze gegevens:

   * Importeren uit de **tpcxbb_1gb.bacpac** dat u hebt gedownload
   * Tijdens de openbare preview kunnen ervoor kiezen de **Gen5/vCore** configuratie voor de nieuwe database
   * Naam van de nieuwe database "tpcxbb_1gb"

## <a name="separate-customers"></a>Afzonderlijke klanten

Maak een nieuwe RScript-bestand in RStudio en voer het volgende script.
U bent klanten langs de volgende dimensies scheiden in de SQL-query:

* **orderRatio** geretourneerde volgorde verhouding (totaal aantal orders volledig of gedeeltelijk geretourneerd ten opzichte van het totale aantal orders) =
* **itemsRatio** = geretourneerde item verhouding (totaal aantal items geretourneerd ten opzichte van het aantal items die zijn gekocht)
* **monetaryRatio** = retourbedrag verhouding (totale monetaire hoeveelheid geretourneerde ten opzichte van de hoeveelheid aangeschafte items)
* **frequentie** terugkeerfrequentie =

In de **plakken** Vervang **Server**, **UID**, en **PWD** met uw eigen verbindingsgegevens.

```r
# Define the connection string to connect to the tpcxbb_1gb database
connStr <- paste("Driver=SQL Server",
               "; Server=", "<Azure SQL Database Server>",
               "; Database=tpcxbb_1gb",
               "; UID=", "<user>",
               "; PWD=", "<password>",
                  sep = "");


#Define the query to select data from SQL Server
input_query <- "
SELECT ss_customer_sk AS customer
    ,round(CASE 
            WHEN (
                       (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio
    ,round(CASE 
            WHEN (
                     (orders_items = 0)
                  OR (returns_items IS NULL)
                  OR (orders_items IS NULL)
                  OR ((returns_items / orders_items) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio
    ,round(CASE 
            WHEN (
                     (orders_money = 0)
                  OR (returns_money IS NULL)
                  OR (orders_money IS NULL)
                  OR ((returns_money / orders_money) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio
    ,round(CASE 
            WHEN (returns_count IS NULL)
            THEN 0.0
            ELSE returns_count
            END, 0) AS frequency
FROM (
    SELECT ss_customer_sk,
        -- return order ratio
        COUNT(DISTINCT (ss_ticket_number)) AS orders_count,
        -- return ss_item_sk ratio
        COUNT(ss_item_sk) AS orders_items,
        -- return monetary amount ratio
        SUM(ss_net_paid) AS orders_money
    FROM store_sales s
    GROUP BY ss_customer_sk
    ) orders
LEFT OUTER JOIN (
    SELECT sr_customer_sk,
        -- return order ratio
        count(DISTINCT (sr_ticket_number)) AS returns_count,
        -- return ss_item_sk ratio
        COUNT(sr_item_sk) AS returns_items,
        -- return monetary amount ratio
        SUM(sr_return_amt) AS returns_money
    FROM store_returns
    GROUP BY sr_customer_sk
    ) returned ON ss_customer_sk = sr_customer_sk
"
```

## <a name="load-the-data-into-a-data-frame"></a>De gegevens in een gegevensframe laden

Nu het volgende script gebruiken om terug te keren van de resultaten van de query naar een R gegevens kader met de **rxSqlServerData** functie.
Als onderdeel van het proces definieert u het type voor de geselecteerde kolommen (met colClasses) om ervoor te zorgen dat de typen correct worden overgedragen naar R.

```r
# Query SQL Server using input_query and get the results back
# to data frame customer_returns
# Define the types for selected columns (using colClasses),
# to make sure that the types are correctly transferred to R
customer_returns <- rxSqlServerData(
                     sqlQuery=input_query,
                     colClasses=c(customer ="numeric",
                                  orderRatio="numeric",
                                  itemsRatio="numeric",
                                  monetaryRatio="numeric",
                                  frequency="numeric" ),
                     connectionString=connStr);

# Transform the data from an input dataset to an output dataset
customer_data <- rxDataStep(customer_returns);

# Take a look at the data just loaded from SQL Server
head(customer_data, n = 5);
```

U ziet het resultaat is vergelijkbaar met de volgende.

```results
  customer orderRatio itemsRatio monetaryRatio frequency
1    29727          0          0      0.000000         0
2    26429          0          0      0.041979         1
3    60053          0          0      0.065762         3
4    97643          0          0      0.037034         3
5    32549          0          0      0.031281         4
```

## <a name="clean-up-resources"></a>Resources opschonen

***Als u niet verder te gaan met deze zelfstudie om***, de database tpcxbb_1gb verwijderen uit uw Azure SQL Database-server.

Voer de volgende stappen uit vanuit de Azure-portal:

1. Selecteer in het menu links in Azure portal **alle resources** of **SQL-databases**.
1. In de **filteren op naam...**  veld **tpcxbb_1gb**, en selecteer uw abonnement.
1. Selecteer uw **tpcxbb_1gb** database.
1. Selecteer **Verwijderen** op de pagina **Overzicht**.

## <a name="next-steps"></a>Volgende stappen

In deel één van deze zelfstudie, moet u deze stappen voltooid:

* Een voorbeelddatabase importeren in een Azure SQL database
* Afzonderlijke klanten langs verschillende dimensies
* De gegevens uit de Azure SQL database laden in een gegevensframe met R

Ga als volgt deel twee van deze serie zelfstudies voor het maken van een machine learning-model die gebruikmaakt van deze gegevens van de klant:

> [!div class="nextstepaction"]
> [Zelfstudie: Maken van een Voorspellend model in R met Azure SQL Database Machine Learning Services (preview)](sql-database-tutorial-clustering-model-build.md)
