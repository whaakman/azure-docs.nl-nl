---
title: Overzicht van Azure Application Insights voor DevOps | Microsoft Docs
description: Informatie over het gebruik van Application Insights in een Dev Ops-omgeving.
author: mrbullwinkle
services: application-insights
documentationcenter: 
manager: carmonm
ms.assetid: 6ccab5d4-34c4-4303-9d3b-a0f1b11e6651
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.custom: mvc
ms.topic: overview
ms.date: 06/26/2017
ms.author: mbullwin
ms.openlocfilehash: b83d08b9dac4fccc033ad4537afd343a6fbe02c2
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="overview-of-application-insights-for-devops"></a>Overzicht van Application Insights voor DevOps

Met [Application Insights](app-insights-overview.md), u kunt snel ontdekken hoe uw app wordt uitgevoerd en wordt gebruikt wanneer het live. Er is een probleem, kunt u informatie over het helpt bij het beoordelen van de impact en helpt u de oorzaak te achterhalen.

Hier volgt een account van een team dat webtoepassingen ontwikkelt:

* *"Een aantal dagen geleden implementeerden we een 'secundaire' hotfix. Een brede testronde niet worden uitgevoerd, maar helaas sommige onverwachte wijziging in de nettolading, waardoor ze niet compatibel zijn tussen de begin- en back-ends is samengevoegd. Direct serveruitzonderingen toenam, onze waarschuwing geactiveerd en er zijn aangebracht op de hoogte van de situatie. Een paar muisklikken opgeslagen op de Application Insights-portal wij voldoende informatie van de uitzondering callstacks om het probleem vast te stellen. We onmiddellijk teruggedraaid en de schade beperkt. Application Insights heeft dit deel van de devops bladeren aangebracht zeer eenvoudig en actie worden uitgevoerd."*

In dit artikel volgen we een team in Fabrikam Bank die het systeem internetbankieren (OBS) om te zien hoe ze Application Insights gebruiken om snel reageren op klanten en wijzigingen ontwikkelt.  

Het team werkt op een DevOps cyclus afgebeeld in de volgende afbeelding:

![DevOps cyclus](./media/app-insights-detect-triage-diagnose/00-devcycle.png)

Vereisten van de feed in de achterstand van hun ontwikkeling (takenlijst). Ze werken in een korte sprints, wat vaak resulteert in een werkende software - gewoonlijk in de vorm van verbeteringen en uitbreidingen voor de bestaande toepassing. De live-app wordt regelmatig bijgewerkt met nieuwe functies. Het live is, wordt het in het team bewaakt om prestaties en gebruik met behulp van Application Insights. Deze gegevens APM-feeds terug naar de achterstand van hun ontwikkeling.

Het team gebruik maakt van Application Insights voor het bewaken van de webtoepassing live nauw voor:

* De prestaties. Ze willen weten hoe reactietijden verschillen naargelang het aantal verzoeken; hoeveel CPU-, netwerk-, schijf- en andere bronnen worden gebruikt; welke toepassingscode vertraagd prestaties; en waar de knelpunten zijn.
* Fouten. Als er uitzonderingen zijn of mislukte aanvragen, of als een prestatiemeteritem buiten de vertrouwd bereik gaat, het team moet snel worden kennen, zodat ze actie kunnen ondernemen.
* Het gebruik. Wanneer er een nieuwe functie wordt uitgebracht, wil het team in welke mate het wordt gebruikt en of gebruikers hebben problemen met het weten.

Laten we zich richten op het feedback deel van de cyclus:

![Detecteren-selectie-onderzoeken](./media/app-insights-detect-triage-diagnose/01-pipe1.png)

## <a name="detect-poor-availability"></a>Slechte beschikbaarheid detecteren
Marcela Markova is een senior ontwikkelaar van het team OBS, en wordt de lead op online-prestaties bewaken. Ze stelt u verschillende [beschikbaarheidstests](app-insights-monitor-web-app-availability.md):

* Een enkele URL-test voor de belangrijkste startpagina voor de app http://fabrikambank.com/onlinebanking/. Ze stelt criteria van HTTP-code, 200 en tekst 'Welkom!'. Als deze test mislukt, is er een ernstig probleem met het netwerk of de servers of misschien een probleem met de implementatie. (Of iemand de welkomstpagina is gewijzigd! bericht op de pagina zonder dat haar weten.)
* Een dieper test met meerdere stappen, die zich aanmeldt en een account van de huidige aanbieding, controle van enkele belangrijke gegevens op elke pagina opgehaald. Deze test controleert of de koppeling naar de database met gebruikersaccounts werkt. Ze gebruikt een fictieve klant-id: een aantal worden bijgehouden voor testdoeleinden.

Met deze tests is ingesteld, is het Marcela er zeker van te zijn dat het team snel over een storing weten.  

Fouten worden weergegeven als rood punten op de web-test-grafiek:

![Weergave van webtests die via de voorafgaande periode hebben uitgevoerd](./media/app-insights-detect-triage-diagnose/04-webtests.png)

Maar belangrijker is, een waarschuwing over een storing in het ontwikkelingsteam per e-mail is verzonden. Op die manier kunnen weten ze voordat u vrijwel alle klanten.

## <a name="monitor-performance"></a>Prestaties bewaken
Op de overzichtspagina in Application Insights wordt een grafiek ziet u diverse [belangrijke metrische gegevens](app-insights-web-monitor-performance.md).

![Verschillende metrische gegevens](./media/app-insights-detect-triage-diagnose/05-perfMetrics.png)

Laadtijd van browserpagina is afgeleid van telemetrie die rechtstreeks van webpagina's. Serverreactietijd, aantal servers-aanvraag en aantal mislukte aanvragen zijn alle gemeten in de webserver en van daaruit naar Application Insights is verzonden.

Marcela is enigszins betrokken zijn bij de server antwoord-grafiek. Deze grafiek toont de gemiddelde tijd tussen wanneer de server een HTTP-aanvraag van de browser van een gebruiker ontvangt, en wanneer het antwoord retourneert. Het is niet ongebruikelijke is een variatie in deze grafiek zien als de belasting op het systeem varieert. Maar in dit geval wordt er lijkt dat een correlatie tussen small verhogingen in de telling van aanvragen, en big komt in de reactietijd. Dat kan duiden op het systeem werkt alleen bij de grenzen.

Ze opent de grafieken Servers:

![Verschillende metrische gegevens](./media/app-insights-detect-triage-diagnose/06.png)

Er lijkt geen teken van bronbeperking, dus misschien dat de dalen in de grafieken van de reactie server zojuist een samenvallen zijn.

## <a name="set-alerts-to-meet-goals"></a>Waarschuwingen instellen om doelstellingen te behalen
Niettemin wil ze de reactietijden gaten houden. Als ze te hoog, dat ze wil weten over het direct.

Zodat ze stelt een [waarschuwing](app-insights-metrics-explorer.md), reactietijden groter is dan een typische drempelwaarde. Hiermee geeft u haar erop vertrouwen dat ze over het weten moet als reactietijden traag zijn.

![Waarschuwingen-blade toevoegen](./media/app-insights-detect-triage-diagnose/07-alerts.png)

Waarschuwingen kunnen worden ingesteld voor een groot aantal andere metrische gegevens. U kunt bijvoorbeeld een e-mailberichten ontvangen als het aantal uitzondering toeneemt, anders wordt de hoeveelheid beschikbaar geheugen laag, of als er een piek in de aanvragen van clients.

## <a name="stay-informed-with-smart-detection-alerts"></a>Blijf op de hoogte met slim Detectiewaarschuwingen
Volgende dag, een e-mailwaarschuwingen van de Application Insights binnenkomen. Maar wanneer ze de wordt geopend, ze vindt dat niet zo is de reactie tijd waarschuwing die ze ingesteld. In plaats daarvan krijgt haar is er een plotselinge toename van mislukte aanvragen - dat wil zeggen, aanvragen die codes van 500 of meer fout geretourneerd.

Mislukte aanvragen zijn waarbij gebruikers een fout - doorgaans na een uitzondering opgetreden in de code hebt gezien. Mogelijk zien ze een bericht tekst "Helaas uw gegevens nu niet bijwerken." Of op absolute gênante ergste geval een Stackdump wordt weergegeven op het scherm van de gebruiker, door de webserver.

Deze waarschuwing is onverwacht, omdat de laatste keer dat ze, gezien het aantal mislukte aanvragen encouragingly lage is. Er is een klein aantal fouten worden verwacht in een overbelaste server.

Het is ook een stukje onverwacht voor haar omdat ze niet beschikbaar om deze waarschuwing te configureren. Application Insights bevatten Slimme detectie. Automatisch wordt aangepast aan de gebruikelijke fout patroon en fouten 'wordt gebruikt voor' op een bepaalde pagina of onder hoge belasting van uw app of gekoppeld aan andere metrische gegevens. Er wordt een waarschuwing gegeven alleen als er een nooit meer dan wat ze kunnen verwachten afkomstig is.

![proactieve diagnoses e](./media/app-insights-detect-triage-diagnose/21.png)

Dit is een zeer nuttig e-mailadres. Het verhogen niet alleen een waarschuwing wordt gegeven. Veel van de selectie en diagnostische werk te gebeurt.

Er wordt weergegeven hoeveel klanten door worden getroffen, en welke webpagina's of bewerkingen. Marcela kunt bepalen of ze nodig heeft om op te halen van het hele team werkt op deze als een detailanalyse fire of of kan worden genegeerd totdat de volgende week.

Het e-mailbericht ziet ook dat een bepaalde uitzondering is opgetreden en - zelfs meer interessante - of de fout gekoppeld aan de mislukte aanroepen met een bepaalde database is. Hier wordt uitgelegd waarom de fout plotseling komt Hoewel team Marcela van updates niet onlangs geïmplementeerd.

Marcella pingt de opvulteken van het database-team op basis van dit e-mailbericht. Ze leert dat ze een hotfix beschikbaar in de afgelopen half uur; vrijgegeven en Oeps, mogelijk er is een secundaire schemawijziging...

Daarom is het probleem op de manier om te worden opgelost, zelfs vóór het onderzoeken van Logboeken en binnen 15 minuten ervan die voortvloeien. Marcela klikt echter op de koppeling om de Application Insights openen. Deze direct door naar een mislukte aanvraag wordt geopend en kan ze zien de mislukte aanroep in de bijbehorende lijst met afhankelijkheidsaanroepen-database.

![mislukte aanvragen](./media/app-insights-detect-triage-diagnose/23.png)

## <a name="detect-exceptions"></a>Uitzonderingen te detecteren
Met een beetje van setup [uitzonderingen](app-insights-asp-net-exceptions.md) automatisch naar Application Insights worden gerapporteerd. Ze kunnen ook worden vastgelegd expliciet door het invoegen van aanroepen naar [TrackException()](app-insights-api-custom-events-metrics.md#trackexception) in de code in:  

    var telemetry = new TelemetryClient();
    ...
    try
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string>
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send the exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }


Het team Fabrikam Bank is altijd telemetrie te verzenden van een uitzondering ontwikkeld, tenzij er een duidelijke herstel.  

In feite hun strategie is zelfs breder is dan: verzenden van telemetrie in alle gevallen waar de klant gefrustreerd is in wat ze doen, wilden of het komt met een uitzondering opgetreden in de code of niet overeen. Bijvoorbeeld, als het systeem externe tussen overschrijving een bericht 'deze transactie kan niet worden voltooid' om een operationele reden (Er is geen fout van de klant geeft) bijhouden vervolgens zij die gebeurtenis.

    var successCode = AttemptTransfer(transferAmount, ...);
    if (successCode < 0)
    {
       var properties = new Dictionary <string, string>
            {{ "Code", returnCode, ... }};
       var measurements = new Dictionary <string, double>
         {{"Value", transferAmount}};
       telemetry.TrackEvent("transfer failed", properties, measurements);
    }

TrackException wordt gebruikt voor het rapporteren van uitzonderingen omdat zendt het een kopie van de stack. TrackEvent wordt gebruikt voor het rapporteren van andere gebeurtenissen. U kunt alle eigenschappen die mogelijk nuttig zijn bij de diagnose koppelen.

Uitzonderingen en gebeurtenissen weergegeven in de [diagnostische gegevens doorzoeken](app-insights-diagnostic-search.md) blade. U kunt inzoomen in deze om te zien van de aanvullende eigenschappen en stacktracering.

![In Diagnostic Search filters gebruiken om bepaalde soorten gegevens weer te geven](./media/app-insights-detect-triage-diagnose/appinsights-333facets.png)


## <a name="monitor-proactively"></a>Proactief bewaken
Marcela niet alleen hoeft verder niets te wachten op waarschuwingen. Snel na elke opnieuw implementeren, ze kijken neemt [reactietijden](app-insights-web-monitor-performance.md) -zowel de algehele afbeelding en de tabel van de traagste aanvragen, evenals een aantal uitzonderingen is.  

![Antwoord tijd grafiek en een raster van reactietijden van de server.](./media/app-insights-detect-triage-diagnose/09-dependencies.png)

Ze kan het effect van de prestaties van elke implementatie doorgaans vergelijken per week met de laatste beoordelen. Als er een plotselinge verslechtering, worden ze die gegeven aan de relevante ontwikkelaars.

## <a name="triage-issues"></a>Selectie problemen
De eerste stap is - voor het beoordelen van de ernst en de omvang van een probleem - selectie na detectie. Moeten we noemen uit het team om middernacht? Of kan het blijven totdat de afstand in de achterstand van de volgende handige? Er zijn enkele belangrijke vragen in de selectie.

Hoe vaak gebeurt dit? De grafieken op de overzichtsblade sommige perspectief om een probleem te geven. Bijvoorbeeld, de toepassing Fabrikam gegenereerd vier web test waarschuwingen één nacht. De grafiek bekijkt in de ochtend, kan het team zien dat er immers rode punten zijn nog steeds de meeste tests is alsof groen. Dieper ingegaan op de grafiek beschikbaarheid, werd het ons duidelijk dat deze problemen uit één testlocatie zijn. Dit is natuurlijk een netwerkprobleem die invloed hebben op slechts één route en schakelt zichzelf waarschijnlijk.  

Daarentegen een indrukwekkende en stabiele stijging van de grafiek uitzondering tellingen of antwoord vaak is natuurlijk iets zorgen over.

Een e-mailbericht nuttig selectie is probeer het zelf. Als u hetzelfde probleem, weet u dat het echte is.

Welke fractie van de gebruikers worden beïnvloed? Als u een ruwe antwoord, het Faalpercentage door het aantal sessies te delen.

![Diagrammen van mislukte aanvragen en sessies](./media/app-insights-detect-triage-diagnose/10-failureRate.png)

Wanneer er trage reacties, vergelijkt u de tabel aanvragen traagste reageert met de frequentie van de informatie over het gebruik van elke pagina.

Het is belangrijk de geblokkeerde scenario? Als dit een functioneel probleem het verhaal van een bepaalde gebruiker worden geblokkeerd, maakt het uit veel? Als klanten hun rekeningen kunnen niet betalen, is dit ernstige; Als ze niet kunnen de kleur scherm Voorkeuren wijzigen, kunt mogelijk het wachten. De details van de gebeurtenis of uitzondering of de identiteit van de pagina langzaam vertelt u klanten hebben waar problemen.

## <a name="diagnose-issues"></a>Problemen vaststellen
Diagnose is niet erg hetzelfde is als het opsporen van fouten. Voordat u tracering via de code, moet u een idee van de reden, hebben waar en wanneer het probleem zich voordoet.

**Wanneer treedt dit?** De historische weergave geleverd door de grafieken gebeurtenissen en metrische gegevens kunt gemakkelijk effecten correleren met mogelijke oorzaken. Als er onregelmatige pieken in tijd of uitzondering respons zijn, bekijkt u het aantal verzoeken: bij deze waar de pieken op hetzelfde moment wordt het lijkt erop dat een probleem met de bronnen. Wilt u meer CPU of geheugen toewijzen? Of is het een afhankelijkheid die de belasting kan niet worden beheerd?

**Het is ons?**  Als u een plotselinge afname in de prestaties van een bepaald soort aanvraag - bijvoorbeeld wanneer de klant wil een rekeningoverzicht - hebt vervolgens bestaat de kans is mogelijk een extern subsysteem in plaats van uw webtoepassing. In Metrics Explorer, selecteer de afhankelijkheidsfout frequentie en duur afhankelijkheid tarieven en hun geschiedenis gedurende de afgelopen paar uren of dagen met het probleem dat u achterhaalt vergelijken. Als er wijzigingen zijn correleren, kan een externe subsysteem te wijten zijn.  


![Grafieken van afhankelijkheidsfout en duur van aanroepen naar afhankelijkheden](./media/app-insights-detect-triage-diagnose/11-dependencies.png)

Sommige afhankelijkheidsproblemen trage zijn geolocatie problemen. Fabrikam Bank Azure virtuele machines worden gebruikt en dat ze had per ongeluk hun webserver en de server van de accountpartner in verschillende landen gevestigde gedetecteerd. Een aanzienlijk is veroorzaakt door het migreren van een van beide.

**Wat er?** Als het probleem zich in een afhankelijkheid niet wordt weergegeven, en als deze is niet altijd er, wordt deze waarschijnlijk veroorzaakt door een recente wijziging. Het historische perspectief geleverd door de grafieken metrische gegevens en gebeurtenissen kunt gemakkelijk plotselinge wijzigingen correleren met implementaties. Die wordt beperkt u de zoekactie voor het probleem. Inschakelen om te identificeren welke regels in de toepassingscode vertraagd de prestaties, Application Insights Profiler. Raadpleeg [Profiling live Azure-web-apps met Application Insights](./app-insights-profiler.md). Nadat de Profiler is ingeschakeld, ziet u een vergelijkbaar met de volgende tracering. In dit voorbeeld is het vervelend dat de methode *GetStorageTableData* het probleem heeft veroorzaakt.  

![App Insights Profiler tracering](./media/app-insights-detect-triage-diagnose/AppInsightsProfiler.png)

**Wat gebeurt er?** Sommige problemen slechts zelden en kunnen het lastig voor het opsporen van door het testen offline. We kunnen doen is om te proberen om vast te leggen van de fout als deze live optreedt. U kunt de stackdumps in uitzonderingenrapporten controleren. Bovendien kunt u tracering-aanroepen, schrijven met uw favoriete framework voor logboekregistratie of TrackTrace() of TrackEvent().  

Fabrikam had een onregelmatig probleem met tussen account overdrachten, maar alleen met bepaalde accounttypen. Om te begrijpen beter gebeurtenissen, ze TrackTrace() aanroepen op de belangrijkste punten in de code wordt het accounttype koppelen als een eigenschap aan elke aanroep ingevoegd. Die het gemakkelijk voor het filteren van alleen de traceringen in Diagnostic Search geworden. Ze ook parameterwaarden als eigenschappen en metingen aan de traceringsaanroepen gekoppeld.

## <a name="respond-to-discovered-issues"></a>Reageren op gedetecteerde problemen
Zodra u het probleem hebt vastgesteld, kunt u een plan om dit te herstellen. Mogelijk moet u een recente wijziging terugdraaien of misschien u kunt alleen opwekken en op te lossen. Als het herstel is voltooid, Application Insights kunt u zien of u is voltooid.  

Het ontwikkelteam van Fabrikam Bank nemen een meer gestructureerde benadering voor prestatiemeting dan ze gebruikt om te voordat ze Application Insights gebruikt.

* Ze prestatiedoelen in termen van specifieke maatregelen hebt ingesteld in de overzichtspagina van Application Insights.
* Ze ontwerpen prestatiemetingen in de toepassing vanaf het begin, zoals de metrische gegevens die het meten van de voortgang van de gebruiker via 'schoorstenen'.  


## <a name="monitor-user-activity"></a>Monitor gebruikersactiviteit
Wanneer reactietijd consistent goed is en er enkele uitzonderingen zijn, kunt de dev-team tot bruikbaarheid verplaatsen. Ze kunnen zien over het verbeteren van de gebruikerservaring en het stimuleren van meer gebruikers om de gewenste doelstellingen te realiseren.

Application Insights kunnen ook worden gebruikt voor meer informatie over wat gebruikers doen met een app. Zodra deze goed blijft werken wordt uitgevoerd, wil het team weten welke functies de meest populaire zijn wat gebruikers wel en hebben problemen met en hoe vaak ze terugkeren. Waarmee ze hun werk toekomstige prioriteren. En ze kunnen plannen waarmee het succes van elke functie als onderdeel van het ontwikkelingsproces bevindt.

Een typische gebruiker reis via de website heeft bijvoorbeeld een duidelijke 'trechter'. Veel klanten bekijken de tarieven van verschillende soorten lening. Een kleiner getal gaat u naar het formulier aanhalingstekens invullen. Van mensen die een aanhalingstekens ophalen, is enkele opwekken en verricht de lening.

![Paginaweergave geteld](./media/app-insights-detect-triage-diagnose/12-funnel.png)

Het bedrijf kunt u overweegt waar het grootste aantal klanten neerzetten, werken over het ophalen van meer gebruikers via naar de onderkant van de trechter. In sommige gevallen is er mogelijk een gebruikersfout ervaring (UX) - bijvoorbeeld de knop 'volgende' is moeilijk te vinden of de instructies zijn niet duidelijk. Er zijn meer waarschijnlijk meer belangrijke zakelijke redenen voor drop-outs: mogelijk de tarieven voor autofinancieringen te hoog zijn.

Ongeacht de redenen, kunt de gegevens u het team komen wat gebruikers doen. Aanroepen van meer bijhouden kunnen u meer details worden ingevoegd. TrackEvent() kan worden gebruikt voor het tellen van eventuele gebruikersacties uit de kleine details op van een afzonderlijke knop naar aanzienlijke prestaties, zoals een lening af te betalen.

Het team wordt ophalen gebruikt om informatie over gebruikersactiviteit. Tegenwoordig wanneer ze een nieuwe functie ontwerpt, werken ze uit hoe ze feedback over het gebruik ervan krijgen. Ze ontwerp bijhouden aanroepen in de functie vanaf het begin. Ze de feedback voor het verbeteren van de functie in elke ontwikkelingscyclus gebruiken.

[Meer informatie over het bijhouden van gebruik](app-insights-usage-overview.md).

## <a name="apply-the-devops-cycle"></a>De cyclus DevOps toepassen
Dit is dus hoe een team gebruik Application Insights niet alleen op afzonderlijke problemen kan oplossen, maar voor het verbeteren van de levenscyclus van de ontwikkeling. Ik hoop dat het enkele ideeën over hoe Application Insights u met de prestaties van Toepassingsbeheer in uw eigen toepassingen helpen kan hebt gekregen.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Volgende stappen
U kunt aan de slag op verschillende manieren, afhankelijk van de kenmerken van uw toepassing. Kies wat u het beste past:

* [ASP.NET-webtoepassing](app-insights-asp-net.md)
* [Java-webtoepassing](app-insights-java-get-started.md)
* [Node.js-webtoepassing](app-insights-nodejs.md)
* Al geïmplementeerde apps, die worden gehost op [IIS](app-insights-monitor-web-app-availability.md), [J2EE](app-insights-java-live.md), of [Azure](app-insights-azure.md).
* [Webpagina's](app-insights-javascript.md) -App met één pagina of normale webpagina - Gebruik deze zelf of in aanvulling op een van de opties van de server.
* [Beschikbaarheidstests](app-insights-monitor-web-app-availability.md) voor het testen van uw app op het openbare internet.
