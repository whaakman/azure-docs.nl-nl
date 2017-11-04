---
title: Operationele Query Store in Azure SQL Database
description: Meer informatie over het werken met de queryopslag in Azure SQL Database
keywords: 
services: sql-database
documentationcenter: 
author: bonova
manager: jhubbard
editor: 
ms.assetid: 0cccf6bd-1327-44f7-a6f9-8eff0c210463
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: sqldb-performance
ms.workload: Inactive
ms.date: 11/08/2016
ms.author: bonova
ms.openlocfilehash: e57f1c51ef5c551f3b2e5d0f0a51a1f462b6c1af
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="operating-the-query-store-in-azure-sql-database"></a>Besturingssysteem van de queryopslag in Azure SQL Database
Query Store in Azure is een volledig beheerde databasefunctie die voortdurend verzamelt en toont gedetailleerde historische informatie over alle query's. U kunt zien over Query Store als vergelijkbaar met een vliegtuig zwarte gegevens doos die aanzienlijk vereenvoudigt queryprestaties probleemoplossing voor cloud en on-premises-klanten. Dit artikel wordt uitgelegd dat bepaalde aspecten van het besturingssysteem van de Query Store in Azure. Met deze vooraf verzamelde querygegevens kunt u snel te analyseren en oplossen van prestatieproblemen en dus besteden meer tijd aan hun bedrijf. 

Query Store is [wereldwijd beschikbare](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) in Azure SQL Database sinds November 2015 opgehaald. Query Store vormt de basis voor analyse van prestaties en het afstemmen van functies, zoals [SQL Database Advisor en Prestatiedashboard](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Op het moment van publicatie van dit artikel wordt wordt Query Store uitgevoerd in meer dan 200.000 gebruikersdatabases in Azure, verzamelen van informatie voor verschillende maanden, zonder onderbreking van de query gerelateerd.

> [!IMPORTANT]
> Microsoft is bezig te activeren Query Store voor alle Azure SQL-databases (bestaande en nieuwe). 
> 
> 

## <a name="optimal-query-store-configuration"></a>Configuratie voor optimale Query Store
Deze sectie beschrijft optimale standaardconfiguratie-instellingen die zijn ontworpen voor betrouwbare werking van de Query Store en afhankelijke onderdelen, zoals [SQL Database Advisor en Prestatiedashboard](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Standaard-configuratie is geoptimaliseerd voor continue gegevensverzameling, is de minimale tijd besteed aan OFF/READ_ONLY statussen.

| Configuratie | Beschrijving | Standaard | Opmerking |
| --- | --- | --- | --- |
| MAX_STORAGE_SIZE_MB |Hiermee geeft u de limiet voor de ruimte die Query Store kunnen worden uitgevoerd in de klantendatabase |100 |Afgedwongen voor nieuwe databases |
| INTERVAL_LENGTH_MINUTES |Grootte van de periode gedurende welke de verzamelde runtime-statistieken voor queryplannen worden samengevoegd en persistent definieert. Elke actieve queryplan is maximaal één rij voor een bepaalde periode met deze configuratie is gedefinieerd |60 |Afgedwongen voor nieuwe databases |
| STALE_QUERY_THRESHOLD_DAYS |Op basis van tijd opschonen beleid dat de bewaarperiode van persistente runtime-statistieken en niet-actieve query's bepaalt |30 |Afgedwongen voor nieuwe databases en databases met een eerdere standaard (367) |
| SIZE_BASED_CLEANUP_MODE |Hiermee geeft u op of automatische opruimen plaats vindt wanneer de gegevensgrootte van het Queryarchief nadert de limiet |AUTO |Afgedwongen voor alle databases |
| QUERY_CAPTURE_MODE |Hiermee geeft u op of alle query's of alleen een subset van query's worden bijgehouden |AUTO |Afgedwongen voor alle databases |
| FLUSH_INTERVAL_SECONDS |Hiermee geeft u de maximale periode gedurende welke de runtime-statistieken in het geheugen worden bewaard voordat naar schijf vastgelegd |900 |Afgedwongen voor nieuwe databases |
|  | | | |

> [!IMPORTANT]
> Deze standaardwaarden worden automatisch toegepast in de laatste fase van de activering van de Query Store in alle Azure SQL-databases (Zie belangrijke opmerking voorgaande). Na deze licht up won't op Azure SQL Database, configuratiewaarden die door klanten, tenzij ze negatieve invloed hebben op primaire werkbelasting of betrouwbare bewerkingen van de Query Store worden gewijzigd.
> 
> 

Als u blijven met de aangepaste instellingen wilt, gebruikt u [ALTER DATABASE met Query Store opties](https://msdn.microsoft.com/library/bb522682.aspx) configuratie om de vorige status te herstellen. Bekijk [Best Practices met het Queryarchief](https://msdn.microsoft.com/library/mt604821.aspx) om te leren hoe boven gekozen optimale configuratieparameters.

## <a name="next-steps"></a>Volgende stappen
[SQL Database Performance Insight](sql-database-performance.md)

## <a name="additional-resources"></a>Aanvullende bronnen
Voor meer informatie het uitchecken van de volgende artikelen:

* [Een data zwarte doos voor uw database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 
* [De prestaties controleren met behulp van de Query Store](https://msdn.microsoft.com/library/dn817826.aspx)
* [Query Store gebruiksscenario 's](https://msdn.microsoft.com/library/mt614796.aspx)
* [De prestaties controleren met behulp van de Query Store](https://msdn.microsoft.com/library/dn817826.aspx) 

