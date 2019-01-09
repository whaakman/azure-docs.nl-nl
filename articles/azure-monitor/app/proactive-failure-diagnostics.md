---
title: 'Slimme detectie: ongewone in Application Insights | Microsoft Docs'
description: U gewaarschuwd voor ongebruikelijke wijzigingen in de frequentie van mislukte aanvragen naar uw web-app en diagnostische analyse biedt. Er is geen configuratie nodig.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/18/2018
ms.reviewer: yossiy
ms.author: mbullwin
ms.openlocfilehash: cfa00504cd2a05985fde2af3357418eac8baceeb
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54117653"
---
# <a name="smart-detection---failure-anomalies"></a>Slimme detectie: afwijkende fouten
[Application Insights](../../azure-monitor/app/app-insights-overview.md) automatisch ontvangt u een melding in bijna realtime als er een abnormale toename van mislukte aanvragen optreedt in uw web-app. Een ongewone stijging van het aantal HTTP-aanvragen of afhankelijkheidsaanroepen die worden gerapporteerd als mislukt wordt gedetecteerd. Voor aanvragen zijn mislukte aanvragen meestal met responscodes van 400 of hoger. Als u wilt, kunt u sorteren en onderzoeken van het probleem een analyse van de kenmerken van de fouten en verwante telemetrie vindt u in de melding. Er zijn ook koppelingen naar de Application Insights-portal voor verdere diagnose. De functie moet geen installatie of configuratie, zoals het machine learning-algoritmen gebruikt om te voorspellen van de normaal foutpercentage.

Deze functie werkt voor Java en ASP.NET-webtoepassingen, die worden gehost in de cloud of op uw eigen servers. Het werkt ook voor elke app die wordt bijvoorbeeld gegenereerd aanvraag of een afhankelijkheid telemetrie - hebt u een werkrol die worden aangeroepen [TrackRequest()](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest) of [TrackDependency()](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency).

Na het instellen van [Application Insights voor uw project](../../azure-monitor/app/app-insights-overview.md), en mits uw app een bepaalde minimale hoeveelheid telemetrie genereert, Slimme detectie van afwijkende fouten 24 uur voor meer informatie over het normale gedrag van uw app, voordat dit duurt is ingeschakeld en meldingen kunt verzenden.

Hier volgt een waarschuwing voor een voorbeeld.

![Voorbeeld van Slimme detectie waarschuwing cluster analyse om fouten heen weergeven](./media/proactive-failure-diagnostics/013.png)

> [!NOTE]
> Standaard krijgt u een kortere indeling mail dan het volgende voorbeeld. Maar u kunt de [overschakelen naar deze gedetailleerde indeling](#configure-alerts).
>
>

U ziet dat u leest:

* Het aantal fouten in vergelijking met een normale app-gedrag.
* Hoeveel gebruikers betrof -, zodat u hoeveel weet zorgen te maken.
* Een kenmerkend patroon dat is gekoppeld aan de fouten. In dit voorbeeld wordt een bepaalde responscode, de naam (bewerking) en de app-versie. Die onmiddellijk informatie over waar u om te beginnen met zoeken in uw code. Andere mogelijkheden kunnen een specifiek besturingssysteem browser of de client zijn.
* De uitzondering, logboektraceringen en afhankelijkheidsfout (databases of andere externe componenten) weergegeven die zijn gekoppeld aan de wordt gekenmerkt fouten.
* Koppelingen naar relevante zoekopdrachten op de telemetrie in Application Insights.

## <a name="benefits-of-smart-detection"></a>Voordelen van Slimme detectie
Gewone [metrische waarschuwingen](../../azure-monitor/app/alerts.md) zien er mogelijk een probleem. Maar Slimme detectie begint het diagnostische werk voor u veel van de analyse die u anders zelf zou moeten zelf doen uitvoeren. U krijgt de resultaten die netjes verpakt, zodat u snel aan de hoofdmap van het probleem.

## <a name="how-it-works"></a>Hoe werkt het?
Slimme detectie bewaakt de telemetrie ontvangen van uw app, en met name de foutpercentages. Deze regel telt het aantal aanvragen waarvoor de `Successful request` eigenschap is ingesteld op false, en het aantal afhankelijkheid aanroepen waarvoor de `Successful call` eigenschap is ingesteld op false. Voor aanvragen voor standaard `Successful request == (resultCode < 400)` (tenzij u aangepaste code hebt geschreven [filter](../../azure-monitor/app/api-filtering-sampling.md#filtering) of genereer uw eigen [TrackRequest](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest) aanroepen). 

Prestaties van uw app heeft een doorsnee patroon van gedrag. Sommige aanvragen of afhankelijkheidsaanroepen worden meer gevoelig voor fouten in dan andere; en het totale aantal fouten kan omhoog gaan wanneer de belasting toeneemt. Slimme detectie maakt gebruik van machine learning om deze afwijkingen vinden.

Als telemetrie in Application Insights uit uw web-app afkomstig is, wordt het huidige gedrag in Slimme detectie vergeleken met de patronen weergegeven gedurende de afgelopen paar dagen. Als er een afwijkende toename in aantal fouten is waargenomen ten opzichte van eerdere prestaties, wordt een analyse geactiveerd.

Wanneer een analyse wordt geactiveerd, voert de service een analyse van het cluster op de mislukte aanvragen, om te proberen een patroon van waarden die kenmerkend zijn voor de fouten identificeren. De analyse heeft gedetecteerd dat de meeste fouten over een specifieke resultaatcode, de Aanvraagnaam van de, Server-URL-host en rolinstantie zijn in het bovenstaande voorbeeld. De analyse heeft daarentegen is gedetecteerd dat de eigenschap van de client-besturingssysteem wordt gedistribueerd over meerdere waarden en zodat deze niet wordt vermeld.

Als uw service is uitgerust met deze telemetrie-aanroepen, wordt de analyzer gezocht naar een uitzondering en een afhankelijkheidsfout die zijn gekoppeld aan aanvragen in het cluster dat het heeft geïdentificeerd, samen met een voorbeeld van alle logboeken met traceringen die zijn gekoppeld aan deze aanvragen.

De resulterende analyse is naar u verzonden als waarschuwing, tenzij u deze niet op hebt geconfigureerd.

Net als de [waarschuwingen die u handmatig ingesteld](../../azure-monitor/app/alerts.md), u kunt de status van de waarschuwing controleren en configureren op de blade waarschuwingen van de Application Insights-resource. Maar in tegenstelling tot andere waarschuwingen, u hoeft niet te installeren of configureren van Slimme detectie. Als u wilt, kunt u uitschakelen of wijzigen van de doel-e-mailadressen.

### <a name="alert-logic-details"></a>Details van waarschuwingslogica

De waarschuwingen worden geactiveerd door onze eigen machine learning-algoritme, zodat we de exacte implementatiedetails niet delen. We begrijpen dat soms u moet voor meer informatie over de werking van de onderliggende logica basis hiervan dient. De primaire factoren die worden geëvalueerd om te bepalen als een waarschuwing moet worden gestart zijn: 

* Analyse van het percentage mislukte aanvragen/afhankelijkheden in een rolling tijdvenster van 20 minuten.
* Een vergelijking van de fout percentage van de laatste twintig minuten aan het tarief in de laatste 40 minuten en de afgelopen zeven dagen en op zoek naar belangrijke afwijkingen die groter is dan X keer dat de standaarddeviatie.
* Met een adaptieve limiet voor het percentage minimum fout, dat afhankelijk is gebaseerd op volume van aanvragen/afhankelijkheden van de app.

## <a name="configure-alerts"></a>Waarschuwingen configureren
U kunt Slimme detectie uitschakelen, wijzigen van het e-mailontvangers, een webhook maken of meldt zich aan voor meer gedetailleerde waarschuwingsberichten.

Open de pagina met waarschuwingen. Afwijkende fouten is opgenomen, samen met eventuele waarschuwingen die u handmatig hebt ingesteld en u kunt zien of deze zich momenteel in de status van de waarschuwing.

![Op de pagina overzicht klikt u op de tegel waarschuwingen. Of klik op de knop meldingen op een willekeurige pagina metrische gegevens.](./media/proactive-failure-diagnostics/021.png)

Klik op de waarschuwing om deze te configureren.

![Configuratie](./media/proactive-failure-diagnostics/032.png)

U ziet dat u Slimme detectie kunt uitschakelen, maar u kunt het niet verwijderen (of maak een nieuwe).

#### <a name="detailed-alerts"></a>Gedetailleerde waarschuwingen
Als u 'Meer gedetailleerde diagnostische gegevens ophalen' bevat het e-mailbericht meer diagnostische informatie. Soms zult u kunnen het probleem alleen van de gegevens in het e-mailadres op te sporen.

Er is een lichte risico dat de meer gedetailleerde waarschuwing kan gevoelige gegevens bevatten, omdat deze uitzondering en tracering berichten bevat. Maar dit gebeurt alleen als uw code tot gevoelige informatie in deze berichten leiden kan.

## <a name="triaging-and-diagnosing-an-alert"></a>Beoordelen en onderzoeken van een waarschuwing
Een waarschuwing geeft aan dat een ongebruikelijke stijging in de Hoev. mislukte aanvragen is gedetecteerd. Het is zeer onwaarschijnlijk dat er een probleem met uw app of de omgeving.

Van het percentage van aanvragen en het aantal getroffen gebruikers, kunt u bepalen hoe dringend het probleem is. In het bovenstaande voorbeeld is het Faalpercentage van 22.5% vergeleken met een normale percentage van de % 1, geeft aan dat er een probleem op. Aan de andere kant zijn alleen 11 gebruikers beïnvloed. Als uw app, zou u mogelijk zijn om te evalueren hoe ernstig die is.

In veel gevallen kunt u zich voor het vaststellen van het probleem snel van de naam, de uitzondering, de afhankelijkheid mislukt en gegevens traceren opgegeven.

Er zijn enkele andere aanwijzingen. Het Faalpercentage van de afhankelijkheid in dit voorbeeld is bijvoorbeeld gelijk zijn aan het aantal uitzonderingen (89,3%). Dit kan erop wijzen dat de uitzondering zich rechtstreeks vanuit de afhankelijkheidsfout voordoet-zodat u een duidelijk inzicht in waar u wilt beginnen met zoeken in uw code.

Nader onderzoeken, de koppelingen in elke sectie gaat u rechtstreeks naar een [zoekpagina](../../azure-monitor/app/diagnostic-search.md) gefilterd op de relevante aanvragen, uitzonderingen, afhankelijkheden of traceringen. Of u kunt de [Azure-portal](https://portal.azure.com), gaat u naar de Application Insights-resource voor uw app en opent u de blade fouten.

In dit voorbeeld opent u de blade van Application Insights zoeken door op de koppeling 'View details van afhankelijkheid fouten' te klikken. Deze ziet u de SQL-instructie een voorbeeld van de hoofdoorzaak bevat: Null-waarden zijn opgegeven op verplichte velden en kan niet worden gevalideerd tijdens de opslagbewerking bewerking.

![Diagnostische gegevens doorzoeken](./media/proactive-failure-diagnostics/051.png)

## <a name="review-recent-alerts"></a>Recente waarschuwingen weergeven

Klik op **Slimme detectie** om te gaan naar de meest recente waarschuwing:

![Samenvatting van waarschuwingen](./media/proactive-failure-diagnostics/070.png)


## <a name="whats-the-difference-"></a>Wat is het verschil...
Slimme detectie van foutafwijkingen is een aanvulling op andere vergelijkbare maar verschillende functies van Application Insights.

* [Metrische waarschuwingen](../../azure-monitor/app/alerts.md) worden ingesteld door u en een breed scala aan metrische gegevens zoals CPU-bezetting, aanvragen, laadtijden voor pagina's en enzovoort kunt bewaken. U kunt deze gebruiken om u te waarschuwen, bijvoorbeeld, als u meer resources toevoegen. Daarentegen, Slimme detectie van afwijkende fouten bevat informatie over een klein aantal essentiële metrische gegevens (momenteel alleen Hoev. mislukte aanvragen), ontworpen om op de hoogte stellen u in bijna realtime manier zodra uw web-app is mislukt aanvragen tarief wordt verhoogd aanzienlijk in vergelijking met web-app normale gedrag.

    Slimme detectie past automatisch de drempelwaarde in reactie op geldende voorwaarden.

    Slimme detectie begint het diagnostische werk voor u.
* [Slimme detectie van prestatieafwijkingen](proactive-performance-diagnostics.md) ook gebruikt machine intellligentie voor het detecteren van ongebruikelijke patronen in uw metrische gegevens en u geen configuratie is vereist. Maar in tegenstelling tot Slimme detectie van afwijkende fouten, het doel van Slimme detectie van prestatieafwijkingen is segmenten van uw gebruik verzamelbuis die mogelijk niet goed worden weergegeven: als u bijvoorbeeld bepaalde pagina's op een specifiek type browser vinden. De analyse wordt dagelijks uitgevoerd en als een resultaat wordt gevonden, is het waarschijnlijk veel minder urgente dan een waarschuwing. Daarentegen is de analyse voor afwijkende fouten op binnenkomende telemetriegegevens continu wordt uitgevoerd en u krijgt bericht binnen enkele minuten als server foutpercentages groter is dan verwacht.

## <a name="if-you-receive-a-smart-detection-alert"></a>Als u een waarschuwing voor slimme detectie ontvangt
*Waarom hebt ik deze waarschuwing ontvangen?*

* Er is een ongebruikelijke stijging in mislukte aanvragen tarief ten opzichte van de normale basislijn van de voorafgaande punt gedetecteerd. Na de analyse van de fouten en de bijbehorende telemetrie denken we dat er een probleem dat u ziet is in.

*Betekent de melding dat er definitief is een probleem?*

* We proberen te waarschuwen bij app-onderbreking of vermindering, maar alleen u volledig inzicht in de semantiek en de gevolgen voor de app of gebruikers.

*Dus, kijken jullie mijn gegevens?*

* Nee. De service is volledig automatisch. Alleen krijgt u de meldingen. Uw gegevens [persoonlijke](../../azure-monitor/app/data-retention-privacy.md).

*Heb ik om u te abonneren op deze waarschuwing?*

* Nee. Elke toepassing dat verzendt telemetrie aanvragen heeft de waarschuwingsregel voor slimme detectie.

*Kan ik zich afmelden of ophalen van de meldingen worden verzonden naar Mijn collega's in plaats daarvan?*

* Ja, In waarschuwingsregels, klikt u op de regel voor slimme detectie te configureren. U kunt de waarschuwing uitschakelen of geadresseerden voor de waarschuwing wijzigen.

*Kan ik het e-mailbericht is verbroken. Waar vind ik de meldingen in de portal?*

* In de activiteitenlogboeken. In Azure, opent u de Application Insights-resource voor uw app en selecteer vervolgens activiteitenlogboeken.

*Sommige van de waarschuwingen zijn over bekende problemen en ik wil niet om ze te ontvangen.*

* We hebben waarschuwingsonderdrukking op gewerkt.

## <a name="next-steps"></a>Volgende stappen
Deze diagnostische hulpprogramma's kunnen u de telemetrie van uw app controleren:

* [Metric explorer](../../azure-monitor/app/metrics-explorer.md)
* [Search explorer](../../azure-monitor/app/diagnostic-search.md)
* [Analytics - krachtige querytaal](../../azure-monitor/log-query/get-started-portal.md)

Slimme detectie worden volledig automatisch uitgevoerd. Maar misschien u graag enkele meer waarschuwingen instellen?

* [Handmatig geconfigureerde metrische waarschuwingen](../../azure-monitor/app/alerts.md)
* [Webtests voor beschikbaarheid](../../azure-monitor/app/monitor-web-app-availability.md)
