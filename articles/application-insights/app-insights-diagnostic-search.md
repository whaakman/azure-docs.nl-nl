---
title: Met de zoekfunctie in Azure Application Insights | Microsoft Docs
description: Zoeken en filteren onbewerkte telemetrie verzonden door uw web-app.
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 2a437555-8043-45ec-937a-225c9bf0066b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: e2d12f807756b778a64920b12a66fba184a99844
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="using-search-in-application-insights"></a>Met behulp van zoeken in Application Insights
Search is een functie van [Application Insights](app-insights-overview.md) waarmee u kunt vinden en verkennen afzonderlijke telemetrie-items, zoals uitzonderingen, paginaweergaven of web-aanvragen. En u kunt bekijken logboektraceringen en gebeurtenissen die u hebt gecodeerd.

(Voor complexere query's over uw gegevens, gebruikt u [Analytics](app-insights-analytics-tour.md).)

## <a name="where-do-you-see-search"></a>Waar ziet u zoeken?
### <a name="in-the-azure-portal"></a>In de Azure portal
U kunt diagnostische gegevens doorzoeken expliciet openen vanuit de Application Insights-overzichtsblade van uw toepassing:

![Open diagnostische gegevens doorzoeken](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)

Ook wordt geopend wanneer u klikt op via bepaalde grafieken en raster artikelen. In dit geval zijn de filters vooraf ingesteld te concentreren op het type item dat u hebt geselecteerd. 

Bijvoorbeeld: op de overzichtsblade er is een staafdiagram van aanvragen die zijn geclassificeerd door reactietijd. Klik op door een prestatiebereik voor een overzicht van afzonderlijke aanvragen in die periode antwoord:

![Klik in de aanvraag-prestaties](./media/app-insights-diagnostic-search/07-open-from-filters.png)

De hoofdtekst van Diagnostic Search is een lijst van telemetrie-items - serveraanvragen pagina weergaven en aangepaste gebeurtenissen die u hebt gecodeerd. Boven aan de lijst is een overzichtstabel weergegeven aantal gebeurtenissen gedurende een bepaalde periode.

Klik op Vernieuwen om nieuwe gebeurtenissen.

### <a name="in-visual-studio"></a>In Visual Studio

In Visual Studio is er ook een venster Application Insights zoeken. Dit is vooral handig voor het weergeven van telemetriegebeurtenissen die worden gegenereerd door de toepassing die u fouten opspoort. Maar kan ook worden weergegeven waarop de gebeurtenissen verzameld van uw gepubliceerde app op de Azure-portal.

Het venster zoeken openen in Visual Studio:

![Visual Studio Application Insights zoeken openen](./media/app-insights-diagnostic-search/32.png)

Het venster zoeken bevat functies die vergelijkbaar is met de web-portal:

![Venster van Visual Studio Application Insights zoeken](./media/app-insights-diagnostic-search/34.png)

Het tabblad bijhouden bewerking is alleen beschikbaar wanneer u een aanvraag of een paginaweergave opent. Een ' bewerking ' is een reeks gebeurtenissen die is gekoppeld aan op een afzonderlijke weergave van de aanvraag of pagina. Bijvoorbeeld, afhankelijkheidsaanroepen, uitzonderingen, traceerlogboeken en aangepaste gebeurtenissen mogelijk deel uit van een enkele bewerking. Het tabblad bijhouden bewerking worden grafisch weergegeven de timing en duur van deze gebeurtenissen ten opzichte van de weergave van de aanvraag of pagina. 

## <a name="inspect-individual-items"></a>Afzonderlijke items te inspecteren
Selecteer een telemetrie-item voor een overzicht van de velden voor sleutels en verwante items. Als u zien van de volledige set van velden wilt, klikt u op '...'. 

![Klik op Nieuw werkitem, de velden bewerken en klik op OK.](./media/app-insights-diagnostic-search/10-detail.png)

## <a name="filter-event-types"></a>Typen gebeurtenissen filteren
Open de blade filteren en kiest u de typen gebeurtenissen die u wilt zien. (Als u later herstellen van de filters waarmee u de blade geopend wilt, klikt u op opnieuw instellen.)

![Filter kiest en selecteert u de typen telemetrie](./media/app-insights-diagnostic-search/02-filter-req.png)

De gebeurtenistypen zijn:

* **Tracering** - [diagnostische logboeken](app-insights-asp-net-trace-logs.md) inclusief TrackTrace, log4Net, NLog en System.Diagnostic.Trace aanroepen.
* **Aanvraag** -HTTP-aanvragen ontvangen door de server-toepassing, met inbegrip van pagina's, scripts, afbeeldingen, Stijlbestanden en gegevens. Deze gebeurtenissen worden gebruikt voor het maken van de aanvraag en antwoord overzichtsgrafieken.
* **Paginaweergave** - [telemetrie verzonden door de webclient](app-insights-javascript.md)pagina view-rapporten maken die worden gebruikt. 
* **Aangepaste gebeurtenis** : als u het aanroepen van TrackEvent() om ingevoegd [gebruik controleren](app-insights-api-custom-events-metrics.md), kunt u deze hier zoeken.
* **Uitzondering** : niet-onderschepte [uitzonderingen in de server](app-insights-asp-net-exceptions.md), die u zich aanmeldt met TrackException() en.
* **Afhankelijkheid** - [aanroepen vanuit de servertoepassing](app-insights-asp-net-dependencies.md) met andere services zoals REST-API's of databases en AJAX-aanroepen van uw [clientcode](app-insights-javascript.md).
* **Beschikbaarheid** -resultaten van [beschikbaarheidstests](app-insights-monitor-web-app-availability.md).

## <a name="filter-on-property-values"></a>Filteren op eigenschapswaarden
Gebeurtenissen van de waarden van de eigenschappen kunt u filteren. De beschikbare eigenschappen, is afhankelijk van de typen gebeurtenissen die u hebt geselecteerd. 

Bijvoorbeeld aanvragen met een specifieke antwoordcode pikken. 

![Vouw een eigenschap en een waarde kiezen](./media/app-insights-diagnostic-search/03-response500.png)

Er zijn geen waarden van een bepaalde eigenschap kiezen heeft hetzelfde effect als het kiezen van alle waarden. Het verandert filter voor die eigenschap uit.

### <a name="narrow-your-search"></a>Beperk uw zoekopdracht
U ziet dat het aantal rechts van de filterwaarden hoeveel exemplaren er in de huidige set gefilterde worden weergegeven. 

In dit voorbeeld is duidelijk dat de 'rapportkoptekst/werknemers' resultaten in de meeste van de '500' fouten aanvragen:

![Vouw een eigenschap en een waarde kiezen](./media/app-insights-diagnostic-search/04-failingReq.png)




## <a name="find-events-with-the-same-property"></a>Gebeurtenissen met dezelfde eigenschap vinden
Alle items met dezelfde waarde voor de eigenschap zoeken:

![Met de rechtermuisknop op een eigenschap](./media/app-insights-diagnostic-search/12-samevalue.png)


## <a name="search-the-data"></a>Gegevens op te zoeken

> [!NOTE]
> Voor het schrijven van complexe query's, open [ **Analytics** ](app-insights-analytics-tour.md) vanaf de bovenkant van de Search-blade.
> 

U kunt zoeken naar de voorwaarden in een van de eigenschapswaarden. Dit is vooral handig als u hebt geschreven [aangepaste gebeurtenissen](app-insights-api-custom-events-metrics.md) met eigenschapswaarden. 

Het is raadzaam een bereik, zoekopdrachten over een kortere bereik zijn sneller tijd in te stellen. 

![Open diagnostische gegevens doorzoeken](./media/app-insights-diagnostic-search/appinsights-311search.png)

Zoeken naar volledige woorden, niet subtekenreeksen. Aanhalingstekens moet u speciale tekens.

| Tekenreeks | is *niet* gevonden door | maar deze worden gevonden |
| --- | --- | --- |
| HomeController.About |thuis<br/>Domeincontroller<br/>out | homecontroller<br/>over<br/>'homecontroller.about'|
|Verenigde Staten|UNI<br/>Ted|Verenigd<br/>statussen<br/>Verenigde Staten en<br/>'Verenigde Staten'

Hier volgen de search-expressies die u kunt gebruiken:

| Voorbeeldquery | Effect |
| --- | --- |
| `apple` |Alle gebeurtenissen in het tijdsbereik waarvan de velden het woord "apple bevatten" zoeken |
| `apple AND banana` |Zoeken naar gebeurtenissen die beide woorden bevatten. Gebruik kapitaal 'en', niet 'en'. |
| `apple OR banana`<br/>`apple banana` |Gebeurtenissen die beide woord bevatten zoeken. Gebruik 'Of', niet "of".<br/>Korte vorm. |
| `apple NOT banana` |Gebeurtenissen die bevatten van één woord en niet op andere zoeken. |



## <a name="sampling"></a>Steekproeven
Als uw app veel telemetrie genereert (en u gebruikt de ASP.NET-SDK-versie 2.0.0-beta3 of hoger), de module adaptieve steekproeven beperkt automatisch het volume dat wordt verzonden naar de portal door te sturen alleen een representatieve fractie van gebeurtenissen. Gebeurtenissen die betrekking op dezelfde aanvraag hebben kunnen echter zijn geselecteerd of gedeselecteerd als groep, zodat u tussen gerelateerde gebeurtenissen kunt navigeren. 

[Meer informatie over steekproeven](app-insights-sampling.md).



## <a name="create-work-item"></a>Werkitem maken
U kunt een bug in GitHub of Visual Studio Team Services maken met de details van elk telemetrie-item. 

![Klik op Nieuw werkitem, de velden bewerken en klik op OK.](./media/app-insights-diagnostic-search/42.png)

De eerste keer dat u dit doet, wordt u gevraagd een koppeling naar uw Team Services-account en het project te configureren.

![Vult u de URL van uw Team Services-server en de naam van het Project en klik op autoriseren](./media/app-insights-diagnostic-search/41.png)

(U kunt ook de koppeling configureren op de blade werkitems.)

## <a name="save-your-search"></a>Uw zoekopdracht opslaan
Als u de gewenste filters hebt ingesteld, kunt u de zoekopdracht als favoriet opslaan. Als u in een organisatie-account werkt, kunt u kiezen of u wilt delen met andere teamleden.

![Klik op favoriet, instellen van de naam en klik op Opslaan](./media/app-insights-diagnostic-search/08-favorite-save.png)

Om te zien van de zoekopdracht opnieuw **gaat u naar de overzichtsblade** en Favorieten openen:

![Tegel Favorieten](./media/app-insights-diagnostic-search/09-favorite-get.png)

Als u met relatief tijdsbereik opgeslagen, heeft de heropend blade de meest recente gegevens. Als u met absoluut tijdsbereik opgeslagen, ziet u dezelfde gegevens elke keer. (Als 'Relatieve' niet beschikbaar wanneer u wilt een favoriet opslaan, tijdsbereik in de header op en een periode op die een aangepast bereik niet instellen.)

## <a name="send-more-telemetry-to-application-insights"></a>Meer telemetrie verzenden naar Application Insights
Naast de out-of-the-box-telemetrie verzonden door de Application Insights-SDK, kunt u het volgende doen:

* Logboektraceringen van uw favoriete framework voor logboekregistratie in vastleggen [.NET](app-insights-asp-net-trace-logs.md) of [Java](app-insights-java-trace-logs.md). Dit betekent dat u kunt uw logboektraceringen doorzoeken en ze te correleren met paginaweergaven, uitzonderingen en andere gebeurtenissen. 
* [Schrijven van code](app-insights-api-custom-events-metrics.md) om aangepaste gebeurtenissen, paginaweergaven en uitzonderingen te verzenden. 

[Ontdek hoe logboeken en aangepaste telemetrie verzendt naar Application Insights](app-insights-asp-net-trace-logs.md).

## <a name="questions"></a>MET Q & A
### <a name="limits"></a>Hoeveel gegevens behouden blijven?

Zie de [limieten samenvatting](app-insights-pricing.md#limits-summary).

### <a name="how-can-i-see-post-data-in-my-server-requests"></a>Hoe kan ik postgegevens zien in mijn serveraanvragen?
Er wordt de POST-gegevens niet automatisch geregistreerd, maar u kunt [TrackTrace of logboek aanroepen](app-insights-asp-net-trace-logs.md). Plaats de POST-gegevens in de bericht-parameter. U kunt niet filteren op het bericht op dezelfde manier die u op Eigenschappen filteren kunt, maar de maximale grootte is langer.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="add"></a>Volgende stappen
* [Schrijven van complexe query's in Analytics](app-insights-analytics-tour.md)
* [Logboeken en aangepaste telemetrie verzenden naar Application Insights](app-insights-asp-net-trace-logs.md)
* [Beschikbaarheid en reactiesnelheid tests instellen](app-insights-monitor-web-app-availability.md)
* [Problemen oplossen](app-insights-troubleshoot-faq.md)
