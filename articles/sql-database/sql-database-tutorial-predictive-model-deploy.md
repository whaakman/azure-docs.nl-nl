---
title: 'Zelfstudie: Implementeren van een Voorspellend model R'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: In deel drie van deze driedelige zelfstudie implementeert u een Voorspellend model R met Machine Learning Services (preview) van Azure SQL Database.
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
ms.openlocfilehash: 17b68f71f4034e5eb637d40b975cc22d94438fb7
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978694"
---
# <a name="tutorial-deploy-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Zelfstudie: Een Voorspellend model R met Machine Learning Services (preview) van Azure SQL Database implementeren

In deel drie van deze driedelige zelfstudie implementeert u een Voorspellend model R met Machine Learning Services (preview) van Azure SQL Database.

Met een ingesloten R-script die Hiermee worden voorspellingen gedaan met behulp van het model maakt u een opgeslagen procedure. Omdat het model wordt uitgevoerd in de Azure SQL database, kan deze eenvoudig worden getraind op basis van gegevens die zijn opgeslagen in de database.

In dit artikel leert u hoe u:

> [!div class="checklist"]
> * Voorspellend model Store in een databasetabel
> * Een opgeslagen procedure die wordt gegenereerd van het model maken
> * Een opgeslagen procedure die Hiermee worden voorspellingen gedaan met behulp van het model maken
> * Het model met nieuwe gegevens worden uitgevoerd

In [deel één](sql-database-tutorial-predictive-model-prepare-data.md), hebt u geleerd hoe u een voorbeelddatabase importeren in een Azure SQL database en vervolgens de gegevens moet worden gebruikt voor het trainen van een Voorspellend model R. voorbereiden

In [deel twee](sql-database-tutorial-predictive-model-build-compare.md), hebt u geleerd hoe u kunt maken en meerdere modellen trainen en vervolgens kiest u de meest nauwkeurige.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Vereisten

* Deel 3 van deze zelfstudie wordt ervan uitgegaan dat u hebt [ **deel één** ](sql-database-tutorial-predictive-model-prepare-data.md) en [ **deel twee**](sql-database-tutorial-predictive-model-build-compare.md).

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Een opgeslagen procedure die wordt gegenereerd van het model maken

In deel twee van deze zelfstudie, kunt u besloten dat een decision tree (dtree)-model de meest nauwkeurige is. Maak nu een opgeslagen procedure (`generate_rental_rx_model`) die traint en genereert de dtree-model met behulp van rxDTree van het pakket RevoScaleR.

Voer de volgende opdrachten in Azure Data Studio of in SSMS.

```sql
-- Stored procedure that trains and generates an R model using the rental_data and a decision tree algorithm
DROP PROCEDURE IF EXISTS generate_rental_rx_model;
GO
CREATE PROCEDURE generate_rental_rx_model (@trained_model VARBINARY(max) OUTPUT)
AS
BEGIN
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
require("RevoScaleR");

rental_train_data$Holiday <- factor(rental_train_data$Holiday);
rental_train_data$Snow    <- factor(rental_train_data$Snow);
rental_train_data$WeekDay <- factor(rental_train_data$WeekDay);

#Create a dtree model and train it using the training data set
model_dtree <- rxDTree(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = rental_train_data);
#Serialize the model before saving it to the database table
trained_model <- as.raw(serialize(model_dtree, connection=NULL));
'
        , @input_data_1 = N'
            SELECT RentalCount
                 , Year
                 , Month
                 , Day
                 , WeekDay
                 , Snow
                 , Holiday
            FROM dbo.rental_data
            WHERE Year < 2015
            '
        , @input_data_1_name = N'rental_train_data'
        , @params = N'@trained_model varbinary(max) OUTPUT'
        , @trained_model = @trained_model OUTPUT;
END;
GO
```

## <a name="store-the-model-in-a-database-table"></a>Het model in een databasetabel Store

Een tabel in de database TutorialDB maakt en sla het model aan de tabel.

1. Een tabel maken (`rental_rx_models`) voor het opslaan van het model.

    ```sql
    USE TutorialDB;
    DROP TABLE IF EXISTS rental_rx_models;
    GO
    CREATE TABLE rental_rx_models (
          model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(MAX) NOT NULL
        );
    GO
    ```

1. Sla het model aan de tabel als een binaire-object met de model-naam 'rxDTree'.

    ```sql
    -- Save model to table
    TRUNCATE TABLE rental_rx_models;
    
    DECLARE @model VARBINARY(MAX);
    
    EXECUTE generate_rental_rx_model @model OUTPUT;
    
    INSERT INTO rental_rx_models (
          model_name
        , model
        )
    VALUES (
         'rxDTree'
        , @model
        );
    
    SELECT *
    FROM rental_rx_models;
    ```

## <a name="create-a-stored-procedure-that-makes-predictions"></a>Een opgeslagen procedure waarmee voorspellingen maken

Een opgeslagen procedure maken (`predict_rentalcount_new`) die Hiermee worden voorspellingen gedaan met behulp van het getrainde model en een set met nieuwe gegevens.

```sql
-- Stored procedure that takes model name and new data as input parameters and predicts the rental count for the new data
DROP PROCEDURE IF EXISTS predict_rentalcount_new;
GO
CREATE PROCEDURE predict_rentalcount_new (
      @model_name VARCHAR(100)
    , @input_query NVARCHAR(MAX)
    )
AS
BEGIN
    DECLARE @rx_model VARBINARY(MAX) = (
            SELECT model
            FROM rental_rx_models
            WHERE model_name = @model_name
            );

    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
require("RevoScaleR");

#Convert types to factors
rentals$Holiday <- factor(rentals$Holiday);
rentals$Snow    <- factor(rentals$Snow);
rentals$WeekDay <- factor(rentals$WeekDay);

#Before using the model to predict, we need to unserialize it
rental_model <- unserialize(rx_model);

#Call prediction function
rental_predictions <- rxPredict(rental_model, rentals);
'
        , @input_data_1 = @input_query
        , @input_data_1_name = N'rentals'
        , @output_data_1_name = N'rental_predictions'
        , @params = N'@rx_model varbinary(max)'
        , @rx_model = @rx_model
    WITH RESULT SETS(("RentalCount_Predicted" FLOAT));
END;
GO
```

## <a name="execute-the-model-with-new-data"></a>Het model met nieuwe gegevens worden uitgevoerd

Nu kunt u de opgeslagen procedure `predict_rentalcount_new` om te voorspellen van het aantal verhuur van nieuwe gegevens.

```sql
-- Use the predict_rentalcount_new stored procedure with the model name and a set of features to predict the rental count
EXECUTE dbo.predict_rentalcount_new @model_name = 'rxDTree'
    , @input_query = '
        SELECT CONVERT(INT,  3) AS Month
             , CONVERT(INT, 24) AS Day
             , CONVERT(INT,  4) AS WeekDay
             , CONVERT(INT,  1) AS Snow
             , CONVERT(INT,  1) AS Holiday
        ';
GO
```

U ziet een resultaat vergelijkbaar met de volgende.

```results
RentalCount_Predicted
332.571428571429
```

U hebt gemaakt, getraind, en een model in een Azure SQL database geïmplementeerd. Vervolgens gebruikt u dit model in een opgeslagen procedure om te voorspellen op basis van waarden op nieuwe gegevens.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met het gebruik van de TutorialDB-database, verwijdert u deze uit uw Azure SQL Database-server.

Voer de volgende stappen uit vanuit de Azure-portal:

1. Selecteer in het menu links in Azure portal **alle resources** of **SQL-databases**.
1. In de **filteren op naam...**  veld **TutorialDB**, en selecteer uw abonnement.
1. Selecteer uw database TutorialDB.
1. Selecteer **Verwijderen** op de pagina **Overzicht**.

## <a name="next-steps"></a>Volgende stappen

In deel drie van deze zelfstudie, moet u deze stappen voltooid:

* Voorspellend model Store in een databasetabel
* Een opgeslagen procedure die wordt gegenereerd van het model maken
* Een opgeslagen procedure die Hiermee worden voorspellingen gedaan met behulp van het model maken
* Het model met nieuwe gegevens worden uitgevoerd

Zie voor meer informatie over het gebruik van R in Machine Learning Services (preview) voor Azure SQL Database:

* [Schrijven van geavanceerde R-functies in Azure SQL Database met behulp van Machine Learning-Services (preview)](sql-database-machine-learning-services-functions.md)
* [Werken met R- en SQL-gegevens in Azure SQL Database Machine Learning Services (preview)](sql-database-machine-learning-services-data-issues.md)
* [Een R-pakket toevoegen aan Azure SQL Database Machine Learning Services (preview)](sql-database-machine-learning-services-add-r-packages.md)
