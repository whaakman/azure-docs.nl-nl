---
title: Azure Service Fabric-Gebeurtenisanalyse met Application Insights | Microsoft Docs
description: Meer informatie over het visualiseren en analyseren van gebeurtenissen met Application Insights voor controle en diagnose van Azure Service Fabric-clusters.
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
ms.openlocfilehash: f9c7a70eae4c49173b3e11b7fbfa901f7e5b89d6
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291042"
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Gebeurtenis analyses en visualisatie met Application Insights

Onderdeel van Azure Monitor, Application Insights is een uitbreidbaar platform voor de toepassingsbewaking en diagnostische gegevens. Het bevat een krachtige analyses en het uitvoeren van query's hulpprogramma, aanpasbaar dashboard en visualisaties en meer opties, waaronder automatische waarschuwingen. De Application Insights-integratie met Service Fabric bevat verschillende hulpprogramma's voor Visual Studio en Azure portal, evenals Service Fabric-specifieke metrische gegevens, biedt een uitgebreide logboekregistratie voor out-of-the-box-ervaring. Hoewel veel logboeken worden automatisch gemaakt en die voor u worden verzameld met Application Insights, raden wij u aan meer aangepaste logboekregistratie voor uw toepassingen voor het maken van een rijkere ervaring voor diagnostische gegevens.

Dit artikel helpt de volgende veelgestelde vragen-adres:

* Hoe weet ik wat er gebeurt in mijn toepassingen en services en verzamelen telemetrie?
* Hoe los ik mijn toepassing, met name services met elkaar communiceren?
* Hoe krijg ik metrische gegevens over hoe mijn services uitvoert, bijvoorbeeld, de laadtijd van browserpagina, HTTP-aanvragen?

Het doel van dit artikel is om te laten zien hoe u inzicht en oplossen van in Application Insights. Als u wilt meer informatie over het instellen en configureren van Application Insights met Service Fabric, bekijk dan deze [zelfstudie](service-fabric-tutorial-monitoring-aspnet.md).

## <a name="monitoring-in-application-insights"></a>Bewaken in Application Insights

Application Insights is een geavanceerde buiten de box-ervaring bij het gebruik van Service Fabric. De pagina overzicht van biedt Application Insights belangrijke informatie over uw service, zoals de reactietijd en het aantal aanvragen dat is verwerkt. Door te klikken op de knop 'Zoeken' aan de bovenkant, ziet u een lijst met recente aanvragen in uw toepassing. Bovendien kunt u zou zich hier mislukte aanvragen weergeven en een diagnose kunnen welke fouten zijn opgetreden.

![Overzicht van Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/ai-overview.png)

In het rechterdeelvenster in de vorige afbeelding, er zijn twee soorten gegevens in de lijst: aanvragen en gebeurtenissen. Aanvragen zijn oproepen naar de API via HTTP-aanvragen van de app in dit geval en gebeurtenissen zijn aangepaste gebeurtenissen, die fungeren als telemetrie die u overal in uw code kunt toevoegen. U kunt verder verkennen instrumenteren van uw toepassingen in [Application Insights-API voor aangepaste gebeurtenissen en metrische gegevens](../application-insights/app-insights-api-custom-events-metrics.md). Te klikken op een aanvraag zou meer details weergeven zoals wordt weergegeven in de volgende afbeelding, met inbegrip van gegevens die specifiek zijn voor Service Fabric, dat wordt verzameld in het nuget-pakket voor Application Insights-Service Fabric. Deze informatie is nuttig voor het oplossen van problemen en weet wat de status van uw toepassing is, en al deze gegevens wordt gezocht binnen de Application Insights

![Details van de aanvraag Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/ai-request-details.png)

Application Insights heeft een aangewezen weergave voor het uitvoeren van query's op alle gegevens die wordt geleverd in. Klik op 'Metrics Explorer' boven aan de pagina overzicht om te navigeren naar de Application Insights-portal. Hier kunt u query's uitvoeren op basis van aangepaste gebeurtenissen al eerder vermeld, aanvragen, uitzonderingen, prestatiemeteritems en andere metrische gegevens met behulp van de Kusto-query-taal. Het volgende voorbeeld ziet alle aanvragen in het afgelopen uur.

![Details van de aanvraag Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/ai-metrics-explorer.png)

Als u wilt de mogelijkheden van de Application Insights-portal verder verkennen, Ga naar de [documentatie voor Application Insights-portal](../application-insights/app-insights-dashboards.md).

### <a name="configuring-application-insights-with-wad"></a>Application Insights configureren met WAD

>[!NOTE]
>Dit is alleen van toepassing op Windows-clusters op dit moment.

Er zijn twee primaire manieren om gegevens te verzenden vanuit WAD naar Azure Application Insights, waarmee wordt bereikt door een sink Application Insights toe te voegen aan de configuratie WAD, zoals beschreven in [in dit artikel](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

#### <a name="add-an-application-insights-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a>Een Application Insights-Instrumentatiesleutel toevoegen bij het maken van een cluster in Azure portal

![Toevoegen van een AIKey](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

Bij het maken van een cluster als diagnostische gegevens 'Aan' is ingeschakeld, ziet u een optioneel veld een sleutel van Application Insights-Instrumentatiepakket in te voeren. Als u uw Application Insights-sleutel hier plakt, worden de Application Insights-sink automatisch voor u in de Resource Manager-sjabloon die wordt gebruikt voor het implementeren van uw cluster geconfigureerd.

#### <a name="add-the-application-insights-sink-to-the-resource-manager-template"></a>Het Sink-Application Insights toevoegen aan de Resource Manager-sjabloon

Toevoegen in de 'WadCfg' van de Resource Manager-sjabloon, een 'Sink' door de volgende twee wijzigingen op te nemen:

1. Toevoegen van de configuratie van de sink direct na het declareren van de `DiagnosticMonitorConfiguration` is voltooid:

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

2. Bevatten de Sink in de `DiagnosticMonitorConfiguration` door toe te voegen van de volgende regel in de `DiagnosticMonitorConfiguration` van de `WadCfg` (vlak voor de `EtwProviders` zijn gedeclareerd):

    ```json
    "sinks": "applicationInsights"
    ```

In zowel de voorgaande codefragmenten, is de naam 'Application Insights' gebruikt om te beschrijven van de sink. Dit is geen vereiste en als de naam van de sink is opgenomen in een 'put', kunt u de naam van de instellen op een willekeurige tekenreeks.

Op dit moment logboeken van het cluster weergegeven als **traceringen** in Application Insights-Logboeken. Omdat de meeste van de traceringen die afkomstig zijn van het platform van niveau 'Ter informatie' zijn, kunt u ook overwegen wijzigen van de sink-configuratie voor het verzenden van alleen logboeken van het type "Critical" of "Error". Dit kan worden gedaan door 'Kanalen' toevoegen aan uw sink, zoals geïllustreerd in [in dit artikel](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

>[!NOTE]
>Als u een onjuiste Application Insights-sleutel in de portal of in het Resource Manager-sjabloon, moet u de sleutel handmatig te wijzigen en bijwerken van het cluster / opnieuw te implementeren.

### <a name="configuring-application-insights-with-eventflow"></a>Configuratie van Application Insights samenstellen met EventFlow

Als u van EventFlow naar gebeurtenissen gebruikmaakt, controleert u of voor het importeren van de `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`NuGet-pakket. De volgende code is vereist in de *levert* sectie van de *eventFlowConfig.json*:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        "instrumentationKey": "***ADD INSTRUMENTATION KEY HERE***"
    }
]
```

Zorg ervoor dat u de benodigde wijzigingen aanbrengen in de filters, evenals alle andere invoer (samen met hun respectieve NuGet-pakketten) bevatten.

## <a name="application-insights-sdk"></a>Application Insights-SDK

Het verdient aanbeveling met EventFlow en WAD aggregatieoplossingen, omdat ze toestaan voor een meer modulaire benadering voor diagnose en controle, dat wil zeggen als u wilt wijzigen van de uitvoer van EventFlow, zonder veranderingen in uw werkelijke instrumentation vereist alleen een eenvoudige wijziging aan het configuratiebestand. Als u echter besluit om door te investeren in met behulp van Application Insights en waarschijnlijk niet wijzigen in een ander platform, moet u zoeken naar nieuwe Application Insights-SDK gebruiken voor het aggregeren van gebeurtenissen en deze te verzenden naar Application Insights. Dit betekent dat u niet langer hoeft EventFlow voor het verzenden van uw gegevens naar Application Insights configureren, maar in plaats daarvan de ApplicationInsight van Service Fabric NuGet-pakket wordt geïnstalleerd. Meer informatie over het pakket kunnen worden gevonden [hier](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

[Application Insights-ondersteuning voor Microservices en Containers](https://azure.microsoft.com/blog/app-insights-microservices/) ziet u enkele van de nieuwe functies die zijn wordt gewerkt (momenteel nog steeds in beta), waarmee u kunt uitgebreidere controle out-of-the-box-opties met Application Insights hebt. Hieronder vallen afhankelijkheid bijhouden (gebruikt voor het bouwen van een toepassingstoewijzing van uw services en toepassingen in een cluster en de communicatie ertussen) en betere correlatie van traceringen die afkomstig zijn van uw services (helpt u bij het beter een probleem in de werkstroom van dicht een toepassing of service).

Als u in .NET ontwikkelt en zal waarschijnlijk worden met behulp van enkele van Service Fabric-programmeermodellen en bereid Application Insights gebruiken als uw platform voor het visualiseren en analyseren van de gebeurtenis-en log, klikt u vervolgens raadzaam dat u via de Application Insights gaat SDK-route als de werkstroom voor controle en diagnostische gegevens. Lezen [dit](../application-insights/app-insights-asp-net-more.md) en [dit](../application-insights/app-insights-asp-net-trace-logs.md) aan de slag met het gebruik van Application Insights te verzamelen en weergeven van uw Logboeken.

## <a name="navigating-the-application-insights-resource-in-azure-portal"></a>Navigeren in de Application Insights-resource in Azure portal

Als u Application Insights hebt geconfigureerd als uitvoer voor uw gebeurtenissen en Logboeken, moet informatie worden weergegeven in uw Application Insights-resource in een paar minuten beginnen. Navigeer naar de Application Insights-resource, die u naar de Application Insights-resource-dashboard gaat. Klik op **zoeken** in de taakbalk van Application Insights om te zien van de meest recente traceringen die is ontvangen en kunnen ze filteren.

*Metrics Explorer* een handig hulpmiddel voor het maken van aangepaste dashboards op basis van metrische gegevens die uw toepassingen, services en -cluster kunnen rapporteren. Zie [verkennen van metrische gegevens in Application Insights](../application-insights/app-insights-metrics-explorer.md) voor het instellen van een aantal grafieken voor zelf is gebaseerd op de gegevens die u verzamelt.

Te klikken op **Analytics** gaat u naar de Application Insights Analytics-portal, waar u kunt een query gebeurtenissen en traceringen met groter bereik en optionaliteit. Meer informatie over deze [analyses in Application Insights](../application-insights/app-insights-analytics.md).

## <a name="next-steps"></a>Volgende stappen

* [Stel waarschuwingen in AI](../application-insights/app-insights-alerts.md) om te worden geïnformeerd over wijzigingen in de prestaties of gebruik
* [Slimme detectie in Application Insights](../application-insights/app-insights-proactive-diagnostics.md) voert een proactieve analyse van de telemetrie wordt verzonden naar Application Insights om te waarschuwen voor mogelijke prestatieproblemen
