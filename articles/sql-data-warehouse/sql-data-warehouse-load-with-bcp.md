---
title: Gegevens laden in SQL Data Warehouse met behulp van BCP | Microsoft Docs
description: Informatie over BCP en het gebruik van BCP voor datawarehousescenario's.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: f9467d11-fcd6-4131-a65a-2022d2c32d24
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 01/22/2018
ms.author: cakarst;barbkess
ms.openlocfilehash: 146c6fdada651551c05b2cbcadc3e1248a40b613
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
# <a name="load-data-with-bcp"></a>Gegevens laden met BCP

**[BCP](/sql/tools/bcp-utility.md)** is een opdrachtregelprogramma voor het bulksgewijs laden van gegevens. Hiermee kunt u gegevens kopiëren van en naar SQL Server, gegevensbestanden en SQL Data Warehouse. Gebruikt BCP om veel rijen in SQL Data Warehouse-tabellen te importeren of gegevens uit SQL Server-tabellen te exporteren naar gegevensbestanden. Voor het gebruik van BCP is geen kennis van Transact-SQL vereist, behalve wanneer u de optie queryout wilt gebruiken.

Met BCP kunt u snel en eenvoudig kleinere gegevenssets verplaatsen van en naar een SQL Data Warehouse-database. De exacte aanbevolen hoeveelheid gegevens die u kunt laden/extraheren met BCP is afhankelijk van uw netwerkverbinding met Azure.  Kleine dimensietabellen kunnen probleemloos worden geladen en direct worden geëxtraheerd met bcp. Polybase, niet bcp, is echter het aanbevolen hulpprogramma voor het laden en extraheren van grote hoeveelheden gegevens. PolyBase is ontworpen voor de uitgebreide parallelle verwerkingsarchitectuur van SQL Data Warehouse.

Met BCP kunt u:

* Gegevens in SQL Data Warehouse laden met behulp van een opdrachtregelprogramma.
* Gegevens in SQL Data Warehouse extraheren met behulp van een opdrachtregelprogramma.

Deze zelfstudie:

* Importeert gegevens in een tabel met behulp van de bcp in-opdracht
* Exporteert gegevens in een tabel met behulp van de bcp out-opdracht

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Een SQL Data Warehouse-database
* bcp- en sqlcmd- opdrachtregelprogramma’s. U kunt deze downloaden via het [Microsoft Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=36433). 

### <a name="data-in-ascii-or-utf-16-format"></a>Gegevens in ASCII- of UTF-16-indeling
Als u deze zelfstudie wilt uitvoeren met uw eigen gegevens, moeten deze zijn gecodeerd in de ASCII- of UTF-16-indeling, omdat de indeling UTF-8 niet wordt ondersteund in BCP. 

UTF-8 wordt wel ondersteund in PolyBase, maar UTF-16 nog niet. Voor het gebruik van bcp voor het exporteren van gegevens en vervolgens PolyBase voor het laden van gegevens, moet u de gegevens transformeren naar UTF-8 na het exporteren uit SQL Server. 

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

Zie [Tabeloverzicht](sql-data-warehouse-tables-overview.md) of de [ CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse.md)-syntaxis voor meer informatie over het maken van een tabel.
 

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
> Denk eraan dat de bestandscodering UTF-8 niet wordt ondersteund in bcp.exe. Gebruik ASCII-bestanden of bestanden met de codering UTF-16 als u bcp.exe gebruikt.
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

De aanvraag zou de volgende resultaten moeten retourneren:

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
Na het laden van gegevens is een laatste stap het maken of bijwerken van statistieken. Dit helpt met het verbeteren van queryprestaties. Zie voor meer informatie [Statistieken](sql-data-warehouse-tables-statistics.md). Het volgende voorbeeld met sqlcmd maakt statistieken voor de tabel die de zojuist geladen gegevens bevat.


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Gegevens uit SQL Data Warehouse exporteren
In deze zelfstudie maakt u een gegevensbestand op basis van een tabel in SQL Data Warehouse. Deze exporteert de gegevens die u hebt geïmporteerd in de vorige sectie. De resultaten gaan naar een bestand met de naam DimDate2_export.txt.

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
Zie [Loading overview](sql-data-warehouse-design-elt-data-loading) (Overzicht Laden) als u het laadproces wilt ontwerpen.  



<!--MSDN references-->



<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
