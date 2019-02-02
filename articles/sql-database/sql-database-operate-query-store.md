---
title: Operationele Query Store in Azure SQL-Database
description: Meer informatie over het uitvoeren van de Query Store in Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 12/19/2018
ms.openlocfilehash: 3ceb8569d952f2947870ce7314f869623b2d87f9
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55562943"
---
# <a name="operating-the-query-store-in-azure-sql-database"></a>Besturingssysteem van de Query Store in Azure SQL-Database

Query Store in Azure is een volledig beheerde database-functie die voortdurend worden verzameld en geeft gedetailleerde historische informatie over alle query's. U kunt zien over de Query Store vergelijken met van een vliegtuig flight data recorder die aanzienlijk eenvoudiger queryprestaties probleemoplossing voor cloud en on-premises-klanten. In dit artikel wordt uitgelegd dat specifieke aspecten van het besturingssysteem van de Query Store in Azure. Met deze vooraf verzamelde querygegevens, kunt u snel vaststellen en dus houd meer tijd te focussen op hun bedrijf en oplossen van problemen met prestaties. 

Query Store is [wereldwijd beschikbaar](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) in Azure SQL Database sinds November 2015. Query Store vormt de basis voor de analyse van prestaties en afstemmen van functies, zoals [SQL Database Advisor en Prestatiedashboard](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Op het moment van publicatie van dit artikel wordt wordt Query Store uitgevoerd in meer dan 200.000 gebruikersdatabases in Azure, verzamelen van informatie met betrekking tot query voor enkele maanden, zonder onderbreking.

> [!IMPORTANT]
> Microsoft wordt momenteel Query Store activeren voor alle Azure SQL-databases (bestaande en nieuwe). 

## <a name="optimal-query-store-configuration"></a>Configuratie van de optimale Query Store

Deze sectie wordt beschreven optimale standaardconfiguratie-instellingen die zijn ontworpen om ervoor te zorgen betrouwbare werking van de Query Store en afhankelijke onderdelen, zoals [SQL Database Advisor en Prestatiedashboard](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Standaard-configuratie is geoptimaliseerd voor continu gegevens te verzamelen, die weinig tijd besteed in OFF/READ_ONLY Staten.

| Configuratie | Description | Standaard | Opmerking |
| --- | --- | --- | --- |
| MAX_STORAGE_SIZE_MB |Hiermee geeft u de limiet voor de gegevensruimte op de Query Store kunt uitvoeren in de klantendatabase |100 |Afgedwongen voor nieuwe databases |
| INTERVAL_LENGTH_MINUTES |Bepaalt de grootte van de periode gedurende welke verzamelde runtime-statistieken voor query-plannen worden samengevoegd en opgeslagen. Elke actieve queryplan is maximaal één rij voor een bepaalde periode gedefinieerd met deze configuratie |60 |Afgedwongen voor nieuwe databases |
| STALE_QUERY_THRESHOLD_DAYS |Op basis van tijd opschoningsbeleid die de bewaarperiode van de persistente runtime-statistieken en niet-actieve query's bepaalt |30 |Afgedwongen voor nieuwe databases en databases met een eerdere standaard (367) |
| SIZE_BASED_CLEANUP_MODE |Hiermee geeft u op of automatische opruimen plaats vindt wanneer de grootte van Query Store de limiet nadert |AUTO |Afgedwongen voor alle databases |
| QUERY_CAPTURE_MODE |Hiermee geeft u op of alle query's of slechts een subset van query's worden bijgehouden |AUTO |Afgedwongen voor alle databases |
| FLUSH_INTERVAL_SECONDS |Hiermee geeft u de maximale periode gedurende welke de runtime-statistieken worden bewaard in het geheugen voor leegmaken naar schijf vastgelegd |900 |Afgedwongen voor nieuwe databases |
|  | | | |

> [!IMPORTANT]
> Deze standaardinstellingen worden automatisch toegepast in de laatste fase van de activering van de Query Store in alle Azure SQL-databases (Zie voorgaande belangrijke opmerking). Na deze licht van, wordt niet op Azure SQL Database,-configuratiewaarden die zijn ingesteld door klanten, tenzij ze een negatieve invloed zijn op primaire werkbelasting of betrouwbare bewerkingen van de Query Store worden gewijzigd.

Als u blijven met uw aangepaste instellingen wilt, gebruikt u [ALTER DATABASE met opties voor Query Store](https://msdn.microsoft.com/library/bb522682.aspx) om terug te zetten van de configuratie van de vorige status. Bekijk [Best Practices met de Query Store](https://msdn.microsoft.com/library/mt604821.aspx) om te leren hoe boven gekozen optimale configuratieparameters.

## <a name="next-steps"></a>Volgende stappen

[SQL Database Performance Insight](sql-database-performance.md)

## <a name="additional-resources"></a>Aanvullende resources

Meer informatie Bekijk voor de volgende artikelen:

- [Een flight data recorder voor uw database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database)
- [Prestaties controleren via de Query Store](https://msdn.microsoft.com/library/dn817826.aspx)
- [Query Store-gebruiksscenario's](https://msdn.microsoft.com/library/mt614796.aspx)
