---
title: Azure Web Apps analytische gegevens bekijken | Microsoft Docs
description: U kunt de Azure Web Apps Analytics-oplossing gebruiken om inzicht te krijgen over uw Azure-Web-Apps door het verzamelen van verschillende metrische gegevens over alle resources in uw Azure-Web-App.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: 20ff337f-b1a3-4696-9b5a-d39727a94220
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: magoedte
ms.openlocfilehash: 5426c9c5727d76d401c00b6e7338688b8f064ad0
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37021234"
---
# <a name="view-analytic-data-for-metrics-across-all-your-azure-web-app-resources"></a>Analytische gegevens weergeven voor de metrische gegevens over alle resources in uw Azure-Web-App

![Symbool van Web-Apps](./media/log-analytics-azure-web-apps-analytics/azure-web-apps-analytics-symbol.png)  

> [!NOTE]
> De Azure Web Apps Analytics-oplossing is afgeschaft.  Klanten die de oplossing al hebt geïnstalleerd kunnen blijven gebruiken, maar Azure Web Apps Analytics kan niet worden toegevoegd aan een nieuwe werkruimten.  Voor het bewaken van uw webtoepassing te gebruiken [Application Insights](../application-insights/app-insights-overview.md). 

De oplossing Azure Web Apps Analytics (Preview) biedt inzicht in uw [Azure Web Apps](../app-service/app-service-web-overview.md) door het verzamelen van verschillende metrische gegevens over alle resources in uw Azure-Web-App. U kunt met de oplossing voor analyseren en zoeken naar metrische gegevens van web-app resource.

Met de oplossing, kunt u weergeven de:

- Bovenste Web-Apps met de hoogste reactietijd
- Het aantal aanvragen in uw Web-Apps, met inbegrip van geslaagde en mislukte aanvragen
- Bovenste Web-Apps met de hoogste binnenkomend en uitgaand verkeer
- Bovenste serviceplannen met hoog gebruik van CPU en geheugen
- Azure Web Apps activiteit logboek-bewerkingen

## <a name="connected-sources"></a>Verbonden bronnen

In tegenstelling tot de meeste andere Log Analytics-oplossingen, is niet gegevens voor Azure Web Apps verzameld door agents. Alle gegevens die worden gebruikt door de oplossing wordt geleverd rechtstreeks uit Azure.

| Verbonden bron | Ondersteund | Beschrijving |
| --- | --- | --- |
| [Windows-agents](log-analytics-windows-agent.md) | Nee | De oplossing verzamelt geen informatie van Windows-agents. |
| [Linux-agents](log-analytics-linux-agents.md) | Nee | De oplossing worden geen gegevens verzameld van Linux-agents. |
| [SCOM-beheergroep](log-analytics-om-agents.md) | Nee | De oplossing worden geen gegevens verzameld van agents in een verbonden SCOM-beheergroep. |
| [Azure Storage-account](log-analytics-azure-storage.md) | Nee | De oplossing biedt geen gegevens naar Azure storage. |

## <a name="prerequisites"></a>Vereisten

- Voor toegang tot Azure-Web-App resource metrische gegevens, moet u een Azure-abonnement hebben.

## <a name="configuration"></a>Configuratie

Voer de volgende stappen uit voor het configureren van de Azure Web Apps Analytics-oplossing voor uw werkruimten.

1. [Inschakelen van logboekregistratie van Azure-resource metrische gegevens met behulp van PowerShell-logboekanalyse](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell).

De Azure Web Apps Analytics-oplossing worden verzameld van twee sets van metrische gegevens van Azure:

- Azure Web Apps metrische gegevens
  - Gemiddeld geheugenwerkset
  - Gemiddelde reactietijd
  - Ontvangen/verzonden bytes
  - CPU-tijd
  - Aanvragen
  - Geheugen werkset
  - Httpxxx
- App Service-Plan metrische gegevens
  - Ontvangen/verzonden bytes
  - CPU-percentage
  - Lengte van de wachtrij voor de schijf
  - Lengte van de HTTP-wachtrij
  - Geheugenpercentage

App Service Plan metrische gegevens worden alleen verzameld als u van een specifieke service-abonnement gebruikmaakt. Dit geldt niet voor beschikbare of gedeelde App Service-abonnementen.

Nadat u de oplossing hebt geconfigureerd, moet gegevens eerst naar uw werkruimte stromende binnen 15 minuten.

## <a name="using-the-solution"></a>De oplossing gebruiken

Wanneer u de Azure Web Apps Analytics-oplossing aan uw werkruimte toevoegt de **Azure Web Apps Analytics** tegel is toegevoegd aan uw dashboard overzicht. Deze tegel geeft een telling van het aantal Azure Web Apps dat de oplossing toegang tot in uw Azure-abonnement heeft.

![Azure Web Apps Analytics tegel](./media/log-analytics-azure-web-apps-analytics/azure-web-apps-analytics-tile.png)

### <a name="view-azure-web-apps-analytics-information"></a>Azure Web Apps Analytics informatie weergeven

Klik op de **Azure Web Apps Analytics** tegel openen de **Azure Web Apps Analytics** dashboard. Het dashboard bevat de blades in de volgende tabel. Elke blade bevat maximaal tien items die overeenkomen met de criteria die blade voor het opgegeven bereik en tijdsbereik. U kunt zoeken in logboeken waarmee alle records worden geretourneerd door onderaan in de blade te klikken op **Alles bekijken** of door te klikken op de koptekst van de blade.


| Kolom | Beschrijving |
| --- | --- |
| Azure Webapps |   |
| Web-Apps aanvraag Trends | Toont een lijndiagram van de trend in de Web-Apps-aanvraag voor het datumbereik dat u hebt geselecteerd en bevat een overzicht van de top tien webaanvragen. Klik op het lijndiagram om uit te voeren een zoekopdracht logboek voor <code>AzureMetrics &#124; where ResourceId == "/MICROSOFT.WEB/SITES/" and (MetricName == "Requests" or MetricName startswith_cs "Http") &#124; summarize AggregatedValue = avg(Average) by MetricName, bin(TimeGenerated, 1h)</code> <br>Klik op een item van de aanvraag web om uit te voeren een zoekopdracht logboek voor de web-aanvraag metrische trend die aanvragen. |
| Reactietijd van Web-Apps | Toont een lijndiagram van de reactietijd van de Web-Apps voor het datumbereik dat u hebt geselecteerd. Ook bevat een overzicht van een lijst van de top tien Web Apps antwoord time-out. Klik op de grafiek om uit te voeren een zoekopdracht logboek voor <code>AzureMetrics &#124; where ResourceId == "/MICROSOFT.WEB/SITES/" and MetricName == "AverageResponseTime" &#124; summarize AggregatedValue = avg(Average) by Resource, bin(TimeGenerated, 1h)</code><br> Klik op een Web-App in een logboek zoekopdracht reactietijden retourneren voor de Web-App uitvoeren. |
| Verkeer van Web-Apps | Toont een lijndiagram voor verkeer van de Web-Apps, in MB en geeft een lijst van de bovenste verkeer van de Web-Apps. Klik op de grafiek om uit te voeren een zoekopdracht logboek voor <code>AzureMetrics &#124; where ResourceId == "/MICROSOFT.WEB/SITES/" and (MetricName == "BytesSent" or MetricName == "BytesReceived") &#124; summarize AggregatedValue = sum(Average) by Resource, bin(TimeGenerated, 1h)</code><br> Deze bevat alle Web-Apps met verkeer voor de laatste minuut. Klik op een Web-App als u wilt uitvoeren van een logboek zoekopdracht bytes ontvangen en verzonden voor de Web-App wordt weergegeven. |
| Azure App Service-plannen |   |
| App Service-plannen met CPU-gebruik &gt; 80% | Geeft het totale aantal App Service-abonnementen die groter is dan 80% CPU-gebruik hebben en een lijst met de top 10-App Service-plannen door CPU-gebruik. Klik op het totale aantal gebied als u wilt uitvoeren van een logboek zoekt <code>AzureMetrics &#124; where ResourceId == "/MICROSOFT.WEB/SERVERFARMS/" and MetricName == "CpuPercentage" &#124; summarize AggregatedValue = avg(Average) by Resource</code><br> Het bevat een overzicht van uw App Service-abonnementen en het gemiddelde CPU-gebruik. Klik op een App Service-Plan om uit te voeren een zoekopdracht in de logboekbestanden met het gemiddelde CPU-gebruik. |
| App Service-plannen met geheugengebruik &gt; 80% | Geeft het totale aantal App Service-abonnementen die groter is dan 80% geheugengebruik hebben en een lijst met de top 10-App Service-plannen door geheugengebruik. Klik op het totale aantal gebied als u wilt uitvoeren van een logboek zoekt <code>AzureMetrics &#124; where ResourceId == "/MICROSOFT.WEB/SERVERFARMS/" and MetricName == "MemoryPercentage" &#124; summarize AggregatedValue = avg(Average) by Resource</code><br> Het bevat een overzicht van uw App Service-abonnementen en hun gemiddelde geheugengebruik. Klik op een App Service-Plan om uit te voeren een zoekopdracht in de logboekbestanden met het gemiddelde geheugengebruik. |
| Activiteitenlogboeken Azure-Web-Apps |   |
| Azure-Web-Apps activiteit Audit | Geeft het totale aantal Web-Apps met [activiteitenlogboeken](log-analytics-activity.md) en vindt u de top 10 activiteit logboek-bewerkingen. Klik op het totale aantal gebied als u wilt uitvoeren van een logboek zoekt <code>AzureActivity #124; where ResourceProvider == "Azure Web Sites" #124; summarize AggregatedValue = count() by OperationName</code><br> Het bevat een overzicht van de activiteit logboek-bewerkingen. Klik op een activiteit log-bewerking voor het uitvoeren van een zoekopdracht logboek met een lijst met de records voor de bewerking. |



### <a name="azure-web-apps"></a>Azure Web Apps

In het dashboard kunt u inzoomen voor meer inzicht verkrijgen in de metrische gegevens van uw Web-Apps. Dit eerst een set blades tonen de trend van de Web-Apps-aanvragen, het aantal fouten (bijvoorbeeld HTTP404), verkeer en gemiddelde reactietijd gedurende een bepaalde periode. U ziet ook een verdeling van deze metrische gegevens voor verschillende Web-Apps.

![Azure Webapps blades](./media/log-analytics-azure-web-apps-analytics/web-apps-dash01.png)

Een primaire reden voor het weergeven van die gegevens is zodat u kunt een Web-App met hoge responstijd identificeren en onderzoek de hoofdoorzaak. De limiet van een drempelwaarde wordt ook toegepast zodat u gemakkelijk herkennen die met problemen.

- Web-Apps in rood weergegeven hebben reactietijd hoger is dan 1 seconde.
- Web-Apps weergegeven in oranje hebben een reactietijd hoger is dan 0,7 seconde en minder dan 1 seconde.
- Web-Apps in het groen weergegeven hebben een reactietijd kleiner is dan 0,7 tweede.

In de volgende logboek zoeken voorbeeldafbeelding, kunt u zien dat de *anugup3* web-app heeft een veel hoger reactietijd dan de web-apps.

![voorbeeld van zoeken in logboek](./media/log-analytics-azure-web-apps-analytics/web-app-search-example.png)

### <a name="app-service-plans"></a>App Service-abonnementen

Als u speciale serviceplannen gebruikt, kunt u ook metrische gegevens verzamelen voor uw App Service-abonnementen. In deze weergave ziet u uw App Service-abonnementen met hoog CPU of geheugen gebruik (&gt; 80%). Ook ziet u de eerste App-services met hoge geheugen of CPU-gebruik. Een drempellimiet wordt op dezelfde manier toegepast zodat u gemakkelijk herkennen die met problemen.

- App Service-abonnementen in rood weergegeven, hebt een CPU/geheugengebruik hoger is dan 80%.
- App Service-abonnementen weergegeven in oranje hebben een CPU/geheugengebruik hoger is dan 60% en lager is dan 80%.
- App Service-abonnementen in het groen weergegeven, hebt een lager is dan 60% CPU/geheugen-gebruik.

![Azure App Service-abonnementen blades](./media/log-analytics-azure-web-apps-analytics/web-apps-dash02.png)

## <a name="azure-web-apps-log-searches"></a>Azure Web Apps logboek zoekopdrachten

De **lijst van populaire Azure Web Apps zoekquery's** ziet u alle gerelateerde activiteitenlogboeken voor Web-Apps, waarmee u inzicht in de bewerkingen die zijn uitgevoerd op de resources van uw Web-Apps. Het bevat ook alle gerelateerde bewerkingen en het aantal keren dat ze zich hebben voorgedaan.

Met een van de logboekbestanden zoekquery's als uitgangspunt, kunt u gemakkelijk maken een waarschuwing. U wilt maken van een waarschuwing als de gemiddelde reactietijd van een waarde groter dan elke 1 seconde is.

## <a name="next-steps"></a>Volgende stappen

- Maak een [waarschuwing](log-analytics-alerts-creating.md) voor specifieke metrische gegevens.
- Gebruik [logboek zoeken](log-analytics-log-searches.md) om gedetailleerde gegevens uit uw activiteitenlogboeken weer te geven.
