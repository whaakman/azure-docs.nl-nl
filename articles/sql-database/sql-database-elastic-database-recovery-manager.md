---
title: Met behulp van Recovery Manager oplossen van problemen van shard-toewijzing | Microsoft Docs
description: De klasse RecoveryManager gebruiken voor het oplossen van problemen met de shard-kaarten
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 45520ca3-6903-4b39-88ba-1d41b22da9fe
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: ddove
ms.openlocfilehash: 967a64fa41a9f9ff16ce173b76231052b22cda99
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="using-the-recoverymanager-class-to-fix-shard-map-problems"></a>Problemen met shard-toewijzingen oplossen met de RecoveryManager-klasse
De [RecoveryManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.aspx) klasse biedt ADO.Net-toepassingen de mogelijkheid om eenvoudig detecteren en los eventuele inconsistenties tussen de globale shard-toewijzing (GSM) en de lokale shard-toewijzing (LSM) in een databaseomgeving met shard-. 

De GSM en LSM houden de toewijzing van elke database in een shard-omgeving. In sommige gevallen kan wordt een einde ingevoegd tussen de GSM en de LSM. In dat geval moet u de klasse RecoveryManager gebruiken om te detecteren en herstellen van het einde.

De klasse RecoveryManager maakt deel uit van de [clientbibliotheek voor elastische Database](sql-database-elastic-database-client-library.md). 

![Shard-kaart][1]

Zie voor definities van de termijn [verklarende woordenlijst hulpprogramma's van elastische Database](sql-database-elastic-scale-glossary.md). Om te begrijpen hoe de **ShardMapManager** wordt gebruikt voor het beheren van gegevens in een shard-oplossing, Zie [Shard kaart management](sql-database-elastic-scale-shard-map-management.md).

## <a name="why-use-the-recovery-manager"></a>Waarom recovery manager gebruiken?
In een databaseomgeving shard-is een tenant per database en veel databases per server. Er kan ook worden veel servers in de omgeving. Elke database is in de shard-toewijzing toegewezen zodat aanroepen kunnen worden doorgestuurd naar de juiste server en database. Databases worden bijgehouden volgens een **sharding sleutel**, en elke shard is toegewezen een **sleutel waardenbereik**. Een sharding-sleutel kan vertegenwoordigen bijvoorbeeld de klantnamen van de uit "D" voor "F." De toewijzing van alle shards (aka databases) en hun toewijzing bereiken zijn opgenomen in de **globale shard-toewijzing (GSM)**. Elke database bevat een overzicht van de bereiken zijn in de shard waarvan bekend is als de **lokale shard-toewijzing (LSM)**. Wanneer een app verbinding met een shard, de toewijzing is opgeslagen in de cache met de app voor snel ophalen. De LSM wordt gebruikt voor het valideren van gegevens in de cache. 

De GSM en LSM kan mogelijk niet gesynchroniseerd voor de volgende redenen:

1. Het verwijderen van een shard waarvan bereik gaat ervan uit dat niet langer in gebruik of de naam wijzigen van een shard. Verwijderen van een shard resulteert in een **shard-toewijzing zwevende**. Op deze manier kan een nieuwe naam database leiden tot een zwevende shard-toewijzing. Afhankelijk van de intentie van de wijziging, de shard moet mogelijk worden verwijderd of de shard-locatie moet worden bijgewerkt. Als u wilt een verwijderde database herstellen, Zie [een verwijderde database terugzetten](sql-database-recovery-using-backups.md).
2. Een geo-failover van de gebeurtenis. Om door te gaan moet een update van de servernaam en databasenaam op van de manager van shard-toewijzing in de toepassing en werk vervolgens de details van de shard-toewijzing voor alle shards in een shard-toewijzing. Als er een geo-failover, moet deze logica herstel binnen de werkstroom failover worden geautomatiseerd. Herstelacties automatiseren kunt een frictionless beheerbaarheid voor geo geschikte databases en handmatige menselijke acties voorkomt. Zie voor meer informatie over opties voor een database herstellen als er een datacentrum, [bedrijfscontinuïteit](sql-database-business-continuity.md) en [herstel na noodgevallen](sql-database-disaster-recovery.md).
3. Een shard of de database ShardMapManager wordt teruggezet naar een eerder punt in tijd. Zie voor meer informatie over herstelpunt met behulp van back-ups, [herstel met behulp van back-ups](sql-database-recovery-using-backups.md).

Zie de volgende onderwerpen voor meer informatie over Azure SQL Database elastische Database-hulpprogramma's, geo-replicatie en herstel: 

* [Overzicht: Cloud zakelijke continuïteit en database noodherstel met SQL Database](sql-database-business-continuity.md) 
* [Aan de slag met hulpprogramma's voor elastische database](sql-database-elastic-scale-get-started.md)  
* [ShardMap Management](sql-database-elastic-scale-shard-map-management.md)

## <a name="retrieving-recoverymanager-from-a-shardmapmanager"></a>RecoveryManager ophalen uit een ShardMapManager
De eerste stap is om een RecoveryManager-exemplaar te maken. De [GetRecoveryManager methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getrecoverymanager.aspx) retourneert de recovery manager voor de huidige [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) exemplaar. Om eventuele inconsistenties op in de shard-toewijzing op te lossen, moet u eerst de RecoveryManager voor de kaart bepaalde shard ophalen. 

   ```
    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString,  
             ShardMapManagerLoadPolicy.Lazy);
             RecoveryManager rm = smm.GetRecoveryManager(); 
   ```

In dit voorbeeld wordt de RecoveryManager geïnitialiseerd vanuit de ShardMapManager. De ShardMapManager met een ShardMap is ook al geïnitialiseerd. 

Aangezien deze toepassingscode bewerkt de shard-toewijzing zelf, moet de referenties in de fabrieksmethode (in het voorgaande voorbeeld, smmConnectionString) referenties met machtigingen voor lezen / schrijven voor de GSM database waarnaar wordt verwezen door de verbinding tekenreeks. Deze referenties zijn doorgaans verschillen van referenties die worden gebruikt voor het openen van de verbindingen voor het doorsturen van afhankelijk zijn van gegevens. Zie voor meer informatie [met behulp van referenties in de client elastische database](sql-database-elastic-scale-manage-credentials.md).

## <a name="removing-a-shard-from-the-shardmap-after-a-shard-is-deleted"></a>Een shard verwijderen uit de ShardMap nadat een shard is verwijderd
De [DetachShard methode](https://msdn.microsoft.com/library/azure/dn842083.aspx) wordt losgekoppeld van de opgegeven shard uit de shard-toewijzing en toewijzingen die zijn gekoppeld aan de shard verwijderd.  

* De locatieparameter is de shard-locatie, speciaal servernaam en databasenaam van de shard wordt losgekoppeld. 
* De parameter shardMapName is de naam van de shard-toewijzing. Dit is alleen vereist wanneer meerdere shard-maps worden beheerd door de dezelfde manager van de shard-toewijzing. Optioneel. 


> [!IMPORTANT]
> Gebruik deze methode alleen als u zeker weet dat het bereik voor de bijgewerkte toewijzing leeg is. De bovenstaande methoden controleren niet gegevens voor het bereik worden verplaatst, is het verstandig om op te nemen van controles in uw code.
>

Dit voorbeeld wordt shards verwijderd uit de shard-toewijzing. 

   ```
   rm.DetachShard(s.Location, customerMap);
   ``` 

De shard-toewijzing weerspiegelt de shard-plaats in de GSM vóór de verwijdering van de shard. Omdat de shard is verwijderd, wordt ervan uitgegaan dit opzettelijk was en het bereik van sharding-sleutel is niet langer in gebruik. Als dat niet het geval is, kunt u een punt in tijd herstel uitvoeren. de shard herstellen vanuit een eerdere point in time. (In dat geval Raadpleeg de volgende sectie om te detecteren shard inconsistenties.) Als u wilt herstellen, Zie [herstelpunt](sql-database-recovery-using-backups.md).

Omdat ervan wordt uitgegaan dat het verwijderen van een database opzettelijk was, wordt de actie laatste beheerdersrechten opschoning is om de vermelding voor de shard in de shard-toewijzing manager te verwijderen. Dit voorkomt dat de toepassing per ongeluk gegevens schrijven naar een bereik dat wordt niet verwacht.

## <a name="to-detect-mapping-differences"></a>Voor het detecteren van toewijzing verschillen
De [DetectMappingDifferences methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.detectmappingdifferences.aspx) selecteert en retourneert een van de shard-toewijzingen (lokaal of globaal) als de bron van waarheid en toewijzingen op beide kaarten shard (GSM en LSM) voor overeenstemming zorgt.

   ```
   rm.DetectMappingDifferences(location, shardMapName);
   ```

* De *locatie* Hiermee geeft u de servernaam en databasenaam op. 
* De *shardMapName* -parameter is de naam van de shard-toewijzing. Dit is alleen vereist als er meerdere shard-toewijzingen worden beheerd door de dezelfde manager van de shard-toewijzing. Optioneel. 

## <a name="to-resolve-mapping-differences"></a>Toewijzing verschillen oplossen
De [ResolveMappingDifferences methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.resolvemappingdifferences.aspx) selecteert u een van de shard-toewijzingen (lokaal of globaal) als de bron van waarheid en toewijzingen op beide kaarten shard (GSM en LSM) voor overeenstemming zorgt.

   ```
   ResolveMappingDifferences (RecoveryToken, MappingDifferenceResolution.KeepShardMapping);
   ```

* De *RecoveryToken* parameter de verschillen in de toewijzingen tussen de GSM en de LSM voor de specifieke shard inventariseert. 
* De [MappingDifferenceResolution opsomming](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.mappingdifferenceresolution.aspx) wordt gebruikt om aan te geven van de methode voor het oplossen van het verschil tussen de shard-toewijzingen. 
* **MappingDifferenceResolution.KeepShardMapping** is raadzaam om bij de LSM de nauwkeurige toewijzing bevat en daarom de toewijzing in de shard moet worden gebruikt. Dit is doorgaans het geval als er een failover: de shard zich nu bevindt op een nieuwe server. Aangezien de shard moet eerst worden verwijderd uit de GSM (met de methode RecoveryManager.DetachShard), wordt een toewijzing niet meer bestaat op de GSM. Daarom moet de LSM worden gebruikt om de shard-toewijzing te herstellen.

## <a name="attach-a-shard-to-the-shardmap-after-a-shard-is-restored"></a>Een shard koppelen aan de ShardMap nadat een shard is hersteld
De [AttachShard methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.attachshard.aspx) de opgegeven shard is gekoppeld aan de shard-toewijzing. Vervolgens inconsistenties kaart shard detecteert en de toewijzingen zodat deze overeenkomen met de shard op het moment van herstel van de shard-updates. Ervan wordt uitgegaan dat de database ook wordt gewijzigd naar aanleiding van de oorspronkelijke databasenaam (voordat de shard is hersteld), omdat het herstel van de punt in tijd standaard ingesteld op een nieuwe database met de tijdstempel toegevoegd. 

   ```
   rm.AttachShard(location, shardMapName)
   ``` 

* De *locatie* parameter is de servernaam en databasenaam van de shard wordt gekoppeld. 
* De *shardMapName* -parameter is de naam van de shard-toewijzing. Dit is alleen vereist wanneer meerdere shard-maps worden beheerd door de dezelfde manager van de shard-toewijzing. Optioneel. 

Dit voorbeeld wordt een shard aan de shard-toewijzing die onlangs is hersteld van een eerder tijdstip punt in. Aangezien de shard (namelijk de toewijzing voor de shard in de LSM) is hersteld, is het mogelijk inconsistent is met de shard-vermelding in de GSM. De shard is buiten deze voorbeeldcode hersteld en gewijzigd in de oorspronkelijke naam van de database. Omdat deze is teruggezet, wordt uitgegaan van dat de toewijzing in de LSM is de vertrouwde toewijzing. 

   ```
   rm.AttachShard(s.Location, customerMap); 
   var gs = rm.DetectMappingDifferences(s.Location); 
      foreach (RecoveryToken g in gs) 
       { 
       rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
       } 
   ```

## <a name="updating-shard-locations-after-a-geo-failover-restore-of-the-shards"></a>Bijwerken van shard-locaties na een geo-failover (herstel) van de shards
Als er een geo-failover, wordt de secundaire database toegankelijk is gemaakt schrijven en wordt de nieuwe primaire database. De naam van de server en mogelijk de database (afhankelijk van uw configuratie), kan afwijken van de oorspronkelijke primaire. Daarom kan de vermeldingen van de toewijzing voor de shard in de GSM en LSM moeten worden opgelost. Op dezelfde manier als de database wordt hersteld naar een andere naam of locatie of naar een eerder tijdstip, hierdoor mogelijk inconsistenties in de shard-toewijzingen. De Shard-toewijzing Manager verwerkt de distributie van een open verbinding naar de juiste database. Distributie is gebaseerd op de gegevens in de shard-toewijzing en de waarde van de sharding-sleutel die het doel van de aanvraag voor toepassingen. Na een geo-failover, moet deze gegevens worden bijgewerkt met de juiste servernaam, databasenaam en shard-toewijzing van de herstelde database. 

## <a name="best-practices"></a>Aanbevolen procedures
Zijn gewoonlijk beheerd door een beheerder van de cloud van de toepassing opzettelijk met behulp van een Azure SQL-Databases zakelijke continuïteit functies operations geo-failover en herstel. Zakelijke continuïteit planning vereist processen, procedures en maatregelen om ervoor te zorgen dat zakelijke activiteiten zonder onderbreking kunnen worden voortgezet. De beschikbare methoden als onderdeel van de klasse RecoveryManager binnen deze werkstroom moet worden gebruikt om te controleren of dat de GSM en LSM altijd zijn bijgewerkt op basis van de uitgevoerde herstelactie. Er zijn vijf eenvoudige stappen om ervoor te zorgen goed dat de GSM en LSM zodat u de juiste informatie na een failover. De toepassingscode deze stappen uitvoeren, kan worden geïntegreerd in de werkstroom en bestaande hulpprogramma's. 

1. De RecoveryManager ophalen van de ShardMapManager. 
2. Loskoppelen van de oude shard uit de shard-toewijzing.
3. De nieuwe shard koppelen aan de shard-kaart, inclusief de nieuwe shard-locatie.
4. Inconsistenties in de toewijzing tussen de GSM en LSM detecteren. 
5. Verschillen tussen de GSM en de LSM, de LSM vertrouwende oplossen. 

In dit voorbeeld voert de volgende stappen uit:

1. Shards verwijdert uit de Shard-toewijzing die aan shard-locaties voor de failover-gebeurtenis.
2. Shards koppelt aan de Shard-toewijzing als gevolg van de nieuwe shard-locaties (de parameter 'Configuration.SecondaryServer' is de naam van de nieuwe server maar dezelfde databasenaam).
3. Haalt de tokens herstel door toewijzing verschillen tussen de GSM en de LSM voor elke shard detecteren. 
4. Wordt de inconsistenties opgelost door de toewijzing van de LSM van elke shard vertrouwen. 
   
   ```
   var shards = smm.GetShards(); 
   foreach (shard s in shards) 
   { 
     if (s.Location.Server == Configuration.PrimaryServer) 
   
         { 
          ShardLocation slNew = new ShardLocation(Configuration.SecondaryServer, s.Location.Database); 
   
          rm.DetachShard(s.Location); 
   
          rm.AttachShard(slNew); 
   
          var gs = rm.DetectMappingDifferences(slNew); 
   
          foreach (RecoveryToken g in gs) 
            { 
               rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
            } 
        } 
    } 
   ```

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-database-recovery-manager/recovery-manager.png

