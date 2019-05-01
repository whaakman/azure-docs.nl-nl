---
title: Maken en train een Voorspellend model in R
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Maken van een eenvoudig voorspelmodel in R met behulp van Azure SQL Database Machine Learning Services (preview) en vervolgens een resultaat met nieuwe gegevens te voorspellen.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/11/2019
ms.openlocfilehash: c1719064de53b79a127146d0ab034f461657cc64
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64714891"
---
# <a name="create-and-train-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Maken en train een Voorspellend model in R met Azure SQL Database Machine Learning Services (preview)

In deze snelstartgids hebt u zult maken en train een Voorspellend model met behulp van R, slaat u het model naar een tabel in uw SQL-database en het model gebruiken om te voorspellen van waarden van nieuwe gegevens met behulp van de openbare preview van [Machine Learning-Services (met R) in Azure SQL Database ](sql-database-machine-learning-services-overview.md). 

Het model dat u in deze snelstartgids gebruikt is een eenvoudige regressiemodel waarmee de afstand van een auto op basis van de snelheid voorspeld. U gebruikt de **auto's** gegevensset die is opgenomen met R, omdat deze kleine en gemakkelijk te begrijpen.

> [!TIP]
> Veel gegevenssets, grote en kleine, zijn opgenomen in de R-runtime. Als u een lijst met gegevenssets die zijn geïnstalleerd met R, typt u `library(help="datasets")` vanaf een opdrachtprompt R.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Vereisten

- Als u geen Azure-abonnement hebt, [maakt u een account](https://azure.microsoft.com/free/) voordat u begint.

- Als u wilt uitvoeren met de voorbeeldcode in deze oefeningen, moet u eerst een Azure SQL database met Machine Learning-Services (met R) ingeschakeld hebben. Na de onboarding voor de openbare preview wordt Machine Learning voor u ingeschakeld voor uw bestaande of nieuwe database. Volg de stappen in [Meld u aan voor de preview-versie](sql-database-machine-learning-services-overview.md#signup).

- Zorg ervoor dat u hebt de meest recente geïnstalleerd [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS). U kunt de R-scripts met behulp van het beheer van andere databases of hulpmiddelen voor query's uitvoeren, maar in deze snelstartgids gebruikt u SSMS.

- Deze snelstart moet u een firewallregel op serverniveau configureren. Zie voor informatie over hoe u dit doet, [maken-firewallregel op serverniveau](sql-database-server-level-firewall-rule.md).

## <a name="create-and-train-a-predictive-model"></a>Maken en train een Voorspellend model

De gegevens van de snelheid van auto's in de **auto's** gegevensset bevat twee kolommen, beide numerieke: **dist** en **snelheid**. De gegevens bevatten meerdere stoppen metingen met verschillende snelheden. In deze gegevens maakt u een lineair regressiemodel waarmee de relatie tussen de snelheid van auto's en de afstand die zijn vereist voor het stoppen van een auto wordt beschreven.

De vereisten van een lineair model zijn eenvoudig:
- Een formule die de relatie tussen de afhankelijke variabele beschrijft definiëren *snelheid* en de onafhankelijke variabele *afstand*.
- Bied de invoergegevens die moeten worden gebruikt om het model te trainen.

> [!TIP]
> Als u nodig hebt voor een opfrisser over lineaire modellen, probeert u deze zelfstudie waarin wordt beschreven van het proces voor het aanpassen van een model met behulp van rxLinMod: [Lineaire modellen aanpassen](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-linear-model)

In de volgende stappen die moet u de trainingsgegevens instellen, een regressiemodel maken, trainen met behulp van de trainingsgegevens en slaat u het model naar een SQL-tabel.

1. Open **SQL Server Management Studio** en maak verbinding met de SQL-database.

   Als u hulp bij het maken van een verbinding nodig hebt, raadpleegt u [Quick Start: SQL Server Management Studio gebruiken om verbinding te maken en query's uit te voeren op een Azure SQL database](sql-database-connect-query-ssms.md).

1. Maak de **CarSpeed** tabel op te slaan de trainingsgegevens.

    ```sql
    CREATE TABLE dbo.CarSpeed (
        speed INT NOT NULL
        , distance INT NOT NULL
        )
    GO
    
    INSERT INTO dbo.CarSpeed (
        speed
        , distance
        )
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'car_speed <- cars;'
        , @input_data_1 = N''
        , @output_data_1_name = N'car_speed'
    GO
    ```

1. Maak een regressie model met `rxLinMod`. 

   Het-model bouwt u de formule in de R-code definieert en u geeft u de trainingsgegevens **CarSpeed** als invoerparameter.

    ```sql
    DROP PROCEDURE IF EXISTS generate_linear_model;
    GO
    CREATE PROCEDURE generate_linear_model
    AS
    BEGIN
      EXECUTE sp_execute_external_script
      @language = N'R'
      , @script = N'
    lrmodel <- rxLinMod(formula = distance ~ speed, data = CarsData);
    trained_model <- data.frame(payload = as.raw(serialize(lrmodel, connection=NULL)));
    '
      , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
      , @input_data_1_name = N'CarsData'
      , @output_data_1_name = N'trained_model'
      WITH RESULT SETS ((model VARBINARY(max)));
    END;
    GO
    ```

     Het eerste argument voor rxLinMod is de *formuleparameter*, waarmee afstand wordt gedefinieerd als afhankelijk van snelheid. De invoergegevens worden opgeslagen in de variabele `CarsData`, die wordt gevuld met de SQL-query.

1. Maak een tabel waarin u het model opslaan zodat u deze later voor voorspelling gebruiken kunt. 

   De uitvoer van een R-pakket dat u een model maakt is meestal een **binaire object**, zodat de tabel een kolom met moet **VARBINARY(max)** type.

    ```sql
    CREATE TABLE dbo.stopping_distance_models (
        model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(max) NOT NULL
        );
    ```

1. Nu de opgeslagen procedure aanroept, het model is gegenereerd en sla deze op in een tabel.

   ```sql
   INSERT INTO dbo.stopping_distance_models (model)
   EXECUTE generate_linear_model;
   ```

   Opmerking: als u deze code een tweede keer uitvoert, krijgt u deze foutmelding:

   ```text
   Violation of PRIMARY KEY constraint...Cannot insert duplicate key in object bo.stopping_distance_models
   ```

   Een optie om te voorkomen dat deze fout is de naam voor elk nieuw model bijwerken. U kunt de naam bijvoorbeeld wijzigen in een meer beschrijvende naam, en extra info opnemen, zoals het modeltype, de dag waarop het model is gemaakt, enzovoort.

   ```sql
   UPDATE dbo.stopping_distance_models
   SET model_name = 'rxLinMod ' + FORMAT(GETDATE(), 'yyyy.MM.HH.mm', 'en-gb')
   WHERE model_name = 'default model'
   ```

## <a name="view-the-table-of-coefficients"></a>De tabel van de coëfficiënten weergeven

Over het algemeen is de uitvoer van R van de opgeslagen procedure [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) beperkt tot één gegevensframe. U kunt echter naast het gegevensframe ook uitvoer van andere typen retourneren, zoals scalaire waarden.

Stel bijvoorbeeld dat u wilt een model te trainen, maar de tabel uit het model onmiddellijk te bekijken. Om dit te doen, maakt u de tabel van de coëfficiënten aangezien het belangrijkste resultaat ingesteld en de uitvoer van het getrainde model in een SQL-variabele. U kunt onmiddellijk opnieuw gebruiken het model door het aanroepen van de variabele of kunt u het model opslaan in een tabel, zoals hier wordt weergegeven.

```sql
DECLARE @model VARBINARY(max)
    , @modelname VARCHAR(30)

EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
speedmodel <- rxLinMod(distance ~ speed, CarsData)
modelbin <- serialize(speedmodel, NULL)
OutputDataSet <- data.frame(coefficients(speedmodel));
'
    , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
    , @input_data_1_name = N'CarsData'
    , @params = N'@modelbin varbinary(max) OUTPUT'
    , @modelbin = @model OUTPUT
WITH RESULT SETS(([Coefficient] FLOAT NOT NULL));

-- Save the generated model
INSERT INTO dbo.stopping_distance_models (
    model_name
    , model
    )
VALUES (
    'latest model'
    , @model
    )
```

**Results**

![Getraind model met extra uitvoer](./media/sql-database-quickstart-r-train-score-model/r-train-model-with-additional-output.png)

## <a name="score-new-data-using-the-trained-model"></a>Nieuwe gegevens met behulp van het getrainde model beoordelen

*Scoring* is een term die wordt gebruikt in de data science om voorspellingen, kansen of andere waarden op basis van nieuwe gegevens die zijn opgenomen in een getraind model genereren. U gebruikt het model dat u hebt gemaakt in de vorige sectie om voorspellingen op basis van nieuwe gegevens te beoordelen.

Hebt u opgemerkt dat de oorspronkelijke trainingsgegevens stoppen bij een snelheid van 25 mijl per uur? Dit komt omdat de oorspronkelijke gegevens zijn gebaseerd op een experiment uit 1920. U vraagt zich misschien af, hoe lang duurt het een auto uit de 1920s stoppen als deze kan aan de slag net zo snel als 60 mph of zelfs 100 mph? Als u wilt deze vraag te beantwoorden, kunt u enkele nieuwe waarden van de snelheid aan uw model opgeven.

1. Maak een tabel met nieuwe snelheid gegevens.

   ```sql
    CREATE TABLE dbo.NewCarSpeed (
        speed INT NOT NULL
        , distance INT NULL
        )
    GO
    
    INSERT dbo.NewCarSpeed (speed)
    VALUES (40)
        , (50)
        , (60)
        , (70)
        , (80)
        , (90)
        , (100)
   ```

2. Stoppen afstand van deze nieuwe waarden van de snelheid te voorspellen.

   Omdat het model is gebaseerd op de **rxLinMod** algoritme geleverd als onderdeel van de **RevoScaleR** pakket, u kunt aanroepen de [rxPredict](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxpredict) functioneren, in plaats van de algemene R `predict` functie.

   Met dit voorbeeldscript:
   - Maakt gebruik van een SELECT-instructie één model ophalen uit de tabel
   - Wordt doorgegeven als invoerparameter
   - Aanroepen van de `unserialize` -functie voor het model
   - Van toepassing is de `rxPredict` functie met de juiste argumenten voor het model
   - Biedt de nieuwe invoergegevens

   > [!TIP]
   > Zie voor realtime scoren [serialisatie functies](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxserializemodel) geleverd door RevoScaleR.

   ```sql
    DECLARE @speedmodel VARBINARY(max) = (
            SELECT model
            FROM dbo.stopping_distance_models
            WHERE model_name = 'latest model'
            );
    
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    current_model <- unserialize(as.raw(speedmodel));
    new <- data.frame(NewCarData);
    predicted.distance <- rxPredict(current_model, new);
    str(predicted.distance);
    OutputDataSet <- cbind(new, ceiling(predicted.distance));
    '
        , @input_data_1 = N'SELECT speed FROM [dbo].[NewCarSpeed]'
        , @input_data_1_name = N'NewCarData'
        , @params = N'@speedmodel varbinary(max)'
        , @speedmodel = @speedmodel
    WITH RESULT SETS((
                new_speed INT
                , predicted_distance INT
                ));
   ```

   **Results**

   ![Resultatenset voor het voorspellen van de remafstand](./media/sql-database-quickstart-r-train-score-model/r-predict-stopping-distance-resultset.png)

> [!NOTE]
> In dit voorbeeldscript wordt de `str` functie is toegevoegd tijdens de testfase om te controleren of het schema van de gegevens worden geretourneerd van R. U kunt de instructie later verwijderen.
>
> De kolomnamen die worden gebruikt in het R-script, worden niet per se doorgegeven aan de uitvoer van de opgeslagen procedure. De component met resultaten definieert hier enkele nieuwe kolomnamen.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over Azure SQL Database Machine Learning-Services met R (preview).

- [Azure SQL Database Machine Learning-Services met R (preview)](sql-database-machine-learning-services-overview.md)
- [Maken en eenvoudige R-scripts uitvoeren in Azure SQL Database Machine Learning-Services (preview)](sql-database-quickstart-r-create-script.md)
- [Schrijven van geavanceerde R-functies in Azure SQL Database met behulp van Machine Learning-Services (preview)](sql-database-machine-learning-services-functions.md)
- [Werken met R- en SQL-gegevens in Azure SQL Database Machine Learning Services (preview)](sql-database-machine-learning-services-data-issues.md)