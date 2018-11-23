---
title: Azure Service Fabric-Gebeurtenisanalyse met Log Analytics | Microsoft Docs
description: Meer informatie over het visualiseren en analyseren van gebeurtenissen met Log Analytics voor controle en diagnose van Azure Service Fabric-clusters.
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
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: ed6a709418871ededc8ddfe06b0eb1ab3e4546e1
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291076"
---
# <a name="event-analysis-and-visualization-with-log-analytics"></a>Gebeurtenis analyses en visualisatie met Log Analytics
 Log Analytics verzamelt en analyseert telemetrie van toepassingen en services die worden gehost in de cloud biedt analyse hulpprogramma's waarmee u hun beschikbaarheid en prestaties te maximaliseren. In dit artikel bevat een overzicht van het uitvoeren van query's in Log Analytics inzicht en problemen met wat er gebeurt in uw cluster. De volgende veelgestelde vragen worden behandeld:

* Hoe kan ik statusgebeurtenissen oplossen?
* Hoe weet ik wanneer een knooppunt wordt uitgeschakeld?
* Hoe weet ik als services van mijn toepassing hebben gestart of gestopt?

## <a name="log-analytics-workspace"></a>Log Analytics-werkruimte

>[!NOTE] 
>Opslagaccount voor diagnostische is standaard ingeschakeld tijdens de aanmaak van het cluster, moet u de Log Analytics-werkruimte nog steeds instellen om te lezen uit het opslagaccount voor diagnostische.

Log Analytics verzamelt gegevens van beheerde resources, met inbegrip van een Azure storage-tabel of een agent, en deze in een centrale opslagplaats bijhoudt. De gegevens kunnen vervolgens worden gebruikt voor analyse, waarschuwingen en visualisatie, of het verder exporteren. Log Analytics biedt ondersteuning voor gebeurtenissen, prestatiegegevens of een andere aangepaste gegevens. Bekijk [stappen voor het configureren van de extensie voor diagnostische gegevens voor](service-fabric-diagnostics-event-aggregation-wad.md) en [stappen voor het maken van een Log Analytics-werkruimte te lezen van de gebeurtenissen in de opslag](service-fabric-diagnostics-oms-setup.md) om ervoor te zorgen dat gegevens worden doorgestuurd naar Log Analytics .

Nadat de gegevens worden ontvangen door Log Analytics, Azure heeft verscheidene *beheeroplossingen* die zijn voorverpakte oplossingen of operationele dashboards voor het bewaken van binnenkomende gegevens, aangepast aan verschillende scenario's. Deze omvatten een *Service Fabric-analyse* oplossing en een *Containers* oplossing, zijn de twee meest relevante diagnostische gegevens en de bewaking van bij het gebruik van Service Fabric-clusters. In dit artikel wordt beschreven hoe u van de Service Fabric-analyse-oplossing die is gemaakt met de werkruimte.

## <a name="access-the-service-fabric-analytics-solution"></a>Toegang tot de Service Fabric-analyse-oplossing

1. In de Azure-Portal, gaat u naar de resourcegroep waarin u de Service Fabric-analyse-oplossing hebt gemaakt.

2. Selecteer de resource **ServiceFabric\<nameOfOMSWorkspace\>**.

2. In `Summary`, ziet u tegels in de vorm van een grafiek voor elk van de oplossingen ingeschakeld, met inbegrip van een Service Fabric. Klik op de **Service Fabric** graph (eerste afbeelding hieronder) om door te gaan naar de Service Fabric-analyse-oplossing (tweede afbeelding hieronder).

    ![Service Fabric-oplossing](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_summary.PNG)

    ![Service Fabric-oplossing](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_solution.PNG)

De bovenstaande afbeelding is de startpagina van de Service Fabric-analyse-oplossing. Dit is een momentopname van wat in uw cluster gebeurt er. Als u diagnostische gegevens bij het maken van een cluster hebt ingeschakeld, kunt u gebeurtenissen voor bekijken 

* [Service Fabric-cluster-gebeurtenissen](service-fabric-diagnostics-event-generation-operational.md)
* [Reliable Actors programming modelgebeurtenissen](service-fabric-reliable-actors-diagnostics.md)
* [Reliable Services programming modelgebeurtenissen](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>Naast de Service Fabric-gebeurtenissen uit het vak, meer gedetailleerde systeemgebeurtenissen kunnen worden verzameld door [bijwerken van de configuratie van de extensie voor diagnostische gegevens](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations).

### <a name="view-service-fabric-events-including-actions-on-nodes"></a>Met inbegrip van acties op knooppunten van Service Fabric-gebeurtenissen op weergeven

1. Klik op de pagina Service Fabric-analyse op de grafiek voor **Service Fabric-gebeurtenissen**.

    ![Service Fabric-oplossing operationele kanaal](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events_selection.png)

2. Klik op **lijst** om de gebeurtenissen in een lijst weer te geven. Zodra u hier alle systeemgebeurtenissen die zijn verzameld ziet. Voor een verwijzing naar deze zijn van de WADServiceFabricSystemEventsTable in de Azure Storage-account en op dezelfde manier de reliable services en vervolgens u ziet actors-gebeurtenissen uit de respectieve tabellen zijn.
    
    ![Query operationele kanaal](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events.png)

U kunt ook klikt u op het Vergrootglas aan de linkerkant en de Kusto-query-taal gebruiken om te vinden wat u zoekt. Bijvoorbeeld, als u wilt zoeken in alle acties die worden uitgevoerd op knooppunten in het cluster, kunt u de volgende query uit. De gebeurtenis-id's gebruikt hieronder vindt u in de [operationele kanaal gebeurtenissen verwijzing](service-fabric-diagnostics-event-generation-operational.md).

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

U kunt een query op veel meer velden, zoals de specifieke knooppunten (Computer) de service (TaskName).

### <a name="view-service-fabric-reliable-service-and-actor-events"></a>Weergave Service Fabric Reliable Service en de Actor-gebeurtenissen

1. Klik op de pagina Service Fabric-analyse op de grafiek voor **Reliable Services**.

    ![Service Fabric Reliable Services-oplossing](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_services_events_selection.png)

2. Klik op **lijst** om de gebeurtenissen in een lijst weer te geven. Hier ziet u gebeurtenissen van de betrouwbare services. U kunt verschillende gebeurtenissen voor zien wanneer de service runasync is gestart en dit gebeurt gewoonlijk op implementaties en upgrades voltooid. 

    ![Query Reliable Services](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_service_events.png)

Reliable actor-gebeurtenissen kunnen op dezelfde manier worden weergegeven. Als u wilt meer gedetailleerde gegevens van gebeurtenissen voor betrouwbare actoren configureert, moet u wijzigen de `scheduledTransferKeywordFilter` in de configuratie voor de diagnostische extensie (Zie hieronder). Details van de waarden voor deze zijn in de [reliable actors-gebeurtenissen verwijzing](service-fabric-reliable-actors-diagnostics.md#keywords).

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

De Kusto-query-taal is krachtig. Er is een andere waardevolle query die u kunt uitvoeren om erachter te komen welke knooppunten de meeste gebeurtenissen genereren. De query in de onderstaande schermafbeelding ziet u Service Fabric operationele gebeurtenissen die worden samengevoegd met de specifieke service en het knooppunt.

![Querygebeurtenissen per knooppunt](media/service-fabric-diagnostics-event-analysis-oms/oms_kusto_query.png)

## <a name="next-steps"></a>Volgende stappen

* Om in te schakelen prestatiemeteritems dat wil zeggen de controle van infrastructuren, Ga naar [toe te voegen de Log Analytics-agent](service-fabric-diagnostics-oms-agent.md). De agent verzamelt prestatiemeters en voegt deze toe aan uw bestaande werkruimte.
* Voor clusters van on-premises biedt Log Analytics een Gateway (http-doorsturen Proxy) die kan worden gebruikt om gegevens te verzenden naar Log Analytics. Meer informatie over die in [computers zonder internettoegang verbinden met Log Analytics met behulp van de Log Analytics-gateway](../log-analytics/log-analytics-oms-gateway.md).
* Configureer [automatische waarschuwingen](../log-analytics/log-analytics-alerts.md) voor detectie en diagnostiek.
* Vertrouwd raken met de functies [zoeken in logboeken en query's uitvoeren](../log-analytics/log-analytics-log-searches.md) die als onderdeel van Log Analytics worden aangeboden.
* Ophalen van een meer gedetailleerd overzicht van Log Analytics en de mogelijkheden van het startpakket, lezen [wat is Log Analytics?](../operations-management-suite/operations-management-suite-overview.md).
