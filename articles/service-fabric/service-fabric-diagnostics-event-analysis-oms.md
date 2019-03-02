---
title: Azure Service Fabric-Gebeurtenisanalyse met Azure Monitor-Logboeken | Microsoft Docs
description: Meer informatie over het visualiseren en analyseren van gebeurtenissen met behulp van Azure Monitor-logboeken voor controle en diagnose van Azure Service Fabric-clusters.
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
ms.date: 02/21/2019
ms.author: srrengar
ms.openlocfilehash: 2f3106b33ab0cbea95efe2ac42c05a8543719190
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57246913"
---
# <a name="event-analysis-and-visualization-with-azure-monitor-logs"></a>Gebeurtenis analyses en visualisatie met Azure Monitor-Logboeken
 Azure Monitor-logboeken verzamelt en analyseert telemetrie van toepassingen en services die worden gehost in de cloud biedt analyse hulpprogramma's waarmee u hun beschikbaarheid en prestaties te maximaliseren. In dit artikel bevat een overzicht van hoe u query's uitvoeren in Azure Monitor-logboeken inzicht en problemen met wat er gebeurt in uw cluster. De volgende veelgestelde vragen worden behandeld:

* Hoe kan ik statusgebeurtenissen oplossen?
* Hoe weet ik wanneer een knooppunt wordt uitgeschakeld?
* Hoe weet ik als services van mijn toepassing hebben gestart of gestopt?

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="overview-of-the-log-analytics-workspace"></a>Overzicht van de Log Analytics-werkruimte

>[!NOTE] 
>Opslagaccount voor diagnostische is standaard ingeschakeld tijdens de aanmaak van het cluster, moet u de Log Analytics-werkruimte nog steeds instellen om te lezen uit het opslagaccount voor diagnostische.

Azure Monitor-logboeken verzamelt gegevens van beheerde resources, met inbegrip van een Azure storage-tabel of een agent, en deze in een centrale opslagplaats onderhoudt. De gegevens kunnen vervolgens worden gebruikt voor analyse, waarschuwingen en visualisatie, of het verder exporteren. Azure Monitor registreert ondersteunt gebeurtenissen, prestatiegegevens of een andere aangepaste gegevens. Bekijk [stappen voor het configureren van de extensie voor diagnostische gegevens voor](service-fabric-diagnostics-event-aggregation-wad.md) en [stappen voor het maken van een Log Analytics-werkruimte te lezen van de gebeurtenissen in de opslag](service-fabric-diagnostics-oms-setup.md) om ervoor te zorgen dat gegevens worden doorgestuurd naar Azure Monitor Hiermee wordt geregistreerd.

Nadat de gegevens worden ontvangen door de logboeken van Azure Monitor, Azure heeft verscheidene *bewaking oplossingen* die zijn voorverpakte oplossingen of operationele dashboards voor het bewaken van binnenkomende gegevens, aangepast aan verschillende scenario's. Deze omvatten een *Service Fabric-analyse* oplossing en een *Containers* oplossing, zijn de twee meest relevante diagnostische gegevens en de bewaking van bij het gebruik van Service Fabric-clusters. In dit artikel wordt beschreven hoe u van de Service Fabric-analyse-oplossing die is gemaakt met de werkruimte.

## <a name="access-the-service-fabric-analytics-solution"></a>Toegang tot de Service Fabric-analyse-oplossing

In de [Azure Portal](https://portal.azure.com), gaat u naar de resourcegroep waarin u de Service Fabric-analyse-oplossing hebt gemaakt.

Selecteer de resource **ServiceFabric\<nameOfOMSWorkspace\>**.

In `Summary`, ziet u tegels in de vorm van een grafiek voor elk van de oplossingen ingeschakeld, met inbegrip van een Service Fabric. Klik op de **Service Fabric** graph om door te gaan naar de Service Fabric-analyse-oplossing.

![Service Fabric-oplossing](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_summary.PNG)

De volgende afbeelding ziet de startpagina van de Service Fabric-analyse-oplossing. Deze pagina introductiepagina bevat een momentopname van wat in uw cluster gebeurt er.

![Service Fabric-oplossing](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_solution.PNG)

 Als u diagnostische gegevens bij het maken van een cluster hebt ingeschakeld, kunt u gebeurtenissen voor bekijken 

* [Service Fabric-cluster-gebeurtenissen](service-fabric-diagnostics-event-generation-operational.md)
* [Reliable Actors programming modelgebeurtenissen](service-fabric-reliable-actors-diagnostics.md)
* [Reliable Services programming modelgebeurtenissen](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>Naast de Service Fabric-gebeurtenissen uit het vak, meer gedetailleerde systeemgebeurtenissen kunnen worden verzameld door [bijwerken van de configuratie van de extensie voor diagnostische gegevens](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations).

## <a name="view-service-fabric-events-including-actions-on-nodes"></a>Service Fabric-gebeurtenissen weergeven, met inbegrip van acties op knooppunten

Klik op de pagina Service Fabric-analyse op de grafiek voor **Service Fabric-gebeurtenissen**.

![Service Fabric-oplossing operationele kanaal](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events_selection.png)

Klik op **lijst** om de gebeurtenissen in een lijst weer te geven. Zodra u hier alle systeemgebeurtenissen die zijn verzameld ziet. Voor een verwijzing naar deze zijn van de **WADServiceFabricSystemEventsTable** in Azure Storage-account en op dezelfde manier de reliable services en actors-gebeurtenissen ziet u naast afkomstig zijn van de respectieve tabellen.
    
![Query operationele kanaal](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events.png)

U kunt ook klikt u op het Vergrootglas aan de linkerkant en de Kusto-query-taal gebruiken om te vinden wat u zoekt. Bijvoorbeeld, als u wilt zoeken in alle acties die worden uitgevoerd op knooppunten in het cluster, kunt u de volgende query uit. De gebeurtenis-id's gebruikt hieronder vindt u in de [operationele kanaal gebeurtenissen verwijzing](service-fabric-diagnostics-event-generation-operational.md).

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

U kunt een query op veel meer velden, zoals de specifieke knooppunten (Computer) de service (TaskName).

## <a name="view-service-fabric-reliable-service-and-actor-events"></a>Weergave Service Fabric Reliable Service en de Actor-gebeurtenissen

Klik op de pagina Service Fabric-analyse op de grafiek voor **Reliable Services**.

![Service Fabric Reliable Services-oplossing](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_services_events_selection.png)

Klik op **lijst** om de gebeurtenissen in een lijst weer te geven. Hier ziet u gebeurtenissen van de betrouwbare services. U kunt verschillende gebeurtenissen voor zien wanneer de service runasync is gestart en dit gebeurt gewoonlijk op implementaties en upgrades voltooid. 

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
* Logboeken van Azure Monitor biedt een Gateway (http-doorsturen Proxy) die kan worden gebruikt om gegevens te verzenden naar Azure Monitor-logboeken voor clusters van on-premises. Meer informatie over die in [computers zonder toegang tot het Internet verbinding te maken met Azure Monitor-logboeken met behulp van de Log Analytics-gateway](../azure-monitor/platform/gateway.md).
* Configureer [automatische waarschuwingen](../log-analytics/log-analytics-alerts.md) voor detectie en diagnostiek.
* Familiarized ophalen met de [zoeken en uitvoeren van query's](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van Azure Monitor-Logboeken.
* Ophalen van een meer gedetailleerd overzicht van Azure Monitor-logboeken en mogelijkheden van het startpakket, lezen [wat is Azure Monitor logboeken?](../operations-management-suite/operations-management-suite-overview.md).
