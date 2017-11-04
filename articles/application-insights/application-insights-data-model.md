---
title: Azure Application Insights telemetrie-gegevensmodel | Microsoft Docs
description: Overzicht beveiligingsmodel Application Insights-gegevens
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: mbullwin
ms.openlocfilehash: b14eea46e773a4b92ba20cd3121cd258f86307c9
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="application-insights-telemetry-data-model"></a>Application Insights telemetrie-gegevensmodel

[Azure Application Insights](app-insights-overview.md) verzendt telemetrie van uw webtoepassing bij de Azure portal, zodat u kunt de prestaties en het gebruik van uw toepassing analyseren. Het model telemetrie is gestandaardiseerd zodat het mogelijk te maken van platform en taalonafhankelijke bewaking. 

Gegevens die worden verzameld door Application Insights modellen dit patroon van een typische toepassing kan worden uitgevoerd:

![Application Insights-toepassingsmodel](./media/application-insights-data-model/application-insights-data-model.png)

De volgende typen telemetrie worden gebruikt voor het bewaken van de uitvoering van uw app. De volgende drie typen worden doorgaans automatisch verzameld door de Application Insights-SDK van web application framework:

* [**Aanvraag** ](application-insights-data-model-request-telemetry.md) - gegenereerde aan te melden van een aanvraag ontvangen door uw app. Bijvoorbeeld, het Application Insights web SDK genereert automatisch een aanvraag telemetrie-item voor elke HTTP-aanvraag die uw web-app ontvangt. 

    Een **bewerking** is de threads van uitvoering die een aanvraag wordt verwerkt. U kunt ook [code schrijven](app-insights-api-custom-events-metrics.md#trackrequest) voor het bewaken van andere typen bewerking, zoals een 'ontwaken' in een web-taak of de functie die regelmatig gegevens verwerkt.  Een id heeft. elke bewerking Deze ID die kan worden gebruikt voor [groep](application-insights-correlation.md) alle telemetrie gegenereerd tijdens de aanvraag wordt verwerkt door uw app. Elke bewerking ofwel slaagt of mislukt en heeft een tijdsduur.
* [**Uitzondering** ](application-insights-data-model-exception-telemetry.md) -vertegenwoordigt doorgaans een uitzondering dat ervoor zorgt dat een bewerking mislukt.
* [**Afhankelijkheid** ](application-insights-data-model-dependency-telemetry.md) -vertegenwoordigt een aanroep van uw app met een externe service of de opslag, zoals een REST-API of de SQL. In ASP.NET, afhankelijkheidsaanroepen naar SQL worden gedefinieerd door `System.Data`. Aanroepen naar HTTP-eindpunten zijn gedefinieerd door `System.Net`. 

Application Insights biedt drie extra gegevenstypen voor aangepaste telemetrie:

* [Tracering](application-insights-data-model-trace-telemetry.md) - gebruikt hetzij rechtstreeks of via een adapter voor het implementeren van logboekregistratie van diagnostische gegevens met behulp van een instrumentation framework dat u vertrouwd voorkomen, zoals is `Log4Net` of `System.Diagnostics`.
* [Gebeurtenis](application-insights-data-model-event-telemetry.md) - worden meestal gebruikt voor het vastleggen van gebruikersinteractie met uw service, om gebruikspatronen te analyseren.
* [Metriek](application-insights-data-model-metric-telemetry.md) : wordt gebruikt voor het rapport periodieke scalaire metingen.

Elk telemetrie-item kunt definiëren de [contextgegevens](application-insights-data-model-context.md) toepassing versie of gebruiker sessie-id. Context is een reeks sterk getypeerde velden die blokkering bepaalde scenario's opgeheven. Wanneer de toepassingsversie correct is geïnitialiseerd, kan Application Insights nieuwe patronen in het gedrag van toepassingen opnieuw implementeren gaande gecorreleerd detecteren. Sessie-id kan worden gebruikt om de onderbreking of een probleem gevolgen voor gebruikers te berekenen. Afhankelijkheid unieke tellingen van sessie-id-waarden voor bepaalde berekenen mislukt, fouttracering of kritieke uitzondering biedt een goed inzicht botsingen.

Application Insights telemetrie model definieert de manier waarop [correleren](application-insights-correlation.md) telemetrie voor de werking hiervan deel uitmaakt. Bijvoorbeeld, een aanvraag kunt u een SQL-Database-aanroepen en de geregistreerde diagnostische gegevens. U kunt de correlatie-context voor de telemetrie-items die deze aan de aanvraagtelemetrie opnieuw koppelen instellen.

## <a name="schema-improvements"></a>Schema-verbeteringen

Application Insights-gegevensmodel is een eenvoudige en eenvoudige maar krachtige manier om te modelleren telemetrie van uw toepassing. Streven we ernaar te houden van het model, eenvoudig en dunne voor essentiële scenario's en toestaan dat het schema voor ervaren gebruikers uit te breiden.

Voor het rapporteren van problemen voor het model of het schema van gegevens en suggesties gebruiken GitHub [ApplicationInsights-startpagina](https://github.com/Microsoft/ApplicationInsights-Home/labels/schema) opslagplaats.

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste telemetrie schrijven](app-insights-api-custom-events-metrics.md)
- Meer informatie over hoe [uitbreiden en het filteren van telemetrie](app-insights-api-filtering-sampling.md).
- Gebruik [steekproeven](app-insights-sampling.md) om hoeveelheid telemetrie op basis van het gegevensmodel te minimaliseren.
- Bekijk [platforms](app-insights-platforms.md) ondersteund door de Application Insights.
