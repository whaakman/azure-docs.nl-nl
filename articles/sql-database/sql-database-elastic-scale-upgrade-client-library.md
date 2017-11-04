---
title: Upgrade naar de meest recente clientbibliotheek voor elastische database | Microsoft Docs
description: Gebruik Nuget om clientbibliotheek upgrade elastische database.
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 0a546510-76e7-465e-9271-f15ff0cfa959
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: ddove
ms.openlocfilehash: 62609f2488a01d9b406c6b730c53ecc9f9b6ea1b
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>Upgrade van een app voor het gebruik van de meest recente clientbibliotheek voor elastische database
Nieuwe versies van de [clientbibliotheek voor elastische Database](sql-database-elastic-database-client-library.md) zijn beschikbaar via de interface NuGetPackage Manager in Visual Studio NuGetand. Upgrades bevatten oplossingen voor problemen en ondersteuning voor nieuwe mogelijkheden van de clientbibliotheek.

**Voor de nieuwste versie:** gaat u naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Opnieuw maken van uw toepassing met de nieuwe bibliotheek, evenals wijzigen van uw bestaande Shard kaart Manager metagegevens die zijn opgeslagen in uw Azure SQL-Databases naar de nieuwe functies ondersteunen.

Deze stappen in volgorde uitvoert, zorgt u ervoor dat oude versies van de clientbibliotheek niet langer aanwezig zijn in uw omgeving zijn wanneer metagegevensobjecten zijn bijgewerkt, wat betekent dat de van de oude versie metagegevensobjecten won't worden gemaakt na de upgrade.   

## <a name="upgrade-steps"></a>Upgradestappen
**1. Werk uw toepassingen.** In Visual Studio downloaden en verwijzen naar de meest recente versie van de client-bibliotheek in alle ontwikkelingsprojecten die gebruikmaken van de bibliotheek; vervolgens opnieuw maken en implementeren. 

* Selecteer in Visual Studio-oplossing **extra** --> **NuGet Package Manager** -->  **NuGet-pakketten beheren voor oplossing**. 
* (Visual Studio 2013) Selecteer in het linkerdeelvenster **Updates**, en selecteer vervolgens de **Update** knop op het pakket **Azure SQL Database-clientbibliotheek met elastische Scale** dat wordt weergegeven in het venster.
* (Visual Studio 2015) Het Filter ingesteld op **Upgrade beschikbaar**. Selecteer het pakket bijwerken en klik op de **bijwerken** knop.
* (Visual Studio 2017) Selecteer aan de bovenkant van het dialoogvenster **Updates**. Selecteer het pakket bijwerken en klik op de **bijwerken** knop.
* Bouwen en implementeren. 

**2. Werk uw scripts.** Als u **PowerShell** scripts voor het beheren van shards, [downloaden van de nieuwe bibliotheekversie](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) en kopieer dit naar de map waarin u scripts uitvoeren. 

**3. Upgrade uw service gesplitste samenvoegen.** Als u het hulpprogramma van de gesplitste samenvoegen elastische database gebruiken voor het ordenen van gedeelde gegevens [downloaden en implementeren van de meest recente versie van het hulpprogramma](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/). Gedetailleerde stappen upgrade voor de Service vindt u [hier](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. Upgrade van uw databases Shard kaart Manager**. Werk de metagegevens van de Shard-kaarten ondersteunende in Azure SQL Database.  Er zijn twee manieren waarop u kunt dit doen met PowerShell of C#. Beide opties worden hieronder weergegeven.

***Optie 1: Upgrade metagegevens met behulp van PowerShell***

1. Download de meest recente opdrachtregelprogramma voor NuGet van [hier](http://nuget.org/nuget.exe) en naar een map op te slaan. 
2. Open een opdrachtprompt, navigeer naar dezelfde map en voert u de opdracht:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`
3. Navigeer naar de submap met de nieuwe clientversie dll-bestand dat die u hebt zojuist hebt gedownload, bijvoorbeeld:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`
4. Downloaden van de client elastische database scriptlet upgrade van de [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), en sla het bestand in dezelfde map met het DLL-bestand.
5. 'PowerShell.\upgrade.ps1' uitvoeren vanaf de opdrachtprompt en volg de aanwijzingen van die map.

***Optie 2: Upgrade metagegevens met C#***

U kunt ook een Visual Studio-toepassing maken die uw ShardMapManager wordt geopend, doorloopt over alle shards en voert de upgrade van de metagegevens door het aanroepen van de methoden [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) en [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) zoals in dit voorbeeld: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 

    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Deze technieken voor upgrades van de metagegevens kunnen meerdere malen worden toegepast zonder schade. Bijvoorbeeld, als een oudere clientversie per ongeluk een shard maakt nadat u al hebt bijgewerkt, kunt u uitvoeren upgrade opnieuw over alle shards om ervoor te zorgen dat de meest recente metagegevensversie in uw infrastructuur aanwezig is. 

**Opmerking:** nieuwe versies van de clientbibliotheek gepubliceerd tot datum blijven werken met eerdere versies van de metagegevens van de Manager van Shard-toewijzing op Azure SQL DB en vice versa.   Maar om te profiteren van enkele van de nieuwe functies in de meest recente client, metagegevens moet worden bijgewerkt.   Houd er rekening mee dat upgrades van de metagegevens niet van invloed op alle gebruikersgegevens of toepassingsspecifieke gegevens, alleen objecten door de Shard-toewijzing Manager gemaakt en gebruikt.  En toepassingen blijven functioneren tot en met het upgradeproces die hierboven worden beschreven. 

## <a name="elastic-database-client-version-history"></a>Versiegeschiedenis van client elastische database
Versiegeschiedenis, gaat u naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png

