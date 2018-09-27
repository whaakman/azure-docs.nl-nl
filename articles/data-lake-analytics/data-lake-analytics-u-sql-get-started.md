---
title: Aan de slag met U-SQL-taal in Azure Data Lake Analytics
description: Leer de basisprincipes van de U-SQL-taal in Azure Data Lake Analytics.
services: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 06/23/2017
ms.openlocfilehash: daef00bc7b39326b79e5f56bf803dc6aad1ac0ff
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47221931"
---
# <a name="get-started-with-u-sql-in-azure-data-lake-analytics"></a>Aan de slag met U-SQL in Azure Data Lake Analytics
U-SQL is een taal die combineert verklarende SQL met imperatieve C# om u te laten verwerken van gegevens op elke schaal. Via de schaalbare, gedistribueerde query-mogelijkheid van U-SQL kunt u gegevens efficiënter analyseren in relationele winkels, zoals Azure SQL Database. U kunt met U-SQL, ongestructureerde gegevens verwerken door het schema toepassen op lezen en het invoegen van aangepaste logica en UDF's. U-SQL omvat daarnaast uitbreidbaarheid waarmee u heel nauwkeurig bepalen hoe u kunt om uit te voeren op schaal. 

## <a name="learning-resources"></a>Learning-resources

* De [U-SQL-zelfstudie](http://aka.ms/usqltutorial) biedt aanwijzingen van de meeste van de U-SQL-taal. Dit document wordt aanbevolen voor alle ontwikkelaars die voor meer informatie over U-SQL lezen.
* Voor gedetailleerde informatie over de **U-SQL-syntaxis**, Zie de [naslaginformatie U-SQL-taal](http://go.microsoft.com/fwlink/p/?LinkId=691348).
* Om te begrijpen de **U-SQL-ontwerpplan**, Zie het blogbericht Visual Studio [Maak kennis met U-SQL: een taal die Big Data-verwerking eenvoudig maakt](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

## <a name="prerequisites"></a>Vereisten

Voordat u de U-SQL-voorbeelden in dit document hebt doorlopen, lezen en uitvoeren [zelfstudie: U-SQL-scripts ontwikkelen met Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md). Deze zelfstudie wordt uitgelegd dat het mechanisme van het gebruik van U-SQL met Azure Data Lake Tools voor Visual Studio.

## <a name="your-first-u-sql-script"></a>Uw eerste U-SQL-script

Het volgende U-SQL-script is eenvoudig en kan we veel aspecten verkennen de U-SQL-taal.

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

OUTPUT @searchlog   
    TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();
```

Met dit script beschikt niet over de transformatiestappen. Deze leest uit de bronbestand met de naam `SearchLog.tsv`, het schematizes en schrijft de rijenset terug in een bestand met de naam SearchLog-eerst-u-sql.csv.

Let op het vraagteken naast de gegevens typt in het `Duration` veld. Dit betekent dat de `Duration` veld kan niet null zijn.

### <a name="key-concepts"></a>Belangrijkste concepten
* **Variabelen voor de rijenset**: elke query-expressie die een rijenset produceert kan worden toegewezen aan een variabele. U-SQL volgt de variabele T-SQL-naamgevingspatroon (`@searchlog`, bijvoorbeeld) in het script.
* De **EXTRAHEREN** trefwoord leest gegevens uit een bestand en bepaalt u het schema op lezen. `Extractors.Tsv` is een ingebouwde U-SQL-extractor voor bestanden van tabblad gescheiden waarden. U kunt aangepaste extractors ontwikkelen.
* De **uitvoer** schrijft gegevens uit een rijenset naar een bestand. `Outputters.Csv()` is een ingebouwde U-SQL-outputter te maken van een bestand met door komma's gescheiden waarden. U kunt aangepaste outputters ontwikkelen.

### <a name="file-paths"></a>Bestandspaden

De instructies uitpakken en uitvoer bestandspaden gebruiken. Bestandspaden kunnen absoluut of relatief zijn:

Deze volgende absolute pad verwijst naar een bestand in een Data Lake Store met de naam `mystore`:

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

Deze volgende pad begint met `"/"`. Deze gegevensset verwijst naar een bestand in het Data Lake Store-account:

    /output/SearchLog-first-u-sql.csv

## <a name="use-scalar-variables"></a>Scalaire variabelen gebruiken

U kunt ook scalaire variabelen gebruiken om uw script onderhoud eenvoudiger. Het vorige U-SQL-script kan ook worden geschreven als:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Rijensets transformeren

Gebruik **Selecteer** rijensets transformeren:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

De WHERE-component gebruikt een [C# Booleaanse expressie](https://msdn.microsoft.com/library/6a71f45d.aspx). De computertaal C#-expressie kunt u uw eigen expressies en functies. U kunt zelfs uitvoeren meer complexe filteren door ze te combineren met logische voegwoorden (and) en disjunctions (or-items).

Het volgende script maakt gebruik van de methode DateTime.Parse() en een combinatie.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datetime.csv"
        USING Outputters.Csv();

 >[!NOTE]
 >De tweede query wordt uitgevoerd op het resultaat van de eerste rijenset, die zorgt voor een samenstelling van de twee filters. U kunt ook opnieuw gebruiken met de naam van een variabele en de namen van de lexicaal zijn gericht.

## <a name="aggregate-rowsets"></a>Cumulatieve rijensets
U-SQL kunt u de vertrouwde ORDER BY, GROUP BY en aggregaties.

De volgende query zoekt de totale duur per regio en geeft de top vijf duur in volgorde.

U-SQL-rijensets blijven niet behouden in de volgorde voor de volgende query. Om te bestellen uitvoer, moet u dus ORDER BY toevoegen aan de uitvoer-instructie:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

De component ORDER BY in de U-SQL, moet u de ophalen-component in een SELECT-expressie.

De component die van U-SQL kan worden gebruikt voor de uitvoer beperkt tot groepen die voldoen aan de HAVING-voorwaarde:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
        GROUP BY Region
        HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Zie voor aggregatie van geavanceerde scenario's, naslagdocumentatie over de U-SQL voor [statistische analyse- en verwijzen naar functies](https://msdn.microsoft.com/library/azure/mt621335.aspx)

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [U-SQL-scripts ontwikkelen met behulp van Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
