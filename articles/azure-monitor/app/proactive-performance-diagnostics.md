---
title: 'Slimme detectie: afwijkende prestaties | Microsoft Docs'
description: Application Insights voert slimme analyse van uw app-Telemetrie en waarschuwt u mogelijke problemen. Deze functie heeft geen installatie nodig.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 6acd41b9-fbf0-45b8-b83b-117e19062dd2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/04/2017
ms.reviewer: antonfr
ms.author: mbullwin
ms.openlocfilehash: b1a3b04427839736359c88f8ad6a8db5eedf8488
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54121240"
---
# <a name="smart-detection---performance-anomalies"></a>Slimme detectie: afwijkende prestaties

[Application Insights](../../azure-monitor/app/app-insights-overview.md) automatisch analyseert de prestaties van uw web-App, en wordt u gewaarschuwd over mogelijke problemen. U mogelijk is het lezen van dit omdat u een van onze Slimme detectie-meldingen ontvangen.

Deze functie is vereist geen speciale instellingen, dan uw app configureren voor Application Insights (op [ASP.NET](../../azure-monitor/app/asp-net.md), [Java](../../azure-monitor/app/java-get-started.md), of [Node.js](../../azure-monitor/app/nodejs.md), en in [webpagina code](../../azure-monitor/app/javascript.md)). Deze is actief wanneer uw app zoveel telemetrie genereert.

## <a name="when-would-i-get-a-smart-detection-notification"></a>Wanneer kan ik een melding voor slimme detectie krijgen?

Application Insights heeft gedetecteerd dat de prestaties van uw toepassing is gedegradeerd in een van de volgende manieren:

* **Degradatie van de serverreactietijd** -uw app is gestart langzamer dan dat het reageren op aanvragen. De wijziging mogelijk snelle, bijvoorbeeld omdat er een regressie in de meest recente implementatie. Of deze misschien is de geleidelijke, mogelijk veroorzaakt door een geheugenlek. 
* **Degradatie van de duur van de afhankelijkheid** -uw app doet aanroepen naar een REST-API, database, of andere afhankelijkheden. De afhankelijkheid is langzamer dan dat het reageert.
* **Trage prestaties patroon** -uw app wordt weergegeven om een prestatieprobleem dat alleen bepaalde aanvragen beïnvloedt. Bijvoorbeeld, worden pagina's trager geladen op een bepaald type van de browser dan andere; of aanvragen langzamer worden aangeboden in een bepaalde server. Op dit moment kijken onze algoritmen laadtijden voor pagina's, aanvragen, reactietijden, en reactietijden voor afhankelijkheden.  

Slimme detectie vereist ten minste acht dagen telemetrie bij een werkbare volume om te kunnen maken van een basislijn van de normale prestaties. Dus nadat de toepassing actief is geweest gedurende die periode, resulteert een aanzienlijke probleem in een melding.


## <a name="does-my-app-definitely-have-a-problem"></a>Beschikt over mijn app absoluut een probleem?

Nee, een melding betekent niet dat uw app beslist een probleem is. Het is gewoon een suggestie over iets dat kunt u om te kijken naar meer nauwkeurig.

## <a name="how-do-i-fix-it"></a>Hoe herstel ik deze?

De meldingen betreffen: diagnostische gegevens. Hier volgt een voorbeeld:


![Hier volgt een voorbeeld van detectie van degradatie van de serverreactietijd](media/proactive-performance-diagnostics/server_response_time_degradation.png)

1. **Sorteren**. De melding ziet u hoeveel gebruikers of het aantal bewerkingen worden beïnvloed. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **Bereik**. Het probleem invloed heeft op al het verkeer of slechts enkele pagina's? Is deze beperkt tot bepaalde browsers of locaties? Deze gegevens kan worden opgehaald van de melding.
3. **Diagnose**. De diagnostische gegevens in de melding wordt vaak de aard van het probleem voorgesteld. Bijvoorbeeld, als reactietijd wordt vertraagd als aanvraagsnelheid hoog is, met suggesties dat van uw server of afhankelijkheden overbelast zijn. 

    Anders opent u de blade Performance in Application Insights. Hier vindt u [Profiler](profiler.md) gegevens. Als er uitzonderingen worden veroorzaakt, kunt u ook proberen de [snapshot debugger](../../azure-monitor/app/snapshot-debugger.md).



## <a name="configure-email-notifications"></a>E-mailmeldingen configureren

Slimme detectie meldingen zijn standaard ingeschakeld en verzonden naar gebruikers die beschikken over [eigenaren, bijdragers en lezers toegang tot de Application Insights-resource](../../azure-monitor/app/resources-roles-access-control.md). Om dit te wijzigen, klik **configureren** in het e-mailmelding, of open Slimme detectie-instellingen in Application Insights. 
  
  ![Instellingen voor slimme detectie](media/proactive-performance-diagnostics/smart_detection_configuration.png)
  
  * U kunt de **Afmelden** koppeling in de e-mail Slimme detectie om te stoppen met het ontvangen van de e-mailmeldingen.

E-mailberichten over slimme detectie prestatieafwijkingen zijn beperkt tot één e-mailadres per dag per Application Insights-resource. Het e-mailbericht ontvangt alleen als er ten minste één nieuwe probleem dat is gedetecteerd op die dag. U kunt herhalingen van elk bericht wordt niet ophalen. 

## <a name="faq"></a>Veelgestelde vragen

* *Medewerkers van Microsoft is dus kijken van mijn gegevens?*
  * Nee. De service is volledig automatisch. Alleen krijgt u de meldingen. Uw gegevens [persoonlijke](../../azure-monitor/app/data-retention-privacy.md).
* *U alle met Application Insights verzamelde gegevens analyseren?*
  * Niet op dit moment. We analyseren op dit moment aanvraag laadtijd reactietijd, de reactietijd voor afhankelijkheden en de pagina. Analyse van aanvullende metrische gegevens is op onze achterstand benieuwd.

* Welke typen toepassingen werkt dit voor?
  * Deze degradations worden gedetecteerd in elke toepassing die de juiste telemetrie genereert. Als u Application Insights in uw web-app hebt geïnstalleerd, worden klikt u vervolgens aanvragen en de afhankelijkheden automatisch bijgehouden. Maar in de back-endservices of andere apps, als u aanroepen naar ingevoegd [TrackRequest()](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest) of [TrackDependency](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency), en vervolgens Slimme detectie op dezelfde manier werkt.

* *Kan ik mijn eigen anomaliedetectie detectieregels maken of bestaande regels aanpassen?*

  * Nog niet, maar u kunt:
    * [Waarschuwingen instellen](../../azure-monitor/app/alerts.md) waarmee wordt aangegeven dat wanneer een metriek een drempelwaarde overschrijdt.
    * [Telemetrie exporteren](../../azure-monitor/app/export-telemetry.md) naar een [database](../../azure-monitor/app/code-sample-export-sql-stream-analytics.md) of [naar PowerBI](../../azure-monitor/app/export-power-bi.md ), waar u ze kunt analyseren zelf.
* *Hoe vaak wordt de analyse uitgevoerd?*

  * We dagelijks wordt uitgevoerd de analyse op de telemetrie van de vorige dag (volledige dag in UTC-tijdzone).
* *Daarom wordt deze vervangen [metrische waarschuwingen](../../azure-monitor/app/alerts.md)?*
  * Nee.  We doorvoeren niet naar elke die u kunt abnormaal overwegen gedrag detecteren.


* *Als ik iets in reactie op een melding niet doet, krijg ik een herinnering?*
  * Nee, u krijgt een bericht over elk probleem slechts één keer. Als het probleem zich blijft voordoen worden, deze bijgewerkt in de Slimme detectie blade-feed.
* *Kan ik het e-mailbericht is verbroken. Waar vind ik de meldingen in de portal?*
  * Klik in het overzicht van de Application Insights van uw app op de **Slimme detectie** tegel. U moet er kunnen alle meldingen van 90 dagen back vinden.

## <a name="how-can-i-improve-performance"></a>Hoe kan ik de prestaties verbeteren?
Trage en mislukte antwoorden zijn een van de grootste frustraties voor gebruikers van de website, zoals u uit eigen ervaring weet. Het is dus belangrijk dat u de problemen op te lossen.

### <a name="triage"></a>Sorteren
Eerst maakt het uit? Als een pagina is altijd langzaam wordt geladen, maar slechts 1% van de gebruikers van uw site hebt om te kijken naar het, misschien hebt u belangrijkere zaken te denken. Anderzijds, als er slechts 1% van de gebruikers openen, maar dit uitzonderingen telkens genereert, die mogelijk onderzoeken waard.

De instructie impact (betrokken gebruikers of % van het verkeer) gebruiken als algemene richtlijn, maar houd er rekening mee dat dit niet het hele verhaal. Verzamel andere bewijs om te bevestigen.

Houd rekening met de parameters van het probleem. Als deze afhankelijk van het Geografie is, stelt u [beschikbaarheidstests](../../azure-monitor/app/monitor-web-app-availability.md) met inbegrip van die regio: er gewoon mogelijk netwerkproblemen op dit gebied.

### <a name="diagnose-slow-page-loads"></a>Diagnose traag laden van pagina
Waar is het probleem? Is de server traag reageren, is de pagina erg lang duurt of is de browser nodig om zich veel werk weer te geven?

Open de blade Browsers met metrische gegevens. De gesegmenteerde weergave van de browser laden tijd wordt aangegeven waar de gaan. 

* Als **aanvraagtijd verzenden** is hoog, de server reageert langzaam of de aanvraag is een bericht met een grote hoeveelheid gegevens. Bekijk de [maatstaven voor prestaties](../../azure-monitor/app/web-monitor-performance.md#metrics) voor het onderzoeken van reactietijden.
* Instellen van [bijhouden van afhankelijkheid](../../azure-monitor/app/asp-net-dependencies.md) om te zien of de traagheid veroorzaakt door externe services of uw database wordt.
* Als **antwoord ontvangen van** overheersende, wordt de pagina en de afhankelijke onderdelen - JavaScript, CSS, afbeeldingen en verder (maar niet asynchroon geladen gegevens) lang zijn. Instellen van een [beschikbaarheidstest](../../azure-monitor/app/monitor-web-app-availability.md), en zorg ervoor dat u de optie voor het laden van afhankelijke onderdelen. Wanneer u bepaalde resultaten, opent u de details van een resultaat en vouwt u het om te zien hoe lang het laden van verschillende bestanden.
* Hoge **Client verwerkingstijd** stelt scripts langzaam worden uitgevoerd. Als de reden niet duidelijk is, kunt u code timing toevoegen en verzendtijden in trackMetric aanroepen.

### <a name="improve-slow-pages"></a>Langzame pagina's verbeteren
Er is een volledige van advies over het verbeteren van uw antwoorden van de server en de laadtijden voor pagina's, zodat we niet wordt geprobeerd alle hier herhalen web. Hier volgen enkele tips die u waarschijnlijk al kent, alleen voor u overweegt:

* Het traag laden vanwege de grote bestanden: De scripts en andere onderdelen asynchroon worden geladen. Gebruik het script bundeling. De hoofdpagina opsplitsen in widgets die hun gegevens afzonderlijk worden geladen. Plain oude HTML-code voor grote tabellen niet verzenden: een script gebruiken om aan te vragen van de gegevens als JSON of andere compacte indeling en vervolgens de tabel in de plaats. Er zijn om te helpen bij al deze geweldige-frameworks. (Ze ook leiden tot grote scripts, natuurlijk.)
* Trage afhankelijkheden: Houd rekening met de geografische locaties van de onderdelen. Bijvoorbeeld, als u Azure gebruikt, moet u ervoor dat de webserver en de database zich in dezelfde regio. Query's worden opgehaald meer gegevens dan ze nodig hebben? Caching aan of batchverwerking help zou?
* Capaciteit problemen: Ga naar de server metrische gegevens van reactietijden en het aantal aanvragen. Als de reactietijden pieken onevenredig met pieken in het aantal aanvragen, is het waarschijnlijk dat uw servers worden uitgebreid.


## <a name="server-response-time-degradation"></a>Degradatie van de serverreactietijd

De reactie tijd verslechtering van de melding vertelt u:

* De reactietijd vergeleken met normale reactietijd voor deze bewerking.
* Hoeveel gebruikers zijn beïnvloed.
* Gemiddelde reactietijd en 90e percentiel reactietijd voor deze bewerking op de dag van de detectie- en zeven dagen voor de. 
* Het aantal aanvragen voor deze bewerking op de dag van de detectie en zeven dagen voor de.
* Correlatie tussen degradatie bij deze bewerking en degradations in de bijbehorende afhankelijkheden. 
* Koppelingen naar informatie waarmee u het probleem vaststellen.
  * Profiler-traceringen om te geven waar bewerkingstijd is besteed (de koppeling is beschikbaar als voorbeelden van Profiler trace voor deze bewerking tijdens de detectieperiode zijn verzameld). 
  * Rapporten in Metric Explorer, waar u kunt en servertelemetrie Tijdfilters bereik voor deze bewerking.
  * Zoeken naar deze aanroep om de aanroep van de specifieke eigenschappen weer te geven.
  * Fout rapporteert - als tellen van > 1 dit betekenen dat er fouten zijn opgetreden in deze bewerking die mogelijk hebben bijgedragen tot verminderde prestaties.

## <a name="dependency-duration-degradation"></a>Degradatie van de duur van afhankelijkheid

Moderne toepassingen vast meer microservices ontwerpbenadering, die in veel gevallen leiden tot zware betrouwbaarheid op externe services. Bijvoorbeeld, als uw toepassing afhankelijk van sommige gegevensplatform of zelfs als is ontwikkelen u uw eigen bot u waarschijnlijk op sommige cognitive services-provider om in te schakelen uw bots kunnen reageren op een nog mensachtigere wijze wordt relay-service en sommige data store-service voor bot voor het ophalen van de antwoorden van m.  

Voorbeeld van de afhankelijkheid verslechtering van de melding:

![Hier volgt een voorbeeld van detectie van verslechtering van de duur van afhankelijkheid](media/proactive-performance-diagnostics/dependency_duration_degradation.png)

U ziet dat u leest:

* De duur in vergelijking met normale reactietijd voor deze bewerking
* Hoeveel gebruikers zijn beïnvloed
* Gemiddelde duur en 90e percentiel duur voor deze afhankelijkheid op de dag van de detectie en zeven dagen voor de
* Aantal afhankelijkheden wordt op de dag van de detectie en zeven dagen voor de
* Koppelingen naar informatie waarmee u het probleem vaststellen
  * Rapporten in Metric Explorer voor deze afhankelijkheid
  * Zoek naar deze afhankelijkheidsaanroepen aanroepen eigenschappen weergeven
  * Mislukte rapporten - als aantal > 1 Dit betekent dat er mislukte afhankelijkheid zijn aangeroepen tijdens de detectieperiode die mogelijk hebben bijgedragen tot verslechtering van de duur. 
  * Analytics openen met query's die deze duur van de afhankelijkheid en aantal berekenen  

## <a name="smart-detection-of-slow-performing-patterns"></a>Slimme detectie van trage prestaties patronen 

Application Insights wordt gezocht naar prestatieproblemen die mogelijk alleen van invloed op een gedeelte van uw gebruikers of alleen van invloed op gebruikers in sommige gevallen. Melding over het laden van pagina's is bijvoorbeeld langzamer zijn op één type browser dan op andere typen browsers, of als aanvragen langzamer worden uitgevoerd vanaf een bepaalde server. Deze kan ook problemen met de combinaties van de eigenschappen vinden, zoals trage pagina wordt geladen in één geografisch gebied voor clients met behulp van specifieke besturingssysteem.  

Afwijkingen zoals deze zijn zeer moeilijk te detecteren door te inspecteren van de gegevens, maar zijn vaker voor dan u denkt. Vaak ze alleen surface wanneer uw klanten klagen. Op dat moment zijn opgelost, het is te laat: de betrokken gebruikers zijn al overschakelen naar uw concurrenten!

Op dit moment kijken onze algoritmen laadtijden voor pagina's, de reactietijden aanvraag op de server, en reactietijden voor afhankelijkheden.  

U hebt geen drempelwaarden instelt of regels configureren. Machine learning en gegevensanalyse-algoritmen worden gebruikt voor het detecteren van afwijkende patronen.

![Klik op de koppeling als u wilt het diagnostische rapport openen in Azure uit de e-mailmelding](./media/proactive-performance-diagnostics/03.png)

* **Wanneer** geeft de tijd die het probleem is gedetecteerd.
* **Wat** wordt beschreven:

  * Het probleem dat is gevonden.
  * De kenmerken van de reeks gebeurtenissen die we vinden weergegeven het gedrag van het probleem.
* De tabel vergelijkt de set slecht presterende met het gemiddelde gedrag van alle andere gebeurtenissen.

Klik op de koppelingen om de Metric Explorer en zoeken op relevante rapporten, gefilterd op de tijd en de eigenschappen van de traag presterende set opent.

Wijzig het tijdsbereik en filters om het verkennen van de telemetrie.

## <a name="next-steps"></a>Volgende stappen
Deze diagnostische hulpprogramma's kunnen u de telemetrie van uw app controleren:

* [Profiler](profiler.md) 
* [Snapshot debugger](../../azure-monitor/app/snapshot-debugger.md)
* [Analytische gegevens](../../azure-monitor/log-query/get-started-portal.md)
* [Analytics slimme diagnostische gegevens](../../azure-monitor/app/analytics.md)

Slimme detectie worden volledig automatisch uitgevoerd. Maar misschien u graag enkele meer waarschuwingen instellen?

* [Handmatig geconfigureerde metrische waarschuwingen](../../azure-monitor/app/alerts.md)
* [Webtests voor beschikbaarheid](../../azure-monitor/app/monitor-web-app-availability.md)
