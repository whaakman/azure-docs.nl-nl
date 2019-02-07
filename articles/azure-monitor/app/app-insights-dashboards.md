---
title: Dashboards en navigatie in de Azure Application Insights | Microsoft Docs
description: Weergaven maken van uw belangrijkste APM-grafieken en query's.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 39b0701b-2fec-4683-842a-8a19424f67bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: afbf2bc32aa737eb5f6dde41035b206d6e260252
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55813756"
---
# <a name="navigation-and-dashboards-in-the-application-insights-portal"></a>Navigatie en Dashboards in de Application Insights-portal
Nadat u hebt [Application Insights instellen voor uw project](../../azure-monitor/app/app-insights-overview.md), telemetrische gegevens over de prestaties en het gebruik van uw app wordt weergegeven in Application Insights-resource van uw project in de [Azure-portal](https://portal.azure.com).

## <a name="find-your-telemetry"></a>Uw telemetrie zoeken
Aanmelden bij de [Azure-portal](https://portal.azure.com) en navigeer naar de Application Insights-resource die u hebt gemaakt voor uw app.

![Klik op Bladeren en selecteer Application Insights en vervolgens uw app.](./media/app-insights-dashboards/00-start.png)

De overzichtsblade (pagina) voor uw app bevat een overzicht van de belangrijkste metrische diagnostische gegevens van uw app en is een gateway aan de andere functies van de portal.

![Primaire routes naar uw telemetrie weergeven](./media/app-insights-dashboards/010-oview.png)

U kunt een van de grafieken en rasters aanpassen en vastmaken aan een dashboard. Op die manier, verzamelt u de belangrijkste telemetrie vanuit andere apps in een centraal dashboard.

## <a name="dashboards"></a>Dashboards
Het eerste wat u ziet nadat u zich aanmeldt bij de [Microsoft Azure portal](https://portal.azure.com) is van een dashboard. Hier verzamelt u de grafieken die het belangrijkst voor u voor al uw Azure-resources zijn, waaronder telemetrie van [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md).

![Een aangepast dashboard.](./media/app-insights-dashboards/31.png)

1. **Zoeken naar specifieke hulpmiddelen** , zoals uw app in Application Insights: Gebruik de linkerbalk.
2. **Ga terug naar het huidige dashboard**, of schakel over naar andere recente weergaven: Gebruik de vervolgkeuzelijst bovenaan links.
3. **Overschakelen van dashboards**: Gebruik de vervolgkeuzelijst op de titel van het dashboard
4. **Maken, bewerken en delen van dashboards** in de werkbalk van het dashboard.
5. **Het dashboard bewerken**: Beweeg de muisaanwijzer over een tegel en gebruik vervolgens de bovenste balk wilt verplaatsen, aanpassen of verwijderen.

## <a name="add-to-a-dashboard"></a>Toevoegen aan een dashboard
Als u geïnteresseerd bent in een blade of een set met grafieken die interessants is, kunt u een kopie van het aan het dashboard kunt vastmaken. U ziet het volgende keer dat u er retourneren.

![Beweeg de muisaanwijzer over het om een grafiek vast, en klik vervolgens op '...' in de header.](./media/app-insights-dashboards/33.png)

1. Grafiek vastmaken aan dashboard. Een kopie van de grafiek wordt weergegeven op het dashboard.
2. De hele blade naar het dashboard vastmaken - deze wordt weergegeven op het dashboard als een tegel die u kunt doorklikken.
3. Klik op de linkerbovenhoek om terug te keren naar het huidige dashboard. Vervolgens kunt u de vervolgkeuzelijst om terug te keren naar de huidige weergave.

U ziet dat de grafieken worden gegroepeerd in tegels: een tegel kan meer dan één grafiek bevatten. U de hele tegel vastmaken aan het dashboard.

De grafiek worden automatisch vernieuwd met een frequentie die afhankelijk zijn van het tijdsbereik van de grafiek:

* Tijdsbereik van 1 uur: Vernieuwen om de 5 minuten
* Bereik 1-24 uur tijd: Vernieuwen om de 15 minuten
* Tijdsbereik hierboven 24 uur: (Tijdsbereik) / 60.

### <a name="pin-any-query-in-analytics"></a>Elke query vastmaken in Analytics
U kunt ook [vastmaken Analytics](../../azure-monitor/log-query/get-started-portal.md) grafieken aan een gedeeld dashboard. Hiermee kunt u diagrammen van elke willekeurige query naast de standaard metrische gegevens toevoegen. 

Resultaten worden elk uur automatisch herberekend. Klik op het pictogram Vernieuwen op de grafiek om te berekenen onmiddellijk. (Browser vernieuwen niet berekenen.)

## <a name="adjust-a-tile-on-the-dashboard"></a>Een tegel op het dashboard aanpassen
Zodra een tegel op het dashboard is, kunt u deze kunt aanpassen.

![Beweeg de muisaanwijzer over een grafiek om te bewerken.](./media/app-insights-dashboards/36.png)

1. Een grafiek toevoegen aan de tegel.
2. Stel de metrische gegevens, groeperen op dimensie en stijl (tabel-, grafiek) van een grafiek.
3. Sleep in het diagram om in te zoomen; Klik op de knop ongedaan maken als u wilt herstellen van de timespan; Filtereigenschappen voor de grafieken worden ingesteld op de tegel.
4. Titel van de tegel instellen.

Tegels die zijn vastgemaakt vanuit metric explorer blades zijn opties voor het bewerken van meer dan de tegels die zijn vastgemaakt vanuit een overzichtsblade.

De oorspronkelijke tegel die u hebt vastgemaakt, wordt niet beïnvloed door uw bewerkingen.

## <a name="switch-between-dashboards"></a>Schakelen tussen dashboards
U kunt meer dan één dashboard opslaan en tussen deze twee schakelt. Wanneer u een grafiek of een blade vastmaakt, moeten ze worden toegevoegd aan het huidige dashboard.

![Schakelen tussen dashboards en klikt u op Dashboard opgeslagen dashboard selecteren. Als u wilt maken en opslaan van een nieuw dashboard, klikt u op Nieuw. Als u wilt wijzigen, klikt u op bewerken.](./media/app-insights-dashboards/32.png)

U kunt bijvoorbeeld hebben één dashboard voor het weergeven van volledig scherm in de Teamkamer en een voor algemene ontwikkeling.

Op het dashboard, een blade wordt weergegeven als een tegel: Klik hierop om naar de blade te gaan. Een grafiek repliceert de grafiek in de oorspronkelijke locatie.

![Klik op een tegel om de blade staat te openen](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a>Dashboards delen
Wanneer u een dashboard hebt gemaakt, kunt u deze delen met andere gebruikers.

![Klik in de header dashboard delen](./media/app-insights-dashboards/41.png)

Meer informatie over [rollen en toegangsbeheer](../../azure-monitor/app/resources-roles-access-control.md).

## <a name="create-dashboards-programmatically"></a>Dashboards via een programma maken
U kunt automatiseren dashboard maken met behulp van [Azure Resource Manager](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically) en een eenvoudige JSON-editor.

## <a name="app-navigation"></a>App-navigatie
De overzichtsblade is de gateway naar meer informatie over uw app.

* **Een grafiek of tegel** : klik op een willekeurige tegel of grafiek om meer details over wat er wordt weergegeven.

### <a name="overview-blade-buttons"></a>Overzicht van blade knoppen
![Overzicht van blade bovenste navigatiebalk](./media/app-insights-dashboards/app-overview-top-nav.png)

* [**Metrics Explorer** ](../../azure-monitor/app/metrics-explorer.md) -maken van uw eigen diagrammen van prestaties en het gebruik.
* [**Search** ](../../azure-monitor/app/diagnostic-search.md) - onderzoek specifieke exemplaren van gebeurtenissen, zoals aanvragen, uitzonderingen, of logtraceringen.
* [**Analytics** ](../../azure-monitor/app/analytics.md) -krachtige query's over uw telemetrie.
* **Tijdsbereik** -aanpassen van het bereik dat door de grafieken op de blade weergegeven.
* **Verwijder** -de Application Insights-resource voor deze app verwijderen. U moet ook de Application Insights-pakketten verwijderen uit de app-code, of bewerken de [instrumentatiesleutel](../../azure-monitor/app/create-new-resource.md #copy-the-instrumentation-key) in uw app om telemetrie naar een andere Application Insights-resource te regelen.

### <a name="essentials-tab"></a>Tabblad met essentiële
* [Instrumentatiesleutel](../../azure-monitor/app/create-new-resource.md #copy-the-instrumentation-key) -geeft de bron van deze app.

### <a name="app-navigation-bar"></a>App-navigatiebalk
![Linkernavigatiebalk](./media/app-insights-dashboards/app-left-nav-bar.png)

* **Overzicht** -gaat u terug naar de blade app-overzicht.
* **Activiteitenlogboek** -waarschuwingen en gebeurtenissen van Azure met beheerdersrechten.
* [**Toegangsbeheer** ](../../azure-monitor/app/resources-roles-access-control.md) -toegang bieden voor leden van een team en anderen.
* [**Tags** ](../../azure-resource-manager/resource-group-using-tags.md) -tags gebruiken voor het groeperen van uw app met anderen.

ONDERZOEKEN

* [**Overzicht van de toepassing** ](app-map.md) -actieve toewijzing van de onderdelen van uw toepassing, die is afgeleid van afhankelijkheidsinformatie.
* [**Slimme detectie** ](../../azure-monitor/app/proactive-diagnostics.md) -recente waarschuwingen bekijken.
* [**Live Stream** ](../../azure-monitor/app/live-stream.md) : een vaste set van vrijwel directe metrische gegevens, handig bij het implementeren van een nieuwe build of foutopsporing.
* [**Beschikbaarheid / Webtests** ](../../azure-monitor/app/monitor-web-app-availability.md) -reguliere aanvragen verzenden naar uw web-app uit om de world.*
* [**Fouten, prestaties** ](../../azure-monitor/app/web-monitor-performance.md) -uitzonderingen, foutpercentages en reactietijden voor aanvragen voor uw app en voor verzoeken van uw app naar [afhankelijkheden](../../azure-monitor/app/asp-net-dependencies.md).
* [**Prestaties** ](../../azure-monitor/app/web-monitor-performance.md) -reactietijd, de reactietijden van afhankelijkheden.
* [Servers](../../azure-monitor/app/web-monitor-performance.md) -prestatiemeteritems. Beschikbaar als u [Installeer Status Monitor](../../azure-monitor/app/monitor-performance-live-website-now.md).
* **Browser** -weergave en prestaties van AJAX-pagina. Beschikbaar als u [instrumenteer uw webpagina's](../../azure-monitor/app/javascript.md).
* **Gebruik** -pagina weergeven, gebruikers,- en sessieaantallen. Beschikbaar als u [instrumenteer uw webpagina's](../../azure-monitor/app/javascript.md).

CONFIGUREREN

* **Aan de slag** -inline-zelfstudie.
* **Eigenschappen van** -instrumentatiesleutel, abonnement en resource-id.
* [Waarschuwingen](../../azure-monitor/app/alerts.md) -configuratie van de metrische waarschuwing.
* [Continue export](../../azure-monitor/app/export-telemetry.md) -exporteren van telemetrie naar Azure storage configureren.
* [Prestatietesten](../../azure-monitor/app/monitor-web-app-availability.md#performance-tests) -instellen van een synthetische werklast op uw website.
* [Quotum en prijzen](../../azure-monitor/app/pricing.md) en [opnamesteekproeven](../../azure-monitor/app/sampling.md).
* **API-toegang** -maken [release-aantekeningen](annotations.md) en voor de Data Access-API.
* [**Werkitems** ](../../azure-monitor/app/diagnostic-search.md#create-work-item) -verbinding maken met een werk volgsysteem zodat u fouten maken kunt tijdens de inspectie van telemetrie.

INSTELLINGEN

* [**Hiermee vergrendelt u** ](../../azure-resource-manager/resource-group-lock-resources.md) -Azure-resources vergrendelen
* [**Automatiseringsscript** ](../../azure-monitor/app/powershell.md) -exporteren van een definitie van de Azure-resource, zodat u deze als sjabloon gebruiken kunt om nieuwe resources te maken.


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Volgende stappen

|  |  |
| --- | --- |
| [Metrics explorer](../../azure-monitor/app/metrics-explorer.md)<br/>Metrische gegevens filteren en te segmenteren |![Een voorbeeld van zoekopdracht](./media/app-insights-dashboards/64.png) |
| [Diagnostische gegevens doorzoeken](../../azure-monitor/app/diagnostic-search.md)<br/>Zoeken en gebeurtenissen, gerelateerde gebeurtenissen controleren en maken van bugs |![Een voorbeeld van zoekopdracht](./media/app-insights-dashboards/61.png) |
| [Analytische gegevens](../../azure-monitor/app/analytics.md)<br/>Krachtige querytaal |![Een voorbeeld van zoekopdracht](./media/app-insights-dashboards/63.png) |
