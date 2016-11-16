---
title: Gegevens vanuit csv-bestand laden in Azure SQL-database (bcp) | Microsoft Docs
description: Maak bij een kleine gegevensomvang gebruik van bcp om gegevens in de Azure SQL-database te importeren.
services: sql-database
documentationcenter: NA
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 875f9b8d-f1a1-4895-b717-f45570fb7f80
ms.service: sql-database
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 09/13/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 8d6e201ba5fb8283d883272e0cb2b8e7c11f43e5


---
# <a name="load-data-from-csv-into-azure-sql-data-warehouse-flat-files"></a>Gegevens vanuit csv laden in Azure SQL Data Warehouse (platte bestanden)
U kunt het opdrachtregelhulpprogramma bcp gebruiken om gegevens uit een csv-bestand te importeren in Azure SQL Database.

## <a name="before-you-begin"></a>Voordat u begint
### <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Een logische Azure SQL-databaseserver en -database
* Het opdrachtregelprogramma BCP (moet zijn geïnstalleerd)
* Het opdrachtregelprogramma SQLCMD (moet zijn geïnstalleerd)

U kunt de opdrachtregelprogramma's BCP en SQLCMD downloaden van het [Microsoft Downloadcentrum][Microsoft Downloadcentrum].

### <a name="data-in-ascii-or-utf16-format"></a>Gegevens in ASCII- of UTF-16-indeling
Als u deze zelfstudie wilt uitvoeren met uw eigen gegevens, moeten deze zijn gecodeerd in de ASCII- of UTF-16-indeling, omdat de indeling UTF-8 niet wordt ondersteund in BCP. 

## <a name="1-create-a-destination-table"></a>1. Een doeltabel maken
Definieer een tabel in SQL Database als de doeltabel. De kolommen in de tabel moeten overeenkomen met de gegevens in elke rij van het gegevensbestand.

Open een opdrachtprompt en voer de volgende opdracht uit met sqlcmd.exe om een tabel te maken:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
"
```


## <a name="2-create-a-source-data-file"></a>2. Een brongegevensbestand maken
Open Kladblok, kopieer de volgende regels met gegevens naar een nieuw tekstbestand en sla dit bestand op in de lokale tijdelijke map C:\Temp\DimDate2.txt. Dit zijn gegevens in ASCII-indeling.

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

(Optioneel) Open een opdrachtprompt en voer de volgende opdracht uit om uw eigen gegevens uit een SQL Server-database te exporteren. Vervang TableName, ServerName, DatabaseName, Username en Password door uw eigen waarden.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```

## <a name="3-load-the-data"></a>3. De gegevens laden
Open een opdrachtprompt en voer de volgende opdracht uit om de gegevens te laden. Vervang de waarden voor ServerName, DatabaseName, Username en Password door uw eigen waarden.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Gebruik deze opdracht om te controleren of de gegevens correct zijn geladen

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

De resultaten horen er als volgt uit te zien:

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

## <a name="next-steps"></a>Volgende stappen
Zie [SQL Server-database migreren](sql-database-cloud-migrate.md) om een SQL-serverdatabase te migreren.

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[Syntaxis voor CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Downloadcentrum]: https://www.microsoft.com/download/details.aspx?id=36433



<!--HONumber=Nov16_HO2-->


