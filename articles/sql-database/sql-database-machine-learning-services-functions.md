---
title: Geavanceerde R-functies schrijven
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Informatie over het schrijven van een R-functie voor geavanceerde statistische berekeningen in Azure SQL Database met behulp van Machine Learning-Services (preview).
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/11/2019
ms.openlocfilehash: 939798d5d9eb2843d7bbbbe74680342e4ce6ce95
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60702449"
---
# <a name="write-advanced-r-functions-in-azure-sql-database-using-machine-learning-services-preview"></a>Schrijven van geavanceerde R-functies in Azure SQL Database met behulp van Machine Learning-Services (preview)

Dit artikel wordt beschreven hoe u R wiskundige insluit en functies in SQL opgeslagen procedure. Geavanceerde statistische functies die zijn ingewikkeld om te implementeren in T-SQL kunnen u doen in R met slechts één regel code.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Vereisten

- Als u geen Azure-abonnement hebt, [maakt u een account](https://azure.microsoft.com/free/) voordat u begint.

- Als u wilt uitvoeren met de voorbeeldcode in deze oefeningen, moet u eerst een Azure SQL database met Machine Learning-Services (met R) ingeschakeld hebben. Na de onboarding voor de openbare preview wordt Machine Learning voor u ingeschakeld voor uw bestaande of nieuwe database. Volg de stappen in [Meld u aan voor de preview-versie](sql-database-machine-learning-services-overview.md#signup).

- Zorg ervoor dat u hebt de meest recente geïnstalleerd [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS). U kunt R-scripts met behulp van het beheer van andere databases of hulpmiddelen voor query's uitvoeren, maar in deze snelstartgids gebruikt u SSMS.

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>Een opgeslagen procedure voor het genereren van willekeurige getallen maken

Voor het gemak gebruiken we de R `stats` -pakket dat is geïnstalleerd en worden standaard geladen met Azure SQL Database met behulp van Machine Learning-Services (preview). Het pakket bevat honderden functies om algemene statistische taken, waaronder de `rnorm` functie. Deze functie genereert een opgegeven aantal willekeurige getallen met behulp van de normale verdeling opgegeven standaarddeviatie en middelen.

De volgende R-code retourneert bijvoorbeeld 100 getallen op een gemiddelde van 50, krijgt de standaarddeviatie van 3.

```R
as.data.frame(rnorm(100, mean = 50, sd = 3));
```

Voer voor het aanroepen van deze regel van R uit T-SQL, `sp_execute_external_script` en voeg de R-functie toe in de R-script-parameter, als volgt:

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- as.data.frame(rnorm(100, mean = 50, sd =3));
'
    , @input_data_1 = N'   ;'
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

Wat gebeurt er als u wilt maken het gemakkelijker voor het genereren van een andere set willekeurige getallen?

Dat kan eenvoudig in combinatie met SQL. U definieert een opgeslagen procedure die wordt opgehaald van de argumenten van de gebruiker, en vervolgens de argumenten doorgeven in de R-script als variabelen.

```sql
CREATE PROCEDURE MyRNorm (
    @param1 INT
    , @param2 INT
    , @param3 INT
    )
AS
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- as.data.frame(rnorm(mynumbers, mymean, mysd));
'
    , @input_data_1 = N'   ;'
    , @params = N' @mynumbers int, @mymean int, @mysd int'
    , @mynumbers = @param1
    , @mymean = @param2
    , @mysd = @param3
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

- De eerste regel definieert elk van de SQL-invoerparameters die vereist zijn wanneer de opgeslagen procedure wordt uitgevoerd.

- De regel die begint met `@params` alle variabelen die worden gebruikt door de R-code en de bijbehorende SQL-gegevenstypen definiëren.

- De namen van de SQL-parameters toewijzen de regels die onmiddellijk volgen aan de bijbehorende namen van de R-variabelen.

Nu dat u de R-functie in een opgeslagen procedure ingepakt hebt, kunt u eenvoudig de functie aanroepen en doorgeven in verschillende waarden, zoals deze:

```sql
EXECUTE MyRNorm @param1 = 100
    , @param2 = 50
    , @param3 = 3
```

## <a name="use-r-utility-functions-for-troubleshooting"></a>R-hulpprogrammafuncties gebruiken voor het oplossen van problemen

De `utils` pakket, wordt standaard geïnstalleerd, biedt tal van functies voor het onderzoeken van de huidige R-omgeving. Deze functies is handig als u bent afwijkingen vinden in de manier waarop die uw R-code wordt uitgevoerd in SQL en externe omgevingen. Bijvoorbeeld, kunt u het R `memory.limit()` functie voor het ophalen van geheugen voor de huidige R-omgeving.

Omdat de `utils` pakket is geïnstalleerd, maar niet geladen standaard, moet u de `library()` functie eerst laden.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
library(utils);
mymemory <- memory.limit();
OutputDataSet <- as.data.frame(mymemory);
'
    , @input_data_1 = N' ;'
WITH RESULT SETS(([Col1] INT NOT NULL));
```

> [!TIP]
> Veel gebruikers willen gebruiken de timing systeemfuncties in R, zoals `system.time` en `proc.time`, zodat de tijd die worden gebruikt door de R-processen vastleggen en analyseren van prestatieproblemen.
