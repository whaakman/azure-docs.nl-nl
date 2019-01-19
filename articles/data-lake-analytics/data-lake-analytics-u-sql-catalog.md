---
title: Aan de slag met de U-SQL-catalogus in Azure Data Lake Analytics
description: Informatie over het gebruik van de U-SQL-catalogus voor het delen van code en gegevens.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.topic: conceptual
ms.date: 05/09/2017
ms.openlocfilehash: a6faa7037ccbacc0547401dd52bb3b19abd1c474
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54401590"
---
# <a name="get-started-with-the-u-sql-catalog-in-azure-data-lake-analytics"></a>Aan de slag met de U-SQL-catalogus in Azure Data Lake Analytics

## <a name="create-a-tvf"></a>Maken van een TVF

In het vorige U-SQL-script herhaald u het gebruik van EXTRAHEREN om te lezen uit het bronbestand dezelfde. Met de U-SQL tabelwaardefunctie (TVF), kunt u de gegevens voor toekomstig hergebruik bevatten.  

Het volgende script maakt een TVF met de naam `Searchlog()` in de standaard-database en het schema:

```
DROP FUNCTION IF EXISTS Searchlog;

CREATE FUNCTION Searchlog()
RETURNS @searchlog TABLE
(
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
)
AS BEGIN
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
RETURN;
END;
```

Het volgende script laat zien hoe u de TVF die is gedefinieerd in het vorige script gebruiken:

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM Searchlog() AS S
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    TO "/output/SearchLog-use-tvf.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-views"></a>Weergaven maken

Hebt u een enkele query-expressie, in plaats van een TVF kunt u een U-SQL-weergave om in te kapselen die expressie.

Het volgende script maakt u een weergave met de naam `SearchlogView` in de standaard-database en het schema:

```
DROP VIEW IF EXISTS SearchlogView;

CREATE VIEW SearchlogView AS  
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
USING Extractors.Tsv();
```

Het volgende script ziet u het gebruik van de gedefinieerde weergave:

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchlogView
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    TO "/output/Searchlog-use-view.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-tables"></a>Tabellen maken
Als met relationele database-tabellen, met U-SQL kunt u een tabel met een vooraf gedefinieerd schema maken of een tabel maken die welk schema van de query bepaalt die in de tabel (ook wel bekend als CREATE TABLE AS SELECT of CTAS) Hiermee wordt gevuld.

Een database en de twee tabellen maken met behulp van het volgende script:

```
DROP DATABASE IF EXISTS SearchLogDb;
CREATE DATABASE SearchLogDb;
USE DATABASE SearchLogDb;

DROP TABLE IF EXISTS SearchLog1;
DROP TABLE IF EXISTS SearchLog2;

CREATE TABLE SearchLog1 (
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string,

            INDEX sl_idx CLUSTERED (UserId ASC)
                DISTRIBUTED BY HASH (UserId)
);

INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

CREATE TABLE SearchLog2(
    INDEX sl_idx CLUSTERED (UserId ASC)
            DISTRIBUTED BY HASH (UserId)
) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here
```

## <a name="query-tables"></a>Querytabellen
U kunt tabellen, zoals die zijn gemaakt in het vorige script, op dezelfde manier dat u de gegevensbestanden query opvragen. In plaats van het maken van een rijenset met behulp van EXTRACT, kunt nu u verwijzen naar de naam van de tabel.

Om te lezen uit de tabellen, wijzigt u de transformatie-script dat u eerder hebt gebruikt:

```
@rs1 =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchLogDb.dbo.SearchLog2
GROUP BY Region;

@res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

OUTPUT @res
    TO "/output/Searchlog-query-table.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

 >[!NOTE]
 >U kunt een SELECT op dit moment uitvoeren op een tabel in hetzelfde script als het account dat waar u de tabel is gemaakt.

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [U-SQL-scripts ontwikkelen met Data Lake-hulpmiddelen voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Azure Data Lake Analytics-taken bewaken en problemen oplossen met Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
