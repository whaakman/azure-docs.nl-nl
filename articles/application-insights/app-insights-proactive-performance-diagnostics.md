---
title: Detectie - afwijkingen van smartcard | Microsoft Docs
description: "Application Insights voert smart analyse van uw app Telemetrie en waarschuwt u potentiële problemen. Er zijn geen instellingen, moet deze functie."
services: application-insights
documentationcenter: windows
author: antonfrMSFT
manager: carmonm
ms.assetid: 6acd41b9-fbf0-45b8-b83b-117e19062dd2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: mbullwin
ms.openlocfilehash: 3310239b5569ca5b63bd39acb4d192a4e54780e4
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="smart-detection---performance-anomalies"></a>Slimme detectie - afwijkingen

[Application Insights](app-insights-overview.md) automatisch de prestaties van uw webtoepassing analyseert en kunt u gewaarschuwd over mogelijke problemen. U kan worden gelezen dit omdat u een van onze Slimme detectie meldingen ontvangen.

Dit onderdeel vereist geen speciale instellingen, dan het configureren van de app voor Application Insights (op [ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), of [Node.js](app-insights-nodejs.md), en in [code webpagina](app-insights-javascript.md)). Het is actief wanneer uw app genoeg telemetrie genereert.

## <a name="when-would-i-get-a-smart-detection-notification"></a>Wanneer een melding Slimme detectie krijgt?

Application Insights heeft gedetecteerd dat de prestaties van uw toepassing is gedegradeerd in een van de volgende manieren:

* **Antwoord tijd vermindering** -de langzamer dan gebruikt om te reageren op aanvragen van uw app is gestart. De wijziging mogelijk is de snelle, bijvoorbeeld omdat er een regressie in uw meest recente implementatie. Of het mogelijk is de geleidelijke, mogelijk veroorzaakt door een geheugenlek. 
* **Afhankelijkheid duur vermindering** -opgeroepen van uw app een REST-API, database of andere afhankelijkheid. De afhankelijkheid is langzamer dan gebruikt om te reageren.
* **Trage prestaties patroon** -uw app wordt weergegeven om een prestatieprobleem dat alleen bepaalde aanvragen beïnvloedt. Bijvoorbeeld, worden pagina's trager geladen op één type browser dan andere; of de aanvragen van een bepaalde server trager zijn wordt geleverd. Op dit moment kijken onze algoritmen laadtijden voor pagina's, reactietijden van de aanvraag en reactietijden van de afhankelijkheid.  

Slimme detectie vereist ten minste acht dagen telemetrie op een volume werkbare om een basislijn van de normale prestaties stand te brengen. Dus nadat uw toepassing actief is geweest gedurende die tijd, leidt eventuele belangrijke problemen tot een melding.


## <a name="does-my-app-definitely-have-a-problem"></a>Beschikt over mijn app zeker een probleem?

Nee, een melding betekent dat uw app zeker een probleem heeft niet. Het is gewoon een suggestie over iets wilt u mogelijk meer nauw bekijken.

## <a name="how-do-i-fix-it"></a>Hoe kan ik oplossen?

De meldingen bevatten diagnostische gegevens. Hier volgt een voorbeeld:


![Hier volgt een voorbeeld van Server antwoord tijd vermindering detectie](./media/app-insights-proactive-diagnostics/server_response_time_degradation.png)

1. **Selectie**. De melding ziet u hoeveel gebruikers of hoeveel bewerkingen worden beïnvloed. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **Bereik**. Het probleem invloed op alle verkeer of slechts enkele's? Is deze beperkt tot bepaalde browsers of locaties? Deze informatie kan worden verkregen van de melding.
3. **Diagnose**. De diagnostische gegevens in de melding wordt vaak de aard van het probleem voorgesteld. Bijvoorbeeld, als reactietijd vertraagd als aanvraagsnelheid hoog is, die stelt voor dat uw server of afhankelijkheden overbelast zijn. 

    Open de blade Performance anders in Application Insights. Daar vindt u [Profiler](app-insights-profiler.md) gegevens. Als er uitzonderingen worden veroorzaakt, kunt u ook proberen de [momentopname foutopsporingsprogramma](app-insights-snapshot-debugger.md).



## <a name="configure-email-notifications"></a>E-mailmeldingen configureren

Smart detectie meldingen zijn standaard ingeschakeld en verzonden naar gebruikers die hebben [eigenaren, bijdragers en lezers toegang tot de Application Insights-resource](app-insights-resources-roles-access-control.md). Om dit te wijzigen, klik **configureren** in het e-mailmeldingen of open Slimme detectie-instellingen in Application Insights. 
  
  ![Detectie-instellingen van smartcard](./media/app-insights-proactive-diagnostics/smart_detection_configuration.png)
  
  * U kunt de **opzeggen** koppeling in de e-mail Slimme detectie om meer te ontvangen van de e-mailmeldingen.

E-mailberichten over afwijkingen Smart detecties zijn beperkt tot één e per dag per Application Insights-resource. Het e-mailbericht zal worden alleen verzonden als er is ten minste één nieuwe probleem dat is gedetecteerd op die dag. U won't wordt herhaald van elk bericht ophalen. 

## <a name="faq"></a>Veelgestelde vragen

* *Medewerkers van Microsoft is dus kijken van mijn gegevens?*
  * Nee. De service is geheel automatisch. Alleen het ophalen van de meldingen. Uw gegevens [persoonlijke](app-insights-data-retention-privacy.md).
* *U de gegevens die zijn verzameld door Application Insights analyseren?*
  * Niet op dit moment. Op dit moment kunnen analyseren we aanvraag die de laadtijd van reactietijd, afhankelijkheid reactietijd en pagina. Analyse van aanvullende gegevens is op onze benieuwd achterstand.

* Welke typen toepassingen werkt dit voor?
  * Deze degradations worden gedetecteerd in elke toepassing die de juiste telemetrie genereert. Als u Application Insights in uw web-app hebt geïnstalleerd, worden klikt u vervolgens aanvragen en afhankelijkheden automatisch bijgehouden. Maar in de back-end-services of andere apps, als u ingevoegd aanroepen naar [TrackRequest()](app-insights-api-custom-events-metrics.md#trackrequest) of [TrackDependency](app-insights-api-custom-events-metrics.md#trackdependency), en vervolgens Slimme detectie op dezelfde manier werkt.

* *Kan ik mijn eigen afwijkingsdetectie detectieregels maken of aanpassen van bestaande regels?*

  * Nog niet, maar u kunt:
    * [Stel waarschuwingen](app-insights-alerts.md) waarmee wordt aangegeven dat wanneer een metriek een drempelwaarde overschrijdt.
    * [Exporteren van telemetrie](app-insights-export-telemetry.md) naar een [database](app-insights-code-sample-export-sql-stream-analytics.md) of [naar Power BI](app-insights-export-power-bi.md), waar u kunt deze analyseren zelf.
* *Hoe vaak wordt de analyse uitgevoerd?*

  * Dagelijks de analyse voor de telemetrie van de vorige dag (volledige dag in UTC-tijdzone).
* *Zo biedt deze vervangen [metrische waarschuwingen](app-insights-alerts.md)?*
  * Nee.  We opslaan niet in elke die u kunt overwegen om abnormaal gedrag te detecteren.


* *Als ik iets in antwoord op een melding niet doet, krijg een herinnering?*
  * Nee, u krijgt een bericht over elk probleem slechts één keer. Als het probleem zich blijft voordoen, wordt deze in de Slimme detectie feed blade bijgewerkt.
* *Ik het e-mailbericht is verbroken. Waar vind ik de meldingen in de portal*
  * In het overzicht van de Application Insights van uw app, klikt u op de **Slimme detectie** tegel. U hebt er alle meldingen van 90 dagen back vinden.

## <a name="how-can-i-improve-performance"></a>Hoe kan ik de prestaties verbeteren?
Trage en mislukte antwoorden zijn een van de grootste frustraties voor gebruikers van de website, zoals u uit uw eigen ervaring weet. Het is dus belangrijk om de problemen te verhelpen.

### <a name="triage"></a>Selectie
Eerst maakt het uit? Als een pagina altijd langzaam is worden geladen, maar slechts 1% van de gebruikers van uw site ooit hebben om te kijken naar het, misschien hebt u belangrijkere zaken om na te denken over. Anderzijds, als er slechts 1% van de gebruikers openen, maar deze uitzonderingen telkens genereert, die mogelijk waard.

De instructie impact (betrokken gebruikers of % van het verkeer) gebruiken als een algemene richtlijn, maar houd er rekening mee dat dit niet het gehele artikel. Verzamel andere bewijs om te bevestigen.

U kunt de parameters van het probleem. Als het Geografie-afhankelijk is, kunt u instellen [beschikbaarheidstests](app-insights-monitor-web-app-availability.md) met inbegrip van die regio: er simpelweg mogelijk problemen met netwerken in het desbetreffende gebied.

### <a name="diagnose-slow-page-loads"></a>Diagnose van trage pagina wordt geladen
Waar bevindt het probleem zich? Is de server traag reageren, is de pagina erg lang of de browser heeft te veel werk weer te geven doen?

Open de metrische blade Browsers. De gesegmenteerde weergave van de browser load tijd wordt aangegeven waar de tijd gaat. 

* Als **aanvraagtijd verzenden** is hoog is, kan de server reageert langzaam of de aanvraag is een bericht met een grote hoeveelheden gegevens. Bekijk de [maatstaven voor prestaties](app-insights-web-monitor-performance.md#metrics) voor het onderzoeken van reactietijden.
* Instellen van [bijhouden van afhankelijkheid](app-insights-asp-net-dependencies.md) te controleren of de logge als gevolg van externe services of de database is.
* Als **reactie ontvangen van** overwegend, wordt de pagina en de afhankelijke onderdelen - JavaScript, CSS, afbeeldingen, enzovoort (maar niet asynchroon geladen gegevens) lang zijn. Instellen van een [beschikbaarheidstest](app-insights-monitor-web-app-availability.md), en zorg ervoor dat de optie voor het laden van afhankelijke onderdelen. Als u bepaalde resultaten krijgt, opent u de details van een resultaat en vouw om te zien of de laadtijden van andere bestanden.
* Hoge **Client verwerkingstijd** wordt voorgesteld scripts langzaam worden uitgevoerd. Als de reden niet duidelijk, overweeg timing code en de tijden verzenden in trackMetric aanroepen.

### <a name="improve-slow-pages"></a>Langzame pagina's verbeteren
Er is een volledige van advies over het verbeteren van uw antwoorden van de server en de laadtijden voor pagina's, zodat we probeert Won't alle hier herhalen web. Hier volgen enkele tips die u waarschijnlijk al kent, net voor je denkt:

* Langzaam laden vanwege de grote bestanden: asynchroon laden van de scripts en andere onderdelen. Gebruik script bundeling. De hoofdpagina opdelen in widgets die hun gegevens afzonderlijk laden. Gewone oude HTML voor lange tabellen niet verzenden: een script gebruiken om aan te vragen van de gegevens als JSON of andere gecomprimeerde indeling en klik vervolgens vult u de tabel in plaats. Er zijn een goede frameworks om te helpen bij dit alles. (Zij ook inhouden big scripts natuurlijk.)
* Trage server afhankelijkheden: Houd rekening met de geografische locaties van de onderdelen van uw. Bijvoorbeeld, als u Azure gebruikt, zorg ervoor dat de webserver en de database zich in dezelfde regio. Query's meer gegevens dan ze nodig hebben ophalen? Caching of help batchverwerking zou?
* Capaciteit problemen: Bekijk de metrische servergegevens van responstijden en het aantal aanvragen. Als de responstijden pieksnelheden niet goed met pieken in aanvraag aantallen, is het waarschijnlijk dat uw servers worden uitgerekt.


## <a name="server-response-time-degradation"></a>Antwoord tijd verslechtering van server

De melding antwoord tijd vermindering vertelt u:

* De reactietijd vergeleken met normale reactietijd voor deze bewerking.
* Hoeveel gebruikers zijn beïnvloed.
* Gemiddelde responstijd en 90 percentiel reactietijd voor deze bewerking op de dag van de detectie en zeven dagen voor. 
* Telling van de aanvragen van deze bewerking op de dag van de detectie en zeven dagen voor.
* Correlatie tussen vermindering in deze bewerking en degradations in de bijbehorende afhankelijkheden. 
* Koppelingen naar informatie waarmee u het probleem te onderzoeken.
  * Profiler traceringen weer te geven waar bewerkingstijd besteed aan het (de koppeling is beschikbaar als Profiler trace voorbeelden voor deze bewerking tijdens de detectieperiode zijn verzameld). 
  * Prestatierapporten in metriek Explorer kunt u segmenteren en Tijdfilters bereik voor deze bewerking te analyseren.
  * Zoeken naar deze aanroepen om specifieke aanroepen eigenschappen weer te geven.
  * Fout rapporteert - als tellen > 1 dit betekenen dat er fouten zijn opgetreden tijdens deze bewerking dat mogelijk hebben bijgedragen tot verminderde prestaties.

## <a name="dependency-duration-degradation"></a>Verslechtering van de duur van de afhankelijkheid

Moderne toepassingen vast meer ontwerpbenadering micro services, wat in veel gevallen leidt tot zware betrouwbaarheid op externe services. Bijvoorbeeld, als uw toepassing is afhankelijk van bepaalde gegevensplatform of zelfs als Maak u uw eigen bot u waarschijnlijk op een aantal cognitieve serviceprovider waarmee uw bots communiceren op meer menselijke manieren wordt relay-service en sommige data store-service voor bot voor het ophalen van de antwoorden van.  

Voorbeeld afhankelijkheid vermindering melding:

![Hier volgt een voorbeeld van afhankelijkheid duur vermindering detectie](./media/app-insights-proactive-diagnostics/dependency_duration_degradation.png)

U ziet dat het vertelt u:

* De duur in vergelijking met normale reactietijd voor deze bewerking
* Hoeveel gebruikers worden beïnvloed
* Gemiddelde duur en 90 percentiel duur voor deze afhankelijkheid van de dag van de detectie en zeven dagen voor
* Aantal afhankelijkheid aanroepen op de dag van de detectie en zeven dagen voor
* Koppelingen naar informatie waarmee u het probleem te onderzoeken
  * Prestatierapporten in Verkenner metrische gegevens voor deze afhankelijkheid
  * Zoek naar deze afhankelijkheidsaanroepen aanroepen eigenschappen weergeven
  * Fout rapporteert - als tellen > 1 dit betekenen dat er waren afhankelijkheidsaanroepen mislukt tijdens de detectieperiode die mogelijk hebben bijgedragen tot verslechtering van de duur. 
  * Open Analytics met query's die deze afhankelijkheid duur en het aantal berekenen  

## <a name="smart-detection-of-slow-performing-patterns"></a>Slimme detectie van trage presterende patronen 

Application Insights vindt prestatieproblemen die mogelijk alleen van invloed op een gedeelte van uw gebruikers of alleen van invloed op gebruikers in sommige gevallen. Is bijvoorbeeld meldingen over het laden van pagina's trager op één type browser dan op andere typen browsers, of als aanvragen langzamer worden geleverd vanuit een bepaalde server. Het kan ook problemen met de combinaties van eigenschappen, detecteren, zoals trage pagina wordt geladen in een geografisch gebied voor clients met specifieke besturingssysteem.  

Afwijkingen zoals deze zijn moeilijk te detecteren door de gegevens te bekijken, maar komen vaker dan u denkt. Vaak surface ze alleen wanneer uw klanten klagen. Die tijd is te laat: de betreffende gebruikers zijn al overschakelen naar uw concurrenten!

Op dit moment kijken onze algoritmen laadtijden voor pagina's, reactietijden van de aanvraag op de server en reactietijden van de afhankelijkheid.  

U hoeft niet te drempelwaarden instelt of regels configureren. Machine learning en gegevensanalyse-algoritmen worden gebruikt voor het detecteren van afwijkende patronen.

![Klik op de koppeling om het diagnostisch rapport openen in Azure uit de e-mailmelding](./media/app-insights-proactive-performance-diagnostics/03.png)

* **Wanneer** geeft de tijd die het probleem is gedetecteerd.
* **Wat** wordt beschreven:

  * Het probleem dat is gedetecteerd;
  * De kenmerken van de reeks gebeurtenissen die we vinden weergegeven het gedrag van het probleem.
* De tabel vergelijkt de set slecht presterende met het gemiddelde gedrag van alle andere gebeurtenissen.

Klik op de koppelingen om open metriek Explorer en zoek op de relevante rapporten, gefilterd op de tijd en de eigenschappen van de langzaam presterende set.

Het tijdsbereik en filters om te verkennen de telemetrie wijzigen.

## <a name="next-steps"></a>Volgende stappen
Deze diagnostische hulpprogramma's kunnen u de telemetrie van uw app te controleren:

* [Profiler](app-insights-profiler.md) 
* [Momentopname-foutopsporingsprogramma](app-insights-snapshot-debugger.md)
* [Analytische gegevens](app-insights-analytics-tour.md)
* [Diagnostische gegevens voor de analyse van de smartcard](app-insights-analytics-diagnostics.md)

Smart detecties zijn volledig automatisch. Maar misschien u sommige meer waarschuwingen instellen?

* [Handmatig geconfigureerde metrische waarschuwingen](app-insights-alerts.md)
* [Webtests voor beschikbaarheid](app-insights-monitor-web-app-availability.md)
