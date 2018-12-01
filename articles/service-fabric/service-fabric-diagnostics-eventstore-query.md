---
title: Query voor Clustergebeurtenissen in Azure Service Fabric-clusters met behulp van de APIs EventStore | Microsoft Docs
description: Informatie over het gebruik van de Azure Service Fabric EventStore APIs aan query voor platform-gebeurtenissen
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/29/2018
ms.author: dekapur
ms.openlocfilehash: 556b3375a0f5d138255ba4c46b034894b1037da0
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52722323"
---
# <a name="query-eventstore-apis-for-cluster-events"></a>Query EventStore APIs voor Clustergebeurtenissen

In dit artikel bevat informatie over hoe u query's de EventStore APIs die beschikbaar zijn in Service Fabric versie 6.2 en later - als u wilt meer informatie over de EventStore-service, raadpleegt u de [overzicht van EventStore service](service-fabric-diagnostics-eventstore.md). Op dit moment de EventStore-service kan alleen toegang tot gegevens voor de afgelopen 7 dagen (dit is gebaseerd op beleid voor Gegevensretentie diagnostische gegevens van uw cluster).

>[!NOTE]
>De APIs EventStore zijn vanaf het Service Fabric versie 6.4 voor alleen Windows clusters die worden uitgevoerd op Azure.

De APIs EventStore zijn toegankelijk via een REST-eindpunt rechtstreeks of via een programma. Afhankelijk van de query zijn er meerdere parameters op die nodig zijn om de juiste gegevens te verzamelen. Deze parameters bevatten doorgaans:
* `api-version`: de versie van de EventStore APIs die u gebruikt
* `StartTimeUtc`: het begin van de periode dat u geïnteresseerd bent in het kijken definieert
* `EndTimeUtc`: einde van de geselecteerde periode

Naast deze parameters zijn optionele parameters beschikbaar zijn, zoals:
* `timeout`: de standaard 60 tweede time-out voor het uitvoeren van de aanvraagbewerking overschrijven
* `eventstypesfilter`: dit biedt u de mogelijkheid om te filteren op specifieke gebeurtenistypen
* `ExcludeAnalysisEvents`: 'Analysis' gebeurtenissen niet retourneren. Standaard wordt EventStore-query's met 'analysis' gebeurtenissen geretourneerd, waar mogelijk. Analyse van gebeurtenissen zijn uitgebreidere operationele kanaal-gebeurtenissen die bieden meer diepgang en bevatten aanvullende context of gegevens buiten een reguliere Service Fabric-gebeurtenis.
* `SkipCorrelationLookup`: niet zoeken naar mogelijke gecorreleerde gebeurtenissen in het cluster. Standaard probeert de EventStore te correleren van gebeurtenissen in een cluster en de gebeurtenissen aan elkaar koppelen, indien mogelijk. 

Elke entiteit in een cluster kan worden query's voor gebeurtenissen. U kunt ook een query voor gebeurtenissen voor alle entiteiten van het type. Bijvoorbeeld, kunt u zoeken naar gebeurtenissen voor een specifiek knooppunt of voor alle knooppunten in uw cluster. De huidige set entiteiten die u kunt een query voor gebeurtenissen is (met de manier waarop de query zou worden gestructureerd):
* Cluster: `/EventsStore/Cluster/Events`
* Knooppunten: `/EventsStore/Nodes/Events`
* Knooppunt: `/EventsStore/Nodes/<NodeName>/$/Events`
* Toepassingen: `/EventsStore/Applications/Events`
* Toepassing: `/EventsStore/Applications/<AppName>/$/Events`
* Services: `/EventsStore/Services/Events`
* Service: `/EventsStore/Services/<ServiceName>/$/Events`
* Partities: `/EventsStore/Partitions/Events`
* Partitie: `/EventsStore/Partitions/<PartitionID>/$/Events`
* Replica's: `/EventsStore/Partitions/<PartitionID>/$/Replicas/Events`
* De replica is: `/EventsStore/Partitions/<PartitionID>/$/Replicas/<ReplicaID>/$/Events`

>[!NOTE]
>Wanneer u verwijst naar een toepassing of servicenaam, de query niet nodig om op te nemen de ' fabric: / "voorvoegsel. Bovendien als uw toepassing of de servicenamen hebt een '/' in de bestanden, het overschakelen naar een ' ~ ' te houden van de werking van de query. Bijvoorbeeld, als uw toepassing wordt weergegeven als ' fabric: / App1/FrontendApp ", uw query's voor app-specifieke zou worden gestructureerd als `/EventsStore/Applications/App1~FrontendApp/$/Events`.
>Bovendien statusrapporten voor services vandaag weergegeven op de betreffende toepassing, zodat u een query wilt uitvoeren voor `DeployedServiceHealthReportCreated` gebeurtenissen voor de juiste Toepassingsentiteit. 

## <a name="query-the-eventstore-via-rest-api-endpoints"></a>Query uitvoeren op de EventStore via REST API-eindpunten

U kunt de EventStore rechtstreeks via een REST-eindpunt, een query door `GET` aanvragen naar: `<your cluster address>/EventsStore/<entity>/Events/`.

Bijvoorbeeld, als u wilt zoeken naar alle Clustergebeurtenissen tussen `2018-04-03T18:00:00Z` en `2018-04-04T18:00:00Z`, uw aanvraag zou er als volgt uitzien:

```
Method: GET 
URL: http://mycluster:19080/EventsStore/Cluster/Events?api-version=6.4&StartTimeUtc=2018-04-03T18:00:00Z&EndTimeUtc=2018-04-04T18:00:00Z
```

Dit kan ofwel er zijn geen gebeurtenissen of de lijst met gebeurtenissen geretourneerd in json geretourneerd:

```json
Response: 200
Body:
[
  {
    "Kind": "ClusterUpgradeStart",
    "CurrentClusterVersion": "0.0.0.0:",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeType": "Rolling",
    "RollingUpgradeMode": "UnmonitoredAuto",
    "FailureAction": "Manual",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:18:59.4313064Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeDomainComplete",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeState": "RollingForward",
    "UpgradeDomains": "(0 1 2)",
    "UpgradeDomainElapsedTimeInMs": "78.5288",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:19:59.5729953Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeDomainComplete",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeState": "RollingForward",
    "UpgradeDomains": "(3 4)",
    "UpgradeDomainElapsedTimeInMs": "0",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:20:59.6271949Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeComplete",
    "TargetClusterVersion": "6.2:1.0",
    "OverallUpgradeElapsedTimeInMs": "120196.5212",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:20:59.8134457Z",
    "HasCorrelatedEvents": false
  }
]
```

Hier zien we dat tussen `2018-04-03T18:00:00Z` en `2018-04-04T18:00:00Z`, de eerste upgrade dit cluster is voltooid als het eerst is nodig, van `"CurrentClusterVersion": "0.0.0.0:"` naar `"TargetClusterVersion": "6.2:1.0"`in `"OverallUpgradeElapsedTimeInMs": "120196.5212"`.

## <a name="query-the-eventstore-programmatically"></a>De EventStore programmatisch opvragen

U kunt ook de EventStore query via een programma, via de [Service Fabric-clientbibliotheek](https://docs.microsoft.com/dotnet/api/overview/azure/service-fabric?view=azure-dotnet#client-library).

Nadat u uw Service Fabric-Client instellen hebt, kunt u zoeken naar gebeurtenissen door het openen van de EventStore als volgt: ` sfhttpClient.EventStore.<request>`

Hier volgt een van de voorbeeldaanvraag voor alle gebeurtenissen tussen cluster `2018-04-03T18:00:00Z` en `2018-04-04T18:00:00Z`, via de `GetClusterEventListAsync` functie.

```csharp
var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl, settings);

var clstrEvents = sfhttpClient.EventsStore.GetClusterEventListAsync(
    "2018-04-03T18:00:00Z",
    "2018-04-04T18:00:00Z")
    .GetAwaiter()
    .GetResult()
    .ToList();
```

Hier volgt een voorbeeld dat query's voor de clusterstatus en alle knooppunt-gebeurtenissen in September 2018 en ze afgedrukt.

```csharp
  const int timeoutSecs = 60;
  var clusterUrl = new Uri(@"http://localhost:19080"); // This example is for a Local cluster
  var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl);

  var clusterHealth = sfhttpClient.Cluster.GetClusterHealthAsync().GetAwaiter().GetResult();
  Console.WriteLine("Cluster Health: {0}", clusterHealth.AggregatedHealthState.Value.ToString());

  
  Console.WriteLine("Querying for node events...");
  var nodesEvents = sfhttpClient.EventsStore.GetNodesEventListAsync(
      "2018-09-01T00:00:00Z",
      "2018-09-30T23:59:59Z",
      timeoutSecs,
      "NodeDown,NodeUp")
      .GetAwaiter()
      .GetResult()
      .ToList();
  Console.WriteLine("Result Count: {0}", nodesEvents.Count());

  foreach (var nodeEvent in nodesEvents)
  {
      Console.Write("Node event happened at {0}, Node name: {1} ", nodeEvent.TimeStamp, nodeEvent.NodeName);
      if (nodeEvent is NodeDownEvent)
      {
          var nodeDownEvent = nodeEvent as NodeDownEvent;
          Console.WriteLine("(Node is down, and it was last up at {0})", nodeDownEvent.LastNodeUpAt);
      }
      else if (nodeEvent is NodeUpEvent)
      {
          var nodeUpEvent = nodeEvent as NodeUpEvent;
          Console.WriteLine("(Node is up, and it was last down at {0})", nodeUpEvent.LastNodeDownAt);
      }
  }
```

## <a name="sample-scenarios-and-queries"></a>Voorbeeldscenario's en query 's

Hier volgen enkele voorbeelden van hoe u de gebeurtenis Store REST-API's voor meer informatie over de status van uw cluster kunt aanroepen.

*Upgrades van cluster:*

Als u wilt zien van de laatste keer dat het cluster correct is of poging tot het laatste week worden bijgewerkt, kunt u de API's voor upgrades van de laatst voltooide opvragen met uw cluster, door het uitvoeren van query's voor de 'ClusterUpgradeCompleted'-gebeurtenissen in de EventStore: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ClusterUpgradeCompleted`

*Cluster upgrade problemen:*

Op dezelfde manier als er problemen zijn met een recente clusterupgrade, raadpleegt u query kunt uitvoeren voor alle gebeurtenissen voor de cluster-entiteit. Hier ziet u verschillende gebeurtenissen, met inbegrip van de initialisatie van upgrades en elke UD waarvoor de upgrade hersteld via is. Ook ziet u gebeurtenissen voor het punt waarop het terugdraaien is gestart en de bijbehorende statusgebeurtenissen. Dit is de query die u voor dit gebruiken zou: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Knooppunt de status wordt gewijzigd:*

Controleer dat de knooppuntstatus van uw is gewijzigd gedurende de afgelopen gebruik paar dagen - wanneer knooppunten omhoog of omlaag, is een fout zijn ingeschakeld of uitgeschakeld (door het platform, de chaos-service, of van de invoer van de gebruiker) - de volgende query: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Nodes/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Toepassingsgebeurtenissen van de:*

U kunt ook uw recente toepassingsimplementaties en upgrades bijhouden. Gebruik de volgende query uit om alle toepassingsgebeurtenissen in uw cluster te bekijken: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Historische status van een toepassing:*

Naast het zojuist application lifecycle-gebeurtenissen ziet, kunt u ook om te zien van historische gegevens over de status van een specifieke toepassing. U kunt dit doen door de naam van de toepassing waarvoor u wenst te verzamelen van de gegevens op te geven. Deze query gebruiken om op te halen van alle statusgebeurtenissen van de toepassing: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/myApp/$/Events?api-version=6.4&starttimeutc=2018-03-24T17:01:51Z&endtimeutc=2018-03-29T17:02:51Z&EventsTypesFilter=ApplicationNewHealthReport`. Als u wilt opnemen van health-gebeurtenissen die mogelijk verlopen (verdwenen hun tijd doorgegeven aan live (TTL)), Voeg `,ApplicationHealthReportExpired` aan het einde van de query, kunt u filteren op twee soorten gebeurtenissen.

*Historische status voor alle services in 'myApp':*

Op dit moment rapport statusgebeurtenissen voor services weergegeven als `DeployedServicePackageNewHealthReport` -gebeurtenissen onder de bijbehorende Toepassingsentiteit. Om te zien hoe uw services hebben gedaan voor 'App1', gebruikt u de volgende query: `https://winlrc-staging-10.southcentralus.cloudapp.azure.com:19080/EventsStore/Applications/myapp/$/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=DeployedServicePackageNewHealthReport`

*Herconfiguratie van de partitie:*

Om te zien van alle de bewegingen van de partitie die hebben plaatsgevonden in uw cluster op te vragen voor de `PartitionReconfigured` gebeurtenis. Dit kan u helpen te achterhalen wat workloads op specifieke tijdstippen, wanneer vaststellen van in uw cluster problemen op welk knooppunt uitgevoerd. Hier volgt een voorbeeldquery die door die worden ondersteund: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Partitions/Events?api-version=6.4&starttimeutc=2018-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=PartitionReconfigured`

*Chaos-service:*

Er is een gebeurtenis voor wanneer de Chaos-service is gestart of gestopt, dat wil zeggen op het clusterniveau van het weergegeven. Als uw recente gebruik van de service Chaos weergeven, gebruikt u de volgende query uit: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ChaosStarted,ChaosStopped`

