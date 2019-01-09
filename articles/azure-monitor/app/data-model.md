---
title: Gegevensmodel voor Azure Application Insights-telemetrie | Microsoft Docs
description: Overzicht van Application Insights data model
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: 749b4077b457eff836ec515f21d97e892e663156
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54120662"
---
# <a name="application-insights-telemetry-data-model"></a>Gegevensmodel voor Application Insights-telemetrie

[Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) verzendt telemetrie van uw webtoepassing met de Azure-portal, zodat u de prestaties en het gebruik van uw toepassing kunt analyseren. De telemetrie-model zijn gestandaardiseerd zodat het is mogelijk om platform en taal-onafhankelijke bewaking te maken. 

Gegevens die zijn verzameld door Application Insights-modellen dit patroon van de uitvoering van typische toepassing:

![Application Insights-toepassingsmodel](./media/data-model/application-insights-data-model.png)

De volgende typen telemetrie worden gebruikt voor het bewaken van de uitvoering van uw app. De volgende drie typen worden doorgaans automatisch verzameld door de Application Insights-SDK van het framework voor webtoepassingen:

* [**Aanvraag** ](data-model-request-telemetry.md) - gegenereerde om aan te melden van een aanvraag die door uw app ontvangen. Bijvoorbeeld, het Application Insights web SDK genereert automatisch een telemetrie-item van de aanvraag voor elke HTTP-aanvraag die uw web-app ontvangt. 

    Een **bewerking** is het aantal threads van de uitvoering waarmee een aanvraag wordt verwerkt. U kunt ook [code schrijven](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest) voor het bewaken van andere typen bewerking, zoals een 'ontwaken"in een web-taak of de functie die periodiek gegevens worden verwerkt.  Elke bewerking heeft een ID. Deze ID die kan worden gebruikt om [groep](../../azure-monitor/app/correlation.md) alle telemetrie die is gegenereerd tijdens uw app de aanvraag wordt verwerkt. Elke bewerking ofwel is geslaagd of mislukt en heeft een tijdsduur.
* [**Uitzondering** ](data-model-exception-telemetry.md) -vertegenwoordigt doorgaans een uitzondering die ervoor zorgt dat een bewerking is mislukt.
* [**Afhankelijkheid** ](data-model-dependency-telemetry.md) -Hiermee geeft u een aanroep vanuit uw app naar een externe service of de opslag, zoals een REST-API of SQL. In ASP.NET, afhankelijkheidsaanroepen naar SQL zijn gedefinieerd door `System.Data`. Aanroepen van HTTP-eindpunten zijn gedefinieerd door `System.Net`. 

Application Insights biedt drie extra gegevenstypen voor aangepaste telemetrie:

* [Tracering](data-model-trace-telemetry.md) - gebruikt rechtstreeks of via een adapter voor het implementeren van logboekregistratie van diagnostische gegevens met behulp van een framework voor instrumentatie dat wordt vertrouwd mee bent, zoals `Log4Net` of `System.Diagnostics`.
* [Gebeurtenis](data-model-event-telemetry.md) : meestal wordt gebruikt om vast te leggen van gebruikersinteractie met de service, om gebruikspatronen te analyseren.
* [Metrische gegevens](data-model-metric-telemetry.md) : wordt gebruikt voor het rapport periodieke scalaire metingen.

Elk telemetrie-item kunt definiëren de [contextinformatie](data-model-context.md) , zoals van toepassing versie of een gebruiker sessie-id. Context is een set van sterk getypeerde velden die beter bepaalde scenario's zicht. Wanneer de toepassingsversie correct is geïnitialiseerd, kan Application Insights nieuwe patronen detecteren in werking van de toepassing verband houden met de opnieuw implementeren. Sessie-id kan worden gebruikt voor het berekenen van de storing of een probleem invloed op gebruikers. Afhankelijkheid tellingen van sessie-id-waarden voor bepaalde berekenen mislukt, fout trace- of kritieke uitzondering biedt een goed begrip van indruk.

Application Insights-telemetrie model definieert de manier waarop [correleren](../../azure-monitor/app/correlation.md) telemetrie naar de bewerking die deel uitmaakt. Bijvoorbeeld een aanvraag voor een SQL-Database aanroepen en diagnostische gegevens vastgelegd. U kunt de correlatie-context voor de telemetrie-items die deze terug naar de aanvraagtelemetrie koppelen instellen.

## <a name="schema-improvements"></a>Schema-verbeteringen

Gegevensmodel voor Application Insights is een eenvoudige en eenvoudige maar krachtige manier om telemetriegegevens van uw toepassing te modelleren. We streven ernaar te houden van het model, eenvoudige en slanke ter ondersteuning van essentiële scenario's en het uitbreiden van het schema voor ervaren gebruikers.

Voor het rapporteren van problemen voor het model of het schema van gegevens en suggesties voor gebruik van GitHub [Application Insights-startpagina](https://github.com/Microsoft/ApplicationInsights-Home/labels/schema) opslagplaats.

## <a name="next-steps"></a>Volgende stappen

- [Schrijf aangepaste telemetrie](../../azure-monitor/app/api-custom-events-metrics.md)
- Meer informatie over het [uitbreiden en telemetrie filteren](../../azure-monitor/app/api-filtering-sampling.md).
- Gebruik [steekproeven](../../azure-monitor/app/sampling.md) om hoeveelheid telemetrie op basis van het gegevensmodel te minimaliseren.
- Bekijk [platforms](../../azure-monitor/app/platforms.md) ondersteund door Application Insights.
