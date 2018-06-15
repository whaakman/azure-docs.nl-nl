---
title: Query voor het gebruik van de EventStore APIs in Azure Service Fabric-clusters Clustergebeurtenissen | Microsoft Docs
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
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: 5c184841602f269555ce2196ef660faba14dbf8a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34204607"
---
# <a name="query-eventstore-apis-for-cluster-events"></a>Query EventStore APIs voor Clustergebeurtenissen

In dit artikel wordt beschreven hoe u een query uitvoeren op de EventStore APIs die beschikbaar zijn in Service Fabric versie 6.2 en later - als u meer informatie over de service EventStore, Zie de [EventStore Serviceoverzicht](service-fabric-diagnostics-eventstore.md). Op dit moment wordt de service EventStore kan alleen toegang tot gegevens voor de afgelopen 7 dagen (dit is gebaseerd op een bewaarbeleid voor gegevens van uw cluster diagnostics).

>[!NOTE]
>Vanaf Service Fabric versie 6.2. de EventStore APIs zijn momenteel in preview voor Windows-clusters die alleen wordt uitgevoerd op Azure. We werken over het overdragen van deze functionaliteit op Linux, evenals onze zelfstandige-clusters.

De EventStore APIs toegankelijk zijn via een REST-eindpunt rechtstreeks of via een programma. Er zijn verschillende parameters die nodig zijn om de juiste gegevens te verzamelen, afhankelijk van de query. Deze omvatten:
* `api-version`: de versie van de EventStore APIs die u gebruikt
* `StartTimeUtc`: het begin van de periode dat u interessante bent kijkt definieert
* `EndTimeUtc`: einde van de periode

Naast deze zijn optionele parameters beschikbaar, zoals:
* `timeout`: de standaard 60 seconden time-out voor het uitvoeren van de aanvraagbewerking overschrijven
* `eventstypesfilter`: Hiermee krijgt u de optie voor het filteren op specifieke gebeurtenistypen
* `ExcludeAnalysisEvents`: 'Analyse' gebeurtenissen niet retourneren. Standaard EventStore query's wordt geretourneerd met gebeurtenissen "analysis" waar mogelijk - analyse gebeurtenissen zijn uitgebreidere operationele kanaal-gebeurtenissen die bevatten aanvullende context of gegevens buiten een reguliere Service Fabric-gebeurtenis en bieden meer diepte.
* `SkipCorrelationLookup`: niet zoeken naar mogelijke gecorreleerde gebeurtenissen in het cluster. Standaard probeert de EventStore te correleren van gebeurtenissen in een cluster en de gebeurtenissen aan elkaar koppelt, indien mogelijk. 

Elke entiteit in een cluster kan worden query's voor gebeurtenissen. U kunt ook een query voor gebeurtenissen voor alle entiteiten van het type. U kunt bijvoorbeeld zoeken naar gebeurtenissen voor een specifiek knooppunt of voor alle knooppunten in het cluster. De huidige set van entiteiten die u kunt een query voor gebeurtenissen is (met hoe de query zou worden gestructureerd):
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
>Wanneer u verwijst naar een toepassing of servicenaam, de query niet hoeft te nemen de "fabric: / ' voorvoegsel. Bovendien als uw toepassing of servicenamen een '/' bevat hebben, overschakelen naar een ' ~ ' om de werking van de query. Bijvoorbeeld, als uw toepassing wordt weergegeven als "fabric: / App1/FrontendApp ', uw query's voor app-specifiek zou gestructureerd als `/EventsStore/Applications/App1~FrontendApp/$/Events`.
>Bovendien statusrapporten voor services vandaag weergegeven onder de betreffende toepassing, zodat u zou een query voor `DeployedServiceHealthReportCreated` gebeurtenissen voor de juiste Toepassingsentiteit. 

## <a name="query-the-eventstore-via-rest-api-endpoints"></a>Query uitvoeren op de EventStore via REST API-eindpunten

U kunt de EventStore rechtstreeks via een REST-eindpunt opvragen door het maken van `GET` aanvragen naar: `<your cluster address>/EventsStore/<entity>/Events/`.

Bijvoorbeeld, om te zoeken naar alle Clustergebeurtenissen tussen `2018-04-03T18:00:00Z` en `2018-04-04T18:00:00Z`, uw aanvraag eruit als:

```
Method: GET 
URL: http://mycluster:19080/EventsStore/Cluster/Events?api-version=6.2-preview&StartTimeUtc=2018-04-03T18:00:00Z&EndTimeUtc=2018-04-04T18:00:00Z
```

Dit kan ofwel een fout geretourneerd als geen gebeurtenissen beschikbaar, of als de query voltooid is, u de gebeurtenissen die worden geretourneerd in json ziet:

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

Hier ziet u dat tussen `2018-04-03T18:00:00Z` en `2018-04-04T18:00:00Z`, dit cluster zijn eerste upgrade is voltooid wanneer deze is eerst bevond zich registreerde, van `"CurrentClusterVersion": "0.0.0.0:"` naar `"TargetClusterVersion": "6.2:1.0"`in `"OverallUpgradeElapsedTimeInMs": "120196.5212"`.

## <a name="query-the-eventstore-programmatically"></a>De EventStore programmatisch query

U kunt ook een query de EventStore programmatisch via de [Service Fabric-clientbibliotheek](https://docs.microsoft.com/dotnet/api/overview/azure/service-fabric?view=azure-dotnet#client-library).

Zodra u uw Service Fabric-Client instellen hebt, u kunt een query voor gebeurtenissen met het openen van de EventStore als volgt: ` sfhttpClient.EventStore.<request>`

Hier volgt een voorbeeld van een aanvraag voor alle gebeurtenissen tussen cluster `2018-04-03T18:00:00Z` en `2018-04-04T18:00:00Z`, via de `GetClusterEventListAsync` functie.

```csharp
var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl, settings);

var clstrEvents = sfhttpClient.EventsStore.GetClusterEventListAsync(
    "2018-04-03T18:00:00Z",
    "2018-04-04T18:00:00Z")
    .GetAwaiter()
    .GetResult()
    .ToList();
```

## <a name="sample-scenarios-and-queries"></a>Voorbeeldscenario's en query 's

Hier volgen enkele voorbeelden van hoe u de gebeurtenis Store REST-API's voor informatie over de status van het cluster kunt aanroepen.

*Cluster-upgrades:*

Overzicht van de laatste keer het cluster correct is of geprobeerd afgelopen week worden bijgewerkt, u kunt een query de API's voor upgrades van de laatst voltooide met uw cluster, door een query uitvoert voor de gebeurtenissen 'ClusterUpgradeComplete' in de EventStore: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ClusterUpgradeComplete`

*Cluster problemen met de upgrade:*

Op dezelfde manier als er problemen met een recente clusterupgrades, u kan een query voor alle gebeurtenissen voor de entiteit van het cluster. Hier ziet u diverse gebeurtenissen, met inbegrip van de initialisatie van upgrades en elke UD waarvoor de upgrade hersteld via is. Ook ziet u gebeurtenissen voor het punt waarop het terugdraaien is gestart en de bijbehorende gebeurtenissen health. Dit is de query die u voor dit gebruiken zou: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Statuswijzigingen knooppunt:*

Controleer dat de status van uw knooppunt is gewijzigd in het afgelopen gebruik paar dagen - wanneer knooppunten is gegaan omhoog of omlaag, of zijn geactiveerd of gedeactiveerd (door het platform, de service chaos of van gebruikersinvoer) - de volgende query: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Nodes/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Toepassingsgebeurtenissen van de:*

U kunt ook uw recente implementaties van toepassingen en -upgrades bijhouden. Gebruik de volgende query om te zien van toepassing op alle gerelateerde gebeurtenissen in het cluster: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Historische status van een toepassing:*

Naast de levenscyclus van toepassingsgebeurtenissen NET ziet, kunt u ook om te zien van historische gegevens over de status van een specifieke toepassing. U kunt dit doen door op te geven van de naam van de toepassing waarvoor u wilt om de gegevens te verzamelen. Gebruik deze query alle gebeurtenissen voor application health: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/myApp/$/Events?api-version=6.2-preview&starttimeutc=2018-03-24T17:01:51Z&endtimeutc=2018-03-29T17:02:51Z&EventsTypesFilter=ProcessApplicationReport`. Als u wilt opnemen van health-gebeurtenissen die mogelijk verlopen (geworden hun tijd doorgegeven aan live (TTL)), Voeg `,ExpiredDeployedApplicationEvent` aan het einde van de query, kunt u filteren op twee soorten gebeurtenissen.

*Historische status voor alle services in 'Mijntoep':*

Op dit moment health rapport gebeurtenissen voor services worden weergegeven als `DeployedServiceHealthReportCreated` -gebeurtenissen onder de overeenkomende Toepassingsentiteit. Gebruik de volgende query om te zien hoe uw services hebben gedaan voor "App1": `https://winlrc-staging-10.southcentralus.cloudapp.azure.com:19080/EventsStore/Applications/myapp/$/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=DeployedServiceHealthReportCreated`

*Herconfiguratie van de partitie:*

Alle zien die de bewegingen van de partitie die hebben plaatsgevonden in uw cluster zoeken naar de `ReconfigurationCompleted` gebeurtenis. Dit kan u helpen te achterhalen wat werkbelastingen op specifieke tijdstippen, wanneer oplossen van in uw cluster problemen op welk knooppunt uitgevoerd. Hier volgt een voorbeeldquery die die biedt: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Partitions/Events?api-version=6.2-preview&starttimeutc=2018-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=PartitionReconfigurationCompleted`

*Chaos service:*

Er is een gebeurtenis voor wanneer de Chaos service is gestart of gestopt die wordt weergegeven op het clusterniveau van het. Als uw recente gebruik van de service Chaos weergeven, gebruikt u de volgende query: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ChaosStarted,ChaosStopped`

