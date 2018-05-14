---
title: Aangepaste rapporten met gegevens van Azure Application Insights automatiseren
description: Aangepaste wekelijks-dagelijks/maandelijkse rapporten met gegevens van Azure Application Insights automatiseren
services: application-insights
documentationcenter: ''
author: sdash
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: sdash
ms.openlocfilehash: 804e8c7a43d1ab16d11b6075be44599b33b46a3e
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="automate-custom-reports-with-azure-application-insights-data"></a>Aangepaste rapporten met gegevens van Azure Application Insights automatiseren

Periodieke rapporten kunt u ervoor zorgen dat een team op de hoogte hoe hun zakelijke essentiële services doen. Ontwikkelaars, DevOps/SRE teams en hun managers kunnen productief zijn met geautomatiseerde rapporten die inzichten geven een betrouwbare leveren zonder iedereen voor aanmelding bij de portal. Deze rapporten kunnen ook helpen identificeren geleidelijke toename in latentie, waarschuwt laden of fout tarieven die niet voor alle activeren mogelijk regels.

Elke enterprise heeft de unieke rapportagevereisten, zoals: 

* Specifieke percentiel aggregaties van metrische gegevens of aangepaste metrische gegevens in een rapport.
* Andere rapporten voor dagelijkse, wekelijkse en maandelijkse roll-ups van gegevens voor verschillende doelgroepen hebben.
* Segmentering door aangepaste kenmerken, zoals regio, of de omgeving. 
* Sommige AI resources groeperen samen in één rapport, zelfs als ze mogelijk in verschillende abonnementen of de resource groepen enzovoort.
* Afzonderlijke rapporten met gevoelige metrische gegevens die worden verzonden naar selectief doelgroep.
* Rapporten voor belanghebbenden die geen toegang tot de portal resources.

> [!NOTE] 
> Wekelijks Application Insights overzicht via e-mail is niet toegestaan voor elke aanpassing en door de volgende aangepaste opties worden afgebroken. Het laatste wekelijkse digest e-mailbericht wordt verzonden op 11 juni 2018. Configureer een van de volgende opties om op te halen vergelijkbare aangepaste rapporten (Gebruik de query voorgestelde hieronder).

## <a name="to-automate-custom-report-emails"></a>Aangepast rapport e-mailberichten automatiseren

U kunt [programmatisch query Application Insights](https://dev.applicationinsights.io/) gegevens om aangepaste rapporten op basis van een planning te genereren. Aan de hand van de volgende opties kunnen u snel aan de slag:

* [Rapporten voor Microsoft-Flow automatiseren](app-insights-automate-with-flow.md)
* [Rapporten met Logic Apps automatiseren](automate-with-logic-apps.md)
* Gebruik het 'Application Insights geplande digest' [Azure functie](https://azure.microsoft.com/services/functions/) sjabloon in het scenario voor bewaking. Hierbij wordt gebruikgemaakt van SendGrid naar de e-mail te bezorgen. 

    ![De functie Azure-sjabloon](./media/automate-custom-reports/azure-function-template.png)

## <a name="sample-query-for-a-weekly-digest-email"></a>Voorbeeldquery voor een wekelijks overzicht via e-mail
De volgende query ziet over meerdere gegevenssets voor een wekelijks overzicht via e-mail zoals rapport toevoegen. Pas deze zo nodig en deze gebruiken met een van de opties hierboven vermelde een wekelijks rapport automatiseren.   

```AIQL
let period=7d;
requests
| where timestamp > ago(period)
| summarize Row = 1, TotalRequests = sum(itemCount), FailedRequests = sum(toint(success == 'False')),
    RequestsDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
| join (
dependencies
| where timestamp > ago(period)
| summarize Row = 1, TotalDependencies = sum(itemCount), FailedDependencies = sum(success == 'False'),
    DependenciesDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row | join (
pageViews
| where timestamp > ago(period)
| summarize Row = 1, TotalViews = sum(itemCount)
) on Row | join (
exceptions
| where timestamp > ago(period)
| summarize Row = 1, TotalExceptions = sum(itemCount)
) on Row | join (
availabilityResults
| where timestamp > ago(period)
| summarize Row = 1, OverallAvailability = iff(isnan(avg(toint(success))), '------', tostring(toint(avg(toint(success)) * 10000) / 100.0)),
    AvailabilityDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row
| project TotalRequests, FailedRequests, RequestsDuration, TotalDependencies, FailedDependencies, DependenciesDuration, TotalViews, TotalExceptions, OverallAvailability, AvailabilityDuration
```

  
## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het maken van [analysequery's](app-insights-analytics-using.md).
- Meer informatie over [programmatisch opvragen van gegevens van Application Insights](https://dev.applicationinsights.io/)
- Meer informatie over [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).
- Meer informatie over [Microsoft Flow](https://ms.flow.microsoft.com).


