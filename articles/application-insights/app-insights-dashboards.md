---
title: Dashboards en navigatie in de Azure Application Insights | Microsoft Docs
description: Weergaven maken van uw belangrijkste APM-grafieken en query's.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 39b0701b-2fec-4683-842a-8a19424f67bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: a6d3c89f0f99128e2bf754391c79c3d616f0730a
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="navigation-and-dashboards-in-the-application-insights-portal"></a>Navigatie en Dashboards in de Application Insights-portal
Nadat u hebt [Application Insights instellen op uw project](app-insights-overview.md), telemetriegegevens over de prestaties en het gebruik van uw app wordt weergegeven in Application Insights-resource voor uw project in de [Azure-portal](https://portal.azure.com).

## <a name="find-your-telemetry"></a>Uw telemetrie vinden
Aanmelden bij de [Azure-portal](https://portal.azure.com) en navigeer naar de Application Insights-resource die u hebt gemaakt voor uw app.

![Klik op Bladeren en selecteer Application Insights en vervolgens uw app.](./media/app-insights-dashboards/00-start.png)

Overzichtsblade (pagina) voor uw app bevat een overzicht van de belangrijkste metrische diagnostische gegevens van uw app en is een gateway naar de andere functies van de portal.

![Belangrijke routes naar uw telemetrie weergeven](./media/app-insights-dashboards/010-oview.png)

U kunt een van de grafieken en rasters aanpassen en aan een dashboard vastmaken. Op die manier u kunt overbrengen samen de belangrijkste telemetrie van andere apps op een centrale dashboard.

## <a name="dashboards"></a>Dashboards
Het eerste wat u ziet nadat u zich aanmeldt bij de [Microsoft Azure-portal](https://portal.azure.com) wordt een dashboard. Hier kunt u overbrengen samen de diagrammen die zeer belangrijk voor u over alle Azure-resources zijn, met inbegrip van telemetrie van [Azure Application Insights](app-insights-overview.md).

![Een aangepaste dashboard.](./media/app-insights-dashboards/31.png)

1. **Navigeer naar de specifieke resources** zoals uw app in Application Insights: de linkerbalk gebruiken.
2. **Keer terug naar de huidige dashboard**, of schakel over naar andere recente weergaven: Gebruik de vervolgkeuzelijst linksboven.
3. **Overschakelen van dashboards**: Gebruik de vervolgkeuzelijst op de titel van het dashboard
4. **Maken, bewerken en dashboards delen** op de werkbalk van het dashboard.
5. **Het dashboard bewerken**: Beweeg de muisaanwijzer over een tegel en vervolgens met de bovenste balk verplaatsen, aanpassen of verwijderen.

## <a name="add-to-a-dashboard"></a>Toevoegen aan een dashboard
Wanneer u een blade of een set van grafieken die interessants kijkt, kunt u een kopie van het naar het dashboard vastmaken. U ziet het volgende keer dat u terugkeert.

![Beweeg de muisaanwijzer over het om een grafiek, en klik op '...' in de header.](./media/app-insights-dashboards/33.png)

1. Grafiek vastmaken aan dashboard. Een kopie van de grafiek wordt weergegeven op het dashboard.
2. De hele blade naar het dashboard vastmaken - deze wordt weergegeven op het dashboard als tegel die u kunt doorklikken.
3. Klik op de linkerbovenhoek om terug te keren naar het huidige dashboard. Vervolgens kunt u de vervolgkeuzelijst om terug te keren naar de huidige weergave.

U ziet dat de grafieken worden gegroepeerd in tegels: een tegel kan meer dan één grafiek bevatten. U vastmaken de hele tegel aan het dashboard.

De grafiek worden automatisch vernieuwd met een frequentie die afhankelijk zijn van de grafiek tijdsbereik:

* Tijd tot een uur liggen: vernieuwen om de 5 minuten
* Bereik 1-24 uur tijd: vernieuwen om de 15 minuten
* Tijdsbereik hierboven 24 uur: (tijdsbereik) / 60.

### <a name="pin-any-query-in-analytics"></a>Een query in Analytics vastmaken
U kunt ook [Analytics vastmaken](app-insights-analytics-using.md#pin-to-dashboard) grafieken naar een [gedeelde](#share-dashboards-with-your-team) dashboard. Hiermee kunt u diagrammen van elke willekeurige query naast de standaard metrische gegevens toevoegen. 

Resultaten automatisch opnieuw elk uur berekend. Klik op het pictogram Vernieuwen op de grafiek worden onmiddellijk berekend. (Browser vernieuwen wordt niet opnieuw worden berekend.)

## <a name="adjust-a-tile-on-the-dashboard"></a>Een tegel op het dashboard aanpassen
Zodra een tegel op het dashboard, kunt u het kunt aanpassen.

![Beweeg de muisaanwijzer over een grafiek om deze te bewerken.](./media/app-insights-dashboards/36.png)

1. Een grafiek toevoegen aan de tegel.
2. Stel de metriek, groeperen op dimensie en stijl (tabel, grafiek) van een grafiek.
3. Sleep over het diagram om in te zoomen; Klik op de knop ongedaan maken om in te stellen de timespan; Filtereigenschappen voor de grafieken worden ingesteld op de tegel.
4. Titel van de tegel instellen.

Tegels die zijn vastgemaakt vanuit metrische explorer blades hebben opties voor het bewerken van meer dan tegels die zijn vastgemaakt vanuit een overzichtsblade.

De oorspronkelijke tegel die u hebt vastgemaakt wordt niet beïnvloed door uw wijzigingen.

## <a name="switch-between-dashboards"></a>Schakelen tussen dashboards
U kunt meer dan één dashboard opslaan en tussen deze twee schakelt. Wanneer u een grafiek of blade vastmaken, zijn ze toegevoegd aan het huidige dashboard.

![Schakelen tussen dashboards, klik op Dashboard en een opgeslagen dashboard selecteren. Als u wilt maken en opslaan van een nieuw dashboard, klikt u op Nieuw. Als u wilt wijzigen, klikt u op bewerken.](./media/app-insights-dashboards/32.png)

U wellicht bijvoorbeeld één dashboard voor het volledige scherm wordt weergegeven in de ruimte van het team en een voor algemene ontwikkeling.

Op het dashboard, een blade wordt weergegeven als een tegel: Klik hierop om naar de blade te gaan. Een grafiek repliceert de grafiek in de oorspronkelijke locatie.

![Klik op een tegel om de blade vertegenwoordigt te openen](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a>Dashboards delen
Wanneer u een dashboard hebt gemaakt, kunt u het delen met andere gebruikers.

![Klik in de header dashboard delen](./media/app-insights-dashboards/41.png)

Meer informatie over [functies en toegangsbeheer](app-insights-resources-roles-access-control.md).

## <a name="create-dashboards-programmatically"></a>Programmatisch dashboards maken
U kunt automatiseren dashboard maken met behulp van [Azure Resource Manager](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically) en een eenvoudige JSON-editor.

## <a name="app-navigation"></a>App-navigatie
De overzichtsblade is de gateway naar meer informatie over uw app.

* **Een grafiek of tegel** : klik op een tegel of grafiek voor meer details over wat Hiermee wordt weergegeven.

### <a name="overview-blade-buttons"></a>Overzicht blade knoppen
![Overzicht blade bovenste navigatiebalk](./media/app-insights-dashboards/app-overview-top-nav.png)

* [**Metrics Explorer** ](app-insights-metrics-explorer.md) -grafieken van de prestaties en gebruik te maken.
* [**Search** ](app-insights-diagnostic-search.md) - specifieke exemplaren van gebeurtenissen, zoals aanvragen, uitzonderingen, te onderzoeken of meld u traceringen.
* [**Analytics** ](app-insights-analytics.md) -krachtige query's over uw telemetrie.
* **Tijdsbereik** -het bereik dat wordt weergegeven in de grafieken op de blade aanpassen.
* **Verwijder** -verwijderen van de Application Insights-resource voor deze app. U moet ook Verwijder de Application Insights-pakketten van uw app-code of bewerk de [instrumentatiesleutel](app-insights-create-new-resource.md#copy-the-instrumentation-key) in uw app telemetrie aan een andere Application Insights-resource wordt omgeleid.

### <a name="essentials-tab"></a>Tabblad met essentiële gegevens
* [Instrumentatiesleutel](app-insights-create-new-resource.md#copy-the-instrumentation-key) -deze app resource identificeert.
* Prijzen - functies beschikbaar is en stel volume caps maken.

### <a name="app-navigation-bar"></a>App-navigatiebalk
![Linkernavigatiebalk](./media/app-insights-dashboards/app-left-nav-bar.png)

* **Overzicht** -terug naar de blade app overzicht.
* **Activiteitenlogboek** -waarschuwingen en Azure Beheergebeurtenissen.
* [**Toegangsbeheer** ](app-insights-resources-roles-access-control.md) -toegang geven tot teamleden en anderen.
* [**Labels** ](../azure-resource-manager/resource-group-using-tags.md) -codes gebruiken om de groep van uw app met anderen.

ONDERZOEKEN

* [**De toepassingstoewijzing** ](app-insights-app-map.md) -Active kaart en geeft de onderdelen van uw toepassing die is afgeleid van afhankelijkheidsinformatie.
* [**Detectie van smartcard** ](app-insights-proactive-diagnostics.md) -recente prestatiewaarschuwingen weergeven.
* [**Livestream** ](app-insights-live-stream.md) : een vaste set nuttig bij het implementeren van een nieuwe build handomdraai maatstaven of foutopsporing.
* [**Beschikbaarheid / Webtests** ](app-insights-monitor-web-app-availability.md) -reguliere aanvragen verzenden naar uw web-app rond de world.*
* [**Fouten, prestaties** ](app-insights-web-monitor-performance.md) -uitzonderingen, uitvalpercentage en responstijden voor aanvragen voor uw app en voor het aanvragen van uw app [afhankelijkheden](app-insights-asp-net-dependencies.md).
* [**Prestaties** ](app-insights-web-monitor-performance.md) -reactietijd, reactietijden afhankelijkheid.
* [Servers](app-insights-web-monitor-performance.md) -prestatiemeteritems. Beschikbaar als u [Status Monitor installeren](app-insights-monitor-performance-live-website-now.md).
* **Browser** -pagina-weergave en AJAX-prestaties. Beschikbaar als u [instrumenteren uw webpagina's](app-insights-javascript.md).
* **Gebruik** -weergave, gebruiker en sessie aantallen pagina. Beschikbaar als u [instrumenteren uw webpagina's](app-insights-javascript.md).

CONFIGUREREN

* **Aan de slag** -inline-zelfstudie.
* **Eigenschappen** -instrumentatiesleutel, abonnement en de resource-id.
* [Waarschuwingen](app-insights-alerts.md) -metrische configuratie van de waarschuwing.
* [Continue export](app-insights-export-telemetry.md) -exporteren van telemetrie naar Azure-opslag configureren.
* [Prestaties testen](app-insights-monitor-web-app-availability.md#performance-tests) -een synthetische werklast op uw website instellen.
* [Quotum en prijzen](app-insights-pricing.md) en [opname steekproeven](app-insights-sampling.md).
* **API-toegang** -maken [release aantekeningen](app-insights-annotations.md) en voor de Data Access-API.
* [**Werkitems** ](app-insights-diagnostic-search.md#create-work-item) -verbinding maken met een werk traceringssysteem zodat u fouten tijdens de inspectie van telemetrie kunt maken.

INSTELLINGEN

* [**Hiermee vergrendelt u** ](../azure-resource-manager/resource-group-lock-resources.md) -Azure-resources vergrendelen
* [**Automatiseringsscript** ](app-insights-powershell.md) -exporteren van een definitie van de Azure-resource, zodat u deze als sjabloon gebruiken kunt voor het maken van nieuwe resources.


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Volgende stappen

|  |  |
| --- | --- |
| [Metrics explorer](app-insights-metrics-explorer.md)<br/>Metrische gegevens filteren en segment |![Voorbeeld van de zoekopdracht](./media/app-insights-dashboards/64.png) |
| [Diagnostische gegevens doorzoeken](app-insights-diagnostic-search.md)<br/>Zoeken en gebeurtenissen, gerelateerde gebeurtenissen controleren en maken van fouten |![Voorbeeld van de zoekopdracht](./media/app-insights-dashboards/61.png) |
| [Analytische gegevens](app-insights-analytics.md)<br/>Krachtige querytaal |![Voorbeeld van de zoekopdracht](./media/app-insights-dashboards/63.png) |
