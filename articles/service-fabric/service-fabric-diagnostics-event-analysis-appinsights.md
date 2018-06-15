---
title: Analyse van Azure Service Fabric-gebeurtenis met Application Insights | Microsoft Docs
description: Meer informatie over het visualiseren en analyseren van gebeurtenissen met Application Insights voor controle en diagnostische gegevens van Azure Service Fabric-clusters.
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
ms.date: 04/04/2018
ms.author: srrengar
ms.openlocfilehash: 29adf362fdacdb793af071fa6d7bd59214536374
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34207750"
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Analyse van gebeurtenis en visualisatie met Application Insights

Azure Application Insights is een uitbreidbaar platform voor bewaking en diagnostische gegevens. Er staan een krachtige analytics en gegevensquery's hulpprogramma, aanpasbare dashboard en visualisaties en meer opties, waaronder geautomatiseerde waarschuwingen. Het is het aanbevolen platform voor controle en diagnostische gegevens voor Service Fabric-toepassingen en services. In dit artikel helpt u bij het adres van de volgende veelgestelde vragen

* Hoe weet ik wat er gebeurt in mijn telemetrie toepassing, services en verzamelen
* Hoe los ik mijn toepassing vooral services met elkaar communiceren
* Hoe krijg ik metrische gegevens over hoe mijn services uitvoert, bijvoorbeeld, laadtijd van pagina, http-aanvragen

Het doel van dit artikel is om te laten zien hoe u inzicht krijgen en oplossen van in App Insights. Als u meer informatie over het instellen en configureren van AI met Service Fabric wilt, Bekijk dit [zelfstudie](service-fabric-tutorial-monitoring-aspnet.md).

## <a name="monitoring-in-app-insights"></a>Bewaking in de App Insights

Application Insights is een uitgebreide buiten de box experience bij gebruik van Service Fabric. Op de overzichtspagina bevat AI belangrijke informatie over uw service zoals de responstijd en het aantal aanvragen verwerkt. Door te klikken op de knop 'Zoeken' aan de bovenkant, ziet u een lijst met recente aanvragen in uw toepassing. U zou ook kunnen zien van mislukte aanvragen hier en onderzoeken welke fouten treden.

![AI-overzicht](media/service-fabric-diagnostics-event-analysis-appinsights/ai-overview.png)

In het rechterpaneel weergegeven in de voorgaande afbeelding, er zijn twee soorten gegevens in de lijst: aanvragen en gebeurtenissen. Aanvragen zijn aanroepen van de app API via HTTP-aanvragen in dit geval en gebeurtenissen zijn aangepaste gebeurtenissen, die fungeren als telemetrie die u overal in uw code kunt toevoegen. U kunt verder verkennen instrumenteren van uw toepassingen in [Application Insights-API voor aangepaste gebeurtenissen en metrische gegevens](../application-insights/app-insights-api-custom-events-metrics.md). Te klikken op een aanvraag zou meer details weergeven zoals weergegeven in de volgende afbeelding, met inbegrip van gegevens die specifiek zijn voor Service Fabric, die wordt verzameld in het AI Service Fabric-nuget-pakket. Deze informatie is nuttig voor het oplossen van problemen en weten wat de status van uw toepassing is en al deze informatie kan worden doorzocht in Application Insights is

![AI aanvraaggegevens](media/service-fabric-diagnostics-event-analysis-appinsights/ai-request-details.png)

Application Insights is een aangewezen weergave voor een query op alle gegevens die wordt geleverd in. Klik op 'Metrics Explorer' boven aan de pagina overzicht om te navigeren naar de AI-portal. Hier kunt u query's uitvoeren met aangepaste gebeurtenissen al eerder vermeld, aanvragen, uitzonderingen, prestatiemeteritems en andere metrische gegevens met behulp van de querytaal Kusto. Het volgende voorbeeld ziet alle aanvragen in het afgelopen uur.

![AI aanvraaggegevens](media/service-fabric-diagnostics-event-analysis-appinsights/ai-metrics-explorer.png)

Als u wilt de mogelijkheden van de App Insights-portal verder verkennen, Ga naar de [Application Insights-portal documentatie](../application-insights/app-insights-dashboards.md).

### <a name="configuring-ai-with-wad"></a>AI configureren met af

>[!NOTE]
>Dit is alleen van toepassing op Windows-clusters op dit moment.

Er zijn twee methoden voor het verzenden van gegevens van af naar Azure AI, die wordt bereikt door een sink AI toe te voegen aan de configuratie af, zoals beschreven in [in dit artikel](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

#### <a name="add-an-ai-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a>Een Instrumentatiesleutel AI toevoegen bij het maken van een cluster in Azure-portal

![Toevoegen van een AIKey](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

Bij het maken van een cluster als Diagnostics 'Op' is ingeschakeld, wordt er een optioneel veld dat u een Application Insights-instrumentatiesleutel wilt weergeven. Als u uw sleutel AI hier plakt, is de AI-sink automatisch geconfigureerd voor u in de Resource Manager-sjabloon die wordt gebruikt voor het implementeren van uw cluster.

#### <a name="add-the-ai-sink-to-the-resource-manager-template"></a>De Sink AI toevoegen aan de Resource Manager-sjabloon

Voeg in de 'WadCfg' van de Resource Manager-sjabloon, een 'Sink' door de volgende twee wijzigingen, waaronder:

1. Toevoegen van de configuratie van de sink direct na het declarerende van de `DiagnosticMonitorConfiguration` is voltooid:

    ```json
    "SinksConfig": {
        "Sink": [
            {
                "name": "applicationInsights",
                "ApplicationInsights": "***ADD INSTRUMENTATION KEY HERE***"
            }
        ]
    }

    ```

2. Bevatten de Sink in de `DiagnosticMonitorConfiguration` door het toevoegen van de volgende regel in de `DiagnosticMonitorConfiguration` van de `WadCfg` (aan vóór de `EtwProviders` zijn gedeclareerd):

    ```json
    "sinks": "applicationInsights"
    ```

In zowel de voorgaande codefragmenten, is de naam 'applicationInsights' gebruikt om te beschrijven de sink. Dit is geen vereiste en als de naam van de sink is opgenomen in een 'put', kunt u de naam instellen op een willekeurige tekenreeks.

Op dit moment logboeken van het cluster worden weergegeven als **traceringen** in AI van Logboeken. Aangezien de meeste van de traceringen afkomstig is van het platform van niveau 'Ter informatie' zijn, kunt u ook overwegen voor het wijzigen van de configuratie van de sink om Logboeken van het type 'Kritiek' of 'Fout'. alleen verzenden Dit kan worden gedaan door 'Kanalen' toevoegen aan uw sink, zoals wordt beschreven in [in dit artikel](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

>[!NOTE]
>Als u een onjuiste AI-sleutel in de portal of in de Resource Manager-sjabloon gebruikt, moet u handmatig de sleutel wijzigen en het cluster bijwerken / opnieuw te implementeren.

### <a name="configuring-ai-with-eventflow"></a>AI met EventFlow configureren

Als u EventFlow voor het verzamelen van gegevens gebruikt, controleert u of voor het importeren van de `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`NuGet-pakket. De volgende code is vereist in de *levert* sectie van de *eventFlowConfig.json*:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        "instrumentationKey": "***ADD INSTRUMENTATION KEY HERE***"
    }
]
```

Zorg ervoor dat de vereiste wijzigingen aanbrengen in uw filters, evenals alle andere invoer (samen met hun respectieve NuGet-pakketten) bevatten.

## <a name="aisdk"></a>AI.SDK

Het verdient aanbeveling met EventFlow en af aggregatieoplossingen, omdat ze toestaan voor een meer modulaire benadering voor diagnostische gegevens en bewaking, dat wil zeggen als u wilt wijzigen van de uitvoer van EventFlow, geen wijziging in uw werkelijke instrumentation vereist alleen een eenvoudige wijziging aan het configuratiebestand. Als u echter besluit te investeren in het gebruik van Application Insights en waarschijnlijk niet wijzigen in een ander platform, moet u zoeken naar met behulp van AI nieuwe SDK voor het aggregeren van gebeurtenissen en ze worden verzonden naar AI. Dit betekent dat u niet langer EventFlow moet voor het verzenden van uw gegevens naar AI configureren, maar in plaats daarvan de ApplicationInsight Service Fabric-NuGet-pakket installeert. Meer informatie over het pakket vindt [hier](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

[Application Insights-ondersteuning voor Microservices en Containers](https://azure.microsoft.com/blog/app-insights-microservices/) bevat enkele van de nieuwe functies die wordt gewerkt (momenteel nog steeds in beta), waarmee u uitgebreide controle out-of-the-box-opties met AI hebben. Deze omvatten het bijhouden van afhankelijkheid (gebruikt bij het bouwen van een AppMap van uw services en toepassingen in een cluster en de communicatie tussen deze twee) en betere correlatie van traceringen afkomstig zijn van uw services (helpt u bij het beter dicht een probleem in de werkstroom van een app of service).

Als u in .NET ontwikkelt en waarschijnlijk van Service Fabric programming modellen en wil AI gebruiken als uw platform gebruikmaken zullen voor het visualiseren en analyseren van gebeurtenissen en logboek gegevens zijn, klikt u vervolgens het is raadzaam dat u via de route AI SDK als vorm van controle en diagnos gaat workflow uitgevoerd. Lees [dit](../application-insights/app-insights-asp-net-more.md) en [dit](../application-insights/app-insights-asp-net-trace-logs.md) aan de slag met AI gebruiken om uw logboeken te verzamelen.

## <a name="navigating-the-ai-resource-in-azure-portal"></a>De resource AI in Azure-portal te navigeren

Wanneer u AI hebt geconfigureerd als uitvoer voor uw gebeurtenissen en Logboeken, wordt informatie moet eerst worden weergegeven in de AI-bron in een paar minuten. Navigeer naar de bron AI, u naar het dashboard AI-resource gaat. Klik op **Search** in de taakbalk AI om te zien van de meest recente traceringen die deze heeft ontvangen en kunnen ze filteren.

*Metrics Explorer* een handig hulpmiddel voor het maken van aangepaste dashboards die op basis van de metrische gegevens die uw toepassingen, services en cluster kunnen rapporteren. Zie [verkennen van metrische gegevens in Application Insights](../application-insights/app-insights-metrics-explorer.md) voor het instellen van een paar grafieken voor uzelf op basis van de gegevens die u verzamelt.

Te klikken op **Analytics** gaat u naar de Application Insights Analytics-portal, waar u kunt een query gebeurtenissen en traceringen met een groter bereik en optionaliteit. Meer informatie over deze [analyses in Application Insights](../application-insights/app-insights-analytics.md).

## <a name="next-steps"></a>Volgende stappen

* [Stel waarschuwingen in AI](../application-insights/app-insights-alerts.md) om te worden geïnformeerd over wijzigingen in de prestatie- of -gebruik
* [Detectie in Application Insights slimme](../application-insights/app-insights-proactive-diagnostics.md) voert een proactieve analyse van de telemetrie die wordt verzonden naar AI om u te waarschuwen u potentiële prestatieproblemen
