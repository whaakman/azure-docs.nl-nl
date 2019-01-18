---
title: Azure Service Fabric Event Store | Microsoft Docs
description: Meer informatie over Azure Service Fabric van EventStore
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/17/2019
ms.author: srrengar
ms.openlocfilehash: 4a23d8c1e72ec453724514e4d1638c5a223d1644
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54389228"
---
# <a name="eventstore-service-overview"></a>Overzicht van EventStore-service

>[!NOTE]
>Vanaf het Service Fabric versie 6.4. De APIs EventStore zijn alleen beschikbaar voor Windows-clusters wordt alleen uitgevoerd op Azure. Er wordt gewerkt aan het overzetten van deze functionaliteit voor zowel Linux als onze zelfstandige clusters.

## <a name="overview"></a>Overzicht

De EventStore-service is geïntroduceerd in versie 6.2, is een optie voor de bewaking in Service Fabric. EventStore biedt een manier om te beoordelen wat de status van uw cluster of de werkbelastingen op een bepaald tijdstip. De EventStore is een stateful Service Fabric-service die wordt onderhouden door gebeurtenissen uit het cluster. De gebeurtenis worden weergegeven via de Service Fabric Explorer, REST en API's. EventStore query's van het cluster rechtstreeks gebruikt om toegang te krijgen van diagnostische gegevens voor elke entiteit in het cluster en moet worden gebruikt om u te helpen:

* Problemen vaststellen op de ontwikkeling en testen, of waar mogelijk gebruikt u een pijplijn voor bewaking
* Bevestig dat beheeracties die u op het cluster onderneemt correct worden verwerkt
* Een 'snapshot' van de manier waarop Service Fabric is interactie met een bepaalde entiteit ophalen

![EventStore](media/service-fabric-diagnostics-eventstore/eventstore.png)

Zie voor een volledige lijst van gebeurtenissen die beschikbaar zijn in de EventStore [Service Fabric-gebeurtenissen](service-fabric-diagnostics-event-generation-operational.md).

>[!NOTE]
>Vanaf het Service Fabric versie 6.2. De APIs EventStore zijn momenteel in Preview-versie van Windows-clusters wordt alleen uitgevoerd op Azure. Er wordt gewerkt aan het overzetten van deze functionaliteit voor zowel Linux als onze zelfstandige clusters.

De EventStore-service kan worden opgevraagd voor gebeurtenissen die beschikbaar voor elke entiteit en het type van de entiteit in het cluster zijn. Dit betekent dat u kunt een query voor gebeurtenissen op de volgende niveaus:
* Cluster: gebeurtenissen die specifiek zijn voor het cluster zelf (bijvoorbeeld clusterupgrade)
* Knooppunten: alle knooppunt op gebeurtenissen
* Knooppunt: gebeurtenissen die specifiek zijn voor één knooppunt, geïdentificeerd door `nodeName`
* Toepassingen: alle gebeurtenissen op toepassingsniveau
* Toepassing: gebeurtenissen die specifiek zijn voor één toepassing die zijn geïdentificeerd door `applicationId`
* Services: gebeurtenissen van alle services in uw clusters
* Service: gebeurtenissen naar een specifieke service geïdentificeerd door `serviceId`
* Partities: gebeurtenissen van alle partities
* Partitie: gebeurtenissen van een specifieke partitie geïdentificeerd door `partitionId`
* Partitiereplica's: gebeurtenissen van alle replica's / instanties binnen een specifieke partitie geïdentificeerd door `partitionId`
* De Replica van de partitie: gebeurtenissen van een specifieke replica / exemplaar geïdentificeerd door `replicaId` en `partitionId`

Voor meer informatie over de API Bekijk de [EventStore-API-verwijzing](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-eventsstore).

De EventStore-service biedt ook de mogelijkheid te correleren van gebeurtenissen in uw cluster. De EventStore-service is door te kijken op gebeurtenissen die zijn geschreven op hetzelfde moment uit verschillende entiteiten die mogelijk van invloed is elkaar worden verbonden, niet koppelen van deze gebeurtenissen om te helpen bij het identificeren van mogelijke oorzaken voor activiteiten in uw cluster. Bijvoorbeeld, als een van uw toepassingen met een slechte status zonder wijzigingen in de geïnduceerde gebeurt, de EventStore wordt ook een overzicht van andere gebeurtenissen die worden weergegeven door het platform en kan correleren van dit met een `Error` of `Warning` gebeurtenis. Dit helpt met snellere foutdetectie en analyse van hoofdoorzaken.

## <a name="enable-eventstore-on-your-cluster"></a>EventStore inschakelen in uw cluster

### <a name="local-cluster"></a>Lokale Cluster

In [fabricSettings.json in uw cluster](service-fabric-cluster-fabric-settings.md), EventStoreService toevoegen als een invoegtoepassing functie en voert u een clusterupgrade van een.

```json
    "addOnFeatures": [
        "EventStoreService"
    ],
```

### <a name="azure-cluster"></a>Azure-cluster

In Azure Resource Manager-sjabloon van uw cluster, kunt u de service EventStore inschakelen door het uitvoeren van een [upgrade voor clusters config](service-fabric-cluster-config-upgrade-azure.md) en de volgende code toe te voegen. De `upgradeDescription` sectie configureert u de upgrade van de configuratie voor het activeren van een opnieuw opstarten van de knooppunten. U kunt de sectie verwijderen in een andere update.

```json
    "fabricSettings": [
          …
          …
          …,
         {
            "name": "EventStoreService",
            "parameters": [
              {
                "name": "TargetReplicaSetSize",
                "value": "3"
              },
              {
                "name": "MinReplicaSetSize",
                "value": "1"
              }
            ]
          }
        ],
        "upgradeDescription": {
          "forceRestart": true,
          "upgradeReplicaSetCheckTimeout": "10675199.02:48:05.4775807",
          "healthCheckWaitDuration": "00:01:00",
          "healthCheckStableDuration": "00:01:00",
          "healthCheckRetryTimeout": "00:5:00",
          "upgradeTimeout": "1:00:00",
          "upgradeDomainTimeout": "00:10:00",
          "healthPolicy": {
            "maxPercentUnhealthyNodes": 100,
            "maxPercentUnhealthyApplications": 100
          },
          "deltaHealthPolicy": {
            "maxPercentDeltaUnhealthyNodes": 0,
            "maxPercentUpgradeDomainDeltaUnhealthyNodes": 0,
            "maxPercentDeltaUnhealthyApplications": 0
          }
        }
```


## <a name="next-steps"></a>Volgende stappen
* Aan de slag met de EventStore-API - [met behulp van de APIs EventStore in Azure Service Fabric-clusters](service-fabric-diagnostics-eventstore-query.md)
* Meer informatie over de lijst met gebeurtenissen die worden aangeboden door EventStore - [Service Fabric-gebeurtenissen](service-fabric-diagnostics-event-generation-operational.md)
* Overzicht van controle en diagnostische gegevens in Service Fabric - [bewaking en diagnose voor Service Fabric](service-fabric-diagnostics-overview.md)
* Bekijk de volledige lijst van API-aanroepen - [EventStore REST API-verwijzing](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-eventsstore)
* Meer informatie over het controleren van uw cluster- [bewaking van het cluster en het platform](service-fabric-diagnostics-event-generation-infra.md).