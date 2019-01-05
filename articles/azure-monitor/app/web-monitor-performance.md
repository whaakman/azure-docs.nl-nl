---
title: De status van uw app en het gebruik met Application Insights bewaken
description: Aan de slag met Application Insights. Analyseer gebruik, beschikbaarheid en prestaties van uw on-premises of de Microsoft Azure-toepassingen.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/10/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 1a75f05c84bd7c94aaa55403d3e605c6c7802325
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54056965"
---
# <a name="monitor-performance-in-web-applications"></a>Prestaties in webtoepassingen controleren


Zorg ervoor dat uw toepassing goed presteert en Ontdek snel fouten. [Application Insights] [ start] wordt informatie over eventuele prestatieproblemen en uitzonderingen, en kunt u vinden en diagnosticeren van de belangrijkste oorzaken.

Application Insights kunnen zowel Java als ASP.NET-webtoepassingen en services, WCF-services bewaken. Ze kunnen worden gehost on-premises, op virtuele machines, of als Microsoft Azure websites. 

Application Insights kan duren voordat telemetrie van webpagina's en een groot aantal apparaten, zoals iOS, Android en Windows Store-apps aan de clientzijde.

## <a name="setup"></a>Instellen van de bewaking van toepassingsprestaties
Als u hebt nog Application Insights aan uw project toegevoegd (dat wil zeggen, als er geen ApplicationInsights.config), kies een van de volgende manieren aan de slag:

* [ASP.NET-web-apps](../../azure-monitor/app/asp-net.md)
  * [Uitzondering bewaking toevoegen](../../azure-monitor/app/asp-net-exceptions.md)
  * [Afhankelijkheidsbewaking toevoegen](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [J2EE-web-apps](../../azure-monitor/app/java-get-started.md)
  * [Afhankelijkheidsbewaking toevoegen](../../azure-monitor/app/java-agent.md)

## <a name="view"></a>Verkennen van metrische gegevens voor prestaties
In [de Azure-portal](https://portal.azure.com), blader naar de Application Insights-resource die u hebt ingesteld voor uw toepassing. De overzichtsblade bevat algemene prestatiegegevens:

Klik op een grafiek voor meer details en om resultaten te bekijken voor een langere periode. Bijvoorbeeld, klikt u op de tegel aanvragen en selecteer vervolgens een tijdsbereik:

![Klik verder naar meer gegevens en selecteer een tijdsbereik](./media/web-monitor-performance/appinsights-48metrics.png)

Klik op een grafiek om te kiezen welke metrische gegevens worden weergegeven, of Voeg een nieuwe grafiek toe en selecteert u de metrische gegevens:

![Klik op een grafiek voor metrische gegevens kiezen](./media/web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> **Schakel alle metrische** om te zien van de volledige selectie dat beschikbaar is. De metrische gegevens kunnen worden onderverdeeld in groepen; Wanneer een lid van een groep is geselecteerd, worden alleen de andere leden van die groep weergegeven.

## <a name="metrics"></a>Wat doet deze alle gemiddelde? Prestaties van tegels en rapporten
Er zijn verschillende maatstaven voor prestaties die kunt u krijgen. Laten we beginnen met die standaard worden weergegeven op de toepassingsblade.

### <a name="requests"></a>Aanvragen
Het aantal HTTP-aanvragen ontvangen in een opgegeven periode. Vergelijk dit met de resultaten in andere rapporten om te zien hoe uw app zich gedraagt wanneer de belasting varieert.

HTTP-aanvragen bevatten alle GET of POST-aanvragen voor pagina's, gegevens en installatiekopieën.

Klik op de tegel om op te halen van tellingen voor specifieke URL's.

### <a name="average-response-time"></a>Gemiddelde reactietijd
Hiermee wordt de tijd tussen een webaanvraag invoeren van uw toepassing en het antwoord wordt geretourneerd.

De punten weergegeven zwevend gemiddelde. Als er veel aanvragen worden ingediend, is er mogelijk enkele die afwijken van het gemiddelde zonder een duidelijke piek of dip in de grafiek.

Zoeken naar ongebruikelijke pieken. In het algemeen verwachten reactietijd toe met een toename van aanvragen. Als de toename van onevenredige, uw app kan worden te maken met een resourcelimiet, zoals CPU of de capaciteit van een service die wordt gebruikt.

Klik op de tegel om terug te keren voor specifieke URL's ophalen.

![](./media/web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a>Langzaamste aanvragen
![](./media/web-monitor-performance/appinsights-44slowest.png)

Laat zien welke aanvragen mogelijk nodig hebt voor het afstemmen van prestaties.

### <a name="failed-requests"></a>Mislukte aanvragen
![](./media/web-monitor-performance/appinsights-46failed.png)

Aantal aanvragen dat heeft een niet-onderschepte uitzonderingen geretourneerd.

Klik op de tegel om te zien van de details van specifieke problemen en selecteer een afzonderlijke aanvraag voor de details. 

Alleen een representatieve steekproef van fouten wordt voor afzonderlijke inspectie bewaard.

### <a name="other-metrics"></a>Andere metrische gegevens
Om te zien wat andere metrische gegevens weergeven, klikt u op een grafiek en schakelt u alle metrische gegevens om te zien van de volledige beschikbaar stelt. Klik op (i) als de definitie van alle gegevens wilt weergeven.

![Schakel alle metrische gegevens om te zien van de hele set](./media/web-monitor-performance/appinsights-62allchoices.png)

Elke meetwaarde selecteren, schakelt de overige beheergroepen die niet kan worden weergegeven op de dezelfde grafiek.

## <a name="set-alerts"></a>Waarschuwingen instellen
Om te worden geïnformeerd via e-mail van ongebruikelijke waarden van elke meetwaarde, moet u een waarschuwing toevoegen. U kunt toekennen aan het e-mailbericht verzenden naar de accountbeheerders, of specifieke e-mailadressen.

![](./media/web-monitor-performance/appinsights-413setMetricAlert.png)

De resource voordat de andere eigenschappen instellen. Kies de resources die de webtest niet als u wilt instellen van waarschuwingen over metrische gegevens voor prestaties of het gebruik.

Wees voorzichtig om te weten de eenheden waarin u wordt gevraagd om in te voeren van de drempelwaarde.

*Ik zie niet de knop Waarschuwing toevoegen.* -Is dit een groep account die u alleen-lezentoegang hebt? Neem contact op met de accountbeheerder.

## <a name="diagnosis"></a>Problemen vaststellen
Hier volgen enkele tips voor het zoeken en oplossen van problemen met prestaties:

* Instellen van [webtests] [ availability] om te worden gewaarschuwd als uw site uitvalt of niet juist of langzaam reageert. 
* Vergelijken met het aantal aanvragen met andere metrische gegevens om te controleren of de mislukte of trage reactie zijn gerelateerd aan het laden.
* [Plaats en zoeken naar trace-instructies] [ diagnostic] in uw code aan problemen met deze functie.
* Bewaken van uw Web-app in de bewerking opnieuw uit met [Live Metrics Stream][livestream].
* De status van uw .net-toepassing met vastleggen [Snapshot Debugger][snapshot].

## <a name="find-and-fix-performance-bottlenecks-with-performance-investigation-experience"></a>Zoeken en herstellen van knelpunten met de ervaring voor het onderzoeken van prestaties

Traag presterende bewerkingen controleren in uw Web-app kunt u de ervaring voor het onderzoeken van prestaties. Kunt u snel een bepaalde trage bewerking selecteren en gebruik [Profiler](../../azure-monitor/app/profiler.md) hoofdmap ertoe leiden dat de trage bewerkingen omlaag naar de code. Met behulp van de van de nieuwe duurdistributie die wordt weergegeven voor de geselecteerde bewerking dat kunt u snel in een oogopslag beoordelen hoe ernstig de ervaring is voor uw klanten. U kunt zien hoeveel van uw gebruikersinteracties betrokken zijn voor elke trage bewerking. In het volgende voorbeeld hebben we besloten te nader bekijken op de ervaring voor de bewerking GET Customers/Details. In de duurdistributie, kunnen we zien dat er drie pieken zijn. Meest linkse piek is ongeveer 400 ms en geweldige reactie van de gebruikerservaring vertegenwoordigt. Middelste piek is rond 1.2 s en vertegenwoordigt een middelmatige ervaring. Ten slotte op de 3.6 s hebben we een andere small piek die het 99e percentiel ervaring die onze klanten te laten ontevreden leidt waarschijnlijk tot vertegenwoordigt. Deze ervaring is tien keer langzamer dan de geweldige ervaring voor dezelfde bewerking. 

![GET Customers/Details drie duur van pieken](./media/web-monitor-performance/PerformanceTriageViewZoomedDistribution.png)

Als u een beter beeld van de gebruikerservaringen voor deze bewerking, selecteren we een grotere tijdsbereik. We kunnen vervolgens ook verfijnen in de tijd op een specifieke periode waarin de bewerking traag is. In het volgende voorbeeld hebt we van de standaardwaarde 24 uur tijdsbereik voor de zeven dagen het tijdsbereik en vervolgens worden ingezoomd op het tijdvenster van 9:47 tot 12:47 tussen di de 12 en woe de 13 overgeschakeld. Zowel de van de duurdistributie en het aantal voorbeeld en profiler-traceringen zijn bijgewerkt aan de rechterkant.

![GET Customers/Details drie duur van pieken in de zeven dagen bereik met een bepaalde periode](./media/web-monitor-performance/PerformanceTriageView7DaysZoomedTrend.png)

Als u wilt beperken op de trage ervaringen, Inzoomen we vervolgens op de duur die tussen 95th en het 99e percentiel ligt. Dit zijn de % 4 van de interactie van gebruikers die traag zijn.

![GET Customers/Details drie duur van pieken in de zeven dagen bereik met een bepaalde periode](./media/web-monitor-performance/PerformanceTriageView7DaysZoomedTrendZoomed95th99th.png)

We kunnen nu een overzicht van de representatieve voorbeelden door te klikken op de knop voorbeelden of op de representatieve profiler-traceringen, door te klikken op de knop Profiler-traceringen. In dit voorbeeld zijn er vier traces die zijn verzameld voor GET Customers/Details in het venster en bereik duur van belang zijn.

Het probleem zich soms niet in uw code, maar in plaats van in een afhankelijkheid uw code wordt aangeroepen. U kunt overschakelen naar het tabblad afhankelijkheden in de weergave prestaties sorteren om te onderzoeken die trage afhankelijkheden. Standaard is de Prestatieweergave trending gemiddelden, maar wat u wilt om te kijken, is het 95e percentiel (of de 99th in het geval u een goed ontwikkelde service bewaakt). In het volgende voorbeeld hebben we ons gericht op de trage afhankelijkheden voor Azure BLOB, waar we PUT fabrikamaccount noemen. Het goede ervaringen cluster ongeveer 40 ms, terwijl de trage aanroepen naar de dezelfde afhankelijkheid drie keer langzamer zijn, clustering ongeveer 120 ms. Het duren niet veel van deze aanroepen om toe te voegen van de desbetreffende bewerking is aanzienlijk vertragen veroorzaken. U kunt inzoomen op de representatieve voorbeelden en profiler-traceringen, net zoals u met de Operations-tabblad kunt.

![GET Customers/Details drie duur van pieken in de zeven dagen bereik met een bepaalde periode](./media/web-monitor-performance/SlowDependencies95thTrend.png)

De ervaring voor het onderzoeken van prestaties bevat relevante inzichten naast de voorbeeldset die u besloten zich kunt richten op. De beste manier om te kijken naar alle van de inzichten die beschikbaar is overschakelen naar een periode van 30 dagen en selecteer vervolgens de algehele om inzichten te bekijken over alle bewerkingen voor de afgelopen maand.

![GET Customers/Details drie duur van pieken in de zeven dagen bereik met een bepaalde periode](./media/web-monitor-performance/Performance30DayOveralllnsights.png)


## <a name="next"></a>Volgende stappen
[Webtests] [ availability] -hebt webaanvragen naar uw toepassing met regelmatige intervallen van over de hele wereld worden verzonden.

[Vastleggen en zoeken van diagnostische traceringen] [ diagnostic] - invoegen van trace-aanroepen en doorzoeken van de resultaten zodat u problemen kunt herkennen.

[Het bijhouden van gebruik] [ usage] -ontdek hoeveel mensen uw toepassing gebruiken.

[Het oplossen van] [ qna] - en Q & A



<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[greenbrown]: ../../azure-monitor/app/asp-net.md
[qna]: ../../azure-monitor/app/troubleshoot-faq.md
[redfield]: ../../azure-monitor/app/monitor-performance-live-website-now.md
[start]: ../../application-insights/app-insights-overview.md
[usage]: usage-overview.md
[livestream]: ../../azure-monitor/app/live-stream.md
[snapshot]: ../../azure-monitor/app/snapshot-debugger.md



