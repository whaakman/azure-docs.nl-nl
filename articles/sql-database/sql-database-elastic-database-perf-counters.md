---
title: Prestatiemeteritems voor shard-toewijzingsbeheer
description: ShardMapManager klasse- en afhankelijke routering prestatiemeteritems
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: b090aba0-2e30-454c-96b3-dffa281f539a
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: ddove
ms.openlocfilehash: 60bb26fe6833998137ede71b363d5d40479a4c60
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="performance-counters-for-shard-map-manager"></a>Prestatiemeteritems voor shard-toewijzingsbeheer
U kunt de prestaties van vastleggen een [shard kaart manager](sql-database-elastic-scale-shard-map-management.md), met name wanneer u [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md). Prestatiemeteritems worden gemaakt met de methoden van de klasse Microsoft.Azure.SqlDatabase.ElasticScale.Client.  

Items worden gebruikt voor het bijhouden van de prestaties van [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) bewerkingen. Deze items zijn toegankelijk in de Prestatiemeter onder de categorie 'Elastische Database: Shard Management'.

**Voor de nieuwste versie:** gaat u naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Zie ook [upgraden van een app voor het gebruik van de meest recente clientbibliotheek voor elastische database](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Vereisten
* Voor het maken van de categorie van prestatiemeteritem en prestatiemeteritems, moet de gebruiker een deel van de lokale **beheerders** groep op de computer die als host fungeert voor de toepassing.  
* Als u wilt maken van een exemplaar van prestatiemeteritem en de tellers bijwerken, moet de gebruiker zijn lid van de **beheerders** of **Prestatiemetergebruikers** groep. 

## <a name="create-performance-category-and-counters"></a>Categorie van prestatiemeteritem en prestatiemeteritems maken
Voor het maken van de tellers, roept u de methode CreatePeformanceCategoryAndCounters van de [ShardMapManagmentFactory klasse](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx). Alleen beheerders kan de methode uitvoeren: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

U kunt ook [dit](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) PowerShell-script voor de methode uit te voeren. De methode maakt u de volgende prestatiemeteritems:  

* **In de cache opgeslagen toewijzingen**: aantal toewijzingen in de cache opgeslagen voor de shard-kaart.
* **DDR-bewerkingen per seconde**: het aantal afhankelijke routering gegevensbewerkingen voor de shard-kaart. Deze teller wordt bijgewerkt wanneer een aanroep van [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) resulteert in een succesvolle verbinding met de shard bestemming. 
* **Toewijzing van lookup cache treffers per seconde**: verhouding van geslaagde cache lookup bewerkingen voor toewijzingen in de shard-toewijzing. 
* **Toewijzing van lookup cache missers per seconde**: het aantal mislukte cache lookup bewerkingen voor toewijzingen in de shard-toewijzing.
* **Toewijzingen toegevoegd of bijgewerkt in de cache per seconde**: snelheid welke toewijzingen worden toegevoegd of bijgewerkt in cache voor de shard-kaart. 
* **Toewijzingen verwijderd uit de cache per seconde**: snelheid waarmee de toewijzingen worden verwijderd uit de cache voor de shard-kaart. 

Prestatiemeteritems zijn gemaakt voor elke kaart in de cache shard per proces.  

## <a name="notes"></a>Opmerkingen
De volgende gebeurtenissen worden geactiveerd door het maken van de prestatiemeteritems:  

* Initialisatie van de [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) met [enthousiaste laden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), als de ShardMapManager shard-kaarten bevat. Deze omvatten de [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) en de [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) methoden.
* Geslaagde opzoeken van een shard-toewijzing (met behulp van [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) of [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 
* Het is gemaakt van shard-toewijzing met CreateShardMap().

De prestatiemeteritems worden bijgewerkt door alle cachebewerkingen die worden uitgevoerd op de shard-toewijzing en toewijzingen. Geslaagde verwijdering van de shard-toewijzing met DeleteShardMap () reults in verwijdering van de prestatie-items-exemplaar.  

## <a name="best-practices"></a>Aanbevolen procedures
* Het maken van de categorie van prestatiemeteritem en prestatiemeteritems moet slechts één keer worden uitgevoerd voordat het maken van ShardMapManager-object. Elke uitvoering van de opdracht CreatePerformanceCategoryAndCounters() Hiermee wist u de vorige tellers (verlies van gegevens die zijn gerapporteerd door alle exemplaren) en nieuwe regels worden gemaakt.  
* Prestaties teller exemplaren worden per proces gemaakt. Een toepassing is vastgelopen of het verwijderen van een shard-toewijzing uit de cache leidt tot verwijdering van de prestaties tellers exemplaren.  

### <a name="see-also"></a>Zie ook
[Overzicht van de functies elastische Database](sql-database-elastic-scale-introduction.md)  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

