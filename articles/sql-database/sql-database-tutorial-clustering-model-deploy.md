---
title: 'Zelfstudie: Clustering-model in R implementeren'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: In deel drie van deze driedelige reeks implementeert u een clustering-model in R met Machine Learning Services (preview) van Azure SQL Database.
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
ms.openlocfilehash: 1fe9df6378884ba55cb1017da87522ae66edaff0
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66419753"
---
# <a name="tutorial-deploy-a-clustering-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Zelfstudie: Clustering-model in R met Machine Learning Services (preview) van Azure SQL Database implementeren

In deel drie van deze driedelige reeks implementeert u een clustering-model in R met Machine Learning Services (preview) van Azure SQL Database.

Met een ingesloten R-script waarmee u clusters maakt u een opgeslagen procedure. Omdat het model wordt uitgevoerd in de Azure SQL database, kan deze eenvoudig worden getraind op basis van gegevens die zijn opgeslagen in de database.

In dit artikel leert u hoe u:

> [!div class="checklist"]
> * Een opgeslagen procedure die wordt gegenereerd van het model maken
> * Clustering in SQL-Database uitvoeren
> * Gebruik de clustering informatie

In [deel één](sql-database-tutorial-clustering-model-prepare-data.md), hebt u geleerd hoe u het voorbereiden van de gegevens van een Azure SQL-database om uit te voeren clustering in R.

In [deel twee](sql-database-tutorial-clustering-model-build.md), hebt u geleerd hoe u een model K-Means om uit te voeren clustering bouwt.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Vereisten

* Deel 3 van deze zelfstudie wordt ervan uitgegaan dat u hebt [ **deel één** ](sql-database-tutorial-clustering-model-prepare-data.md) en [ **deel twee**](sql-database-tutorial-clustering-model-build.md).

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Een opgeslagen procedure die wordt gegenereerd van het model maken

De volgende T-SQL-script voor het maken van de opgeslagen procedure uitvoeren. De procedure stelt de stappen die zijn ontwikkeld in delen één en twee van deze reeks zelfstudies:

* classificeren van klanten op basis van hun aankopen en geschiedenis retourneren
* vier clusters van klanten met een algoritme K-Means genereren

De procedure worden de resulterende toewijzingen van de klant-cluster in de database-tabel **customer_return_clusters**.

```sql
USE [tpcxbb_1gb]
DROP PROC IF EXISTS generate_customer_return_clusters;
GO
CREATE procedure [dbo].[generate_customer_return_clusters]
AS
/*
  This procedure uses R to classify customers into different groups
  based on their purchase & return history.
*/
BEGIN
    DECLARE @duration FLOAT
    , @instance_name NVARCHAR(100) = @@SERVERNAME
    , @database_name NVARCHAR(128) = db_name()
-- Input query to generate the purchase history & return metrics
    , @input_query NVARCHAR(MAX) = N'
SELECT ss_customer_sk AS customer,
    round(CASE 
            WHEN (
                    (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio,
    round(CASE 
            WHEN (
                    (orders_items = 0)
                    OR (returns_items IS NULL)
                    OR (orders_items IS NULL)
                    OR ((returns_items / orders_items) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio,
    round(CASE 
            WHEN (
                    (orders_money = 0)
                    OR (returns_money IS NULL)
                    OR (orders_money IS NULL)
                    OR ((returns_money / orders_money) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio,
    round(CASE 
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
 '
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
# Define the connection string
connStr <- paste("Driver=SQL Server; Server=", instance_name,
               "; Database=", database_name,
               "; Trusted_Connection=true; ",
                  sep="" );

# Input customer data that needs to be classified.
# This is the result we get from the query.
customer_returns <- RxSqlServerData(
                     sqlQuery=input_query,
                     colClasses=c(customer ="numeric",
                                  orderRatio="numeric",
                                  itemsRatio="numeric",
                                  monetaryRatio="numeric",
                                  frequency="numeric" ),
                     connectionString=connStr);
# Output table to hold the customer cluster mappings
return_cluster = RxSqlServerData(table = "customer_return_clusters",
                                 connectionString = connStr);

# Set seed for random number generator for predictability
set.seed(10);

# Generate clusters using rxKmeans and output clusters to a table
# called "customer_return_clusters"
clust <- rxKmeans( ~ orderRatio + itemsRatio + monetaryRatio + frequency,
                   customer_returns,
                   numClusters = 4,
                   outFile = return_cluster,
                   outColName = "cluster",
                   writeModelVars = TRUE ,
                   extraVarsToWrite = c("customer"),
                   overwrite = TRUE);
'
    , @input_data_1 = N''
    , @params = N'@instance_name nvarchar(100), @database_name nvarchar(128), @input_query nvarchar(max), @duration float OUTPUT'
    , @instance_name = @instance_name
    , @database_name = @database_name
    , @input_query = @input_query
    , @duration = @duration OUTPUT;
END;

GO
```

## <a name="perform-clustering-in-sql-database"></a>Clustering in SQL-Database uitvoeren

Nu u de opgeslagen procedure hebt gemaakt, voert u uit het volgende script om uit te voeren clustering.

```sql
--Empty table of the results before running the stored procedure
TRUNCATE TABLE customer_return_clusters;

--Execute the clustering
--This will load the table customer_return_clusters with cluster mappings
EXECUTE [dbo].[generate_customer_return_clusters];
```

Controleer of het werkt en we hebben de lijst met klanten en hun toewijzingen van het cluster.

```sql
--Select data from table customer_return_clusters
--to verify that the clustering data was loaded
SELECT TOP (5) *
FROM customer_return_clusters;
```

```result
cluster  customer  orderRatio  itemsRatio  monetaryRatio  frequency
1        29727     0           0           0              0
4        26429     0           0           0.041979       1
2        60053     0           0           0.065762       3
2        97643     0           0           0.037034       3
2        32549     0           0           0.031281       4
```

## <a name="use-the-clustering-information"></a>Gebruik de clustering informatie

Omdat u de clustering procedure in de database opgeslagen, kan het uitvoeren om efficiënt clustering op basis van klantgegevens opgeslagen in dezelfde database. Wanneer uw klantgegevens wordt bijgewerkt en de bijgewerkte clustering gegevens, kunt u de procedure uitvoeren.

Stel dat u wilt verzenden van een e-mails naar klanten in cluster 3, de groep met meer actieve geretourneerde gedrag (u kunt zien hoe de vier clusters zijn beschreven in [deel twee](sql-database-tutorial-clustering-model-build.md#analyze-the-results)). De volgende code wordt het e-mailadressen van klanten in cluster 3 geselecteerd.

```sql
USE [tpcxbb_1gb]

SELECT customer.[c_email_address],
    customer.c_customer_sk
FROM dbo.customer
JOIN [dbo].[customer_return_clusters] AS r ON r.customer = customer.c_customer_sk
WHERE r.cluster = 3
```

U kunt wijzigen de **r.cluster** die moeten worden geretourneerd van e-mailadressen voor klanten in andere clusters.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar met deze zelfstudie bent, kunt u de database tpcxbb_1gb verwijderen uit uw Azure SQL Database-server.

Voer de volgende stappen uit vanuit de Azure-portal:

1. Selecteer in het menu links in Azure portal **alle resources** of **SQL-databases**.
1. In de **filteren op naam...**  veld **tpcxbb_1gb**, en selecteer uw abonnement.
1. Selecteer uw **tpcxbb_1gb** database.
1. Selecteer **Verwijderen** op de pagina **Overzicht**.

## <a name="next-steps"></a>Volgende stappen

In deel drie van deze zelfstudie, moet u deze stappen voltooid:

* Een opgeslagen procedure die wordt gegenereerd van het model maken
* Clustering in SQL-Database uitvoeren
* Gebruik de clustering informatie

Zie voor meer informatie over het gebruik van R in Machine Learning Services (preview) voor Azure SQL Database:

* [Zelfstudie: Gegevens met het trainen van een Voorspellend model R met Machine Learning Services (preview) van Azure SQL Database voorbereiden](sql-database-tutorial-predictive-model-prepare-data.md)
* [Schrijven van geavanceerde R-functies in Azure SQL Database met behulp van Machine Learning-Services (preview)](sql-database-machine-learning-services-functions.md)
* [Werken met R- en SQL-gegevens in Azure SQL Database Machine Learning Services (preview)](sql-database-machine-learning-services-data-issues.md)
* [Een R-pakket toevoegen aan Azure SQL Database Machine Learning Services (preview)](sql-database-machine-learning-services-add-r-packages.md)
