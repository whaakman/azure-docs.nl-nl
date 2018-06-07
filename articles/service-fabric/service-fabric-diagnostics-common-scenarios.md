---
title: Azure Service Fabric sporen algemene scenario's | Microsoft Docs
description: Meer informatie over het oplossen van veelvoorkomende scenario's met Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/16/2018
ms.author: srrengar
ms.openlocfilehash: bd7a7e0288ced0219a0600034b273d1acba6b09b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655242"
---
# <a name="diagnose-common-scenarios-with-service-fabric"></a>Algemene scenario's met Service Fabric onderzoeken

In dit artikel ziet u veelvoorkomende scenario's die gebruikers hebben aangetroffen in het gebied van controle en diagnostische gegevens met Service Fabric. De scenario's die zijn gepresenteerd hebben betrekking op alle 3 lagen van het service fabric: toepassings-, Cluster- en infrastructuur. Elke oplossing maakt gebruik van Application Insights en Log Analytics, Azure controlehulpprogramma's, elk scenario te voltooien. De stappen in elke oplossing geven gebruikers een inleiding op het gebruik van Application Insights en Log Analytics in de context van de Service Fabric.

## <a name="prerequisites-and-recommendations"></a>Vereisten en aanbevelingen

De oplossingen in dit artikel wordt de volgende hulpprogramma's gebruiken. U wordt aangeraden dat u hebt deze ingesteld en geconfigureerd:

* [Application Insights met Service Fabric](service-fabric-tutorial-monitoring-aspnet.md)
* [Inschakelen van Azure Diagnostics op het cluster](service-fabric-diagnostics-event-aggregation-wad.md)
* [Instellen van een OMS Log Analytics-werkruimte](service-fabric-diagnostics-oms-setup.md)
* [OMS-Agent voor het bijhouden van prestatiemeteritems](service-fabric-diagnostics-oms-agent.md)

## <a name="how-can-i-see-unhandled-exceptions-in-my-application"></a>Hoe ziet niet-verwerkte uitzonderingen in mijn toepassing?

1. Navigeer naar uw Application Insights-resource die uw toepassing is geconfigureerd met.
2. Klik op *Search* linksboven. Klik vervolgens op het filter op het volgende deelvenster.

    ![AI-overzicht](media/service-fabric-diagnostics-common-scenarios/ai-search-filter.png)

3. Hier ziet u veel typen gebeurtenissen (traceringen, aanvragen, aangepaste gebeurtenissen). Kies 'Uitzondering' als filter.

    ![AI-filterlijst](media/service-fabric-diagnostics-common-scenarios/ai-filter-list.png)

    Door te klikken op een uitzondering opgetreden in de lijst, kunt u meer informatie, zoals de servicecontext als u van de Service Fabric Application Insights-SDK gebruikmaakt bekijken.

    ![AI-uitzondering](media/service-fabric-diagnostics-common-scenarios/ai-exception.png)

## <a name="how-do-i-view-which-http-calls-are-used-in-my-services"></a>Hoe weergeven waarmee HTTP-aanroepen in mijn services worden gebruikt?

1. In de dezelfde Application Insights-resource, kunt u filteren op 'aanvragen' in plaats van uitzonderingen en alle aanvragen weergeven
2. Als u van de Service Fabric Application Insights-SDK gebruikmaakt, ziet u een visuele representatie van uw services met elkaar verbonden en het aantal is voltooid en mislukte aanvragen. Klik op 'Toepassingstoewijzing' aan de linkerkant.

    ![AI App kaart Blade](media/service-fabric-diagnostics-common-scenarios/app-map-blade.png) ![AI-App-kaart](media/service-fabric-diagnostics-common-scenarios/app-map-new.png)

    Voor meer informatie over de toepassing-kaart, gaat u naar de [toepassingstoewijzing documentatie](../application-insights/app-insights-app-map.md)

## <a name="how-do-i-create-an-alert-when-a-node-goes-down"></a>Hoe maak ik een waarschuwing wanneer een knooppunt uitgeschakeld wordt

1. Knooppunt gebeurtenissen worden bijgehouden door de Service Fabric-cluster. Navigeer naar de Service Fabric Analytics-oplossing-resource met de naam **ServiceFabric(NameofResourceGroup)**
2. Klik op de grafiek aan de onderkant van de blade met de titel 'Samenvatting'

    ![OMS-oplossing](media/service-fabric-diagnostics-common-scenarios/oms-solution-azure-portal.png)

3. U hebt hier veel grafieken en tegels verschillende metrische gegevens om weer te geven. Klik op een van de grafieken en deze gaat u naar het logboek zoeken. Hier kunt u een query voor clustergebeurtenissen of -prestatiemeteritems.
4. Voer de volgende query. Deze gebeurtenis-id's zijn gevonden in de [knooppunt gebeurtenissen verwijzing](service-fabric-diagnostics-event-generation-operational.md#application-events)

    ```kusto
    ServiceFabricOperationalEvent
    | where EventId >= 25623 or EventId <= 25626
    ```

5. Klik op 'Nieuwe waarschuwingsregel' aan de bovenkant en nu telkens wanneer een gebeurtenis is ontvangen op basis van deze query, ontvangt u een waarschuwing in de gekozen methode voor communicatie.

    ![Nieuwe OMS-waarschuwing](media/service-fabric-diagnostics-common-scenarios/oms-create-alert.png)

## <a name="how-can-i-be-alerted-of-application-upgrade-rollbacks"></a>Hoe kan ik gewaarschuwd van upgrade Rollback toepassing?

1. Voer op de dezelfde logboek zoekvenster als voordat de volgende query voor de upgrade Rollback. Deze gebeurtenis-id's zijn gevonden onder [documentatie voor toepassingsbeheer-gebeurtenissen](service-fabric-diagnostics-event-generation-operational.md#application-events)

    ```kusto
    ServiceFabricOperationalEvent
    | where EventId == 29623 or EventId == 29624
    ```

2. Klik op 'Nieuwe waarschuwingsregel' aan de bovenkant en nu telkens wanneer een gebeurtenis is ontvangen op basis van deze query, ontvangt u een waarschuwing.

## <a name="how-do-i-see-container-metrics"></a>Hoe kan ik container metrische gegevens zien?

In dezelfde weergave met alle grafieken ziet u enkele tegels voor de prestaties van uw containers. U moet de OMS-Agent en [oplossing Container bewaking](service-fabric-diagnostics-oms-containers.md) voor deze tegels te vullen.

![Metrische gegevens OMS-Container](media/service-fabric-diagnostics-common-scenarios/containermetrics.png)

>[!NOTE]
>Naar instrument telemetrie van **binnen** de container die u wilt toevoegen de [nuget-pakket voor Application Insights voor containers](https://github.com/Microsoft/ApplicationInsights-servicefabric#microsoftapplicationinsightsservicefabric--for-service-fabric-lift-and-shift-scenarios).

## <a name="how-can-i-monitor-performance-counters"></a>Hoe kan ik prestatiemeteritems controleren?

1. Nadat u de OMS-Agent aan uw cluster hebt toegevoegd die u wilt toevoegen de specifieke-prestatiemeteritems die u wilt traceren. Navigeer naar de OMS-werkruimte pagina in de portal – vanaf het tabblad werkruimte op het menu links wordt van de oplossing-pagina.

    ![Tabblad OMS-werkruimte](media/service-fabric-diagnostics-common-scenarios/workspacetab.png)

2. Als u op de pagina van de werkruimte bent, klik op 'Geavanceerd' in het menu met dezelfde links.

    ![OMS geavanceerde instellingen](media/service-fabric-diagnostics-common-scenarios/advancedsettingsoms.png)

3. Klik op gegevens > Windows-prestatiemeteritems (gegevens > Linux-prestatiemeteritems voor Linux-machines) te starten met het verzamelen van specifieke items van uw knooppunten via de OMS-Agent. Hier volgen enkele voorbeelden van de indeling voor prestatiemeteritems toevoegen

    * `.NET CLR Memory(<ProcessNameHere>)\\# Total committed Bytes`
    * `Processor(_Total)\\% Processor Time`
    * `Service Fabric Service(*)\\Average milliseconds per request`

    In de Snelstartgids VotingData en VotingWeb zijn de procesnamen gebruikt, dus deze items bijhouden als eruit

    * `.NET CLR Memory(VotingData)\\# Total committed Bytes`
    * `.NET CLR Memory(VotingWeb)\\# Total committed Bytes`

    ![OMS-prestatiemeteritems](media/service-fabric-diagnostics-common-scenarios/omsperfcounters.png)

4. Hiermee kunt u zien hoe uw infrastructuur verwerkt uw werkbelastingen en relevante waarschuwingen op basis van Resourcegebruik instellen. U wilt bijvoorbeeld – instellen van een waarschuwing als het totale processorgebruik dan 90% of minder dan 5 procent gaat. De naam van de prestatiemeteritems die u voor dit gebruiken zou is "% processortijd." U kunt dit doen door een waarschuwingsregel maken voor de volgende query:

    ```kusto
    Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" | where CounterValue >= 90 or CounterValue <= 5.
    ```

## <a name="how-do-i-track-performance-of-my-reliable-services-and-actors"></a>Hoe kan ik de prestaties van mijn Reliable Services en actoren bijhouden

U moet ook de tellers Service Fabric acteur, Actormethode, Service en Service-methode toevoegen voor het bijhouden van de prestaties van Reliable Services of actoren in uw toepassingen. U kunt deze prestatiemeteritems toevoegen op een vergelijkbare manier als het bovenstaande scenario, Hier volgen enkele voorbeelden van betrouwbare service en actor prestatiemeteritems toe te voegen in OMS

* `Service Fabric Service(*)\\Average milliseconds per request`
* `Service Fabric Service Method(*)\\Invocations/Sec`
* `Service Fabric Actor(*)\\Average milliseconds per request`
* `Service Fabric Actor Method(*)\\Invocations/Sec`

Controleer de volgende koppelingen voor de volledige lijst met prestatiemeteritems op betrouwbaar [Services](service-fabric-reliable-serviceremoting-diagnostics.md) en [Actors](service-fabric-reliable-actors-diagnostics.md)

## <a name="next-steps"></a>Volgende stappen

* [Stel waarschuwingen in AI](../application-insights/app-insights-alerts.md) om te worden geïnformeerd over wijzigingen in de prestatie- of -gebruik
* [Detectie in Application Insights slimme](../application-insights/app-insights-proactive-diagnostics.md) voert een proactieve analyse van de telemetrie die wordt verzonden naar AI om u te waarschuwen u potentiële prestatieproblemen
* Meer informatie over OMS Log Analytics [waarschuwingen](../log-analytics/log-analytics-alerts.md) voor detectie en diagnostische gegevens.
* OMS biedt voor lokale clusters, een Gateway (http-doorsturen Proxy) die kan worden gebruikt om gegevens te verzenden aan OMS. Lees meer over die in [computers zonder toegang tot het Internet verbinden met OMS met behulp van de OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md)
* Familiarized ophalen met de [zoeken en uitvoeren van query's in logboek registreren](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van logboekanalyse
* Ophalen van een meer gedetailleerd overzicht van logboekanalyse en wat biedt, lezen [wat is er Log Analytics?](../operations-management-suite/operations-management-suite-overview.md)
