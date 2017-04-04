---
title: Gegevens uit SQL Server laden in Azure SQL Data Warehouse (PolyBase) | Microsoft Docs
description: Gebruikt bcp om gegevens uit SQL Server te exporteren naar platte bestanden, AZCopy om gegevens te importeren in een Azure Blob Storage en PolyBase om de gegevens op te nemen in Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4d42786a-fb28-43c9-9c3b-72d19c0ecc11
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 21b4cc704e271ac220fd606305f8f97c9b2593bb
ms.lasthandoff: 03/22/2017



---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-azcopy"></a>Gegevens uit SQL Server laden in Azure SQL Data Warehouse (AZCopy)
Gebruik de opdrachtregelprogramma's BCP en AZCopy om gegevens uit SQL Server te laden in Azure Blob-opslag. Gebruik vervolgens PolyBase of Azure Data Factory om de gegevens in Azure SQL Data Warehouse te laden. 

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Een SQL Data Warehouse-database
* Het opdrachtregelprogramma BCP (moet zijn geïnstalleerd)
* Het opdrachtregelprogramma SQLCMD (moet zijn geïnstalleerd)

> [!NOTE]
> U kunt de opdrachtregelprogramma's BCP en SQLCMD downloaden van het [Microsoft Downloadcentrum][Microsoft Download Center].
> 
> 

## <a name="import-data-into-sql-data-warehouse"></a>Gegevens importeren in SQL Data Warehouse
In deze zelfstudie maakt u een tabel in Azure SQL Data Warehouse en importeert u gegevens in de tabel.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Stap 1: een tabel maken in Azure SQL Data Warehouse
Open een opdrachtprompt en voer de volgende query voor het maken van een tabel op uw exemplaar uit met SQLCMD:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```

> [!NOTE]
> Zie [Tabeloverzicht][Table Overview] of [Syntaxis voor CREATE TABLE][CREATE TABLE syntax] voor meer informatie over het maken van een tabel in SQL Data Warehouse en de beschikbare opties in de WITH-clausule.
> 
> 

### <a name="step-2-create-a-source-data-file"></a>Stap 2: een brongegevensbestand maken
Open Kladblok, kopieer de volgende regels met gegevens naar een nieuw tekstbestand en sla dit bestand op in de lokale tijdelijke map C:\Temp\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

> [!NOTE]
> Denk eraan dat de bestandscodering UTF-8 niet wordt ondersteund in bcp.exe. Gebruik ASCII-bestanden of bestanden met de bestandscodering UTF-16 als u bcp.exe gebruikt.
> 
> 

### <a name="step-3-connect-and-import-the-data"></a>Stap 3: verbinding maken en de gegevens importeren
Met BCP kunt u verbinding maken en de gegevens importeren met de volgende opdracht, waarbij u de waarden waar nodig vervangt:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

U kunt controleren of de gegevens zijn geladen door de volgende query uit te voeren met SQLCMD:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Hierdoor zouden de volgende resultaten moeten worden geretourneerd:

| DateId | CalendarQuarter | FiscalQuarter |
| --- | --- | --- |
| 20150101 |1 |3 |
| 20150201 |1 |3 |
| 20150301 |1 |3 |
| 20150401 |2 |4 |
| 20150501 |2 |4 |
| 20150601 |2 |4 |
| 20150701 |3 |1 |
| 20150801 |3 |1 |
| 20150801 |3 |1 |
| 20151001 |4 |2 |
| 20151101 |4 |2 |
| 20151201 |4 |2 |

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Stap 4: statistieken maken voor uw zojuist geladen gegevens
Azure SQL Data Warehouse bevat nog geen functionaliteit voor het automatisch maken of bijwerken van statistieken. Voor optimale resultaten van uw query's is het belangrijk dat u statistieken maakt voor alle kolommen van alle tabellen nadat de gegevens voor het eerst zijn geladen of wanneer de gegevens substantieel zijn gewijzigd. Zie het onderwerp [Statistieken][Statistics] in de groep onderwerpen voor ontwikkelaars voor gedetailleerde uitleg van statistieken. Hieronder ziet u een kort voorbeeld van het maken van statistieken voor de tabellen die zijn geladen in dit voorbeeld

Voer de volgende CREATE STATISTICS-instructies uit vanaf een SQLCMD-opdrachtprompt:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Gegevens uit SQL Data Warehouse exporteren
In deze zelfstudie maakt u een gegevensbestand op basis van een tabel in SQL Data Warehouse. U exporteert de eerder gemaakte gegevens naar een nieuw gegevensbestand met de naam DimDate2_export.txt.

### <a name="step-1-export-the-data"></a>Stap 1: de gegevens exporteren
Met het hulpprogramma BCP kunt u verbinding maken en gegevens exporteren met de volgende opdracht, waarbij u de waarden waar nodig vervangt:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
U kunt controleren of de gegevens correct zijn geëxporteerd door het nieuwe bestand te openen. De gegevens in het bestand moeten overeenkomen met de onderstaande tekst:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

> [!NOTE]
> Vanwege de aard van gedistribueerde systemen kan de gegevensvolgorde per SQL Data Warehouse-database verschillen. In plaats van de hele tabel te exporteren kunt u ook een query voor het extraheren van gegevens schrijven met de functie **queryout** van BCP.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Zie [Gegevens laden in SQL Data Warehouse][Load data into SQL Data Warehouse] voor een overzicht van het laden.
Zie [Overzicht van SQL Data Warehouse voor ontwikkelaars][SQL Data Warehouse development overview] voor meer tips voor ontwikkelaars.

<!--Image references-->

<!--Article references-->

[Load data into SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Table Overview]: ./sql-data-warehouse-tables-overview.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433

