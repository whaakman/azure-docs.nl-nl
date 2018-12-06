---
title: Op basis van een logboek en vooraf samengevoegde metrische gegevens in Azure Application Insights | Microsoft Docs
description: Waarom het gebruik van log gebaseerde versus vooraf samengevoegde metrische gegevens in Azure Application Insights
services: application-insights
keywords: ''
author: vgorbenko
ms.author: vitaly.gorbenko
ms.reviewer: mbullwin
ms.date: 09/18/2018
ms.service: application-insights
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d095be8c5d921001ab4a492d2385938020fa8ce0
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52967211"
---
# <a name="log-based-and-pre-aggregated-metrics-in-application-insights"></a>Op basis van een logboek en vooraf samengevoegde metrische gegevens in Application Insights

In dit artikel wordt uitgelegd dat het verschil tussen "traditionele" Application Insights-metrische gegevens die zijn gebaseerd op Logboeken en vooraf samengevoegde metrische gegevens die zich momenteel in openbare preview. Beide soorten metrische gegevens zijn beschikbaar voor de gebruikers van Application Insights en elk zorgt voor een unieke waarde in de bewaking van status van de toepassing, diagnostische gegevens en analyses. De ontwikkelaars die toepassingen zijn instrumenteren kunnen bepalen welk type metrische gegevens is het beste geschikt voor een bepaald scenario, afhankelijk van de grootte van de toepassing, verwachte hoeveelheid Telemetrie en zakelijke vereisten voor de precisie van de metrische gegevens en waarschuwingen.

## <a name="log-based-metrics"></a>Logboek op basis van metrische gegevens

Tot voor kort, wordt het gegevensmodel van telemetrie in Application Insights voor toepassingsbewaking is uitsluitend gebaseerd op een klein aantal vooraf gedefinieerde typen gebeurtenissen, zoals aanvragen, uitzonderingen, afhankelijkheidsaanroepen, paginaweergaven, enzovoort. Ontwikkelaars kunnen de SDK gebruiken om te verzenden ofwel deze gebeurtenissen handmatig (door code te schrijven die expliciet de SDK roept) of ze kunnen afhankelijk zijn van het automatisch verzamelen van gebeurtenissen van automatische instrumentatie. In beide gevallen moet de Application Insights back-end alle verzamelde gebeurtenissen worden opgeslagen als Logboeken en de Application Insights-blades in Azure portal fungeren als een analytische en diagnostische hulpprogramma voor het visualiseren van gegevens op basis van gebeurtenissen in Logboeken.

Met behulp van Logboeken te bewaren van een volledige set gebeurtenissen kunt voordelige analytische en diagnostische brengen. U kunt bijvoorbeeld een exacte aantal aanvragen voor een bepaalde URL ophalen met het aantal unieke gebruikers die deze aanroepen. Of u krijgt gedetailleerde diagnostische traceringen, inclusief uitzonderingen en afhankelijkheidsaanroepen voor een gebruikerssessie. Met dit soort informatie kunt u inzicht in de status van de toepassing en het gebruik, zodat de tijd die nodig zijn voor het vaststellen van problemen met een app terugdringen aanzienlijk verbeteren. 

Op hetzelfde moment, het verzamelen van gebeurtenissen een compleet pakket mogelijk niet praktisch (of zelfs onmogelijk) voor toepassingen die veel telemetrie te genereren. Voor situaties wanneer het aantal gebeurtenissen te hoog is is, Application Insights implementeert verschillende telemetrie volume vermindering technieken, zoals [steekproeven](https://docs.microsoft.com/azure/application-insights/app-insights-sampling) en [filteren](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling) die Verminder het aantal de gebeurtenissen die worden verzameld en opgeslagen. Helaas, het aantal opgeslagen gebeurtenissen te verlagen ook verlaagt de nauwkeurigheid van de metrische gegevens die op de achtergrond moet aggregaties van de query-tijd van de gebeurtenissen die zijn opgeslagen in Logboeken uitvoeren.

> [!NOTE]
> De metrische gegevens die zijn gebaseerd op het moment dat de query aggregatie van gebeurtenissen en metingen die zijn opgeslagen in de logboeken worden in Application Insights, log op basis van metrische gegevens worden genoemd. Deze metrische gegevens hebben doorgaans meerdere dimensies van de eigenschappen van gebeurtenis, waardoor ze superieure voor analyses, maar de nauwkeurigheid van deze metrische gegevens is negatief worden beïnvloed door sampling en filteren.

## <a name="pre-aggregated-metrics"></a>Vooraf samengevoegde metrische gegevens

Naast het logboek op basis van metrische gegevens verzonden de Application Insights-team in daling van 2018, een openbare preview van metrische gegevens die zijn opgeslagen in een specifieke opslagplaats die is geoptimaliseerd voor tijdreeks. De nieuwe metrische gegevens zijn niet meer opgeslagen als afzonderlijke gebeurtenissen met een groot aantal eigenschappen. In plaats daarvan worden opgeslagen als vooraf samengevoegde tijdreeksen en alleen met belangrijke dimensies. Hierdoor wordt de nieuwe metrische gegevens superieure bij uitvoeren van query's: het ophalen van gegevens veel sneller gebeurt en vereist minder rekenkracht. Dit als gevolg daarvan maakt nieuwe scenario's zoals [bijna realtime waarschuwingen voor de afmetingen van metrische gegevens](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts), responsievere [dashboards](https://docs.microsoft.com/azure/application-insights/app-insights-dashboards), en nog veel meer.

> [!IMPORTANT]
> Zowel, op basis van een logboek en vooraf samengevoegde metrische gegevens worden gecombineerd in Application Insights. Om te onderscheiden van de twee, in de Application Insights UX de vooraf samengevoegde metrische gegevens heten voortaan 'Standaard metrische gegevens (preview)', terwijl de traditionele metrische gegevens van de gebeurtenissen zijn gewijzigd in 'metrische gegevens op basis van logboek'.

De nieuwere SDK's ([Application Insights 2.7](https://www.nuget.org/packages/Microsoft.ApplicationInsights/2.7.2) SDK of hoger voor .NET) vooraf cumulatieve metrische gegevens tijdens de verzameling voordat telemetrie technieken voor het volume worden gehouden, trappen. Dit betekent dat de nauwkeurigheid van de nieuwe metrische gegevens wordt niet beïnvloed door sampling en filteren bij het gebruik van de meest recente Application Insights-SDK's.

Voor de SDK's die vooraf aggregatie (dat wil zeggen, oudere versies van Application Insights-SDK's of voor de browser instrumentation) niet implementeren vult back-end van de Application Insights nog steeds de nieuwe metrische gegevens door samenvoeging van de gebeurtenissen die zijn ontvangen door de toepassing Insights-verzamelingseindpunt voor gebeurtenis. Dit betekent dat als u niet van de lagere hoeveelheid gegevens die via de kabel worden verzonden profiteren, kunt u nog steeds gebruik van de vooraf samengevoegde metrische gegevens en betere prestaties en ondersteuning van de bijna realtime dimensionale waarschuwingen met SDK's die geen ervaring vooraf cumulatieve metrische gegevens tijdens de verzameling.

Het is opgemerkt dat het verzamelingseindpunt van vooraf aggregeert gebeurtenissen voordat opnamesteekproeven, wat dat betekent [opnamesteekproeven](https://docs.microsoft.com/azure/application-insights/app-insights-sampling) wordt nooit impact de nauwkeurigheid van de vooraf samengevoegde metrische gegevens, ongeacht de SDK-versie u gebruiken met uw toepassing.  

## <a name="using-pre-aggregation-with-application-insights-custom-metrics"></a>Met behulp van vooraf aggregatie met aangepaste metrische gegevens van Application Insights

Met aangepaste metrische gegevens kunt u vooraf aggregatie. De twee belangrijkste voordelen zijn de mogelijkheid om te configureren en ontvang een waarschuwing voor een dimensie van een aangepaste meetwaarde en verminderen de hoeveelheid gegevens die vanaf de SDK verzonden naar het verzamelingseindpunt van Application Insights.

Er zijn verschillende [manieren van het verzenden van aangepaste metrische gegevens van de Application Insights SDK](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics). Als uw versie van de SDK biedt de [GetMetric en TrackValue](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#getmetric) methoden, dit is de beste manier verzenden aangepaste metrische gegevens, omdat in dit geval vooraf aggregatie in de SDK gebeurt, niet alleen verminderen de hoeveelheid gegevens die zijn opgeslagen in Azure, maar ook de hoeveelheid gegevens van de SDK verzonden naar Application Insights. Gebruik anders de [trackMetric](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackmetric) methode, die worden vooraf metrische gebeurtenissen verzameld tijdens de opname van gegevens.

## <a name="custom-metrics-dimensions-and-pre-aggregation"></a>Aangepaste metrische gegevens dimensies en vooraf aggregeren

Alle metrische gegevens die u met behulp van verzendt [trackMetric](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackmetric) of [GetMetric en TrackValue](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#getmetric) API-aanroepen worden automatisch opgeslagen in zowel logboeken en metrische gegevens. Terwijl de versie op basis van het logboek van uw aangepaste metrische gegevens van altijd alle dimensies behoudt, wordt het vooraf samengevoegde versie van de metrische gegevens echter standaard met er zijn geen dimensies opgeslagen. U kunt de verzameling van dimensies van aangepaste metrische gegevens inschakelen op de [gebruik en geschatte kosten](https://docs.microsoft.com/azure/application-insights/app-insights-pricing) tabblad checkt u 'Inschakelen waarschuwingen voor de aangepaste metrische dimensies': 

![Gebruik en geschatte kosten](./media/pre-aggregated-metrics-log-metrics/001-cost.png)

## <a name="why-is-collection-of-custom-metrics-dimensions-turned-off-by-default"></a>Waarom is verzameling van aangepaste metrische gegevens dimensies standaard uitgeschakeld?

De verzameling van aangepaste metrische gegevens dimensies is standaard uitgeschakeld omdat het opslaan van aangepaste metrische gegevens met dimensies in de toekomst wordt apart in rekening gebracht van Application Insights, tijdens het opslaan van de niet-dimensionale aangepaste metrische gegevens blijven gratis (tot een quotum) . U kunt meer informatie over de aanstaande prijswijzigingen met een model op onze officiële [pagina met prijzen](https://azure.microsoft.com/pricing/details/monitor/).

## <a name="creating-charts-and-exploring-log-based-and-standard-pre-aggregated-metrics"></a>Diagrammen wilt maken en op basis van een logboek en standard vooraf samengevoegde metrische gegevens verkennen

Azure Monitor Metrics Explorer gebruiken om te grafieken van metrische gegevens over vooraf samengevoegde en op basis van een logboek en aan de auteur van dashboards met grafieken te tekenen. Na het selecteren van de gewenste Application Insights-resource, de kiezer naamruimte gebruiken om over te schakelen tussen standard (preview) en logboekbestanden op basis van metrische gegevens of Selecteer een aangepaste metrische naamruimte:

![Metrische naamruimte](./media/pre-aggregated-metrics-log-metrics/002-metric-namespace.png)

## <a name="next-steps"></a>Volgende stappen

* [In de buurt van real-time waarschuwingen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts)
* [GetMetric en TrackValue](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#getmetric)
