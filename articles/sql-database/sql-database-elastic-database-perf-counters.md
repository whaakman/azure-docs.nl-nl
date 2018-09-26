---
title: Prestatiemeteritems voor shard-toewijzingsbeheer
description: ShardMapManager klasse- en afhankelijke routering van prestatiemeteritems
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/31/2018
ms.openlocfilehash: d4ecfe700c90beb94455e3607cee4ea30227bd0e
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166215"
---
# <a name="performance-counters-for-shard-map-manager"></a>Prestatiemeteritems voor shard-toewijzingsbeheer
U kunt de prestaties van vastleggen een [shard-Toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md), met name wanneer u [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md). Items worden gemaakt met de methoden van de klasse Microsoft.Azure.SqlDatabase.ElasticScale.Client.  

Items worden gebruikt voor het bijhouden van de prestaties van [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) bewerkingen. Deze items zijn toegankelijk in de Prestatiemeter, onder de categorie 'Elastische Database: Shard Management'.

**Voor de meest recente versie:** gaat u naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Zie ook [een app voor het gebruik van de meest recente-clientbibliotheek voor elastic database upgraden](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Vereisten
* Voor het maken van de prestatiemeteritems en categorie van het prestatiemeteritem, moet de gebruiker een deel van de lokale **beheerders** groep op de computer die als host fungeert voor de toepassing.  
* Voor het maken van een exemplaar van prestatiemeteritem en bijwerken van de prestatiemeteritems, moet de gebruiker lid is van een van beide de **beheerders** of **Prestatiemetergebruikers** groep. 

## <a name="create-performance-category-and-counters"></a>Categorie van de prestaties en de tellers maken
Voor het maken van de prestatiemeteritems, roept u de methode CreatePeformanceCategoryAndCounters van de [ShardMapManagmentFactory klasse](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx). Alleen beheerders kan de methode uitvoeren: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

U kunt ook [dit](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) PowerShell-script voor de methode uit te voeren. De methode maakt u de volgende prestatiemeteritems:  

* **In de cache opgeslagen toewijzingen**: aantal toewijzingen in de cache opgeslagen voor de shard-toewijzing.
* **DDR's per seconde**: het aantal gegevens afhankelijke routering bewerkingen voor de shard-toewijzing. Deze teller wordt bijgewerkt wanneer een aanroep van [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) resulteert in een succesvolle verbinding met de doel-shard. 
* **Toewijzing van lookup cache treffers per seconde**: het aantal geslaagde cache opzoekbewerkingen voor toewijzingen in de shard-toewijzing. 
* **Toewijzing van lookup cache missers per seconde**: het aantal mislukte cache opzoekbewerkingen voor toewijzingen in de shard-toewijzing.
* **Toewijzingen toegevoegd of bijgewerkt in de cache per seconde**: bij welke toewijzingen worden toegevoegd of bijgewerkt in cache voor de shard-toewijzing. 
* **Toewijzingen verwijderd uit de cache per seconde**: snelheid waarmee de toewijzingen worden verwijderd uit de cache voor de shard-toewijzing. 

Prestatiemeteritems worden gemaakt voor elke in de cache shard-toewijzing per proces.  

## <a name="notes"></a>Opmerkingen
De volgende gebeurtenissen activeren voor het maken van de prestatiemeteritems:  

* De initialisatie van de [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) met [enthousiaste laden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), als de ShardMapManager een shard-toewijzingen bevat. Hieronder vallen de [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) en de [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) methoden.
* Geslaagde opzoeken van een shard-toewijzing (met behulp van [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) of [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 
* Met behulp van CreateShardMap()-shard-toewijzing is gemaakt.

De prestatiemeteritems wordt bijgewerkt door alle cachebewerkingen die worden uitgevoerd op de shard-toewijzing en toewijzingen. Geslaagde verwijdering van de shard-toewijzing met DeleteShardMap () reults in het verwijderen van de prestatie-items-exemplaar.  

## <a name="best-practices"></a>Aanbevolen procedures
* Het maken van de prestatiemeteritems en categorie van het prestatiemeteritem moet slechts één keer worden uitgevoerd voordat het maken van ShardMapManager object. Elke uitvoering van de opdracht CreatePerformanceCategoryAndCounters() wist u de vorige items (verlies van gegevens die zijn gerapporteerd door alle exemplaren), en nieuwe regels worden gemaakt.  
* Prestaties teller exemplaren worden per proces gemaakt. Elke toepassing is vastgelopen of verwijdering van een shard-toewijzing uit de cache, leidt verwijdering van de prestaties van tellers instanties.  

### <a name="see-also"></a>Zie ook
[Overzicht over functies voor Elastic Database](sql-database-elastic-scale-introduction.md)  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

