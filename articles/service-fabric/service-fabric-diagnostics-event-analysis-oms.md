---
title: Analyse van Azure Service Fabric-gebeurtenis met logboekanalyse | Microsoft Docs
description: Meer informatie over het visualiseren en analyseren van gebeurtenissen met Log Analytics voor controle en diagnostische gegevens van Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/29/2018
ms.author: srrengar
ms.openlocfilehash: 49d9b5306a0fcf51cc0de036c725fca8345cd0ec
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36302179"
---
# <a name="event-analysis-and-visualization-with-log-analytics"></a>Analyse van gebeurtenis en visualisatie met Log Analytics
Log Analytics verzamelt, analyseert telemetrie van toepassingen en services die worden gehost in de cloud en voorziet in analyse hulpprogramma's waarmee u kunt de beschikbaarheid en prestaties te maximaliseren. In dit artikel bevat een overzicht van het uitvoeren van query's in logboekanalyse inzicht en problemen oplossen wat gebeurt er in uw cluster. De volgende veelgestelde vragen worden behandeld:

* Hoe kan ik gebeurtenissen health oplossen?
* Hoe weet ik wanneer een knooppunt uitvalt?
* Hoe weet ik of mijn toepassing services zijn gestart of gestopt?

## <a name="log-analytics-workspace"></a>Log Analytics-werkruimte

Log Analytics verzamelt gegevens van beheerde bronnen, met inbegrip van de tabel van een Azure-opslag of een agent en onderhoudt dat bij een centrale opslagplaats. De gegevens kunnen vervolgens worden gebruikt voor analyse, waarschuwingen en visualisatie of meer geëxporteerd. Log Analytics ondersteunt gebeurtenissen, prestatiegegevens of andere aangepaste gegevens. Bekijk [stappen voor het configureren van de extensie voor diagnostische gegevens te verzamelen van gegevens](service-fabric-diagnostics-event-aggregation-wad.md) en [stappen voor het maken van een werkruimte voor logboekanalyse voor het lezen van de gebeurtenissen in de opslag](service-fabric-diagnostics-oms-setup.md) om ervoor te zorgen dat gegevens is die binnenkomen in Log Analytics .

Nadat de gegevens worden ontvangen door Log Analytics, Azure heeft diverse *beheeroplossingen* die voorverpakte oplossingen voor het bewaken van binnenkomende gegevens, aangepast aan verschillende scenario's zijn. Deze omvatten een *Service Fabric Analytics* oplossing en een *Containers* oplossing, zijn de twee meest relevante diagnostische gegevens en controleren wanneer met behulp van Service Fabric-clusters. Dit artikel wordt beschreven hoe u de Service Fabric Analytics-oplossing die is gemaakt met de werkruimte.

## <a name="access-the-service-fabric-analytics-solution"></a>Toegang tot de Service Fabric Analytics-oplossing

1. Ga naar de resourcegroep waarin u de Service Fabric Analytics-oplossing hebt gemaakt in de Azure-Portal.

2. Selecteer de resource **ServiceFabric\<nameOfOMSWorkspace\>**.

2. Kortom ziet u tegels in de vorm van een grafiek voor elk van de oplossingen die zijn ingeschakeld, met inbegrip van één voor Service Fabric. Klik op de **Service Fabric** graph (eerste afbeelding hieronder) om door te gaan met de Service Fabric Analytics-oplossing (tweede afbeelding hieronder).

    ![Service Fabric-oplossing](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_summary.PNG)

    ![Service Fabric-oplossing](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_solution.PNG)

De afbeelding hierboven is de startpagina van de Service Fabric Analytics-oplossing. Dit is de weergave van een momentopname van wat er in uw cluster gebeurt. Als u diagnostische gegevens bij het maken van het cluster hebt ingeschakeld, kunt u gebeurtenissen voor bekijken 

* [Operationele kanaal](service-fabric-diagnostics-event-generation-operational.md): een hoger niveau bewerkingen waarmee de Service Fabric-platform (verzameling systeemservices) wordt uitgevoerd.
* [Reliable Actors programming model gebeurtenissen](service-fabric-reliable-actors-diagnostics.md)
* [Reliable Services programming model gebeurtenissen](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>Naast het operationele kanaal kan meer gedetailleerde systeemgebeurtenissen worden verzameld door [bijwerken van de configuratie van de extensie voor diagnostische gegevens](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations).

### <a name="view-service-fabric-events-including-actions-on-nodes"></a>Met inbegrip van acties op knooppunten van Service Fabric-gebeurtenissen op weergeven

1. Klik op de pagina Service Fabric Analytics op de grafiek voor **gebeurtenissen van de Service Fabric**.

    ![Service Fabric-oplossing operationele kanaal](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events_selection.png)

2. Klik op **lijst** om de gebeurtenissen in een lijst weer te geven. Zodra u hier de systeemgebeurtenissen die zijn verzameld ziet. Ter referentie: dit zijn van de WADServiceFabricSystemEventsTable in de Azure Storage-account en op dezelfde manier de reliable services en actoren gebeurtenissen die vervolgens u ziet uit de respectieve tabellen zijn.
    
    ![Het operationele querykanaal](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events.png)

U kunt ook de querytaal Kusto en klik op het Vergrootglas aan de linkerkant te vinden wat u zoekt. Bijvoorbeeld, als u wilt zoeken in alle acties die op knooppunten in het cluster, kunt u de volgende query. De gebeurtenis-id's gebruikt hieronder vindt u in de [operationele kanaal gebeurtenissen verwijzing](service-fabric-diagnostics-event-generation-operational.md).

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

U kunt een query op veel meer velden, zoals de specifieke knooppunten (Computer) de service (TaskName).

### <a name="view-service-fabric-reliable-service-and-actor-events"></a>Weergave Service Fabric betrouwbare Service en Actor-gebeurtenissen

1. Klik op de grafiek voor op de pagina Service Fabric Analytics **Reliable Services**.

    ![Service Fabric-oplossing Reliable Services](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_services_events_selection.png)

2. Klik op **lijst** om de gebeurtenissen in een lijst weer te geven. Hier ziet u gebeurtenissen van de betrouwbare services. U kunt verschillende gebeurtenissen voor zien wanneer de service runasync wordt gestart en dit gebeurt meestal op implementaties en upgrades voltooid. 

    ![Query Reliable Services](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_service_events.png)

Betrouwbare actor gebeurtenissen kunnen op dezelfde manier worden weergegeven. Als u wilt meer gedetailleerde gegevens van gebeurtenissen voor reliable actors configureren, moet u wijzigen de `scheduledTransferKeywordFilter` in de configuratie voor de extensie voor diagnostische (Zie hieronder). Details van de waarden voor deze zijn in de [betrouwbare actoren gebeurtenissen verwijzing](service-fabric-reliable-actors-diagnostics.md#keywords).

```json
"EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
```

De taal van de query Kusto is krachtige. Er is een andere waardevolle query die u kunt uitvoeren om erachter te komen welke knooppunten de meeste gebeurtenissen genereren. De query in de onderstaande schermafbeelding toont Service Fabric operationele gebeurtenissen die worden samengevoegd met de specifieke service en het knooppunt.

![Querygebeurtenissen per knooppunt](media/service-fabric-diagnostics-event-analysis-oms/oms_kusto_query.png)

## <a name="next-steps"></a>Volgende stappen

* Om in te schakelen infrastructuurbewaking dat wil zeggen prestatiemeteritems, Ga naar [toevoegen van de agent logboekanalyse](service-fabric-diagnostics-oms-agent.md). De agent verzamelt prestatiemeteritems en voegt deze toe aan uw bestaande werkruimte.
* Log Analytics biedt voor lokale clusters, een Gateway (http-doorsturen Proxy) die kan worden gebruikt om gegevens te verzenden met logboekanalyse. Lees meer over die in [computers zonder toegang tot het Internet verbinden met logboekanalyse met behulp van de OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md)
* Configureer [geautomatiseerde waarschuwingen](../log-analytics/log-analytics-alerts.md) voor detectie en diagnostische gegevens
* Familiarized ophalen met de [zoeken en uitvoeren van query's in logboek registreren](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van logboekanalyse
* Ophalen van een meer gedetailleerd overzicht van logboekanalyse en wat biedt, lezen [wat is er Log Analytics?](../operations-management-suite/operations-management-suite-overview.md)
