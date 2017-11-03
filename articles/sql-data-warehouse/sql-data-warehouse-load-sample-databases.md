---
title: Voorbeeldgegevens laden in SQL Data Warehouse | Microsoft Docs
description: Voorbeeldgegevens laden in SQL Data Warehouse
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e338ecf8-cfee-419b-b7b6-98108d381c62
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 1e0df958a2f18fe1e988168918e5cfd293f84e64
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="load-sample-data-into-sql-data-warehouse"></a>Voorbeeldgegevens laden in SQL Data Warehouse
Deze eenvoudige stappen om te laden en de voorbeelddatabase van Adventure Works een query. Deze scripts gebruiken eerst sqlcmd uitvoeren van SQL die tabellen weergaven en. Zodra tabellen zijn gemaakt, zal de scripts bcp gebruiken om gegevens te laden.  Als u nog sqlcmd en bcp geïnstalleerd hebt, volgt u deze koppelingen naar [bcp installeren] [ install bcp] en [sqlcmd installeren][install sqlcmd].

## <a name="load-sample-data"></a>Voorbeeldgegevens laden
1. Download de [voorbeeldscripts van Adventure Works voor SQL Data Warehouse] [ Adventure Works Sample Scripts for SQL Data Warehouse] zip-bestand.
2. Pak de bestanden uit het gedownloade zip naar een map op uw lokale computer.
3. De aw_create.bat uitgepakte bestand bewerken en instellen van de volgende variabelen aan de bovenkant van het bestand gevonden.  Zorg ervoor dat geen spatie tussen laat het isgelijkteken (=) en de parameter.  Hieronder vindt u voorbeelden van hoe uw bewerkingen uitzien.
   
    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```
4. Voer de bewerkte aw_create.bat van een Windows-opdrachtprompt.  Zorg ervoor dat u in de map waar u uw bewerkte versie van aw_create.bat opgeslagen.
   Dit script wordt...
   
   * Adventure Works tabellen of weergaven die al bestaan in de database verwijderen
   * De Adventure Works tabellen en weergaven maken
   * Laden van elke Adventure Works tabel met bcp
   * Het aantal rij voor elke tabel Adventure Works valideren
   * Statistieken voor elke kolom voor elke tabel Adventure Works verzamelen

## <a name="query-sample-data"></a>Voorbeeldgegevens query
Wanneer u voorbeeldgegevens in uw SQL Data Warehouse hebt geladen, kunt u snel enkele query's uitvoeren.  Als u wilt een query uitvoert, verbinding maken met de zojuist gemaakte Adventure Works-database in Azure SQL DW met behulp van Visual Studio en SSDT, zoals beschreven in de [query met Visual Studio] [ query with Visual Studio] document.

Voorbeeld van eenvoudige select-instructie om op te halen van de gegevens van de werknemers:

```sql
SELECT * FROM DimEmployee;
```

Voorbeeld van een complexere query constructies zoals GROUP BY om te kijken naar het totaalbedrag voor alle verkoop op elke dag gebruiken:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Voorbeeld van een SELECT met een WHERE-component uitfilteren orders uit voor een bepaalde datum:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

SQL Data Warehouse ondersteunt bijna alle T-SQL-constructs die ondersteuning biedt voor SQL Server.  Eventuele verschillen zijn gedocumenteerd in onze [code migreren] [ migrate code] documentatie.

## <a name="next-steps"></a>Volgende stappen
Nu dat u een aantal query's met voorbeeldgegevens uitproberen hebt, zoeken over [ontwikkelen][develop], [laden][load], of [migreren] [ migrate] naar SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[query with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[migrate code]: sql-data-warehouse-migrate-code.md
[install bcp]: sql-data-warehouse-load-with-bcp.md
[install sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works Sample Scripts for SQL Data Warehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
