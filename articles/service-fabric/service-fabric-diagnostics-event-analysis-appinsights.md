---
title: Analyse van Azure Service Fabric-gebeurtenis met Application Insights | Microsoft Docs
description: Meer informatie over het visualiseren en analyseren van gebeurtenissen met Application Insights voor controle en diagnostische gegevens van Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 34f14f42150e46edae2d1352827f96a411117a62
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Analyse van gebeurtenis en visualisatie met Application Insights

Azure Application Insights is een uitbreidbaar platform voor bewaking en diagnostische gegevens. Er staan een krachtige analytics en gegevensquery's hulpprogramma, aanpasbare dashboard en visualisaties en meer opties, waaronder geautomatiseerde waarschuwingen. Het is het aanbevolen platform voor controle en diagnostische gegevens voor Service Fabric-toepassingen en services.

## <a name="setting-up-application-insights"></a>Application Insights instellen

### <a name="creating-an-ai-resource"></a>Maken van een Resource AI

Maken van een resource AI head over naar de Azure Marketplace en zoek naar 'Application Insights'. Het moet weergegeven als de eerste oplossing (het zich onder de categorie 'Web + Mobile'). Klik op **maken** wanneer u de juiste resource kijkt (bevestigen dat het pad overeenkomt met de onderstaande afbeelding).

![Nieuwe Application Insights-resource](media/service-fabric-diagnostics-event-analysis-appinsights/create-new-ai-resource.png)

U moet om sommige gegevens voor het inrichten van de resource correct in te vullen. In de *toepassingstype* veld gebruik 'ASP.NET-webtoepassing' als u van een Service Fabric gebruikmaakt de programming modellen of publiceren een .NET-toepassing aan het cluster. Gebruik 'Algemene' als u Gast uitvoerbare bestanden en containers implementeren wilt. In het algemeen standaard gebruik van 'ASP.NET-webtoepassing' geopend te houden die uw opties in de toekomst. De naam is tot uw voorkeur en de resourcegroep en de abonnement zijn gewijzigd na de implementatie van de resource. Het is raadzaam dat uw AI-resource in dezelfde resourcegroep als uw cluster is. Als u meer informatie nodig hebt, raadpleegt u [een Application Insights-resource maken](../application-insights/app-insights-create-new-resource.md)

U moet de Instrumentatiesleutel AI AI configureren met de gebeurtenis aggregatie-hulpprogramma. Zodra uw AI-resource (duurt een paar minuten nadat de implementatie is gevalideerd) is geconfigureerd, Ga naar het en zoek de **eigenschappen** sectie op de linkernavigatiebalk. Een nieuwe blade geopend waarin een *INSTRUMENTATIESLEUTEL*. Als u wijzigen van het abonnement of resourcegroep van de bron wilt, kan het worden gedaan hier ook.

### <a name="configuring-ai-with-wad"></a>AI configureren met af

>[!NOTE]
>Dit is alleen van toepassing op Windows-clusters op dit moment.

Er zijn twee methoden voor het verzenden van gegevens van af naar Azure AI, die wordt bereikt door een sink AI toe te voegen aan de configuratie af, zoals beschreven in [in dit artikel](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

#### <a name="add-an-ai-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a>Een Instrumentatiesleutel AI toevoegen bij het maken van een cluster in Azure-portal

![Toevoegen van een AIKey](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

Bij het maken van een cluster als Diagnostics 'Op' is ingeschakeld, wordt er een optioneel veld dat u een Application Insights-instrumentatiesleutel wilt weergeven. Als u uw AI IKey hier plakt, de sink AI automatisch geconfigureerd voor u in de Resource Manager-sjabloon die wordt gebruikt voor het implementeren van uw cluster.

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

In zowel de codefragmenten hierboven, is de naam 'applicationInsights' gebruikt om te beschrijven de sink. Dit is geen vereiste en als de naam van de sink is opgenomen in een 'put', kunt u de naam instellen op een willekeurige tekenreeks.

Op dit moment wordt logboeken van het cluster weergegeven als de traceringen in AI van Logboeken. Aangezien de meeste van de traceringen afkomstig is van het platform van niveau 'Ter informatie' zijn, kunt u ook overwegen voor het wijzigen van de configuratie van de sink om alleen de logboeken van het type 'Kritiek' of 'Fout' verzenden. Dit kan worden gedaan door 'Kanalen' toevoegen aan uw sink, zoals wordt beschreven in [in dit artikel](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

>[!NOTE]
>Als u een onjuiste AI IKey in de portal of in de Resource Manager-sjabloon gebruikt, moet u handmatig de sleutel wijzigen en het cluster bijwerken / opnieuw te implementeren. 

### <a name="configuring-ai-with-eventflow"></a>AI met EventFlow configureren

Als u EventFlow voor het verzamelen van gegevens gebruikt, controleert u of voor het importeren van de `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`NuGet-pakket. De volgende moet worden opgenomen in de *levert* sectie van de *eventFlowConfig.json*:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        // (replace the following value with your AI resource's instrumentation key)
        "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
]
```

Zorg ervoor dat de vereiste wijzigingen aanbrengen in uw filters, evenals alle andere invoer (samen met hun respectieve NuGet-pakketten) bevatten.

## <a name="aisdk"></a>AI. SDK

Het is raadzaam EventFlow en af te gebruiken als aggregatieoplossingen, omdat ze is toegestaan voor een meer modulaire benadering voor diagnostische gegevens en bewaking, dat wil zeggen dat als u wilt wijzigen van de uitvoer van EventFlow, hiervoor geen wijziging in uw werkelijke instrumentation alleen een eenvoudige wijziging aan het configuratiebestand. Als u echter besluit te investeren in het gebruik van Application Insights en waarschijnlijk niet wijzigen in een ander platform, moet u zoeken naar met behulp van AI nieuwe SDK voor het aggregeren van gebeurtenissen en ze worden verzonden naar AI. Dit betekent dat u niet langer EventFlow moet voor het verzenden van uw gegevens naar AI configureren, maar in plaats daarvan de ApplicationInsight Service Fabric-NuGet-pakket installeert. Meer informatie over het pakket vindt [hier](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

[Application Insights-ondersteuning voor Microservices en Containers](https://azure.microsoft.com/app-insights-microservices/) bevat enkele van de nieuwe functies die wordt gewerkt (momenteel nog steeds in beta), waarmee u uitgebreide controle out-of-the-box-opties met AI hebben. Deze omvatten het bijhouden van afhankelijkheid (gebruikt bij het bouwen van een AppMap van uw services en toepassingen in een cluster en de communicatie tussen deze twee) en betere correlatie van traceringen afkomstig zijn van uw services (helpt u bij het beter dicht een probleem in de werkstroom van een app of service).

Als u in .NET ontwikkelt en waarschijnlijk van Service Fabric programming modellen en wil AI gebruiken als uw platform gebruikmaken zullen voor het visualiseren en analyseren van gebeurtenissen en logboek gegevens zijn, klikt u vervolgens het is raadzaam dat u via de route AI SDK als vorm van controle en diagnos gaat workflow uitgevoerd. Lees [dit](../application-insights/app-insights-asp-net-more.md) en [dit](../application-insights/app-insights-asp-net-trace-logs.md) aan de slag met AI gebruiken om uw logboeken te verzamelen.

## <a name="navigating-the-ai-resource-in-azure-portal"></a>De resource AI in Azure-portal te navigeren

Wanneer u AI hebt geconfigureerd als uitvoer voor uw gebeurtenissen en Logboeken, wordt informatie moet eerst worden weergegeven in de AI-bron in een paar minuten. Navigeer naar de bron AI, u naar het dashboard AI-resource gaat. Klik op **Search** in de taakbalk AI om te zien van de meest recente traceringen die deze heeft ontvangen en kunnen ze filteren.

*Metrics Explorer* een handig hulpmiddel voor het maken van aangepaste dashboards die op basis van de metrische gegevens die uw toepassingen, services en cluster kunnen rapporteren. Zie [verkennen van metrische gegevens in Application Insights](../application-insights/app-insights-metrics-explorer.md) voor het instellen van een paar grafieken voor uzelf op basis van de gegevens die u verzamelt.

Te klikken op **Analytics** gaat u naar de Application Insights Analytics-portal, waar u kunt een query gebeurtenissen en traceringen met een groter bereik en optionaliteit. Meer informatie over deze [analyses in Application Insights](../application-insights/app-insights-analytics.md).

## <a name="next-steps"></a>Volgende stappen

* [Stel waarschuwingen in AI](../application-insights/app-insights-alerts.md) om te worden geïnformeerd over wijzigingen in de prestatie- of -gebruik
* [Detectie in Application Insights slimme](../application-insights/app-insights-proactive-diagnostics.md) voert een proactieve analyse van de telemetrie die wordt verzonden naar AI om u te waarschuwen u potentiële prestatieproblemen