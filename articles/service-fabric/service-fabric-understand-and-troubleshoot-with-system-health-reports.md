---
title: Problemen met systeemstatusrapporten oplossen | Microsoft Docs
description: Beschrijving van de statusrapporten dat is verzonden door Azure Service Fabric-onderdelen en hun gebruik voor het oplossen van problemen cluster of problemen
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: ''
ms.assetid: 52574ea7-eb37-47e0-a20a-101539177625
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2018
ms.author: oanapl
ms.openlocfilehash: d62fd909d10515c9217a4dd0aa760afa376b8d7c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57838898"
---
# <a name="use-system-health-reports-to-troubleshoot"></a>Systeemstatusrapporten gebruiken om fouten op te lossen
Azure Service Fabric-onderdelen bieden systeemstatusrapporten op alle entiteiten in het cluster kant. De [health store](service-fabric-health-introduction.md#health-store) maken en verwijderen van de entiteiten die zijn gebaseerd op systeemrapporten van het. Ook worden ze in een hiërarchie die entiteit interacties worden vastgelegd.

> [!NOTE]
> Meer informatie voor meer informatie over concepten met betrekking tot de gezondheid van, [Service Fabric-statusmodel](service-fabric-health-introduction.md).
> 
> 

Systeemstatusrapporten bieden inzicht in het cluster en de functionaliteit van de toepassing en de vlag problemen. Voor toepassingen en services controleren systeemstatusrapporten of entiteiten worden geïmplementeerd en correct vanuit het perspectief van de Service Fabric zich gedragen. De rapporten bieden niet een statuscontrole van de bedrijfslogica van de service of detectie van vastgelopen processen. Services van de gebruiker kunnen de health-gegevens met informatie die specifiek zijn voor de logica verrijken.

> [!NOTE]
> Statusrapporten die zijn verzonden door de gebruiker watchdogs zijn alleen zichtbaar *nadat* de onderdelen van het systeem een entiteit maken. Wanneer een entiteit wordt verwijderd, worden alle statusrapporten die zijn gekoppeld aan deze automatisch verwijderd door de health-store. Hetzelfde geldt wanneer een nieuw exemplaar van de entiteit is gemaakt. Een voorbeeld is wanneer een nieuw exemplaar van de stateful persistent gemaakte service-replica wordt gemaakt. Alle rapporten die zijn gekoppeld aan het oude exemplaar worden verwijderd en wordt deze verwijderd uit de store.
> 
> 

Onderdeel van de rapporten worden geïdentificeerd door de bron, die met begint het "**System.**" het voorvoegsel. Watchdogs niet hetzelfde voorvoegsel gebruiken voor hun bronnen, zoals rapporten met ongeldige parameters worden afgewezen.

Laten we kijken sommige systeemrapporten om te begrijpen wat ze activeert en voor informatie over het corrigeren van de potentiële problemen staan.

> [!NOTE]
> Service Fabric blijft rapporten toevoegen van de voorwaarden die van belang dat u inzicht in wat in het cluster en de toepassingen gebeurt er te verbeteren. Bestaande rapporten kunnen worden uitgebreid met meer informatie om u te helpen het probleem sneller op te lossen.
> 
> 

## <a name="cluster-system-health-reports"></a>Cluster-systeemstatusrapporten
De cluster health entiteit wordt automatisch gemaakt in de health-store. Als alles goed werkt, is het geen een rapport.

### <a name="neighborhood-loss"></a>Het verlies van de
**System.Federation** meldt een fout wanneer er een het verlies van de wordt gedetecteerd. Het rapport is van afzonderlijke knooppunten en het knooppunt-ID is opgenomen in de eigenschapsnaam. Als één groep in de hele Service Fabric-ring verbroken is, kunt u doorgaans twee gebeurtenissen die staan voor beide zijden van het rapport tussenruimte verwachten. Als er meer groepen verloren gaan, moet u er meer gebeurtenissen zijn.

Het rapport geeft de time-out voor de globale lease als de time-to-live (TTL). Het rapport is elke helft van de duur van de TTL-waarde voor verzonden als de voorwaarde actief blijft. De gebeurtenis wordt automatisch verwijderd wanneer het verloopt. Verwijderen als verlopen gedrag zorgt ervoor dat het rapport wordt opgeschoond uit het statusarchief goed, zelfs als het reporting-knooppunt niet actief is.

* **SourceId**: System.Federation
* **De eigenschap**: Begint met **netwerkomgeving** en informatie gegeven knooppunt.
* **Volgende stappen**: Onderzoek waarom de omgeving gaat verloren. Bijvoorbeeld: Controleer de communicatie tussen knooppunten van het cluster.

### <a name="rebuild"></a>Opnieuw samenstellen

Informatie over de clusterknooppunten worden beheerd door de service Failover Manager (FM). Wanneer FM verliest de gegevens en probeert het verlies van gegevens, is er geen garantie dat deze de meest actuele informatie over de clusterknooppunten heeft. In dit geval wordt het systeem wordt opnieuw opbouwen en System.FM verzamelt gegevens uit alle knooppunten in het cluster om de status opnieuw. Soms vanwege netwerk- of knooppunt problemen kunt opnieuw vastgelopen of vastgelopen. Hetzelfde kan zich voordoen met de service Failover Manager Master (FMM). De FMM is een staatloze systeemservice die wordt bijgehouden van waar alle FMs zich bevinden in het cluster. Primaire van de FMM is altijd het knooppunt met de ID die het dichtst bij 0. Als dit knooppunt wordt verwijderd, wordt opnieuw opbouwen geactiveerd.
Als een van de vorige voorwaarden gebeurt, **System.FM** of **System.FMM** via een foutenrapport worden gemarkeerd. Het herbouwen van zitten vast in een van twee fasen:

* **Een ogenblik geduld**: FM/FMM wacht tot het broadcast-bericht-antwoord van de andere knooppunten.

  * **Volgende stappen**: Onderzoek of er een netwerkprobleem verbinding tussen de knooppunten is.
* **Wachten op knooppunten**: FM/FMM is al een reactie op de uitzending van de andere knooppunten te ontvangen en wordt gewacht tot een reactie van specifieke knooppunten. Het statusrapport geeft een lijst van de knooppunten waarvoor de FM/FMM is wachten op reactie.
   * **Volgende stappen**: Onderzoek de netwerkverbinding tussen de FM/FMM en de vermelde knooppunten. Elke vermelde knooppunt voor andere mogelijke problemen onderzoeken.

* **SourceID**: System.FM of System.FMM
* **De eigenschap**: Opnieuw opbouwen.
* **Volgende stappen**: Onderzoek de netwerkverbinding tussen de knooppunten, evenals de status van een specifieke knooppunten die worden vermeld in de beschrijving van het statusrapport.

## <a name="node-system-health-reports"></a>Knooppunt systeemstatusrapporten
System.FM, die staat voor de Failover Manager-service, wordt de instantie van die informatie over de knooppunten van het cluster beheert. Elk knooppunt moet één rapport van System.FM met de status hebben. De entiteiten knooppunt worden verwijderd wanneer de status wordt verwijderd. Zie voor meer informatie, [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync).

### <a name="node-updown"></a>Knooppunt omhoog/omlaag
System.FM rapporten als OK wanneer het knooppunt lid wordt van de ring (dit is actief en werkend). Deze meldt een fout bij het knooppunt de ring vertrekt (service niet actief is, een voor het upgraden of gewoon omdat deze is mislukt). De health-hiërarchie die zijn gemaakt door de health store fungeert voor geïmplementeerde entiteiten in de correlatie met System.FM knooppunt rapporten. Er wordt rekening gehouden het knooppunt een bovenliggende virtuele van alle geïmplementeerde entiteiten. De geïmplementeerde entiteiten op dat knooppunt worden weergegeven via query's als het knooppunt als maximaal wordt gerapporteerd door System.FM met hetzelfde exemplaar als het exemplaar dat is gekoppeld aan de entiteiten. Wanneer System.FM wordt gemeld dat het knooppunt is niet actief of opnieuw wordt opgestart, als een nieuw exemplaar, de health store automatisch opschonen van de geïmplementeerde entiteiten die alleen op het knooppunt omlaag of op het vorige exemplaar van het knooppunt kunnen bestaan.

* **SourceId**: System.FM
* **De eigenschap**: Status.
* **Volgende stappen**: Als het knooppunt is niet beschikbaar vanwege een upgrade, moet deze terugkeren van nadat het bijgewerkt. In dit geval moet de status van de Ga terug naar OK. Als het knooppunt niet u terug keert of deze mislukt, moet het probleem meer onderzoek.

Het volgende voorbeeld wordt de gebeurtenis System.FM met een status van OK voor knooppunt weergegeven:

```PowerShell
PS C:\> Get-ServiceFabricNodeHealth  _Node_0

NodeName              : _Node_0
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 8
                        SentAt                : 7/14/2017 4:54:51 PM
                        ReceivedAt            : 7/14/2017 4:55:14 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```


### <a name="certificate-expiration"></a>Certificaat verloopt
**System.FabricNode** rapporten van een waarschuwing wanneer certificaten die worden gebruikt door het knooppunt bijna is verlopen zijn. Er zijn drie certificaten per knooppunt: **Certificate_cluster**, **Certificate_server**, en **Certificate_default_client**. Wanneer de vervaldatum ten minste twee weken is, is de status van de rapport OK. Wanneer de vervaldatum binnen twee weken is, is het rapporttype dat een waarschuwing. TTL-waarde van deze gebeurtenissen is oneindig, en worden ze verwijderd als een knooppunt de cluster verlaat.

* **SourceId**: System.FabricNode
* **De eigenschap**: Begint met **certificaat** en bevat meer informatie over het certificaattype.
* **Volgende stappen**: De certificaten bijwerken als ze bijna is verlopen zijn.

### <a name="load-capacity-violation"></a>Schending van de Load-capaciteit
De Service Fabric Load Balancer rapporteert een waarschuwing wanneer er een schending van de capaciteit knooppunt wordt gedetecteerd.

* **SourceId**: System.PLB
* **De eigenschap**: Begint met **capaciteit**.
* **Volgende stappen**: Controleer de opgegeven metrische gegevens en de huidige capaciteit weergeven op het knooppunt.

### <a name="node-capacity-mismatch-for-resource-governance-metrics"></a>Knooppunt capaciteit komt niet overeen voor metrische gegevens voor het beheer van resources
System.Hosting rapporteert een waarschuwing als knooppuntcapaciteit gedefinieerd in het clustermanifest groter is dan de werkelijke knooppuntcapaciteiten voor resource governance metrische gegevens (geheugen en CPU-kernen) zijn. Een statusrapport wordt weergegeven wanneer het eerste pakket van de service die gebruikmaakt van [resourcebeheer](service-fabric-resource-governance.md) registreert op een opgegeven knooppunt.

* **SourceId**: System.Hosting
* **De eigenschap**: **ResourceGovernance**.
* **Volgende stappen**: Dit probleem kan een probleem zijn, omdat voor service-pakketten worden niet afgedwongen zoals verwacht en [resourcebeheer](service-fabric-resource-governance.md) werkt niet goed. Het clustermanifest bijwerken met de juiste knooppuntcapaciteiten voor deze metrische gegevens, of geen geeft u deze en laat Service Fabric automatisch detecteren van beschikbare resources.

## <a name="application-system-health-reports"></a>Toepassing systeemstatusrapporten
System.CM, dat de Cluster Manager-service vertegenwoordigt, is de instantie van die informatie over een toepassing beheert.

### <a name="state"></a>Status
System.CM rapporteert als OK wanneer de toepassing heeft gemaakt of bijgewerkt. De store health informeert wanneer de toepassing wordt verwijderd, zodat het kan worden verwijderd uit de store.

* **SourceId**: System.CM
* **De eigenschap**: Status.
* **Volgende stappen**: Als de toepassing is gemaakt of bijgewerkt, moet deze het statusrapport Clusterbeheer bevatten. Controleer de status van de toepassing door een query uitvoert. Gebruik bijvoorbeeld de PowerShell-cmdlet **Get-ServiceFabricApplication - ApplicationName** *applicationName*.

Het volgende voorbeeld ziet u de gebeurtenis status van de **fabric: / WordCount** toepassing:

```PowerShell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/14/2017 4:55:10 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="service-system-health-reports"></a>Service-systeemstatusrapporten
System.FM, die staat voor de Failover Manager-service, wordt de instantie van die informatie over services beheert.

### <a name="state"></a>Status
System.FM rapporteert als OK wanneer de service is gemaakt. Wordt de entiteit in health store verwijderd wanneer de service is verwijderd.

* **SourceId**: System.FM
* **De eigenschap**: Status.

Het volgende voorbeeld ziet u de gebeurtenis status van de service **fabric: / WordCount/WordCountWebService**:

```PowerShell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountWebService -ExcludeHealthStatistics


ServiceName           : fabric:/WordCount/WordCountWebService
AggregatedHealthState : Ok
PartitionHealthStates : 
                        PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
                        AggregatedHealthState : Ok
                        
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 14
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="service-correlation-error"></a>Correlatie-servicefout
**System.PLB** meldt een fout wanneer wordt gedetecteerd dat het bijwerken van een service met een andere service die wordt gemaakt van een affiniteitsketen worden gecorreleerd. Het rapport is uitgeschakeld als een geslaagde update gebeurt.

* **SourceId**: System.PLB
* **De eigenschap**: **ServiceDescription**.
* **Volgende stappen**: Controleer de servicebeschrijvingen van gecorreleerde.

## <a name="partition-system-health-reports"></a>Partitie systeemstatusrapporten
System.FM, die staat voor de Failover Manager-service, wordt de instantie van die informatie over servicepartities beheert.

### <a name="state"></a>Status
System.FM rapporten als OK wanneer de partitie is gemaakt en in orde is. Wordt de entiteit in health store verwijderd wanneer de partitie wordt verwijderd.

Als de partitie lager dan het aantal minimale replica's is, wordt een fout. Als de partitie niet lager dan het aantal minimale replica's is, maar deze lager dan het aantal replica's van doel is, wordt een waarschuwing. Als de partitie sprake van quorumverlies is, rapporteert System.FM een fout.

Andere gebeurtenissen die aandacht vereisen, wordt er een waarschuwing bevatten wanneer de nieuwe configuratie langer duurt dan verwacht en wanneer de build langer duurt dan verwacht. De verwachte tijden voor de build en herconfiguratie kunnen geconfigureerd worden op basis van de service-scenario's. Bijvoorbeeld, als een service een terabyte aan status, zoals Azure SQL Database heeft, langer de build duurt dan voor een service met een kleine hoeveelheid staat.

* **SourceId**: System.FM
* **De eigenschap**: Status.
* **Volgende stappen**: Als de status niet OK is, is het mogelijk dat sommige replica's niet zijn gemaakt, geopend, of gepromoveerd tot primaire of secundaire correct. 

Als de beschrijving van de beschrijving van quorumverlies, klikt u vervolgens weer het gedetailleerde statusrapport voor replica's die niet beschikbaar zijn bestudeerd en maak een back-up van helpt om de partitie worden gezet online.

Als de beschrijving van de beschrijving van een partitie vastgelopen [herconfiguratie](service-fabric-concepts-reconfiguration.md), en vervolgens het statusrapport op de primaire replica aanvullende informatie bevat.

Voor andere statusrapporten System.FM zou er rapporten op de replica's of de partitie of -service van andere onderdelen van het systeem. 

De volgende voorbeelden worden enkele van deze rapporten beschreven. 

Het volgende voorbeeld ziet u een partitie in orde:

```PowerShell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountWebService | Get-ServiceFabricPartitionHealth -ExcludeHealthStatistics -ReplicasFilter None

PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 70
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Het volgende voorbeeld ziet de status van een partitie die lager is dan het aantal doel-replica's. De volgende stap is om op te halen van de Partitiebeschrijving, die laat zien hoe deze geconfigureerd: **MinReplicaSetSize** is drie en **TargetReplicaSetSize** zeven. Afgeleverde het aantal knooppunten in het cluster, die in dit geval vijf is. Worden dus in dit geval twee replica's kunnen niet geplaatst, omdat het beoogd aantal replica's hoger is dan het aantal knooppunten dat beschikbaar is.

```PowerShell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None -ExcludeHealthStatistics


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 123
                        SentAt                : 7/14/2017 4:55:39 PM
                        ReceivedAt            : 7/14/2017 4:55:44 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/S Ready _Node_2 131444422260002646
                          N/S Ready _Node_4 131444422293113678
                          N/S Ready _Node_3 131444422293113679
                          N/S Ready _Node_1 131444422293118720
                          N/P Ready _Node_0 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5)
                        
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:55:44 PM, LastOk = 1/1/0001 12:00:00 AM
                        
                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131445250939703027
                        SentAt                : 7/14/2017 4:58:13 PM
                        ReceivedAt            : 7/14/2017 4:58:14 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        Secondary replica could not be placed due to the following constraints and properties:  
                        TargetReplicaSetSize: 7
                        Placement Constraint: N/A
                        Parent Service: N/A
                        
                        Constraint Elimination Sequence:
                        Existing Secondary Replicas eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        Existing Primary Replica eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.
                        
                        Nodes Eliminated By Constraints:
                        
                        Existing Secondary Replicas -- Nodes with Partition's Existing Secondary Replicas/Instances:
                        --
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        
                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None
                        
                        
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:56:14 PM, LastOk = 1/1/0001 12:00:00 AM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | select MinReplicaSetSize,TargetReplicaSetSize

MinReplicaSetSize TargetReplicaSetSize
----------------- --------------------
                2                    7                        

PS C:\> @(Get-ServiceFabricNode).Count
5
```

Het volgende voorbeeld ziet de status van een partitie die vastgelopen in herconfiguratie vanwege de gebruiker niet naleven van de annulering token in de **RunAsync** methode. Het statusrapport van alle replica's gemarkeerd als primair (P) onderzoeken kan helpen om in te zoomen op het probleem verder.

```PowerShell
PS C:\utilities\ServiceFabricExplorer\ClientPackage\lib> Get-ServiceFabricPartitionHealth 0e40fd81-284d-4be4-a665-13bc5a6607ec -ExcludeHealthStatistics 


PartitionId           : 0e40fd81-284d-4be4-a665-13bc5a6607ec
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', 
                        ConsiderWarningAsError=false.
                                               
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 7
                        SentAt                : 8/27/2017 3:43:09 AM
                        ReceivedAt            : 8/27/2017 3:43:32 AM
                        TTL                   : Infinite
                        Description           : Partition reconfiguration is taking longer than expected.
                        fabric:/app/test1 3 1 0e40fd81-284d-4be4-a665-13bc5a6607ec
                          P/S Ready Node1 131482789658160654
                          S/P Ready Node2 131482789688598467
                          S/S Ready Node3 131482789688598468
                          (Showing 3 out of 3 replicas. Total available replicas: 3)                        
                        
                        For more information see: https://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 8/27/2017 3:43:32 AM, LastError = 1/1/0001 12:00:00 AM
```
Dit rapport health toont de status van de replica's van de partitie die momenteel herconfiguratie uitgevoerd: 

```
  P/S Ready Node1 131482789658160654
  S/P Ready Node2 131482789688598467
  S/S Ready Node3 131482789688598468
```

Het statusrapport bevat voor elke replica:
- Rol van de vorige configuratie
- Configuratie van de huidige rol
- [Replicastatus](service-fabric-concepts-replica-lifecycle.md)
- Knooppunt waarop de replica wordt uitgevoerd
- Replica-ID

In geval als in het voorbeeld verder onderzoek nodig. De status van elke afzonderlijke replica die beginnen met de replica's gemarkeerd als onderzoeken `Primary` en `Secondary` (131482789658160654 en 131482789688598467) in het vorige voorbeeld.

### <a name="replica-constraint-violation"></a>Schending van de replica-beperking
**System.PLB** rapporteert een waarschuwing als er een replica-Beperkingsfout detecteert en alle partitiereplica's kan niet worden geplaatst. Details van het rapport weergeven welke beperkingen en eigenschappen te voorkomen dat de plaatsing van de replica.

* **SourceId**: System.PLB
* **De eigenschap**: Begint met **ReplicaConstraintViolation**.

## <a name="replica-system-health-reports"></a>Replica systeemstatusrapporten
**System.RA**, die het onderdeel reconfiguration agent vertegenwoordigt de instantie voor de replicastatus van de is.

### <a name="state"></a>Status
System.RA rapporten OK wanneer de replica is gemaakt.

* **SourceId**: System.RA
* **De eigenschap**: Status.

Het volgende voorbeeld ziet u een goede replica:

```PowerShell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422293118721
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131445248920273536
                        SentAt                : 7/14/2017 4:54:52 PM
                        ReceivedAt            : 7/14/2017 4:55:13 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_0
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:13 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="replicaopenstatus-replicaclosestatus-replicachangerolestatus"></a>ReplicaOpenStatus, ReplicaCloseStatus, ReplicaChangeRoleStatus
Deze eigenschap wordt gebruikt om aan te geven waarschuwingen of fouten bij het openen van een replica, sluiten van een replica of een replica van de ene rol naar de andere worden overgezet. Zie voor meer informatie, [levenscyclus van Replica](service-fabric-concepts-replica-lifecycle.md). De fouten mogelijk uitzonderingen uit de API-aanroepen of crashes van het hostproces van de service gedurende deze tijd. Voor storingen als gevolg van API-aanroepen van C# Service Fabric-code wordt de uitzondering en de stack-trace toegevoegd aan het statusrapport.

Deze waarschuwingen worden gegenereerd na het opnieuw proberen van de actie lokaal een aantal keren (afhankelijk van het beleid). Service Fabric pogingen de actie tot een drempel voor maximum aantal. Nadat de drempelwaarde is bereikt, kan het proberen om te fungeren als u wilt de situatie oplossen. Dit kan leiden tot deze waarschuwingen wordt gewist als er wordt opgegeven op de actie op dit knooppunt. Bijvoorbeeld, als een replica is mislukt om te openen op een knooppunt, Service Fabric leidt tot een status-waarschuwing. Als de replica mislukken blijft te openen, fungeert de Service Fabric om te herstellen. Deze actie kan betrekking hebben op dezelfde bewerking op een ander knooppunt probeert. Dit zorgt ervoor dat de waarschuwing is gegenereerd voor deze replica worden gewist. 

* **SourceId**: System.RA
* **De eigenschap**: **ReplicaOpenStatus**, **ReplicaCloseStatus**, en **ReplicaChangeRoleStatus**.
* **Volgende stappen**: De code van de onderzoeken of crashdumps om te bepalen waarom de bewerking is mislukt.

Het volgende voorbeeld ziet de status van een replica die wordt ArgumentOutOfRangeException `TargetInvocationException` uit de open-methode. De beschrijving bevat de storingspunt **IStatefulServiceReplica.Open**, het uitzonderingstype **TargetInvocationException**, en de stack-trace.

```PowerShell
PS C:\> Get-ServiceFabricReplicaHealth -PartitionId 337cf1df-6cab-4825-99a9-7595090c0b1b -ReplicaOrInstanceId 131483509874784794


PartitionId           : 337cf1df-6cab-4825-99a9-7595090c0b1b
ReplicaId             : 131483509874784794
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.RA', Property='ReplicaOpenStatus', HealthState='Warning', 
                        ConsiderWarningAsError=false.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : ReplicaOpenStatus
                        HealthState           : Warning
                        SequenceNumber        : 131483510001453159
                        SentAt                : 8/27/2017 11:43:20 PM
                        ReceivedAt            : 8/27/2017 11:43:21 PM
                        TTL                   : Infinite
                        Description           : Replica had multiple failures during open on _Node_0 API call: IStatefulServiceReplica.Open(); Error = System.Reflection.TargetInvocationException (-2146232828)
Exception has been thrown by the target of an invocation.
   at Microsoft.ServiceFabric.Replicator.RecoveryManager.d__31.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Replicator.LoggingReplicator.d__137.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Replicator.DynamicStateManager.d__109.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Replicator.TransactionalReplicator.d__79.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Replicator.StatefulServiceReplica.d__21.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Services.Runtime.StatefulServiceReplicaAdapter.d__0.MoveNext()

    For more information see: https://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/27/2017 11:43:21 PM, LastOk = 1/1/0001 12:00:00 AM                        
```

Het volgende voorbeeld ziet u een replica die voortdurend gecrasht tijdens sluiten:

```PowerShell
C:>Get-ServiceFabricReplicaHealth -PartitionId dcafb6b7-9446-425c-8b90-b3fdf3859e64 -ReplicaOrInstanceId 131483565548493142


PartitionId           : dcafb6b7-9446-425c-8b90-b3fdf3859e64
ReplicaId             : 131483565548493142
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.RA', Property='ReplicaCloseStatus', HealthState='Warning', 
                        ConsiderWarningAsError=false.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : ReplicaCloseStatus
                        HealthState           : Warning
                        SequenceNumber        : 131483565611258984
                        SentAt                : 8/28/2017 1:16:01 AM
                        ReceivedAt            : 8/28/2017 1:16:03 AM
                        TTL                   : Infinite
                        Description           : Replica had multiple failures during close on _Node_1. The application 
                        host has crashed.
                        
                        For more information see: https://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 1:16:03 AM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="reconfiguration"></a>Herconfiguratie
Deze eigenschap wordt gebruikt om aan te geven bij het uitvoeren van een replica een [herconfiguratie](service-fabric-concepts-reconfiguration.md) detecteert dat de herconfiguratie is vastgelopen of vastgelopen. Dit statusrapport kan zijn op de waarvan de huidige rol is primair, behalve in het geval van een primaire herconfiguratie wisselen waar het kan zijn op de replica die wordt gedegradeerd van primaire naar de actieve secundaire replica.

De nieuwe configuratie kan blijven steken voor een van de volgende redenen:

- Een actie voor de lokale replica, dezelfde replica als het uitvoeren van de nieuwe configuratie, wordt niet voltooid. In dit geval u wilt onderzoeken in de statusrapporten op deze replica uit de andere onderdelen, System.RAP of System.RE, mogelijk bevatten aanvullende informatie.

- Een actie wordt niet voltooid op een externe replica. Replica's waarvoor acties in behandeling zijn, worden weergegeven in het statusrapport. Verder onderzoek moet worden uitgevoerd op statusrapporten voor deze externe replica's. Er kan ook worden communicatieproblemen tussen dit knooppunt en het externe knooppunt.

In zeldzame gevallen kan de nieuwe configuratie zijn vastgelopen vanwege communicatie of andere problemen tussen dit knooppunt en de Failover Manager-service.

* **SourceId**: System.RA
* **De eigenschap**: Herconfiguratie.
* **Volgende stappen**: Lokale of externe replica's, afhankelijk van de beschrijving van het statusrapport onderzoeken.

Het volgende voorbeeld ziet een statusrapport waar een herconfiguratie is vastgelopen op de lokale replica's. In dit voorbeeld wordt wordt het vanwege een service niet naleven van de token van de annulering.

```PowerShell
PS C:\> Get-ServiceFabricReplicaHealth -PartitionId 9a0cedee-464c-4603-abbc-1cf57c4454f3 -ReplicaOrInstanceId 131483600074836703


PartitionId           : 9a0cedee-464c-4603-abbc-1cf57c4454f3
ReplicaId             : 131483600074836703
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.RA', Property='Reconfiguration', HealthState='Warning', 
                        ConsiderWarningAsError=false.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : Reconfiguration
                        HealthState           : Warning
                        SequenceNumber        : 131483600309264482
                        SentAt                : 8/28/2017 2:13:50 AM
                        ReceivedAt            : 8/28/2017 2:13:57 AM
                        TTL                   : Infinite
                        Description           : Reconfiguration is stuck. Waiting for response from the local replica
                        
                        For more information see: https://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 2:13:57 AM, LastOk = 1/1/0001 12:00:00 AM
```

Het volgende voorbeeld wordt een status rapporteren wanneer een herconfiguratie is vastgelopen wachten op een reactie van de twee externe replica's. In dit voorbeeld worden drie replica's in de partitie, met inbegrip van de huidige primaire. 

```Powershell
PS C:\> Get-ServiceFabricReplicaHealth -PartitionId  579d50c6-d670-4d25-af70-d706e4bc19a2 -ReplicaOrInstanceId 131483956274977415


PartitionId           : 579d50c6-d670-4d25-af70-d706e4bc19a2
ReplicaId             : 131483956274977415
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.RA', Property='Reconfiguration', HealthState='Warning', ConsiderWarningAsError=false.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : Reconfiguration
                        HealthState           : Warning
                        SequenceNumber        : 131483960376212469
                        SentAt                : 8/28/2017 12:13:57 PM
                        ReceivedAt            : 8/28/2017 12:14:07 PM
                        TTL                   : Infinite
                        Description           : Reconfiguration is stuck. Waiting for response from 2 replicas
                        
                        Pending Replicas: 
                        P/I Down 40 131483956244554282
                        S/S Down 20 131483956274972403
                        
                        For more information see: https://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 12:07:37 PM, LastOk = 1/1/0001 12:00:00 AM
```

Dit rapport health ziet u dat de herconfiguratie is vastgelopen wachten op een reactie van twee replica's: 

```
    P/I Down 40 131483956244554282
    S/S Down 20 131483956274972403
```

Voor elke replica wordt verkregen met de volgende informatie:
- Rol van de vorige configuratie
- Configuratie van de huidige rol
- [Replicastatus](service-fabric-concepts-replica-lifecycle.md)
- Knooppunt-id
- Replica-ID

De nieuwe configuratie deblokkeren:
- De **omlaag** replica's moeten worden opgeroepen. 
- De **inbuild** replica's moeten voltooien van de build en overstappen op gereed.

### <a name="slow-service-api-call"></a>Trage service API-aanroep
**System.RAP** en **System.Replicator** rapporteren van een waarschuwing als een aanroep van de code van de gebruiker langer duurt dan de geconfigureerde tijd. De waarschuwing is uitgeschakeld wanneer de aanroep is voltooid.

* **SourceId**: System.RAP of System.Replicator
* **De eigenschap**: De naam van de langzame-API. De beschrijving bevat meer informatie over de tijd die de API in behandeling is.
* **Volgende stappen**: Onderzoek waarom de aanroep duurt langer dan verwacht.

Het volgende voorbeeld ziet de statusgebeurtenis van System.RAP voor een betrouwbare service die niet van de annulering naleven wordt token in **RunAsync**:

```PowerShell
PS C:\> Get-ServiceFabricReplicaHealth -PartitionId 5f6060fb-096f-45e4-8c3d-c26444d8dd10 -ReplicaOrInstanceId 131483966141404693


PartitionId           : 5f6060fb-096f-45e4-8c3d-c26444d8dd10
ReplicaId             : 131483966141404693
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.RA', Property='Reconfiguration', HealthState='Warning', ConsiderWarningAsError=false.
                        
HealthEvents          :                         
                        SourceId              : System.RAP
                        Property              : IStatefulServiceReplica.ChangeRole(S)Duration
                        HealthState           : Warning
                        SequenceNumber        : 131483966663476570
                        SentAt                : 8/28/2017 12:24:26 PM
                        ReceivedAt            : 8/28/2017 12:24:56 PM
                        TTL                   : Infinite
                        Description           : The api IStatefulServiceReplica.ChangeRole(S) on _Node_1 is stuck. Start Time (UTC): 2017-08-28 12:23:56.347.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 12:24:56 PM, LastOk = 1/1/0001 12:00:00 AM
                        
```

De eigenschap en de tekst moet u aangeven welke API is vastgelopen. De volgende stappen om te maken met verschillende vastgelopen API's zijn verschillend. API's op de *IStatefulServiceReplica* of *IStatelessServiceInstance* is meestal een bug in de servicecode. De volgende sectie wordt beschreven hoe deze wordt omgezet naar de [Reliable Services-model](service-fabric-reliable-services-lifecycle.md):

- **IStatefulServiceReplica.Open**: Deze waarschuwing geeft aan dat een aanroep van `CreateServiceInstanceListeners`, `ICommunicationListener.OpenAsync`, of als onderdrukt, `OnOpenAsync` is vastgelopen.

- **IStatefulServiceReplica.Close** en **IStatefulServiceReplica.Abort**: De meeste gevallen is een service niet naleven van de annulering-token dat is doorgegeven aan `RunAsync`. Het kan ook zijn dat `ICommunicationListener.CloseAsync`, of als onderdrukt, `OnCloseAsync` is vastgelopen.

- **IStatefulServiceReplica.ChangeRole (S)** en **IStatefulServiceReplica.ChangeRole(N)**: De meeste gevallen is een service niet naleven van de annulering-token dat is doorgegeven aan `RunAsync`.

- **IStatefulServiceReplica.ChangeRole(P)**: De meeste gevallen is de service heeft geen geretourneerd voor een taak in de `RunAsync`.

Andere API-aanroepen die kunnen zitten zijn op de **IReplicator** interface. Bijvoorbeeld:

- **IReplicator.CatchupReplicaSet**: Deze waarschuwing geeft aan dat er twee dingen. Er zijn onvoldoende van replica's. Als u wilt zien als dit het geval is, bekijkt u de status van de replica van de replica's in de partitie of het statusrapport System.FM voor een vastgelopen herconfiguratie. Of de replica's zijn bewerkingen niet bevestigd. De PowerShell-cmdlet `Get-ServiceFabricDeployedReplicaDetail` kan worden gebruikt om de voortgang van alle replica's te bepalen. Het probleem wordt veroorzaakt door replica's waarvan `LastAppliedReplicationSequenceNumber` waarde bevindt zich achter een van de primaire `CommittedSequenceNumber` waarde.

- **IReplicator.BuildReplica(<Remote ReplicaId>)**: Deze waarschuwing wijst op een probleem in het bouwproces. Zie voor meer informatie, [levenscyclus van Replica](service-fabric-concepts-replica-lifecycle.md). Het kan zijn vanwege een onjuiste configuratie van de replicatie-adres. Zie voor meer informatie, [stateful Reliable Services configureren](service-fabric-reliable-services-configuration.md) en [bronnen opgeven in een servicemanifest](service-fabric-service-manifest-resources.md). Het kan ook een probleem op het externe knooppunt zijn.

### <a name="replicator-system-health-reports"></a>Replicatie systeemstatusrapporten
**De replicatiewachtrij is vol:**
**System.Replicator** rapporten van een waarschuwing wanneer de replicatiewachtrij vol. is. Op de primaire raakt de replicatiewachtrij doorgaans vol omdat een of meer secundaire replica's worden traag uitgevoerd om te bevestigen van bewerkingen. Op de secundaire, dit gebeurt meestal wanneer de service is traag om toe te passen van de bewerkingen. De waarschuwing wordt gewist wanneer de wachtrij vol is.

* **SourceId**: System.Replicator
* **De eigenschap**: **PrimaryReplicationQueueStatus** of **SecondaryReplicationQueueStatus**, afhankelijk van de replicarol.
* **Volgende stappen**: Als het rapport op de primaire, controleert u de verbinding tussen de knooppunten in het cluster. Als alle verbindingen in orde zijn, is het mogelijk dat er ten minste één trage secundaire met een hoge latentie om toe te passen van bewerkingen. Als het rapport op de secundaire, eerst in het gebruik van de schijf en de prestaties op het knooppunt. Controleer vervolgens de uitgaande verbinding van de trage knooppunt naar de primaire.

**RemoteReplicatorConnectionStatus:**
**System.Replicator** rapporten op de primaire replica een waarschuwing wanneer de verbinding met een secundaire (extern) replicatie niet in orde is. De externe replicatieverbinding adres wordt weergegeven in het bericht van het rapport, waardoor het gemakkelijker om te detecteren als de verkeerde configuratie is doorgegeven of als er netwerkproblemen tussen de distributeurs zijn.

* **SourceId**: System.Replicator
* **De eigenschap**: **RemoteReplicatorConnectionStatus**.
* **Volgende stappen**: Controleer het foutbericht en zorg ervoor dat het adres van externe replicatieverbinding correct is geconfigureerd. Bijvoorbeeld, als de externe replicatieverbinding wordt geopend met de 'localhost' listen-adres, het is niet bereikbaar is vanaf de buitenkant. Als het adres is gelukt, controleert u de verbinding tussen het primaire knooppunt en het externe adres te vinden van eventuele mogelijke netwerkproblemen.

### <a name="replication-queue-full"></a>De replicatiewachtrij is vol
**System.Replicator** rapporten van een waarschuwing wanneer de replicatiewachtrij vol. is. Op de primaire raakt de replicatiewachtrij doorgaans vol omdat een of meer secundaire replica's worden traag uitgevoerd om te bevestigen van bewerkingen. Op de secundaire, dit gebeurt meestal wanneer de service is traag om toe te passen van de bewerkingen. De waarschuwing wordt gewist wanneer de wachtrij vol is.

* **SourceId**: System.Replicator
* **De eigenschap**: **PrimaryReplicationQueueStatus** of **SecondaryReplicationQueueStatus**, afhankelijk van de replicarol.

### <a name="slow-naming-operations"></a>Trage Naming-bewerkingen
**System.NamingService** de status op de primaire replica wordt gerapporteerd als een Naming-bewerking langer duurt dan acceptabel. Voorbeelden van Naming bewerkingen zijn [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) of [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync). Meer methoden kunnen u vinden onder FabricClient. Bijvoorbeeld, ze kunnen u vinden onder [service beheermethoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) of [eigenschap beheermethoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).

> [!NOTE]
> De Naming-service wordt servicenamen omgezet naar een locatie in het cluster. Gebruikers kunnen deze gebruiken voor het beheren van servicenamen en eigenschappen. Het is een Service Fabric gepartitioneerd-persistente-service. Een van de partities vertegenwoordigt de *Authority Owner*, waarin metagegevens over alle Service Fabric-namen en services. De namen van de Service Fabric worden toegewezen aan verschillende partities, genaamd *Name Owner* partities, zodat de service uitgebreid worden. Meer informatie over de [Naming service](service-fabric-architecture.md).
> 
> 

Wanneer een Naming-bewerking langer duurt dan verwacht, wordt de bewerking is gemarkeerd met een rapport van de waarschuwing op de primaire replica van de partitie die Naming-service die de bewerking fungeert. Als de bewerking voltooid is, wordt de waarschuwing is uitgeschakeld. Als de bewerking is voltooid met een fout, bevat het statusrapport details over de fout.

* **SourceId**: System.NamingService
* **De eigenschap**: Begint met het voorvoegsel "**Duration_**' en identificeert de trage bewerking en de Service Fabric-naam op die de bewerking wordt toegepast. Bijvoorbeeld, als service maken op de naam **fabric: / Mijntoep/MijnService** duurt te lang, de eigenschap is **Duration_AOCreateService.fabric:/MyApp/MyService**. "Door de AO" verwijst naar de rol van de partitie Naming voor deze naam en de bewerking.
* **Volgende stappen**: Selectievakje om te zien waarom de Naming-bewerking is mislukt. Elke bewerking kan verschillende oorzaken hebben. Bijvoorbeeld, de delete-service kan blijven steken. De service kan blijven steken omdat de toepassingshost op een knooppunt vanwege een fout van gebruiker in de servicecode vastlopen houdt.

Het volgende voorbeeld ziet u een bewerking van de service maken. De bewerking duurde langer dan de geconfigureerde duur. 'Door de AO' nieuwe pogingen en werk verzendt naar "Nee" 'Nee' voltooid de laatste bewerking met time-out. In dit geval is dezelfde replica primaire voor zowel de 'door de AO' en 'Nee' rollen.

```PowerShell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>DeployedApplication systeemstatusrapporten
**System.Hosting** is de instantie voor geïmplementeerde entiteiten.

### <a name="activation"></a>Activering
System.Hosting rapporten als OK wanneer een toepassing is geactiveerd op het knooppunt. Anders wordt een fout.

* **SourceId**: System.Hosting
* **De eigenschap**: **Activering**, met inbegrip van de versie van de implementatie.
* **Volgende stappen**: Als de toepassing niet in orde is, moet u onderzoeken waarom de activering is mislukt.

Het volgende voorbeeld ziet u een geslaagde activering:

```PowerShell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ExcludeHealthStatistics

ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_1
                                     AggregatedHealthState : Ok
                                     
HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131445249083836329
                                     SentAt                : 7/14/2017 4:55:08 PM
                                     ReceivedAt            : 7/14/2017 4:55:14 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>Downloaden
System.Hosting meldt een fout als het pakket downloaden van de toepassing is mislukt.

* **SourceId**: System.Hosting
* **De eigenschap**: **Download**, met inbegrip van de versie van de implementatie.
* **Volgende stappen**: Onderzoek waarom de download is mislukt op het knooppunt.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage systeemstatusrapporten
**System.Hosting** is de instantie voor geïmplementeerde entiteiten.

### <a name="service-package-activation"></a>Activeren van pakket
System.Hosting rapporteert als OK als de activering van de service-pakket op het knooppunt geslaagd is. Anders wordt een fout.

* **SourceId**: System.Hosting
* **De eigenschap**: De activering.
* **Volgende stappen**: Onderzoek waarom de activering is mislukt.

### <a name="code-package-activation"></a>Pakket-codeactivering
System.Hosting rapporteert als OK voor ieder codepakket als de activering geslaagd is. Als de activering is mislukt, wordt een waarschuwing weergegeven zoals geconfigureerd. Als **CodePackage** niet kan activeren of eindigt met een fout die groter is dan de geconfigureerde **CodePackageHealthErrorThreshold**, die als host fungeert meldt een fout. Als een servicepakket meerdere pakketten bevat, wordt een activering-rapport wordt gegenereerd voor elk criterium.

* **SourceId**: System.Hosting
* **De eigenschap**: Maakt gebruik van het voorvoegsel **CodePackageActivation** en bevat de naam van het codepakket en het toegangspunt dat als *CodePackageActivation:CodePackageName:SetupEntryPoint / EntryPoint*. Bijvoorbeeld, **CodePackageActivation:Code:SetupEntryPoint**.

### <a name="service-type-registration"></a>De service type is geregistreerd
System.Hosting rapporteert als OK als het servicetype is geregistreerd. Een fout gerapporteerd als de registratie niet is gedaan in de tijd, zoals deze is geconfigureerd met behulp van **ServiceTypeRegistrationTimeout**. Als de runtime is gesloten, wordt het servicetype is niet geregistreerd in het knooppunt en wordt een waarschuwing die als host fungeert rapporten.

* **SourceId**: System.Hosting
* **De eigenschap**: Maakt gebruik van het voorvoegsel **ServiceTypeRegistration** en bevat de naam van de service-type. Bijvoorbeeld, **ServiceTypeRegistration:FileStoreServiceType**.

Het volgende voorbeeld ziet u een pakket in orde geïmplementeerde service:

```PowerShell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_1
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131445249084026346
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131445249084306362
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131445249088096842
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>Downloaden
System.Hosting meldt een fout als het downloaden van het service-pakket is mislukt.

* **SourceId**: System.Hosting
* **De eigenschap**: **Download**, met inbegrip van de versie van de implementatie.
* **Volgende stappen**: Onderzoek waarom de download is mislukt op het knooppunt.

### <a name="upgrade-validation"></a>Validatie van de upgrade
System.Hosting, wordt er een fout gemeld als validatie tijdens de upgrade mislukt of als de upgrade op het knooppunt mislukt.

* **SourceId**: System.Hosting
* **De eigenschap**: Maakt gebruik van het voorvoegsel **FabricUpgradeValidation** en de upgrade-versie bevat.
* **Beschrijving**: Verwijst naar de fout is opgetreden.

### <a name="undefined-node-capacity-for-resource-governance-metrics"></a>Niet-gedefinieerde knooppuntcapaciteit voor metrische gegevens voor het beheer van resources
System.Hosting rapporteert een waarschuwing als knooppuntcapaciteit niet zijn gedefinieerd in het clustermanifest en de configuratie voor automatische detectie is uitgeschakeld. Service Fabric genereert een status-waarschuwing wanneer het servicepakket die gebruikmaakt van [resourcebeheer](service-fabric-resource-governance.md) registreert op een opgegeven knooppunt.

* **SourceId**: System.Hosting
* **De eigenschap**: **ResourceGovernance**.
* **Volgende stappen**: De voorkeursmethode voor het oplossen van dit probleem is om te wijzigen van het clustermanifest om in te schakelen van automatische detectie van beschikbare resources. Een andere manier is het bijwerken van het clustermanifest met correct opgegeven knooppuntcapaciteiten voor deze metrische gegevens.

## <a name="next-steps"></a>Volgende stappen
* [Service Fabric-statusrapporten weergeven](service-fabric-view-entities-aggregated-health.md)

* [Het servicestatus rapporteren en controleren](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

* [Controle en diagnose van services lokaal](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

* [Service Fabric-toepassingsupgrade](service-fabric-application-upgrade.md)

