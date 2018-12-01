---
title: Met zoeken in Azure Application Insights | Microsoft Docs
description: Zoeken en filteren onbewerkte telemetrie die is verzonden door uw web-app.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 2a437555-8043-45ec-937a-225c9bf0066b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: mbullwin
ms.openlocfilehash: 5bff5c5cfd022134c1d86883a267528717969c59
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721677"
---
# <a name="using-search-in-application-insights"></a>Zoeken in Application Insights gebruiken
Search is een functie van [Application Insights](app-insights-overview.md) waarmee u kunt vinden en afzonderlijk telemetrie-items, zoals paginaweergaven, uitzonderingen, verkennen of webaanvragen. En u kunt weergeven, logboektraceringen en gebeurtenissen die u hebt gecodeerd.

(Voor complexere query's over uw gegevens, gebruikt u [Analytics](../log-analytics/query-language/get-started-analytics-portal.md).)

## <a name="where-do-you-see-search"></a>Waar kan u Search zien?

### <a name="in-the-azure-portal"></a>In de Azure-portal

U kunt diagnostische gegevens doorzoeken expliciet openen vanuit de blade overzicht van Application Insights van uw toepassing:

![Open diagnostische gegevens doorzoeken](./media/app-insights-diagnostic-search/001.png)

![Schermafbeelding van diagnostische gegevens doorzoeken grafieken](./media/app-insights-diagnostic-search/002.png)

De hoofdtekst van het doorzoeken van diagnostische gegevens wordt een lijst met pagina van de telemetrie-items: serveraanvragen, weergaven en aangepaste gebeurtenissen die u hebt gecodeerd. Aan de bovenkant van de lijst is een grafiek met samenvattingen van aantallen gebeurtenissen gedurende een periode weergegeven.

Klik op Vernieuwen om nieuwe gebeurtenissen.

### <a name="in-visual-studio"></a>In Visual Studio

In Visual Studio is er ook een Application Insights-zoekvenster. Dit is vooral nuttig zijn voor het weergeven van telemetriegebeurtenissen die worden gegenereerd door de toepassing waarmee u fouten opspoort. Maar het kan ook de gebeurtenissen die worden verzameld van de gepubliceerde app op de Azure-portal weergeven.

Open het venster Zoeken in Visual Studio:

![Visual Studio Application Insights-zoekopdracht openen](./media/app-insights-diagnostic-search/32.png)

Het venster zoeken bevat functies die vergelijkbaar is met de web-portal:

![Visual Studio Application Insights-zoekvenster](./media/app-insights-diagnostic-search/34.png)

Het tabblad bijhouden bewerking is alleen beschikbaar bij het openen van een aanvraag of een paginaweergave. Een ' bewerking ' is een reeks gebeurtenissen die is gekoppeld aan een enkele aanvraag of paginaweergave-weergave. Bijvoorbeeld, kunnen afhankelijkheidsaanroepen, uitzonderingen, logboeken met traceringen en aangepaste gebeurtenissen deel uitmaken van één bewerking. Het tabblad bijhouden bewerking worden grafisch weergegeven voor de timing en de duur van deze gebeurtenissen ten opzichte van de aanvraag of paginaweergave-weergave. 

## <a name="inspect-individual-items"></a>Afzonderlijke items controleren

Selecteer een telemetrie-item om te zien van de velden voor sleutels en verwante items.

![Schermafbeelding van de aanvraag van een afzonderlijke afhankelijkheid](./media/app-insights-diagnostic-search/003.png)

Hiermee wordt de end-to-end-transactie-detailweergave geopend:

![Schermopname van de detailweergave van end-to-end-transactie.](./media/app-insights-diagnostic-search/004.png)

## <a name="filter-event-types"></a>Gebeurtenistypen filteren
Open de blade Filter en kiest u de typen gebeurtenissen die u wilt zien. (Als u later herstellen van de filters waarmee u de blade geopend wilt, klikt u op opnieuw instellen.)

![Kies Filter en selecteer de telemetrietypen](./media/app-insights-diagnostic-search/02-filter-req.png)

De gebeurtenistypen zijn:

* **Tracering** - [diagnostische logboeken](app-insights-asp-net-trace-logs.md) inclusief TrackTrace, log4Net, NLog en System.Diagnostic.Trace aanroepen.
* **Aanvraag** -HTTP-aanvragen die zijn ontvangen door de server-toepassing, met inbegrip van pagina's, scripts, afbeeldingen, Stijlbestanden en gegevens. Deze gebeurtenissen worden gebruikt voor het maken van de aanvraag en respons overzichtsgrafieken.
* **Paginaweergave** - [telemetrie wordt verzonden door de webclient](app-insights-javascript.md), die wordt gebruikt om pagina weergaverapporten te maken. 
* **Aangepaste gebeurtenis** : als u aanroepen van TrackEvent() om ingevoegd [-gebruik controleren](app-insights-api-custom-events-metrics.md), kunt u deze hier zoeken.
* **Uitzondering** - niet-onderschepte [uitzonderingen in de server](app-insights-asp-net-exceptions.md), en bronnen die u zich aanmeldt met behulp van TrackException().
* **Afhankelijkheid** - [aanroepen vanuit de servertoepassing](app-insights-asp-net-dependencies.md) met andere services, zoals REST-API's of databases en AJAX-aanroepen van uw [clientcode](app-insights-javascript.md).
* **Beschikbaarheid** -resultaten van [beschikbaarheidstests](app-insights-monitor-web-app-availability.md).

## <a name="filter-on-property-values"></a>Filteren op basis van eigenschapswaarden
U kunt gebeurtenissen op de waarden van hun eigenschappen filteren. De beschikbare eigenschappen zijn afhankelijk van de typen gebeurtenissen die u hebt geselecteerd. 

Bijvoorbeeld aanvragen met een specifieke antwoordcode te kiezen. 

![Vouw een eigenschap en een waarde kiezen](./media/app-insights-diagnostic-search/03-response500.png)

Er zijn geen waarden van een bepaalde eigenschap kiezen heeft hetzelfde effect als het kiezen van alle waarden. Het verandert filter voor die eigenschap uit.

### <a name="narrow-your-search"></a>Verfijn uw zoekopdracht
U ziet dat het aantal's aan de rechterkant van de filterwaarden tonen hoeveel exemplaren er zijn in de huidige gefilterde set. 

In dit voorbeeld is het duidelijk dat de 'rapportkoptekst/werknemers' resultaten in de meeste van de '500-fouten aanvragen:

![Vouw een eigenschap en een waarde kiezen](./media/app-insights-diagnostic-search/04-failingReq.png)

## <a name="find-events-with-the-same-property"></a>Gebeurtenissen met dezelfde eigenschap zoeken
Alle items met dezelfde eigenschapswaarde voor zoeken:

![Met de rechtermuisknop op een eigenschap](./media/app-insights-diagnostic-search/12-samevalue.png)

## <a name="search-the-data"></a>Zoeken naar de gegevens

> [!NOTE]
> Voor het schrijven van complexe query's, open [ **Analytics** ](../log-analytics/query-language/get-started-analytics-portal.md) vanaf de bovenkant van de blade zoeken.
> 

U kunt zoeken naar termen in een van de eigenschapswaarden. Dit is vooral handig als u hebt geschreven [aangepaste gebeurtenissen](app-insights-api-custom-events-metrics.md) met eigenschapswaarden. 

Het is raadzaam een bereik, zoekacties in een kortere bereik zijn sneller tijd in te stellen. 

![Open diagnostische gegevens doorzoeken](./media/app-insights-diagnostic-search/appinsights-311search.png)

Zoeken naar volledige woorden, niet subtekenreeksen. Gebruik aanhalingstekens tussen de speciale tekens.

| tekenreeks | is *niet* gevonden | maar deze vinden |
| --- | --- | --- |
| HomeController.About |start<br/>domeincontroller<br/>out | homecontroller<br/>info<br/>"homecontroller.about"|
|Verenigde Staten|UNI<br/>Ted|Verenigde<br/>statussen<br/>Verenigde Staten en<br/>"VS"

Hier volgen de uitdrukkingen zoeken die u kunt gebruiken:

| Voorbeeldquery | Effect |
| --- | --- |
| `apple` |Alle gebeurtenissen niet vinden in het tijdsbereik waarvan de velden het woord 'apple bevatten' |
| `apple AND banana` <br/>`apple banana` |Zoeken naar gebeurtenissen die beide woorden bevatten. Gebruik kapitaal 'en', niet 'en'. <br/>Verkorte vorm. |
| `apple OR banana` |Gebeurtenissen die een woord bevatten vinden. Gebruik 'Of', niet 'of'. |
| `apple NOT banana` |Zoeken naar gebeurtenissen die niet door de andere, maar één woord bevatten. |

## <a name="sampling"></a>Steekproeven
Als uw app veel telemetrie genereert (en u de ASP.NET-SDK-versie 2.0.0-beta3 of hoger), de adaptieve steekproefmodule automatisch het volume dat naar de portal wordt verzonden door alleen een representatieve fractie van de gebeurtenissen sturen vermindert. Gebeurtenissen die gerelateerd zijn aan dezelfde aanvraag zijn echter geselecteerd of gedeselecteerd als een groep, zodat u tussen gerelateerde gebeurtenissen kunt navigeren. 

[Meer informatie over steekproeven](app-insights-sampling.md).

## <a name="create-work-item"></a>Werkitem maken
U kunt een bug in GitHub of Azure DevOps met de details van een telemetrie-item maken. 

![Klik op Nieuw werkitem, bewerkt u de velden en klik vervolgens op OK.](./media/app-insights-diagnostic-search/42.png)

De eerste keer dat u dit doet, moet u gevraagd het configureren van een koppeling naar uw Azure DevOps-organisatie en uw project.

![Vul de URL van uw Azure DevOps-Services en de naam van het Project en klik op autoriseren](./media/app-insights-diagnostic-search/41.png)

(U kunt ook de koppeling configureren op de blade werkitems.)

## <a name="send-more-telemetry-to-application-insights"></a>Meer telemetrie wordt verzonden naar Application Insights
Naast de out-of-the-box-telemetrie die is verzonden door Application Insights-SDK, kunt u het volgende doen:

* Logboektraceringen vanuit uw favoriete framework voor logboekregistratie in vastleggen [.NET](app-insights-asp-net-trace-logs.md) of [Java](app-insights-java-trace-logs.md). Dit betekent dat u kunt uw logboektraceringen doorzoeken en correlaties met paginaweergaven, uitzonderingen en andere gebeurtenissen. 
* [Code schrijven](app-insights-api-custom-events-metrics.md) om aangepaste gebeurtenissen, paginaweergaven en uitzonderingen te verzenden. 

[Meer informatie over het verzenden van Logboeken en aangepaste telemetrie naar Application Insights](app-insights-asp-net-trace-logs.md).

## <a name="questions"></a>Q &AMP; A
### <a name="limits"></a>Hoeveel gegevens worden bewaard?

Zie de [limieten samenvatting](app-insights-pricing.md#limits-summary).

### <a name="how-can-i-see-post-data-in-my-server-requests"></a>Hoe kan ik POST-gegevens in mijn serveraanvragen bekijken?
We niet de POST-gegevens automatisch vastleggen, maar u kunt [TrackTrace of log aanroepen](app-insights-asp-net-trace-logs.md). De POST-gegevens in de parameter bericht geplaatst. U kunt niet filteren op het bericht op dezelfde manier die u op Eigenschappen filteren kunt, maar de maximale grootte is langer.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="add"></a>Volgende stappen
* [Schrijven van complexe query's in Analytics](../log-analytics/query-language/get-started-analytics-portal.md)
* [Logboeken en aangepaste telemetrie verzenden naar Application Insights](app-insights-asp-net-trace-logs.md)
* [Beschikbaarheid en reactiesnelheid tests instellen](app-insights-monitor-web-app-availability.md)
* [Problemen oplossen](app-insights-troubleshoot-faq.md)
