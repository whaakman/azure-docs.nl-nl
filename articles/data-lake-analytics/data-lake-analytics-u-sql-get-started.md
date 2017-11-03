---
title: Aan de slag met U-SQL-taal | Microsoft Docs
description: Leer de basisbeginselen van de U-SQL-taal.
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/23/2017
ms.author: saveenr
ms.openlocfilehash: 38c4e1b9bd24ef0b8a81f6154620f3f98d3b5ac1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-u-sql"></a>Aan de slag met U-SQL
U-SQL is een taal die combineert declaratieve SQL met imperatieve C# om u te laten verwerken van gegevens op elke schaal. Via de functie schaalbare, gedistribueerde query van U-SQL, kunt u gegevens efficiënt te analyseren in relationele winkels zoals Azure SQL Database. U kunt met de U-SQL, ongestructureerde gegevens verwerken met schema toepassen op lezen en het invoegen van aangepaste regels en UDF's. Daarnaast bevat U-SQL uitbreidbaarheid waarmee u fijnmazig controle over het uitvoeren van op grote schaal. 

## <a name="learning-resources"></a>Trainingsmateriaal

* De [U-SQL-zelfstudie](http://aka.ms/usqltutorial) biedt aanwijzingen van de meeste van de U-SQL-taal. Dit document wordt aanbevolen voor alle ontwikkelaars willen U-SQL te leren lezen.
* Voor gedetailleerde informatie over de **U-SQL-syntaxis**, Zie de [naslaginformatie U-SQL-taal](http://go.microsoft.com/fwlink/p/?LinkId=691348).
* Om te begrijpen de **U-SQL-ontwerpplan**, Zie het blogbericht Visual Studio [introductie van U-SQL: een taal die gemakkelijk grote gegevensverwerking](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

## <a name="prerequisites"></a>Vereisten

Voordat u de U-SQL-voorbeelden in dit document doorloopt, lezen en voltooien [zelfstudie: U-SQL-scripts ontwikkelen met Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md). Deze zelfstudie wordt uitgelegd hoe het mechanisme van het gebruik van U-SQL met Azure Data Lake Tools voor Visual Studio.

## <a name="your-first-u-sql-script"></a>Uw eerste U-SQL-script

Het volgende U-SQL-script is eenvoudig en kan we verkennen veel aspecten van de U-SQL-taal.

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

Dit script is geen transformatie stappen. Gelezen uit het bronbestand aangeroepen `SearchLog.tsv`, het schematizes en de rijenset terug naar een bestand met de naam SearchLog-first-u sql.csv schrijft.

Let op het vraagteken naast de gegevens typt in het `Duration` veld. Betekent dit dat de `Duration` veld kan niet null zijn.

### <a name="key-concepts"></a>Belangrijkste concepten
* **Variabelen voor de rijenset**: elke query-expressie die een rijenset produceert kan worden toegewezen aan een variabele. U-SQL volgt de TSQL-variabele naamgevingspatroon (`@searchlog`, bijvoorbeeld) in het script.
* De **EXTRAHEREN** sleutelwoord leest de gegevens uit een bestand en definieert u het schema bij lezen. `Extractors.Tsv`is een ingebouwde U-SQL zelfstandig uitpakken voor bestanden tabblad's gescheiden waarden. U kunt aangepaste extractie ontwikkelen.
* De **uitvoer** schrijft gegevens uit een rijenset naar een bestand. `Outputters.Csv()`is een ingebouwde outputter U-SQL te maken van een bestand met door komma's gescheiden waarden. U kunt aangepaste outputters ontwikkelen.

### <a name="file-paths"></a>Bestandspaden

De instructies uitpakken en uitvoer bestandspaden gebruiken. Absolute of relatieve zijn paden:

Deze volgende absoluut bestandspad verwijst naar een bestand in een Data Lake Store met de naam `mystore`:

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

Deze volgende bestandspad begint met `"/"`. Het verwijst naar een bestand in de standaard Data Lake Store-account:

    /output/SearchLog-first-u-sql.csv

## <a name="use-scalar-variables"></a>Scalaire variabelen gebruiken

U kunt ook scalaire variabelen gebruiken om uw script-onderhoud te vereenvoudigen. Het vorige U-SQL-script kan ook worden opgeslagen als:

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

Gebruik **Selecteer** voor het transformeren van rijensets:

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

De WHERE-component gebruikt een [C# Boole-expressie](https://msdn.microsoft.com/library/6a71f45d.aspx). De taal C#-expressie kunt u uw eigen expressies en functies doen. U kunt zelfs uitvoeren complexere filteren ze te combineren met logische voegwoorden (programmapad en) en disjunctions (ORs).

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
 >De tweede query wordt uitgevoerd op het resultaat van de eerste rijenset die wordt gemaakt van een samenstelling van twee filters. U kunt ook een variabelenaam hergebruiken en de namen lexically zijn gericht.

## <a name="aggregate-rowsets"></a>Cumulatieve rijensets
U-SQL biedt u de vertrouwde ORDER BY, GROUP BY en aggregaties.

De volgende query vindt de totale duur per regio en wordt vervolgens de bovenste vijf duur in volgorde.

U-SQL-rijensets blijven niet behouden in de volgorde voor de volgende query. Als u wilt bestellen uitvoer, moet u dus ORDER BY toevoegen aan de uitvoer-instructie:

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

De component ORDER BY in de U-SQL, moet u de component FETCH in een SELECT-expressie.

De component U-SQL die kan worden gebruikt te beperken tot de uitvoer van de groepen die voldoen aan de HAVING-voorwaarde:

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

Voor geavanceerde aggregatie-scenario's, Zie de documentatie van de verwijzing naar de U-SQL voor [statistische analyse- en verwijzingstypen van functies](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [U-SQL-scripts ontwikkelen met Data Lake-hulpmiddelen voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
