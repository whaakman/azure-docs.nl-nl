---
title: Problemen oplossen met systeemstatusrapporten | Microsoft Docs
description: Beschrijft de statusrapporten dat is verzonden door Azure Service Fabric-onderdelen en hun gebruik voor het oplossen van problemen cluster of toepassingsproblemen
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 52574ea7-eb37-47e0-a20a-101539177625
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/11/2017
ms.author: oanapl
ms.openlocfilehash: cd9a144baf06422b425a0bc6c516600d6fcd4b97
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="use-system-health-reports-to-troubleshoot"></a>Systeemstatusrapporten gebruiken om fouten op te lossen
Azure Service Fabric-onderdelen bieden systeemstatusrapporten op alle entiteiten in het cluster uit de verpakking. De [health store](service-fabric-health-introduction.md#health-store) maken en verwijderen van de entiteiten die zijn gebaseerd op systeemrapporten van het. Ook ordent ze in een hiërarchie die entiteit interacties worden vastgelegd.

> [!NOTE]
> Om te begrijpen health-gerelateerde begrippen, leest u meer op [Service Fabric-statusmodel](service-fabric-health-introduction.md).
> 
> 

Systeemstatusrapporten bieden inzicht in het cluster en de functionaliteit van de toepassing en de vlag problemen. Voor toepassingen en services controleren systeemstatusrapporten of entiteiten worden geïmplementeerd en correct vanuit het perspectief van de Service Fabric werkt zijn. De rapporten bieden niet een statuscontrole van de zakelijke logica van de service of detectie van vastgelopen processen. Services van de gebruiker kunnen de health-gegevens met informatie die specifiek zijn voor hun logica aanvullen.

> [!NOTE]
> Statusrapporten verzonden door de gebruiker watchdogs zijn alleen zichtbaar *nadat* Maak een entiteit van de onderdelen van het systeem. Wanneer een entiteit wordt verwijderd, worden alle statusrapporten gekoppeld in de health store automatisch verwijderd. Geldt ook wanneer een nieuw exemplaar van de entiteit is gemaakt, bijvoorbeeld wanneer een nieuwe stateful persistent service-exemplaar van replica is gemaakt. Alle rapporten die zijn gekoppeld aan het oude exemplaar worden verwijderd en opgeschoond vanuit de store.
> 
> 

Het systeemonderdeel rapporten zijn geïdentificeerd door de bron die met begint de '**System.**' voorvoegsel. Watchdogs niet hetzelfde voorvoegsel gebruiken voor hun bronnen, zoals rapporten met ongeldige parameters worden afgewezen.

Bekijken we enkele systeemrapporten om te begrijpen wat ze activeert en voor informatie over het corrigeren van de potentiële problemen die ze vertegenwoordigen.

> [!NOTE]
> Service Fabric blijft rapporten toevoegen van de voorwaarden van belang dat u inzicht in wat er in het cluster gebeurt te verbeteren en de toepassingen bestaande rapporten kunnen worden uitgebreid met meer details om u te helpen bij het oplossen van het probleem sneller.
> 
> 

## <a name="cluster-system-health-reports"></a>Systeemstatusrapporten cluster
De entiteit van de health cluster wordt automatisch gemaakt in de health store. Als alles goed werkt, heeft geen system-rapport.

### <a name="neighborhood-loss"></a>Verlies van groep
**System.Federation** meldt fout wanneer er een groep verlies wordt gedetecteerd. Het rapport is van afzonderlijke knooppunten en de knooppunt-ID is opgenomen in de eigenschapsnaam. Als een groep in de hele Service Fabric-ring verbroken is, kunt u twee gebeurtenissen die beide zijden van het rapport hiaat vertegenwoordigen meestal verwachten. Als er meer groepen verloren gaan, moet u er meer gebeurtenissen zijn.

Het rapport geeft de algemene lease time-out als de time-to-live (TTL). Het rapport opnieuw elke helft van de duur van de TTL voor verzonden als de voorwaarde actief blijft. De gebeurtenis wordt automatisch verwijderd wanneer het verloopt. Verwijderen wanneer verlopen gedrag zorgt ervoor dat het rapport wordt opgeschoond van de health store correct, zelfs als het reporting knooppunt niet actief is.

* **SourceId**: System.Federation
* **De eigenschap**: begint met **groep** en informatie gegeven knooppunt.
* **Volgende stappen**: onderzoeken waarom de groep bijvoorbeeld verloren is, controleert u de communicatie tussen clusterknooppunten.

### <a name="rebuild"></a>Opnieuw samenstellen

De **Failover Manager** service (**FM**) beheer van informatie over de clusterknooppunten. Wanneer FM de gegevens verliest en probeert het verlies van gegevens die kan niet dat garanderen is de meest actuele informatie over de clusterknooppunten. In dit geval wordt het systeem doorloopt een **opnieuw samenstellen**, en **System.FM** verzamelt gegevens van alle knooppunten in het cluster om de status opnieuw opbouwen. Soms als gevolg van netwerk- of knooppunt problemen kan rebuild ophalen vastgelopen of vastgelopen. Hetzelfde kan optreden met de **Failover Manager Master** service (**FMM**). De **FMM** is een staatloze systeemservice waarin wordt bijgehouden waar alle van de **FMs** zijn in het cluster. De **FMMs** primaire is altijd het knooppunt met de ID die het dichtst bij 0. Als u dat knooppunt wegvalt, een **opnieuw samenstellen** wordt geactiveerd.
Als een van de vorige voorwaarden optreden, **System.FM** of **System.FMM** markeren via een foutenrapport. Rebuild kan blijven steken bij een van twee fasen:

* Een ogenblik geduld: **FM/FMM** wordt gewacht op het antwoord broadcast-bericht van de andere knooppunten. **Volgende stappen:** onderzoek of er een netwerkprobleem verbinding tussen knooppunten is.   
* Wachten op knooppunten: **FM/FMM** al een broadcast-antwoord ontvangen van de andere knooppunten en wacht op een antwoord van de specifieke knooppunten. Het statusrapport geeft een lijst van de knooppunten waarvoor de **FM/FMM** wachten op reactie. **Volgende stappen:** onderzoeken van de netwerkverbinding tussen de **FM/FMM** en de vermelde knooppunten. Elke vermelde knooppunt voor andere mogelijke problemen onderzoeken.

* **SourceID**: System.FM of System.FMM
* **De eigenschap**: opnieuw opbouwen.
* **Volgende stappen**: de netwerkverbinding tussen de knooppunten, evenals de status van de specifieke knooppunten die worden vermeld op de beschrijving van het statusrapport onderzoeken.

## <a name="node-system-health-reports"></a>Systeemstatusrapporten knooppunt
**System.FM**, die staat voor de Failover Manager service, wordt de instantie die het beheer van informatie over de clusterknooppunten. Elk knooppunt moet één rapport van System.FM met de status hebben. De knooppunt-entiteiten worden verwijderd wanneer de status van knooppunt is verwijderd. Zie voor meer informatie [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync).

### <a name="node-updown"></a>Knooppunt omhoog/omlaag
System.FM rapporteert als OK als lid van het knooppunt de ring (dit is actief en werkend). Een fout gemeld wanneer het knooppunt de ring vertrekt (service niet actief is, ofwel voor het upgraden of gewoon omdat deze is mislukt). De health-hiërarchie gebouwd door de health store omgaat met geïmplementeerde entiteiten in correlatie met System.FM knooppunt rapporten. Er vanuit het knooppunt een bovenliggende virtuele van alle geïmplementeerde entiteiten. De geïmplementeerde entiteiten op dat knooppunt worden weergegeven via query's of het knooppunt als bedrijfs is gemeld door System.FM met hetzelfde exemplaar als de instantie die is gekoppeld aan de entiteiten. Wanneer System.FM dat meldt het knooppunt is niet beschikbaar of opnieuw wordt opgestart, als een nieuw exemplaar de health store automatisch opruimen van de geïmplementeerde entiteiten die alleen op het knooppunt omlaag of op het vorige exemplaar van het knooppunt kunnen bestaan.

* **SourceId**: System.FM
* **De eigenschap**: status.
* **Volgende stappen**: als het knooppunt niet actief voor een upgrade is, deze moet terugkeren van nadat het bijgewerkt. In dit geval wordt moet de status overschakelen naar OK. Als het knooppunt niet u terug keert of deze is mislukt, moet het probleem meer onderzoek.

Het volgende voorbeeld ziet de gebeurtenis System.FM met een status OK voor knooppunt:

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
**System.FabricNode** rapporten van een waarschuwing wanneer certificaten worden gebruikt door het knooppunt bijna is verlopen zijn. Er zijn drie certificaten per knooppunt: **Certificate_cluster**, **Certificate_server**, en **Certificate_default_client**. Wanneer de vervaldatum ten minste twee weken is, is de status van het rapport OK. Wanneer de vervaldatum binnen twee weken is, is het rapporttype dat een waarschuwing. TTL van deze gebeurtenissen is oneindig, en ze worden verwijderd wanneer een knooppunt de cluster verlaat.

* **SourceId**: System.FabricNode
* **De eigenschap**: begint met **certificaat** en meer informatie over het certificaattype bevat.
* **Volgende stappen**: bijwerken van de certificaten als ze bijna is verlopen zijn.

### <a name="load-capacity-violation"></a>Schending van de Load-capaciteit
De Service Fabric Load Balancer rapporten in een waarschuwing wanneer er een schending van de capaciteit knooppunt wordt gedetecteerd.

* **SourceId**: System.PLB
* **De eigenschap**: begint met **capaciteit**.
* **Volgende stappen**: Controleer de opgegeven metrische gegevens en de huidige capaciteit op het knooppunt.

### <a name="node-capacity-mismatch-for-resource-governance-metrics"></a>Knooppunt capaciteit komt niet overeen voor resource governance metrische gegevens
Rapporten System.Hosting een waarschuwing als knooppunt capaciteit gedefinieerd in het clustermanifest zijn groter dan de capaciteit echte knooppunt voor resource governance metrische gegevens (geheugen en cpu-kernen). Statusrapport van worden weergegeven bij het eerste pakket van de service die gebruikmaakt van [resource governance](service-fabric-resource-governance.md) registreert op een opgegeven knooppunt.

* **SourceId**: System.Hosting
* **De eigenschap**: ResourceGovernance
* **Volgende stappen**: dit is een probleem zoals van bestuur servicepakketten niet gelden zoals verwacht en [resource governance](service-fabric-resource-governance.md) werkt niet goed. Bijwerken van het clustermanifest met de juiste knooppunt capaciteit voor deze metrische gegevens of niet Geef ze op alle en laten Service Fabric voor het automatisch detecteren van beschikbare resources.

## <a name="application-system-health-reports"></a>Systeemstatusrapporten toepassing
**System.CM**, die staat voor de Cluster Manager-service, wordt de instantie van die gegevens over een toepassing beheert.

### <a name="state"></a>Status
System.CM rapporteert als OK wanneer de toepassing heeft gemaakt of bijgewerkt. Informeert de health store wanneer de toepassing is verwijderd, zodat het kan worden verwijderd uit de store.

* **SourceId**: System.CM
* **De eigenschap**: status.
* **Volgende stappen**: als de toepassing is gemaakt of bijgewerkt, moet deze het statusrapport Clusterbeheer opnemen. Bekijk anders de status van de toepassing door uitgifte van een query, bijvoorbeeld de PowerShell-cmdlet **Get-ServiceFabricApplication - ApplicationName** *applicationName*.

Het volgende voorbeeld ziet u de gebeurtenis status op de **fabric: / WordCount** toepassing:

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

## <a name="service-system-health-reports"></a>Systeemstatusrapporten service
**System.FM**, die staat voor de service Failover Manager is de instantie die informatie over services beheert.

### <a name="state"></a>Status
System.FM rapporteert als OK wanneer de service is gemaakt. Worden verwijderd de entiteit van de health store wanneer de service is verwijderd.

* **SourceId**: System.FM
* **De eigenschap**: status.

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

### <a name="service-correlation-error"></a>Fout van de correlatie-service
**System.PLB** meldt fout wanneer er wordt gedetecteerd dat een service bijwerken worden gecorreleerd met een andere service die wordt gemaakt van een affiniteitsketen. Het rapport wordt gewist wanneer een geslaagde update gebeurt.

* **SourceId**: System.PLB
* **De eigenschap**: ServiceDescription.
* **Volgende stappen**: Controleer de servicebeschrijvingen van gecorreleerde.

## <a name="partition-system-health-reports"></a>Systeemstatusrapporten partitie
**System.FM**, die staat voor de service Failover Manager is de instantie die informatie over servicepartities beheert.

### <a name="state"></a>Status
System.FM rapporteert als OK wanneer de partitie is gemaakt en is in orde. Worden verwijderd de entiteit van de health store wanneer de partitie wordt verwijderd.

Als de partitie lager dan het aantal minimale replica is, is een fout rapporteert. Als de partitie niet lager dan het aantal minimale replica is, maar dit lager dan het aantal replica's van doel is, wordt een waarschuwing. Als de partitie is sprake van quorumverlies, rapporteert System.FM een fout opgetreden.

Andere belangrijke gebeurtenissen bevatten een waarschuwing wanneer de herconfiguratie langer duurt dan verwacht en wanneer de build langer duurt dan verwacht. De verwachte tijden voor de build en herconfiguratie geconfigureerd worden op basis van de service-scenario's. Bijvoorbeeld, als een service een terabyte van status, zoals Azure SQL Database heeft, duurt de build langer dan voor een service met een kleine hoeveelheid staat.

* **SourceId**: System.FM
* **De eigenschap**: status.
* **Volgende stappen**: als de status niet OK is, is het mogelijk dat sommige replica's niet zijn gemaakt, geopend, of juist gepromoveerd tot primaire of secundaire site. 

Als de beschrijving quorumverlies beschrijft, vervolgens weer het gedetailleerde statusrapport voor replica's die niet beschikbaar zijn onderzoek en maak een back-up van helpt om de partitie worden gezet online.

Als de beschrijving van de beschrijving van een partitie is vastgelopen in [herconfiguratie](service-fabric-concepts-reconfiguration.md), en vervolgens het statusrapport op de primaire replica aanvullende informatie bevat.

Andere statusrapporten System.FM zou worden er rapporten van de replica's of de partitie of service van andere onderdelen van het systeem. 

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

Het volgende voorbeeld toont de status van een partitie die lager is dan het aantal doel-replica's. De volgende stap is om op te halen van de Partitiebeschrijving, waarin configuratie: **MinReplicaSetSize** is drie en **TargetReplicaSetSize** zeven. Vervolgens het aantal knooppunten in het cluster dat in dit geval vijf ophalen Worden dus in dit geval twee replica's kunnen niet geplaatst, omdat het doelaantal replica's hoger dan het aantal knooppunten beschikbaar is.

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

Het volgende voorbeeld ziet u de status van een partitie die vastgelopen in herconfiguratie als gevolg van de gebruiker niet naleven van de annulering token in de **RunAsync** methode. Het onderzoeken van het statusrapport van elke replica gemarkeerd als primair (P) kunt u inzoomen op het probleem verder.

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
                        
                        For more information see: http://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 8/27/2017 3:43:32 AM, LastError = 1/1/0001 12:00:00 AM
```
Dit rapport health toont de status van de replica's van de partitie momenteel herconfiguratie uitgevoerd: 

```
  P/S Ready Node1 131482789658160654
  S/P Ready Node2 131482789688598467
  S/S Ready Node3 131482789688598468
```

Het statusrapport bevat voor elke replica:
- Rol van de vorige configuratie
- De huidige rol van de configuratie
- [Replicastatus](service-fabric-concepts-replica-lifecycle.md)
- Knooppunt waarop de replica wordt uitgevoerd
- Replica-ID

In geval als in het voorbeeld verder onderzoek nodig. Onderzoek de status van elke afzonderlijke replica die beginnen met de replica's gemarkeerd als `Primary` en `Secondary` (131482789658160654 en 131482789688598467) in het vorige voorbeeld.

### <a name="replica-constraint-violation"></a>Replica-Beperkingsfout
**System.PLB** rapporteert een waarschuwing als het een overtreding van een replica wordt gedetecteerd en alle replica's van partitie kan niet worden geplaatst. Details van het rapport weergeven welke beperkingen en eigenschappen te voorkomen dat de replica-plaatsing.

* **SourceId**: System.PLB
* **De eigenschap**: begint met **ReplicaConstraintViolation**.

## <a name="replica-system-health-reports"></a>Systeemstatusrapporten replica
**System.RA**, die staat voor het onderdeel reconfiguration agent is de instantie voor de replicastatus van de.

### <a name="state"></a>Status
System.RA rapporten OK wanneer de replica is gemaakt.

* **SourceId**: System.RA
* **De eigenschap**: status.

Het volgende voorbeeld ziet u een replica in orde:

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
Deze eigenschap wordt gebruikt om aan te geven waarschuwingen of fouten bij een poging tot openen van een replica, sluiten van een replica of een replica van de ene rol naar de andere worden overgezet. Zie voor meer informatie [Replica lifecycle](service-fabric-concepts-replica-lifecycle.md). De storingen mogelijk uitzonderingen van de API-aanroepen of crashes van het hostproces van de service gedurende deze tijd zijn. Service Fabric wordt voor storingen ten gevolge van API-aanroepen van C#-code, de uitzondering en de stacktracering toegevoegd aan het statusrapport.

Deze health-waarschuwingen worden gegenereerd na de actie opnieuw lokaal een aantal keren (afhankelijk van beleid). Service Fabric pogingen de actie tot een maximale drempelwaarde. Nadat de maximale drempelwaarde is bereikt, wellicht het proberen om te fungeren voor de situatie oplossen. Dit kan leiden tot deze waarschuwingen gewist als het op de actie op dit knooppunt geeft. Bijvoorbeeld, als een replica niet openen op een knooppunt, Service Fabric leidt tot een waarschuwing. Als de replica mislukken om te openen blijft, fungeert de Service Fabric om te herstellen. Deze actie kan erbij betrekken bij dezelfde bewerking op een ander knooppunt. Dit zorgt ervoor dat de waarschuwing is gegenereerd voor deze replica worden gewist. 

* **SourceId**: System.RA
* **De eigenschap**: **ReplicaOpenStatus**, **ReplicaCloseStatus**, en **ReplicaChangeRoleStatus**.
* **Volgende stappen**: onderzoeken van de service-code of vastlopen dumpbestanden om te bepalen waarom de bewerking is mislukt.

Het volgende voorbeeld toont de status van een replica die die `TargetInvocationException` van de methode open. De beschrijving bevat het storingspunt, **IStatefulServiceReplica.Open**, het uitzonderingstype **TargetInvocationException**, en de stack-trace.

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

    For more information see: http://aka.ms/sfhealth
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
                        
                        For more information see: http://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 1:16:03 AM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="reconfiguration"></a>Herconfiguratie
Deze eigenschap wordt gebruikt om aan te geven bij het uitvoeren van een replica een [herconfiguratie](service-fabric-concepts-reconfiguration.md) detecteert dat de nieuwe configuratie is vastgelopen of vastgelopen. Dit rapport health mogelijk op de replica waarvan de huidige rol is de primaire, behalve in het geval van een primaire herconfiguratie wisselen waar dit mogelijk is op de replica die van primaire naar de actieve secundaire wordt gedegradeerd.

De nieuwe configuratie kan blijven steken voor een van de volgende redenen:

- Een actie op de lokale replica, dezelfde replica als het uitvoeren van de nieuwe configuratie, niet is voltooid. In dit geval bijvoorbeeld de statusrapporten op deze replica uit andere onderdelen onderzoeken, System.RAP of System.RE, aanvullende gegevens bevatten.

- Een actie is niet op een externe replica is voltooid. Replica's waarvoor acties in behandeling zijn, worden vermeld in het statusrapport. Verder onderzoek moet worden gedaan op statusrapporten voor deze externe replica's. Er kan ook zijn communicatieproblemen tussen dit knooppunt en het externe knooppunt.

In zeldzame gevallen kan de herconfiguratie worden vastgelopen vanwege communicatie of andere problemen tussen dit knooppunt en de Failover Manager-service.

* **SourceId**: System.RA
* **De eigenschap**: **herconfiguratie**.
* **Volgende stappen**: lokale of externe replica's, afhankelijk van de beschrijving van het statusrapport onderzoeken.

Het volgende voorbeeld ziet een statusrapport waar een herconfiguratie uitgevoerd op de lokale replica is vastgelopen. In dit voorbeeld wordt wordt het als gevolg van een service niet naleven van het token voor annulering.

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
                        
                        For more information see: http://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 2:13:57 AM, LastOk = 1/1/0001 12:00:00 AM
```

Het volgende voorbeeld ziet u een health rapporteren wanneer een nieuwe configuratie is vastgelopen wachten op reactie van de twee externe replica's. In dit voorbeeld worden drie replica's in de partitie, met inbegrip van de huidige primaire. 

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
                        
                        For more information see: http://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 12:07:37 PM, LastOk = 1/1/0001 12:00:00 AM
```

Dit statusrapport bevat de nieuwe configuratie is vastgelopen wachten op reactie van twee replica's: 

```
    P/I Down 40 131483956244554282
    S/S Down 20 131483956274972403
```

Voor elke replica wordt verkregen met de volgende informatie:
- Rol van de vorige configuratie
- De huidige rol van de configuratie
- [Replicastatus](service-fabric-concepts-replica-lifecycle.md)
- Knooppunt-id
- Replica-ID

De herconfiguratie deblokkeren:
- De **omlaag** replica's moeten worden opgeroepen. 
- De **inbuild** replica's moeten de build voltooien en overgang op gereed.

### <a name="slow-service-api-call"></a>Trage service API-aanroep
**System.RAP** en **System.Replicator** rapporteren van een waarschuwing als een aanroep van de code van de gebruiker langer dan de geconfigureerde tijd duurt. De waarschuwing wordt gewist wanneer de aanroep is voltooid.

* **SourceId**: System.RAP of System.Replicator
* **De eigenschap**: de naam van de langzaam API. De beschrijving biedt meer informatie over de tijd die de API in behandeling is.
* **Volgende stappen**: onderzoeken waarom de aanroep duurt langer dan verwacht.

Het volgende voorbeeld wordt de statusgebeurtenis van System.RAP voor een betrouwbare service die niet van de annulering naleven wordt token in **RunAsync**:

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

De eigenschap en tekst aangeven welke API is vastgelopen. De volgende stappen te nemen voor verschillende vastgelopen API's verschilt. API's op de *IStatefulServiceReplica* of *IStatelessServiceInstance* is meestal een fout in de servicecode. De volgende sectie wordt beschreven hoe deze kan vertalen naar de [model voor Reliable Services](service-fabric-reliable-services-lifecycle.md):

- **IStatefulServiceReplica.Open**: deze waarschuwing geeft aan dat een aanroep van `CreateServiceInstanceListeners`, `ICommunicationListener.OpenAsync`, of als genegeerd, `OnOpenAsync` is vastgelopen.

- **IStatefulServiceReplica.Close** en **IStatefulServiceReplica.Abort**: de meeste gevallen is een service die niet naleven van de annulering-token dat is doorgegeven aan `RunAsync`. Het kan ook zijn dat `ICommunicationListener.CloseAsync`, of als genegeerd, `OnCloseAsync` is vastgelopen.

- **IStatefulServiceReplica.ChangeRole (S)** en **IStatefulServiceReplica.ChangeRole(N)**: de meeste gevallen is een service die niet naleven van de annulering-token dat is doorgegeven aan `RunAsync`.

- **IStatefulServiceReplica.ChangeRole(P)**: de meeste gevallen is de service heeft geen geretourneerd voor een taak vanuit `RunAsync`.

Andere API-aanroepen die hangen kunnen zijn op de **IReplicator** interface. Bijvoorbeeld:

- **IReplicator.CatchupReplicaSet**: deze waarschuwing geeft aan een van twee dingen. Ofwel er zijn onvoldoende up van replica's kunnen worden bepaald door de status van de replica van de replica's in de partitie of het statusrapport System.FM voor een vastgelopen herconfiguratie kijken. Of de replica's zijn geen bewerkingen zijn bevestigd. De PowerShell-opdracht-let `Get-ServiceFabricDeployedReplicaDetail` kan worden gebruikt om te bepalen van de voortgang van de replica's. Het probleem wordt veroorzaakt met replica's waarvan `LastAppliedReplicationSequenceNumber` zich achter de primaire `CommittedSequenceNumber`.

- **IReplicator.BuildReplica (<Remote ReplicaId>)**: deze waarschuwing wijst op een probleem in de buildproces. Zie voor meer informatie [Replica lifecycle](service-fabric-concepts-replica-lifecycle.md). Het is misschien vanwege een onjuiste configuratie van het adres replicator. Zie voor meer informatie [stateful Reliable Services configureren](service-fabric-reliable-services-configuration.md) en [bronnen opgeven in een servicemanifest](service-fabric-service-manifest-resources.md). Het kan ook een probleem op het externe knooppunt zijn.

### <a name="replication-queue-full"></a>Volledige replicatiewachtrij
**System.Replicator** rapporten van een waarschuwing wanneer de replicatiewachtrij vol is. Op de primaire raakt de wachtrij voor replicatie staan doorgaans vol omdat een of meer secundaire replica's langzaam zijn bevestiging operations. Op de secundaire, dit gebeurt meestal wanneer de service langzaam is worden de bewerkingen toepassen. De waarschuwing wordt gewist wanneer de wachtrij vol is.

* **SourceId**: System.Replicator
* **De eigenschap**: **PrimaryReplicationQueueStatus** of **SecondaryReplicationQueueStatus**, afhankelijk van de replicarol.

### <a name="slow-naming-operations"></a>Trage Naming bewerkingen
**System.NamingService** rapporteert de status op de primaire replica wanneer u een naam geven bewerking duurt langer dan de aanvaardbare. Voorbeelden van Naming bewerkingen zijn [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) of [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync). Meer methoden kunnen u vinden onder FabricClient, bijvoorbeeld onder [service beheermethoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) of [eigenschap beheermethoden](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).

> [!NOTE]
> De service Naming servicenamen worden omgezet in een locatie in het cluster en kan gebruikers beheren servicenamen en eigenschappen. Dit is een Service Fabric gepartitioneerd persistent service. Een van de partities vertegenwoordigt de *Authority Owner*, die de metagegevens over alle Service Fabric-namen en -services bevat. De Service Fabric-namen worden toegewezen aan verschillende partities, genaamd *Name Owner* partities, zodat de service kan uitgebreid worden. Meer informatie over de [Naming service](service-fabric-architecture.md).
> 
> 

Wanneer een Naming bewerking langer duurt dan verwacht, wordt de bewerking gemarkeerd met een rapport van de waarschuwing op de primaire replica van de Naming service partitie die de bewerking fungeert. Als de bewerking voltooid is, wordt de waarschuwing is uitgeschakeld. Als de bewerking is voltooid met een fout, bevat het statusrapport meer informatie over de fout.

* **SourceId**: System.NamingService
* **De eigenschap**: begint met het voorvoegsel '**Duration_**' en identificeert de trage bewerking en de Service Fabric-naam waarop de bewerking wordt toegepast. Bijvoorbeeld, als service maken op de naam van **fabric: / MyApp/MijnService** duurt te lang, de eigenschap is **Duration_AOCreateService.fabric:/MyApp/MyService**. "AO" verwijst naar de rol van de partitie Naming voor deze naam en het opnieuw.
* **Volgende stappen**: Controleer waarom de Naming-bewerking is mislukt. Elke bewerking kan verschillende oorzaken hebben. Bijvoorbeeld: de service verwijderen kan blijven steken. De service kan blijven steken omdat toepassingshost op een knooppunt als gevolg van een gebruiker fout in de servicecode vastlopen houdt.

Het volgende voorbeeld ziet een servicebewerking maken. De bewerking duurt langer dan de geconfigureerde duur. 'AO' pogingen en werk verzendt naar "Nee" 'Nee' laatste bewerking voltooid met time-out. In dit geval is de dezelfde replica primaire voor zowel de 'AO' als 'Nee' rollen.

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

## <a name="deployedapplication-system-health-reports"></a>Systeemstatusrapporten DeployedApplication
**System.Hosting** is de instantie voor geïmplementeerde entiteiten.

### <a name="activation"></a>Activering
System.Hosting rapporteert als OK als u een toepassing is geactiveerd op het knooppunt. Anders wordt een fout opgetreden.

* **SourceId**: System.Hosting
* **De eigenschap**: activering, waaronder de implementatie-versie.
* **Volgende stappen**: als de toepassing niet in orde is, moet u onderzoeken waarom de activering is mislukt.

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
* **De eigenschap**: **downloaden:***RolloutVersion*.
* **Volgende stappen**: onderzoeken waarom het downloaden is mislukt op het knooppunt.

## <a name="deployedservicepackage-system-health-reports"></a>Systeemstatusrapporten DeployedServicePackage
**System.Hosting** is de instantie voor geïmplementeerde entiteiten.

### <a name="service-package-activation"></a>Het activeren van service
System.Hosting rapporten als OK als de activering van de service-pakket op het knooppunt geslaagd is. Anders wordt een fout opgetreden.

* **SourceId**: System.Hosting
* **De eigenschap**: activering.
* **Volgende stappen**: onderzoeken waarom de activering is mislukt.

### <a name="code-package-activation"></a>Pakket-codeactivering
System.Hosting rapporten als OK voor elk codepakket als de activering geslaagd is. Als de activering mislukt, wordt een waarschuwing weergegeven zoals geconfigureerd. Als **CodePackage** niet kan activeren of eindigt met een groter is dan de geconfigureerde fout **CodePackageHealthErrorThreshold**, die als host fungeert een fout gemeld. Als een servicepakket meerdere code pakketten bevat, wordt een rapport activering gegenereerd voor elk criterium.

* **SourceId**: System.Hosting
* **De eigenschap**: maakt gebruik van het voorvoegsel **CodePackageActivation** en bevat de naam van het codepakket en het toegangspunt dat als **CodePackageActivation:***CodePackageName* :*Entrypoint/EntryPoint*. Bijvoorbeeld: **CodePackageActivation:Code:SetupEntryPoint**.

### <a name="service-type-registration"></a>Service type is geregistreerd
System.Hosting rapporteert als OK als het servicetype is geregistreerd. Een fout gerapporteerd als de registratie is niet uitgevoerd in de tijd, zoals deze is geconfigureerd met behulp van **ServiceTypeRegistrationTimeout**. Als de runtime is gesloten, wordt het servicetype is niet geregistreerd in het knooppunt en wordt een waarschuwing die als host fungeert rapporteert.

* **SourceId**: System.Hosting
* **De eigenschap**: maakt gebruik van het voorvoegsel **ServiceTypeRegistration** en bevat de naam van de service-type. Bijvoorbeeld: **ServiceTypeRegistration:FileStoreServiceType**.

Het volgende voorbeeld ziet u een gezonde geïmplementeerd servicepakket:

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
System.Hosting meldt een fout als het downloaden van het service-pakket mislukt.

* **SourceId**: System.Hosting
* **De eigenschap**: **downloaden:***RolloutVersion*.
* **Volgende stappen**: onderzoeken waarom het downloaden is mislukt op het knooppunt.

### <a name="upgrade-validation"></a>Validatie van upgrade
System.Hosting, wordt er een fout gemeld als validatie tijdens de upgrade mislukt of als de upgrade op het knooppunt mislukt.

* **SourceId**: System.Hosting
* **De eigenschap**: maakt gebruik van het voorvoegsel **FabricUpgradeValidation** en de upgrade-versie bevat.
* **Beschrijving**: verwijst naar de volgende fout aangetroffen.

### <a name="undefined-node-capacity-for-resource-governance-metrics"></a>Niet-gedefinieerde knooppunt capaciteit voor resource governance metrische gegevens
System.Hosting meldt een waarschuwing als het knooppunt capaciteitswaarden zijn niet gedefinieerd in het clustermanifest en configuratie voor automatische detectie is uitgeschakeld. Service Fabric verhoogt waarschuwing wanneer servicepakket die gebruikmaakt van [resource governance](service-fabric-resource-governance.md) registreert op een opgegeven knooppunt.

* **SourceId**: System.Hosting
* **De eigenschap**: ResourceGovernance
* **Volgende stappen**: de aanbevolen manier om dit probleem oplossen wordt gewijzigd van het clustermanifest om in te schakelen van automatische detectie van beschikbare resources. Een andere manier is het clustermanifest met correct opgegeven knooppunt capaciteit voor deze metrische gegevens bijwerken.

## <a name="next-steps"></a>Volgende stappen
[Service Fabric-statusrapporten weergeven](service-fabric-view-entities-aggregated-health.md)

[Het rapport en controleer de servicestatus van de](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Controle en diagnose van lokaal services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade van de service Fabric-toepassing](service-fabric-application-upgrade.md)

