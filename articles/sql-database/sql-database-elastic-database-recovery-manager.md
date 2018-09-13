---
title: Oplossen van problemen met shard met Recovery Manager | Microsoft Docs
description: Gebruik de RecoveryManager-klasse voor het oplossen van problemen met shard-toewijzingen
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 3aeee7cd4c588460a16b93237b08f13d8422a72a
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44721295"
---
# <a name="using-the-recoverymanager-class-to-fix-shard-map-problems"></a>Problemen met shard-toewijzingen oplossen met de RecoveryManager-klasse
De [RecoveryManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.aspx) klasse biedt ADO.Net toepassingen de mogelijkheid om eenvoudig detecteren en corrigeer eventuele inconsistenties tussen de globale shard-toewijzing (GSM) en de lokale-shard-toewijzing (LSM) in een omgeving met shard-database. 

De GSM en LSM houden de toewijzing van elke database in een gedeelde omgeving. Af en toe optreedt een onderbreking tussen de GSM en de LSM. In dat geval gebruikt u de RecoveryManager-klasse om te detecteren en herstellen van het einde.

De RecoveryManager-klasse maakt deel uit van de [Elastic Database-clientbibliotheek](sql-database-elastic-database-client-library.md). 

![Shard-toewijzing][1]

Zie voor definities van de termijn [woordenlijst voor hulpprogramma's elastische Database](sql-database-elastic-scale-glossary.md). Om te begrijpen hoe de **ShardMapManager** wordt gebruikt voor het beheren van gegevens in een shard-oplossing, Zie [Shard-Toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md).

## <a name="why-use-the-recovery-manager"></a>Waarom recovery manager gebruiken?
In een omgeving met shard-database is één tenant per database en een groot aantal databases per server. Er kan ook worden veel servers in de omgeving. Elke database is in de shard-toewijzing toegewezen, zodat er aanroepen kunnen worden doorgestuurd naar de juiste server en database. Databases worden bijgehouden volgens een **sharding-sleutel**, en elke shard wordt toegewezen een **reeks sleutelwaarden**. Bijvoorbeeld, een sharding-sleutel de klantnamen van de mogelijk weer uit "D" tot "F." De toewijzing van alle shards (ook wel databases) en hun toewijzing bereiken zijn opgenomen in de **globale shard-toewijzing (GSM)**. Elke database bevat ook een overzicht van de bereiken ligt die zijn opgenomen in de shard die bekend als staat de **lokale shard-toewijzing (LSM)**. Wanneer een app maakt verbinding met een shard, de toewijzing is opgeslagen in de cache met de app voor het ophalen van snelle. De LSM wordt gebruikt voor het valideren van gegevens in de cache. 

Het GSM en LSM kunnen worden gesynchroniseerd voor de volgende redenen:

1. Het verwijderen van een shard waarvan bereik gaat ervan uit dat niet langer in gebruik of het wijzigen van de naam van een shard. Verwijderen van een shard resulteert in een **shard-toewijzing zwevende**. Op deze manier een zwevende shard-toewijzing kan leiden tot een nieuwe naam database. Afhankelijk van het doel van de wijziging, moet de shard mogelijk worden verwijderd of de shard-locatie moet worden bijgewerkt. Als u wilt een verwijderde database herstellen, Zie [een verwijderde database herstellen](sql-database-recovery-using-backups.md).
2. Een geo-failover-gebeurtenis optreedt. Als u wilt doorgaan, moet een de servernaam en databasenaam op van de shard-Toewijzingsbeheer in de toepassing bijwerken en vervolgens de details van de shard-toewijzing voor alle shards worden gewijzigd in een shard-toewijzing bijwerken. Als er een geo-failover, moet deze logische herstel binnen de werkstroom van de worden geautomatiseerd. Herstelacties te automatiseren, kunt een moeiteloze beheerbaarheid voor geo-geschikte databases en vermijdt u handmatige menselijk handelen. Zie voor meer informatie over opties voor een database herstellen als er een storing in het datacenter, [bedrijfscontinuïteit](sql-database-business-continuity.md) en [herstel na noodgevallen](sql-database-disaster-recovery.md).
3. Een shard of de ShardMapManager-database wordt hersteld naar een eerder punt in tijd. Zie voor meer informatie over punt in tijd herstel met behulp van back-ups, [herstel met behulp van back-ups](sql-database-recovery-using-backups.md).

Zie de volgende onderwerpen voor meer informatie over hulpprogramma's voor Azure SQL Database Elastic Database, geo-replicatie en herstel: 

* [Overzicht: Cloud business bedrijfscontinuïteit en noodherstel met SQL-Database](sql-database-business-continuity.md) 
* [Aan de slag met elastische Databasehulpprogramma 's](sql-database-elastic-scale-get-started.md)  
* [ShardMap Management](sql-database-elastic-scale-shard-map-management.md)

## <a name="retrieving-recoverymanager-from-a-shardmapmanager"></a>Het ophalen van RecoveryManager van een ShardMapManager
De eerste stap is het maken van een RecoveryManager-exemplaar. De [GetRecoveryManager methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getrecoverymanager.aspx) retourneert de recovery manager voor de huidige [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) exemplaar. Om inconsistenties in de shard-toewijzing op te lossen, moet u eerst de RecoveryManager voor de specifieke shard-toewijzing ophalen. 

   ```
    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString,  
             ShardMapManagerLoadPolicy.Lazy);
             RecoveryManager rm = smm.GetRecoveryManager(); 
   ```

In dit voorbeeld wordt de RecoveryManager geïnitialiseerd vanuit de ShardMapManager. De ShardMapManager met een ShardMap is ook al geïnitialiseerd. 

Omdat de code voor deze toepassing de shard-toewijzing zelf wordt bewerkt, moet de referenties die worden gebruikt in de fabrieksmethode (in het vorige voorbeeld, smmConnectionString) referenties met machtigingen voor lezen / schrijven voor de GSM-database waarnaar wordt verwezen door de verbinding tekenreeks. Deze referenties zijn doorgaans verschillend van de referenties die worden gebruikt voor het openen van verbindingen voor gegevensafhankelijke routering van. Zie voor meer informatie, [met behulp van referenties in de elastic databaseclient](sql-database-elastic-scale-manage-credentials.md).

## <a name="removing-a-shard-from-the-shardmap-after-a-shard-is-deleted"></a>Een shard verwijderen uit de ShardMap nadat een shard is verwijderd
De [DetachShard methode](https://msdn.microsoft.com/library/azure/dn842083.aspx) losgekoppeld van de opgegeven shard van de shard-toewijzing en toewijzingen die zijn gekoppeld aan de shard wordt verwijderd.  

* De locatieparameter is de locatie van de shard, speciaal servernaam en databasenaam van de shard wordt losgekoppeld. 
* De parameter shardMapName is de naam van de shard-toewijzing. Dit is alleen vereist wanneer meerdere shard-toewijzingen worden beheerd door de dezelfde shard-toewijzing. Optioneel. 


> [!IMPORTANT]
> Gebruik deze methode alleen als u zeker weet dat het bereik voor de toewijzing van de bijgewerkte leeg is. De methoden die hierboven controleren niet voor het bereik worden verplaatst, dus het is raadzaam om op te nemen van controles in uw code.
>

Shards verwijdert in het volgende voorbeeld uit de shard-toewijzing. 

   ```
   rm.DetachShard(s.Location, customerMap);
   ``` 

De shard-toewijzing komt overeen met de shard-locatie in de GSM vóór de verwijdering van de shard. Omdat de shard is verwijderd, wordt ervan uitgegaan dat opzettelijk was en het bereik van sharding-sleutel is niet langer in gebruik. Als dat niet het geval is, kunt u punt in tijd herstel uitvoeren. aan de shard herstellen vanaf een eerdere point-in-time. (In dat geval Raadpleeg de volgende sectie voor het detecteren van shard inconsistenties.) Als u wilt herstellen, Zie [punt in tijd herstel](sql-database-recovery-using-backups.md).

Omdat ervan wordt uitgegaan dat het verwijderen van een database doelbewust is gemaakt, wordt de actie laatste administratieve opschonen is om het item naar de shard in de shard-Toewijzingsbeheer te verwijderen. Dit voorkomt dat de toepassing per ongeluk gegevens schrijven naar een adresbereik dat niet wordt verwacht.

## <a name="to-detect-mapping-differences"></a>Voor het detecteren van toewijzing verschillen
De [DetectMappingDifferences methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.detectmappingdifferences.aspx) selecteert en retourneert een van de shard-toewijzingen (lokaal of globaal) als de bron is en voor overeenstemming zorgt met toewijzingen op beide shard-toewijzingen (GSM en LSM).

   ```
   rm.DetectMappingDifferences(location, shardMapName);
   ```

* De *locatie* Hiermee geeft u de servernaam en databasenaam. 
* De *shardMapName* parameter is de naam van de shard-toewijzing. Dit is alleen vereist als er meerdere shard-toewijzingen worden beheerd door de dezelfde shard-toewijzing. Optioneel. 

## <a name="to-resolve-mapping-differences"></a>Toewijzing-verschillen oplossen
De [ResolveMappingDifferences methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.resolvemappingdifferences.aspx) selecteert u een van de shard-toewijzingen (lokaal of globaal) als de bron is en voor overeenstemming zorgt met toewijzingen op beide shard-toewijzingen (GSM en LSM).

   ```
   ResolveMappingDifferences (RecoveryToken, MappingDifferenceResolution.KeepShardMapping);
   ```

* De *RecoveryToken* parameter inventariseert de verschillen in de toewijzingen tussen de GSM en de LSM voor de specifieke shard. 
* De [MappingDifferenceResolution opsomming](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.mappingdifferenceresolution.aspx) wordt gebruikt om aan te geven van de methode voor het oplossen van het verschil tussen de shard-toewijzingen. 
* **MappingDifferenceResolution.KeepShardMapping** die wordt aanbevolen wanneer de LSM de nauwkeurige toewijzing bevat en daarom de toewijzing in de shard moet worden gebruikt. Dit is meestal het geval als er een failover: de shard zich nu bevindt op een nieuwe server. Omdat de shard moet eerst worden verwijderd uit de GSM (met behulp van de methode RecoveryManager.DetachShard), wordt een toewijzing niet meer bestaat op de GSM. Daarom moet de LSM worden gebruikt om het opnieuw in te stellen de shard-toewijzing.

## <a name="attach-a-shard-to-the-shardmap-after-a-shard-is-restored"></a>Een shard toevoegen aan de ShardMap nadat een shard is hersteld
De [AttachShard methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.attachshard.aspx) de opgegeven shard is gekoppeld aan de shard-toewijzing. Vervolgens shard map inconsistenties gedetecteerd en werkt de toewijzingen bij zodat deze overeenkomt met de shard op het moment van de shard-herstelbewerking. Ervan wordt uitgegaan dat de database ook aanleiding van de databasenaam van de oorspronkelijke (voordat de shard is hersteld), is gewijzigd sinds de herstelbewerking punt in tijd standaard ingesteld op een nieuwe database met de tijdstempel is toegevoegd. 

   ```
   rm.AttachShard(location, shardMapName)
   ``` 

* De *locatie* parameter is de servernaam en databasenaam van de shard wordt gekoppeld. 
* De *shardMapName* parameter is de naam van de shard-toewijzing. Dit is alleen vereist wanneer meerdere shard-toewijzingen worden beheerd door de dezelfde shard-toewijzing. Optioneel. 

In dit voorbeeld wordt een shard toegevoegd aan de shard-toewijzing die onlangs is hersteld van een eerder tijdstip punt in. Omdat de shard (namelijk de toewijzing voor de shard in de LSM) is hersteld, is het mogelijk inconsistent is met de shard-vermelding in de GSM. De shard is buiten deze voorbeeldcode hersteld en gewijzigd in de oorspronkelijke naam van de database. Omdat deze is hersteld, wordt uitgegaan van dat de toewijzing in de LSM is de toewijzing van het vertrouwde. 

   ```
   rm.AttachShard(s.Location, customerMap); 
   var gs = rm.DetectMappingDifferences(s.Location); 
      foreach (RecoveryToken g in gs) 
       { 
       rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
       } 
   ```

## <a name="updating-shard-locations-after-a-geo-failover-restore-of-the-shards"></a>Shard-locaties na een geo-failover (herstel) van de shards bijwerken
Als er een geo-failover, wordt de secundaire database is geschreven toegankelijk is gemaakt en wordt de nieuwe primaire database. De naam van de server, en mogelijk de database (afhankelijk van uw configuratie), kan afwijken van de oorspronkelijke primaire database. Daarom kan de toewijzingen voor de shard in de GSM en LSM moeten worden opgelost. Op dezelfde manier als de database is hersteld naar een andere naam of locatie of naar een eerder tijdstip, dit kan leiden tot inconsistenties in de shard-toewijzingen. De Shard-Toewijzingsbeheer verwerkt de distributie van open-verbindingen met de juiste database. Distributie is gebaseerd op de gegevens in de shard-toewijzing en de waarde van de sharding-sleutel die het doel van de aanvraag voor toepassingen. Na een geo-failover, moet deze gegevens worden bijgewerkt met de juiste servernaam, databasenaam en shard-toewijzing van de herstelde database. 

## <a name="best-practices"></a>Aanbevolen procedures
Geo-failover en herstel zijn bewerkingen meestal beheerd door de beheerder van een cloud van de toepassing met opzet op een van de Azure SQL-databases functies voor bedrijfscontinuïteit. Zakelijke continuïteit planning vereist processen en procedures en maatregelen om ervoor te zorgen dat zakelijke activiteiten zonder onderbreking blijven kunnen. De methoden die beschikbaar als onderdeel van de RecoveryManager-klasse binnen deze werkstroom moet worden gebruikt om te controleren of dat de GSM en LSM worden up-to-date gehouden op basis van de uitgevoerde herstelactie. Er zijn vijf eenvoudige stappen voor het juist ervoor te zorgen dat de GSM en LSM zodat u de juiste informatie na een failover. De code van de toepassing voor het uitvoeren van deze stappen kan worden geïntegreerd in bestaande hulpprogramma's en werkstroom. 

1. De RecoveryManager ophalen met de ShardMapManager. 
2. Loskoppelen van de oude shard van de shard-toewijzing.
3. De nieuwe shard koppelen aan de shard-toewijzing, met inbegrip van de nieuwe locatie in de shard.
4. Inconsistenties in de toewijzing tussen de GSM en LSM detecteren. 
5. Verschillen tussen de GSM en de LSM, vertrouwen van de LSM oplossen. 

In dit voorbeeld voert de volgende stappen uit:

1. Hiermee verwijdert u shards van de Shard-toewijzing die overeenkomen met shard-locaties voordat de failover plaatsvond.
2. Shards koppelt aan de Shard-toewijzing zetten op basis van de nieuwe shard-locaties (de parameter 'Configuration.SecondaryServer' is de naam van de nieuwe server, maar de naam van de dezelfde database).
3. Hiermee haalt u de tokens herstel door het detecteren van toewijzing verschillen tussen de GSM en de LSM voor elke shard. 
4. De inconsistenties opgelost door het vertrouwen van de toewijzing van de LSM van elke shard. 
   
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

