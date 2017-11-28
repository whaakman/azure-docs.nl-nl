---
title: Analytics - krachtige zoeken en queryprogramma van Azure Application Insights | Microsoft Docs
description: 'Overzicht van Analytics, het hulpprogramma krachtige diagnostische gegevens doorzoeken van de Application Insights. '
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 0a2f6011-5bcf-47b7-8450-40f284274b24
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: mbullwin
ms.openlocfilehash: adda6335b702470cd491f07d750236c368325a9e
ms.sourcegitcommit: 21a58a43ceceaefb4cd46c29180a629429bfcf76
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2017
---
# <a name="analytics-in-application-insights"></a>Analytics in Application Insights
Analytics is de krachtig hulpprogramma voor de zoekopdracht en van [Application Insights](app-insights-overview.md). Analytics is een hulpprogramma web zodat er zijn geen instellingen vereist. Als u Application Insights al hebt geconfigureerd voor een van uw apps, dan u uw app-gegevens analyseren kunt door het openen van Analytics van uw app [overzichtsblade](app-insights-dashboards.md).

![Open portal.azure.com open uw Application Insights-resource en klik op Analytics.](./media/app-insights-analytics/001.png)

U kunt ook de [Analytics playground](https://go.microsoft.com/fwlink/?linkid=859557) dit is een vrije demo-omgeving met veel voorbeeldgegevens.
<br>
<br>
> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

## <a name="query-data-in-analytics"></a>Opvragen van gegevens in Analytics
Een typische query begint met een tabelnaam gevolgd door een reeks *operators* gescheiden door `|`.
Bijvoorbeeld, we willen weten hoeveel aanvragen onze app tijdens de afgelopen drie uur uit verschillende landen ontvangen:
```AIQL
requests
| where timestamp > ago(3h)
| summarize count() by client_CountryOrRegion
| render piechart
```

We beginnen met de naam van de tabel *aanvragen* en Voeg elementen doorgesluisd naar behoefte.  Eerst definiëren we een tijdfilter voor alleen records uit de laatste drie uur.
We telt het aantal records per land (of gegevens die worden gevonden in de kolom *client_CountryOrRegion*). Ten slotte weer we de resultaten in een cirkeldiagram.
<br>

![Queryresultaten](./media/app-insights-analytics/030.png)

De taal heeft veel voordelen bieden:

* [Filter](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) uw app onbewerkte telemetrie op alle velden, met inbegrip van uw aangepaste eigenschappen en metrische gegevens.
* [Deelnemen aan](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) meerdere tabellen – correleer aanvragen met paginaweergaven, afhankelijkheidsaanroepen, uitzonderingen en logboektraceringen.
* Krachtige statistische [aggregaties](https://docs.loganalytics.io/docs/Learn/Tutorials/Aggregation-functions).
* Onmiddellijke en krachtige visualisaties.
* [REST-API](https://dev.applicationinsights.io/) waarmee u kunt query's uitvoeren via een programma, bijvoorbeeld via PowerShell.

De [Naslaggids voor volledige](https://go.microsoft.com/fwlink/?linkid=856079) gegevens van elke opdracht ondersteund en worden regelmatig updates.

## <a name="next-steps"></a>Volgende stappen
* Aan de slag met de [Analytics-portal](https://go.microsoft.com/fwlink/?linkid=856587)
* Aan de slag [schrijven van query's](https://go.microsoft.com/fwlink/?linkid=856078)
* Controleer de [SQL-gebruikers cheats blad](https://aka.ms/sql-analytics) voor vertalingen van de meest voorkomende idioms.
* Uitprobeert Analytics op onze [playground](https://analytics.applicationinsights.io/demo) als uw app wordt niet van gegevens naar Application Insights nog verzenden.
* Bekijk de [Introductievideo](https://applicationanalytics-media.azureedge.net/home_page_video.mp4).