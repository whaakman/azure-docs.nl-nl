---
title: Een upgrade uitvoeren naar de meest recente clientbibliotheek van elastische database | Microsoft Docs
description: Gebruik Nuget om te upgraden elastische database-clientbibliotheek.
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
ms.date: 01/03/2019
ms.openlocfilehash: 02c09e5a2ad0c3e23fe641bf445a646477f7db2c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57993974"
---
# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>Een app voor het gebruik van de meest recente-clientbibliotheek voor elastic database upgraden

Nieuwe versies van de [Elastic Database-clientbibliotheek](sql-database-elastic-database-client-library.md) zijn beschikbaar via de interface NuGetPackage beheer in Visual Studio NuGetand. Upgrades bevatten oplossingen voor problemen en ondersteuning voor nieuwe mogelijkheden van de clientbibliotheek.

**Voor de meest recente versie:** Ga naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Opnieuw opbouwen van uw toepassing met de nieuwe-bibliotheek, evenals wijzigen van uw bestaande Shard-Toewijzingsbeheer metagegevens die zijn opgeslagen in uw Azure SQL-databases voor de ondersteuning van nieuwe functies.

Deze stappen uitvoert in volgorde zorgt ervoor dat oude versies van de clientbibliotheek niet meer aanwezig in uw omgeving zijn wanneer de van metagegevensobjecten zijn bijgewerkt, wat betekent dat de van de oude versie metagegevensobjecten na de upgrade wordt niet worden gemaakt.

## <a name="upgrade-steps"></a>Upgradestappen

**1. Upgrade uw toepassingen.** In Visual Studio downloaden en te verwijzen naar de meest recente versie van de client-bibliotheek in al uw ontwikkelingsprojecten die gebruikmaken van de bibliotheek; vervolgens opnieuw maken en implementeren.

* Selecteer in uw Visual Studio-oplossing **extra** --> **NuGet Package Manager** -->  **NuGet-pakketten beheren voor oplossing**.
* (Visual Studio 2013) Selecteer in het linkerdeelvenster **Updates**, en selecteer vervolgens de **Update** knop op het pakket **Azure SQL Database-clientbibliotheek met elastische schaal** die wordt weergegeven in het venster.
* (Visual Studio 2015) Stel in het vak filteren op **Upgrade beschikbaar**. Selecteer het pakket bijwerken en klik op de **bijwerken** knop.
* (Visual Studio 2017) Aan de bovenkant van het dialoogvenster, selecteer **Updates**. Selecteer het pakket bijwerken en klik op de **bijwerken** knop.
* Bouw en implementeer.

**2. Upgrade uw scripts.** Als u **PowerShell** scripts voor het beheren van shards, [downloaden van de nieuwe versie van de bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) en kopieer deze in de map waarin u scripts uit te voeren.

**3. Upgrade uw service voor splitsen en samenvoegen.** Als u de elastic database-hulpprogramma voor splitsen en samenvoegen met opnieuw indelen van gedeelde gegevens [downloaden en implementeren van de meest recente versie van het hulpprogramma](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/). Upgradestappen voor de Service vindt u gedetailleerde [hier](sql-database-elastic-scale-overview-split-and-merge.md).

**4. Upgrade van uw databases Shard-Toewijzingsbeheer**. De metagegevens van de ondersteuning van de Shard-toewijzingen in Azure SQL Database een upgrade uitvoert.  Er zijn twee manieren die u kunt dit doen met behulp van PowerShell of C#. Beide opties worden hieronder weergegeven.

***Optie 1: Metagegevens met behulp van PowerShell bijwerken***

1. Download de meest recente opdrachtregel-hulpprogramma voor NuGet van [hier](https://nuget.org/nuget.exe) en op te slaan naar een map.
2. Open een opdrachtprompt en navigeer naar dezelfde map met de opdracht: `nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`
3. Navigeer naar de submap met de nieuwe client-dll-versie die u hebt zojuist hebt gedownload, bijvoorbeeld: `cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`
4. Downloaden van de elastic database client upgrade-script uit de [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), en sla deze op in dezelfde map met de DLL-bestand.
5. 'PowerShell.\upgrade.ps1' uitvoeren vanaf de opdrachtprompt en volg de aanwijzingen van die map.

***Optie 2: Met behulp van de metagegevens van een upgrade uitvoertC#***

U kunt ook Visual Studio een toepassing maken die wordt geopend uw ShardMapManager, alle shards te herhalen en voert de upgrade van de metagegevens door het aanroepen van de methoden [UpgradeLocalStore](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore) en [UpgradeGlobalStore ](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore) zoals in dit voorbeeld:

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy);
    smm.UpgradeGlobalStore();

    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations())
    {
       smm.UpgradeLocalStore(loc);
    }

Deze technieken voor upgrades van de metagegevens kunnen meerdere keren worden toegepast zonder schade. Bijvoorbeeld, als een oudere versie van de client per ongeluk een shard maakt nadat u al hebt bijgewerkt, kunt u uitvoeren upgrade opnieuw over alle shards om ervoor te zorgen dat de meest recente metagegevensversie in uw infrastructuur aanwezig is.

**Opmerking:**  Nieuwe versies van de clientbibliotheek gepubliceerd tot heden blijven werken met eerdere versies van de metagegevens van de Shard-Toewijzingsbeheer op Azure SQL DB, en vice versa.   Maar om te profiteren van enkele van de nieuwe functies in de meest recente client, metagegevens moet worden bijgewerkt.   Houd er rekening mee dat metagegevens upgrades heeft geen invloed op alle gegevens van de gebruiker of toepassingsspecifieke gegevens, alleen objecten die zijn gemaakt en gebruikt door de Shard-toewijzing.  En toepassingen blijven functioneren door de updatevolgorde die hierboven worden beschreven.

## <a name="elastic-database-client-version-history"></a>Versiegeschiedenis van elastic database client

Versiegeschiedenis, gaat u naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png