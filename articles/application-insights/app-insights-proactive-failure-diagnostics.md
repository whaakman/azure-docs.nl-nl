---
title: Detectie - fout-afwijkingen in Application Insights van smartcard | Microsoft Docs
description: Hiermee wordt u gewezen op ongewone wijzigingen in de frequentie van mislukte aanvragen voor uw web-app en bevat diagnostische analyse. Is geen configuratie nodig.
services: application-insights
documentationcenter: 
author: yorac
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: bwren
ms.openlocfilehash: e82d35459110e122ec8438b406a52df61922b0fc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="smart-detection---failure-anomalies"></a>Slimme detectie - fout afwijkingen
[Application Insights](app-insights-overview.md) automatisch een melding in bijna realtime als uw web-app optreedt in een abnormale toename in de frequentie van mislukte aanvragen. Een ongewone stijging van de snelheid van HTTP-aanvragen of afhankelijkheidsaanroepen die worden gerapporteerd als mislukt wordt gedetecteerd. Voor aanvragen voor zijn mislukte aanvragen meestal die met reactiecodes van 400 of hoger. Om te sorteren en onderzoeken van het probleem, wordt een analyse van de kenmerken van de fouten en gerelateerde telemetrie beschikbaar in de melding. Er zijn ook koppelingen naar de Application Insights-portal voor verdere diagnose. Moet de functie geen installatie of de configuratie, zoals het machine learning-algoritmen gebruikt om te voorspellen van de normale Faalpercentage.

Deze functie werkt voor Java- en ASP.NET web-apps, die in de cloud of op uw eigen servers worden gehost. Werkt het ook voor elke app die aanvraag of een afhankelijkheid telemetrie - bijvoorbeeld genereert als u hebt een werkrol die aanroept [TrackRequest()](app-insights-api-custom-events-metrics.md#trackrequest) of [TrackDependency()](app-insights-api-custom-events-metrics.md#trackdependency).

Na het instellen van [Application Insights voor uw project](app-insights-overview.md), en het opgegeven uw app in een bepaalde minimale hoeveelheid telemetrie genereert, Slimme detectie van de fout afwijkingen 24 uur voor meer informatie over de normale werking van uw app, voordat deze wordt duurt ingeschakeld en waarschuwingen kunt verzenden.

Hier volgt een voorbeeld-waarschuwing.

![Voorbeeld Slimme detectie waarschuwing cluster analysis rond de fout wordt weergegeven](./media/app-insights-proactive-failure-diagnostics/013.png)

> [!NOTE]
> Standaard krijgt u een kortere indeling mail dan het volgende voorbeeld. Maar u kunt [overschakelen naar deze gedetailleerde indeling](#configure-alerts).
>
>

U ziet dat het vertelt u:

* Het Faalpercentage vergeleken met een normale app-gedrag.
* Hoeveel gebruikers zijn beïnvloed – zodat u hoeveel weet zorgen te maken.
* Een kenmerkend patroon die zijn gekoppeld aan de fouten. In dit voorbeeld is een bepaalde responscode, de Aanvraagnaam (bewerking) en de app-versie worden gebruikt. Die vertelt onmiddellijk u waar u begint te zoeken in uw code. Andere mogelijkheden mogelijk een specifiek besturingssysteem browser of de client.
* De uitzondering, logboektraceringen en afhankelijkheidsfout (databases of andere externe componenten) weergegeven die zijn gekoppeld aan de wordt gekenmerkt mislukt.
* Koppelingen naar relevante zoekopdrachten voor de telemetrie in Application Insights.

## <a name="benefits-of-smart-detection"></a>Voordelen van Slimme detectie
Gewone [metrische waarschuwingen](app-insights-alerts.md) zien er mogelijk een probleem. Maar Slimme detectie begint het diagnostische werk voor u het uitvoeren van veel van de analyse die u anders zelf zou moeten zelf. Krijgt u de resultaten netjes verpakt, zodat u snel aan de hoofdmap van het probleem.

## <a name="how-it-works"></a>Hoe werkt het?
Slimme detectie bewaakt de telemetrie ontvangen van uw app en met name de tarieven is mislukt. Deze regel telt het aantal aanvragen waarvoor de `Successful request` eigenschap is ingesteld op false en het aantal afhankelijkheid aanroepen waarvoor de `Successful call` eigenschap is ingesteld op false. Voor aanvragen, standaard `Successful request == (resultCode < 400)` (tenzij u aangepaste code hebt geschreven [filter](app-insights-api-filtering-sampling.md#filtering) of genereren van uw eigen [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest) aanroepen). 

Prestaties van uw app heeft een doorsnee patroon gedrag. Sommige aanvragen of afhankelijkheidsaanroepen worden meer gevoelig voor fouten dan andere; en de algehele Faalpercentage oplopen als de belasting toeneemt. Slimme detectie gebruikt machine learning deze afwijkingen vinden.

Als telemetrie in Application Insights uit uw web-app afkomstig is, wordt het huidige gedrag in Slimme detectie vergeleken met de patronen gezien gedurende de afgelopen paar dagen. Als een abnormale toename in Faalpercentage wordt waargenomen ten opzichte van eerdere prestaties, wordt een analyse wordt geactiveerd.

Wanneer een analyse wordt geactiveerd, wordt in de service de analyse van een cluster uitvoert op de mislukte aanvragen, om te proberen een patroon van de waarden die kenmerkend zijn voor de fouten te identificeren. De analyse heeft gedetecteerd dat de meeste storingen over een specifieke resultaatcode, de Aanvraagnaam van de, Server-URL-host en rolinstantie zijn in het bovenstaande voorbeeld. De analyse is daarentegen is gedetecteerd dat de eigenschap van de client-besturingssysteem wordt gedistribueerd over meerdere waarden en zodat deze niet wordt weergegeven.

Wanneer uw service is uitgerust met deze telemetrie-aanroepen, wordt de analyser gezocht naar een uitzondering en een afhankelijkheidsfout die zijn gekoppeld aan aanvragen in het cluster dat deze is geïdentificeerd, samen met een voorbeeld van alle traceringslogboeken zoeken die zijn gekoppeld aan deze aanvragen.

De resulterende analyse is naar u verzonden als waarschuwing, tenzij u deze niet op hebt geconfigureerd.

Als de [waarschuwt u handmatig instellen](app-insights-alerts.md), u kunt de status van de waarschuwing te inspecteren en dit configureren op de blade waarschuwingen van uw Application Insights-resource. Maar in tegenstelling tot andere waarschuwingen, u hoeft niet te installeren of Slimme detectie configureren. Als u wilt, kunt u uitschakelen of wijzigen van de doel-e-mailadressen.

## <a name="configure-alerts"></a>Waarschuwingen configureren
U kunt Slimme detectie uitschakelen, wijzigen van het e-mailontvangers, een webhook maken of meldt zich aan voor meer gedetailleerde waarschuwingsberichten.

Open de pagina waarschuwingen. Fout afwijkingen is opgenomen samen met eventuele waarschuwingen die u handmatig hebt ingesteld en u kunt zien of deze zich momenteel in de status van de waarschuwing.

![Klik op de tegel waarschuwingen op de pagina overzicht. Of klik op de knop waarschuwingen op elke pagina metrische gegevens.](./media/app-insights-proactive-failure-diagnostics/021.png)

Klik op de waarschuwing om deze te configureren.

![Configuratie](./media/app-insights-proactive-failure-diagnostics/032.png)

U ziet dat u Slimme detectie kunt uitschakelen, maar u kunt het niet verwijderen (of maak een nieuwe).

#### <a name="detailed-alerts"></a>Gedetailleerde waarschuwingen
Als u selecteert 'Meer gedetailleerde diagnostische gegevens ophalen' bevat het e-mailbericht meer diagnostische informatie. Soms zult u kunnen het probleem alleen van de gegevens in het e-mailadres op te sporen.

Er is een lichte risico dat de meer gedetailleerde waarschuwing kan gevoelige gegevens bevatten, omdat deze uitzondering en tracering berichten bevat. Echter, dit gebeurt alleen als uw code gevoelige gegevens in deze berichten kunnen zou.

## <a name="triaging-and-diagnosing-an-alert"></a>Gesorteerd en het onderzoeken van een waarschuwing
Een waarschuwing geeft aan dat een abnormale toename in het percentage mislukte aanvragen is gevonden. Is het waarschijnlijk dat er een probleem is met uw app of de omgeving.

Van het percentage aanvragen en het aantal beïnvloede gebruikers, kunt u bepalen hoe urgente het probleem is. In het bovenstaande voorbeeld is het Faalpercentage van 22.5% worden vergeleken met een normale percentage van de % 1, geeft aan dat er een probleem op. Aan de andere kant zijn alleen 11 gebruikers beïnvloed. Als uw app, kunt u zou kunnen beoordelen hoe ernstig die is.

In veel gevallen kunt u zich kunt het probleem snel uit Aanvraagnaam, uitzondering, afhankelijkheid fout- en traceringsbestanden gegevens op te sporen.

Er zijn een aantal andere aanwijzingen. Het Faalpercentage van afhankelijkheid in dit voorbeeld is hetzelfde als de uitzondering snelheid (89,3%). Dit kan erop wijzen dat de uitzondering zich rechtstreeks vanuit de afhankelijkheidsfout voordoet-zodat u een duidelijk beeld van waar u begint te zoeken in uw code.

U kunt verder onderzoeken, de koppelingen in elke sectie gaat u rechtstreeks naar een [zoekpagina](app-insights-diagnostic-search.md) gefilterd op de relevante aanvragen, uitzonderingen, afhankelijkheid of traceringen. Of u kunt de [Azure-portal](https://portal.azure.com), navigeer naar de Application Insights-resource voor uw app en open de blade fouten.

In dit voorbeeld te klikken op de koppeling 'afhankelijkheid fouten details weergeven, opent u de Application Insights zoeken blade. De SQL-instructie een voorbeeld van de hoofdoorzaak bevat te zien: null-waarden zijn opgegeven op verplichte velden en kan niet worden gevalideerd tijdens het opslaan bewerking.

![Diagnostische gegevens doorzoeken](./media/app-insights-proactive-failure-diagnostics/051.png)

## <a name="review-recent-alerts"></a>Recente waarschuwingen weergeven

Klik op **Slimme detectie** naar de meest recente waarschuwing:

![Samenvatting van waarschuwingen](./media/app-insights-proactive-failure-diagnostics/070.png)


## <a name="whats-the-difference-"></a>Wat is het verschil...
Slimme detectie van afwijkingen fout vormt een aanvulling op andere vergelijkbare maar verschillende functies van Application Insights.

* [Metrische waarschuwingen](app-insights-alerts.md) worden ingesteld door u en een breed scala aan metrische gegevens zoals CPU-bezetting, aanvraag tarieven, laadtijden voor pagina's en enzovoort kunt bewaken. U kunt deze gebruiken om u te waarschuwen, bijvoorbeeld, als u wilt meer resources toevoegen. Daarentegen Slimme detectie van afwijkingen van de fout bevat informatie over een klein aantal belangrijke metrische gegevens (momenteel alleen frequentie van mislukte aanvragen), ontworpen om u te waarschuwen u in bijna realtime manier zodra uw web-app kan aanvragen tarief wordt verhoogd aanzienlijk vergeleken met de web-app normaal gedrag.

    Slimme detectie wordt de drempelwaarde in reactie op geldende voorwaarden automatisch aangepast.

    Slimme detectie begint het diagnostische werk voor u.
* [Detectie van afwijkingen van smartcard](app-insights-proactive-performance-diagnostics.md) ook gebruikt intelligence voor het detecteren van ongewone patronen in de metrische gegevens van uw computer en is geen configuratie door u vereist. Maar in tegenstelling tot Slimme detectie fout afwijkingen kunnen worden gedetecteerd, het doel van Slimme detectie van afwijkingen is segmenten van uw gebruik verzamelbuis die mogelijk niet goed worden geleverd: bijvoorbeeld, als bepaalde pagina's op een specifiek type browser vinden. De analyse wordt dagelijks uitgevoerd en als een van de resultaten wordt gevonden, is het waarschijnlijk veel minder urgente dan een waarschuwing. Daarentegen is de analyse op afwijkingen fout continu wordt uitgevoerd op binnenkomende telemetriegegevens en u ontvangt een melding binnen enkele minuten als server uitvalpercentage groter zijn dan verwacht.

## <a name="if-you-receive-a-smart-detection-alert"></a>Als u een waarschuwing Slimme detectie ontvangt
*Waarom hebt ik deze waarschuwing ontvangen?*

* Er is een abnormale toename in de frequentie van mislukte aanvragen vergeleken met de normale basislijn van de voorafgaande periode vastgesteld. Na analyse van de fouten en de bijbehorende telemetrie zien we er is een probleem dat u ziet in.

*Betekent de melding dat er contact is een probleem?*

* We willen waarschuwing bij app onderbreking of vermindering, maar alleen u volledig inzicht in de semantiek en de impact op de app of gebruikers.

*Ja, bekijkt u die mijn gegevens?*

* Nee. De service is geheel automatisch. Alleen het ophalen van de meldingen. Uw gegevens [persoonlijke](app-insights-data-retention-privacy.md).

*Heb ik om u te abonneren op deze waarschuwing?*

* Nee. Elke toepassing dat verzendt telemetrie aanvragen heeft de waarschuwingsregel Slimme detectie.

*Kan ik afmelden of de meldingen worden verzonden naar Mijn collega's in plaats daarvan ophalen?*

* Ja, In waarschuwingsregels, klikt u op de regel Slimme detectie te configureren. U kunt de waarschuwing uitschakelen of geadresseerden voor de waarschuwing wijzigen.

*Ik het e-mailbericht is verbroken. Waar vind ik de meldingen in de portal*

* In de logboeken van de activiteit. In Azure, opent u de Application Insights-resource voor uw app en selecteer vervolgens activiteitenlogboeken.

*Sommige van de waarschuwingen zijn over bekende problemen en ik wil niet om ze te ontvangen.*

* Waarschuwingsonderdrukking hebben we op onze achterstand.

## <a name="next-steps"></a>Volgende stappen
Deze diagnostische hulpprogramma's kunnen u de telemetrie van uw app te controleren:

* [Metrische explorer](app-insights-metrics-explorer.md)
* [Search explorer](app-insights-diagnostic-search.md)
* [Analytics - krachtige querytaal](app-insights-analytics-tour.md)

Smart detecties zijn volledig automatisch. Maar misschien u sommige meer waarschuwingen instellen?

* [Handmatig geconfigureerde metrische waarschuwingen](app-insights-alerts.md)
* [Webtests voor beschikbaarheid](app-insights-monitor-web-app-availability.md)
