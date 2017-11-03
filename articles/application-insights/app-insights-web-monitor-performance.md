---
title: De status en informatie over het gebruik met Application Insights van uw app bewaken
description: Aan de slag met Application Insights. Analyseer gebruik, beschikbaarheid en prestaties van uw on-premises of de Microsoft Azure-toepassingen.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: mbullwin
ms.openlocfilehash: 32000f5a85c84913aa820df00f1bb7f877bf037f
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="monitor-performance-in-web-applications"></a>Prestaties in webtoepassingen controleren


Zorg ervoor dat uw toepassing goed presteert en vindt u informatie snel over fouten. [Application Insights] [ start] u vertellen over prestatieproblemen en uitzonderingen en kunt u vinden en de hoofdoorzaken analyseren.

Application Insights kunnen Java- en ASP.NET-webtoepassingen en services, WCF-services bewaken. Ze kunnen worden gehost op locatie, op virtuele machines, of als Microsoft Azure websites. 

Application Insights kan duren voordat telemetrie van webpagina's en een groot aantal apparaten met iOS, Android en Windows Store-apps aan de clientzijde.

>[!Note]
> We hebben een nieuwe ervaring beschikbaar gesteld voor langzame pagina's uitvoeren in uw webtoepassing kan vinden. Als u geen toegang tot deze, inschakelen door het configureren van de preview-opties, waarbij de [Preview blade](app-insights-previews.md). Meer informatie over deze nieuwe ervaring in [opsporen en oplossen van knelpunten met de interactieve prestaties onderzoek](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).

## <a name="setup"></a>Instellen van de bewaking van toepassingsprestaties
Als u dit nog niet hebt (dat wil zeggen, als er geen ApplicationInsights.config) nog Application Insights toegevoegd aan uw project, kies een van de volgende manieren aan de slag:

* [ASP.NET-web-apps](app-insights-asp-net.md)
  * [Uitzondering bewaking toevoegen](app-insights-asp-net-exceptions.md)
  * [Bewaking van afhankelijkheid toevoegen](app-insights-monitor-performance-live-website-now.md)
* [J2EE-web-apps](app-insights-java-get-started.md)
  * [Bewaking van afhankelijkheid toevoegen](app-insights-java-agent.md)

## <a name="view"></a>Maatstaven voor prestaties verkennen
In [de Azure-portal](https://portal.azure.com), blader naar de Application Insights-resource die u hebt ingesteld voor uw toepassing. De overzichtsblade ziet u eenvoudige prestatiegegevens:

Klik op een grafiek om meer details en om resultaten te bekijken voor een langere periode. Klik bijvoorbeeld op de tegel aanvragen en selecteer vervolgens een tijdsbereik:

![Klik verder voor meer gegevens en selecteer een tijdsbereik](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

Klik op een grafiek om te kiezen welke metrische gegevens worden weergegeven, of Voeg een nieuwe grafiek toe en selecteer de metrische gegevens:

![Klik op een grafiek om metrische gegevens te selecteren](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> **Schakel het selectievakje van de metrische gegevens** om te zien van de volledige selectie die beschikbaar is. De metrische gegevens kunnen worden onderverdeeld in groepen; Wanneer een lid van een groep is geselecteerd, worden alleen de andere leden van die groep weergegeven.

## <a name="metrics"></a>Wat doet u alle gemiddelde? Tegels van de prestaties en rapporten
Er zijn verschillende maatstaven voor prestaties die kunt u krijgen. Laten we beginnen met degenen die standaard worden weergegeven op de blade van de toepassing.

### <a name="requests"></a>Aanvragen
Het aantal HTTP-aanvragen ontvangen in een opgegeven periode. Vergelijk deze met de resultaten op andere rapporten om te zien hoe uw app omgaat als de belasting varieert.

HTTP-aanvragen bevatten alle GET of POST-aanvragen voor pagina's, gegevens en installatiekopieën.

Klik op de tegel voor tellingen voor specifieke URL's.

### <a name="average-response-time"></a>Gemiddelde reactietijd
Hiermee wordt de tijd tussen een webaanvraag invoeren van uw toepassing en het antwoord.

De punten weergeven zwevend gemiddelde. Als er een groot aantal aanvragen, is er mogelijk een aantal die afwijken van het gemiddelde zonder een duidelijke piek of dip in de grafiek.

Zoek naar ongebruikelijke pieken. In het algemeen verwachten reactietijd toenemen met een toename van aanvragen. Als de stijging onevenredige is, is het mogelijk dat uw app een limiet resource zoals CPU of de capaciteit van een service die wordt gebruikt roept.

Klik op de tegel voor tijden voor specifieke URL's.

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a>Traagste aanvragen
![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

Toont welke aanvragen mogelijk prestaties afstemmen.

### <a name="failed-requests"></a>Mislukte aanvragen
![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

Een aantal aanvragen dat niet-onderschepte uitzonderingen heeft geretourneerd.

Klik op de tegel om de details van specifieke problemen en selecteert u een afzonderlijke aanvraag naar de details. 

Alleen een representatieve steekproef van fouten wordt voor afzonderlijke inspectie bewaard.

### <a name="other-metrics"></a>Andere metrische gegevens
Om te zien wat stelt andere metrische gegevens weergeven, klikt u op een grafiek en schakelt u de metrische gegevens voor een overzicht van de volledige beschikbaar. Klik (i) voor elke metriek definitie.

![Schakel alle metrische gegevens voor een overzicht van de hele set](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)

Een metriek selecteren, schakelt anderen die op dezelfde grafiek kan niet worden weergegeven.

## <a name="set-alerts"></a>Waarschuwingen instellen
Ontvangen van e-mailadres van ongebruikelijke waarden van alle metrische gegevens, moet u een waarschuwing toevoegen. U kunt ofwel het e-mailbericht verzenden naar de accountbeheerders, of specifieke e-mailadressen.

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

De bron voor de overige eigenschappen instellen. Kies de bronnen webtest geen als u wilt waarschuwingen instellen voor prestaties of gebruik metrische gegevens.

Wees voorzichtig te weten de eenheden waarin u wordt gevraagd om in te voeren van de drempelwaarde.

*Ik ziet de waarschuwing toevoegen knop niet.* -Is dit een groep account waartoe u alleen-lezen toegang hebben? Neem contact op met de accountbeheerder.

## <a name="diagnosis"></a>Oplossen van problemen
Hier volgen enkele tips voor het zoeken en onderzoeken van prestatieproblemen:

* Instellen van [webtests] [ availability] om te worden gewaarschuwd als uw website uitvalt of onjuist of traag reageert. 
* Vergelijk het aantal verzoeken met andere metrische gegevens om te controleren of het mislukte of trage reactie laden zijn gerelateerd.
* [Invoegen en zoek trace-instructies] [ diagnostic] in uw code te helpen problemen te lokaliseren.
* Bewaken van uw Web-app in bewerking met [livestream metrische gegevens][livestream].
* De status van uw .net-toepassing met vastleggen [momentopname foutopsporingsprogramma][snapshot].

>[!Note]
> We zijn bezig de overgang van Application Insights prestaties onderzoek naar een interactieve ervaring voor volledig scherm. De volgende documentatie bevat informatie over de nieuwe ervaring eerst en controleert vervolgens de ervaring geval u die nog steeds nodig om deze te openen, maar niet beschikbaar in de overgang.

## <a name="find-and-fix-performance-bottlenecks-with-an-interactive-full-screen-performance-investigation"></a>Zoek en corrigeer de knelpunten met een interactieve volledig scherm prestaties onderzoek

U kunt het nieuwe Application Insights interactieve prestaties onderzoek trage presterende bewerkingen te controleren in uw Web-app. Kunt u snel een specifieke trage bewerking selecteren en gebruik [Profiler](app-insights-profiler.md) hoofdmap leiden tot de trage bewerkingen naar beneden code. Met behulp van de nieuwe duur distributie weergegeven voor de geselecteerde bewerking dat kunt u snel in één oogopslag beoordelen hoe ernstig de ervaring is voor uw klanten. In feite voor elke trage bewerking kunt u zien hoeveel van uw interactie van gebruikers zijn beïnvloed. In het volgende voorbeeld hebben we besloten te laten de ervaring voor klanten/Details ophalen bewerking nader bekijken. In de duur van de distributie zien we dat er drie pieken zijn. Meest linkse piek is ongeveer 400ms en geweldige responsief ervaring vertegenwoordigt. Middelste piek is ongeveer 1.2s en vertegenwoordigt een middelmatige ervaring. Ten slotte op de 3.6s hebben we een andere kleine piek die de 99th percentiel ervaring, waarschijnlijk tot onze klanten te laten ontevreden vertegenwoordigt. Ervaring is tien keer langzamer dan de uitmuntende ervaring voor dezelfde bewerking. 

![GET-klanten/Details drie duur pieken](./media/app-insights-web-monitor-performance/PerformanceTriageViewZoomedDistribution.png)

Als u een beter beeld van de gebruikerservaringen voor deze bewerking, kunt we een grotere tijdsbereik selecteren. We kunnen vervolgens ook afbakenen in de tijd op een specifiek tijdvenster waarop de bewerking bijzonder traag is. In het volgende voorbeeld hebben we van de standaardwaarde 24 uur tijdsbereik voor de zeven dagen tijdsbereik en vervolgens ingezoomd op het tijdvenster 9:47 tot 12:47 tussen di de 12de en Wed de 13 overgeschakeld. Houd er rekening mee dat zowel de duur van de distributie en het aantal profiler en voorbeelden traceringen zijn bijgewerkt aan de rechterkant.

![Bekijk klanten/informatie drie duur pieken in 7 dagen met een tijdvenster liggen](./media/app-insights-web-monitor-performance/PerformanceTriageView7DaysZoomedTrend.png)

Om te beperken op de trage ervaringen, Inzoomen we vervolgens de duur die tussen 95th en de 99th percentiel vallen. Dit zijn de 4% van de interactie van gebruikers die bijzonder traag zijn.

![Bekijk klanten/informatie drie duur pieken in 7 dagen met een tijdvenster liggen](./media/app-insights-web-monitor-performance/PerformanceTriageView7DaysZoomedTrendZoomed95th99th.png)

We kunnen nu beide blik op de representatieve voorbeelden te klikken op de knop voorbeelden, of de traceringen representatief profiler door te klikken op de knop Profiler-traceringen. In dit voorbeeld zijn er vier traceringen die zijn verzameld voor klanten/Details ophalen in het venster en bereik duur van belang.

Het probleem worden soms niet in uw code, maar in plaats daarvan in een afhankelijkheid die u code aanroepen. U kunt overschakelen naar het tabblad afhankelijkheden in de weergave prestaties selectie voor het onderzoeken van dergelijke trage afhankelijkheden. Standaard is de Prestatieweergave trends gemiddelden, maar wat u wilt om te kijken naar de 95e percentiel is (of de 99th als u een zeer volwassen service bewaakt). In het volgende voorbeeld hebben we gericht op de trage Azure BLOB-afhankelijkheid, waar we PUT fabrikamaccount noemen. Het cluster goed ervaringen rond 40ms, terwijl de trage aanroepen naar de dezelfde afhankelijkheid drie keer langzamer zijn, ongeveer 120ms clustering. Deze wordt pas van veel van deze aanroepen om op te tellen leiden dat de betreffende bewerking aanzienlijk vertragen. U kunt inzoomen op de representatieve voorbeelden en profiler traceringen, net zoals u op het tabblad bewerkingen kunt.

![Bekijk klanten/informatie drie duur pieken in 7 dagen met een tijdvenster liggen](./media/app-insights-web-monitor-performance/SlowDependencies95thTrend.png)

Een andere echt krachtige functie die nieuw is voor het onderzoek interactieve volledig scherm prestaties is de integratie met insights. Application Insights kunnen opsporen en surface als insights reactiesnelheid regressies evenals help u identificeren van algemene eigenschappen in de voorbeeldset die u besloten te concentreren op. De beste manier om te kijken naar alle van de beschikbare inzicht is overschakelen naar een periode van 30 dagen en selecteer vervolgens de algemene insights zien tussen alle bewerkingen voor de afgelopen maand.

![Bekijk klanten/informatie drie duur pieken in 7 dagen met een tijdvenster liggen](./media/app-insights-web-monitor-performance/Performance30DayOveralllnsights.png)

Application Insights in de nieuwe selectie in de Prestatieweergave kunt u de naalden niet vinden in de hooiberg die resulteren in slechte ervaring voor gebruikers van uw app letterlijk.

## <a name="deprecated-find-and-fix-performance-bottlenecks-with-a-narrow-bladed-legacy-performance-investigation"></a>Afgeschafte: Opsporen en oplossen van knelpunten met een onderzoek smalle bladed verouderde prestaties

Het verouderde onderzoek van Application Insights bladed prestaties kunt u vinden gebieden van uw Web-app die de algehele prestaties vertragen. U kunt zoeken naar specifieke pagina die vertragen en gebruikt de [Profiler](app-insights-profiler.md) om te traceren, de hoofdoorzaak van deze problemen naar beneden code. 

### <a name="create-a-list-of-slow-performing-pages"></a>Een lijst met langzame presterende pagina's maken 

De eerste stap voor het vinden van prestatieproblemen is voor een lijst van de traag reageert niet meer pagina's. De schermafbeelding hieronder ziet u met behulp van de blade Performance om een lijst van mogelijke's verder onderzoeken. Snel ziet u op deze pagina dat er een vertraging in de reactietijd van de app op ongeveer 18:00 uur en nogmaals op ongeveer 10 uur was. U ziet ook dat de GET-bewerking klantgegevens/bepaalde langlopende bewerkingen met een gemiddelde reactietijd van 507.05 milliseconden had. 

![Application Insights interactieve prestaties](./media/app-insights-web-monitor-performance/performance1.png)

### <a name="drill-down-on-specific-pages"></a>Inzoomen op bepaalde pagina 's

Zodra u een momentopname van de prestaties van uw app hebt, kunt u meer informatie krijgen op specifieke bewerkingen trage prestaties. Klik op elke bewerking in de lijst om de details te bekijken, zoals hieronder wordt weergegeven. U kunt zien als de prestaties is gebaseerd op een afhankelijkheid van de grafiek. U kunt ook zien hoeveel gebruikers de verschillende reactietijden heeft ondergaan. 

![Application Insights operations blade](./media/app-insights-web-monitor-performance/performance5.png)

### <a name="drill-down-on-a-specific-time-period"></a>Inzoomen op een specifieke tijdsperiode

Nadat u een punt in tijd voor het onderzoeken van hebt geïdentificeerd, kunt u inzoomen zelfs verder blik op de specifieke bewerkingen die er de oorzaak van zijn dat de vertraging van de prestaties. Als u klikt op in een bepaald punt in tijd krijgt u de details van de pagina, zoals hieronder wordt weergegeven. In het voorbeeld hieronder u ziet de bewerkingen die worden vermeld voor een bepaalde periode samen met de server reactiecodes en de duur van de bewerking zijn. Hebt u ook de url voor het openen van een TFS-werkitem als u moet deze informatie wordt verzonden naar uw ontwikkelteam.

![Application Insights tijdsegment](./media/app-insights-web-monitor-performance/performance2.png)

### <a name="drill-down-on-a-specific-operation"></a>Inzoomen op een specifieke bewerking

Nadat u een punt in tijd voor het onderzoeken van hebt geïdentificeerd, kunt u inzoomen zelfs verder blik op de specifieke bewerkingen die er de oorzaak van zijn dat de vertraging van de prestaties. Klik op een bewerking uit de lijst om de details van de bewerking te bekijken, zoals hieronder wordt weergegeven. In dit voorbeeld ziet u dat de bewerking is mislukt, en Application Insights de details van de uitzondering heeft voor de toepassing heeft gekregen. U kunt een TFS-werkitem opnieuw gemakkelijk van deze blade maken.

![Application Insights bewerking blade](./media/app-insights-web-monitor-performance/performance3.png)


## <a name="next"></a>Volgende stappen
[Webtests] [ availability] -webaanvragen verzonden naar uw toepassing met regelmatige tussenpozen van de hele wereld hebben.

[Vastleggen en zoeken van diagnostische traceringen] [ diagnostic] - traceringsaanroepen invoegen en de resultaten op de speldenpunt problemen doorzoeken.

[Gebruik bijhouden] [ usage] -weten hoe mensen uw toepassing gebruiken.

[Het oplossen van problemen] [ qna] - en Q & A



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[usage]: app-insights-web-track-usage.md
[livestream]: app-insights-live-stream.md
[snapshot]: app-insights-snapshot-debugger.md



