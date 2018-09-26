---
title: Analytics - de krachtige zoek- en query-hulpprogramma van Azure Application Insights | Microsoft Docs
description: 'Overzicht van analyse, het hulpprogramma krachtige diagnostische gegevens doorzoeken van Application Insights. '
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 0a2f6011-5bcf-47b7-8450-40f284274b24
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 02/08/2018
ms.author: mbullwin
ms.openlocfilehash: 6db98332fc7d896613a3318421e9a96bbb50cd15
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47159139"
---
# <a name="analytics-in-application-insights"></a>Analyses in Application Insights
Analytics is het krachtig hulpprogramma voor zoekopdrachten en query's van [Application Insights](app-insights-overview.md). Analytics is een web-hulpprogramma, zodat er geen installatie vereist is. Als u Application Insights al hebt geconfigureerd voor een van uw apps vervolgens u de gegevens van uw app analyseren kunt door te Analytics openen vanuit uw app [overzichtsblade](app-insights-dashboards.md).

![Portal.azure.com Open, open uw Application Insights-resource en klik op Analytics.](./media/app-insights-analytics/001.png)

U kunt ook de [Analytics Speelplaats](https://go.microsoft.com/fwlink/?linkid=859557) dit is een gratis demo-omgeving met een groot aantal voorbeeldgegevens.
<br>
<br>
> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

## <a name="query-data-in-analytics"></a>Query uitvoeren op gegevens in Analytics
Een typische query begint met een tabelnaam wordt opgegeven gevolgd door een reeks *operators* gescheiden door `|`.
Bijvoorbeeld, we willen weten hoeveel aanvragen onze app tijdens de afgelopen 3 uur uit verschillende landen, ontvangen:
```AIQL
requests
| where timestamp > ago(3h)
| summarize count() by client_CountryOrRegion
| render piechart
```

We beginnen met de naam van de tabel *aanvragen* en toe te voegen elementen doorgesluisd naar behoefte.  Eerst definiëren we een tijdfilter om te controleren van alleen de records van de afgelopen 3 uur.
We telt het aantal records per land (of gegevens die worden gevonden in de kolom *client_CountryOrRegion*). We weergegeven ten slotte de resultaten in een cirkeldiagram.
<br>

![Queryresultaten](./media/app-insights-analytics/030.png)

De taal heeft veel voordelen bieden:

* [Filter](/azure/kusto/query/whereoperator) uw onbewerkte app-telemetrie met velden, met inbegrip van uw aangepaste eigenschappen en metrische gegevens.
* [Deelnemen aan](/azure/kusto/query/joinoperator) meerdere tabellen – te correleren met paginaweergaven, afhankelijkheidsaanroepen, uitzonderingen en logboektraceringen aanvragen.
* Krachtige statistische [aggregaties](/azure/kusto/query/summarizeoperator).
* Direct en krachtige visualisaties.
* [REST-API](https://dev.applicationinsights.io/) waarmee u kunt query's uitvoeren via een programma, bijvoorbeeld vanuit PowerShell.

De [Naslaggids voor volledige](https://go.microsoft.com/fwlink/?linkid=856079) details van elke opdracht die wordt ondersteund en regelmatig bijgewerkt.

## <a name="next-steps"></a>Volgende stappen
* Aan de slag met de [Analytics-portal](https://go.microsoft.com/fwlink/?linkid=856587)
* Aan de slag [schrijven van query's](https://go.microsoft.com/fwlink/?linkid=856078)
* Controleer de [overzichtskaart van SQL-gebruikers](https://aka.ms/sql-analytics) voor vertalingen van de meest voorkomende idioms.
* Test drive Analytics op onze [Speelplaats](https://analytics.applicationinsights.io/demo) als uw app wordt niet van gegevens naar Application Insights nog verzenden.
* Bekijk de [inleidende video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4).