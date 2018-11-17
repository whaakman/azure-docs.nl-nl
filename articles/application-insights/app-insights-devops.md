---
title: Web application performance monitoring - Azure Application Insights | Microsoft Docs
description: Hoe Application Insights aansluit bij de devOps-cyclus
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 479522a9-ff5c-471e-a405-b8fa221aedb3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: 521bf044f280b91f668d42d760ba2a726b555f12
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51822667"
---
# <a name="deep-diagnostics-for-web-apps-and-services-with-application-insights"></a>Diepe diagnostische gegevens voor web-apps en services met Application Insights
## <a name="why-do-i-need-application-insights"></a>Waarom moet ik Application Insights?
Application Insights bewaakt uw actieve web-app. Het vertelt u over de fouten en prestatieproblemen en helpt u bij het analyseren hoe klanten uw app gebruiken. Het werkt voor apps die worden uitgevoerd op verschillende platforms (ASP.NET, J2EE, Node.js,...) en in de Cloud of on-premises wordt gehost. 

![Aspecten van de complexiteit van het leveren van web-apps](./media/app-insights-devops/010.png)

Het is essentieel voor het bewaken van een moderne toepassing terwijl deze wordt uitgevoerd. Het belangrijkste is dat wilt u detecteren van fouten voordat de meeste van uw klanten. U ook wilt detecteren en oplossen van prestatieproblemen, terwijl niet catastrofale, wat mogelijk vertragen of ongemak kan veroorzaken voor uw gebruikers. En wanneer het systeem naar uw tevredenheid uitvoert, u wilt weten wat de gebruikers bij het doen zijn: ze de meest recente functie gebruikt? Worden ze verwerkt ermee?

Moderne webtoepassingen zijn ontwikkeld in een cyclus van continue levering: een nieuwe functie of verbetering; release Bekijk hoe goed werkt voor gebruikers. de volgende verhoging van de ontwikkeling op basis van deze kennis van plan bent. Een belangrijk onderdeel van de cyclus is de fase waarneming. Application Insights biedt de hulpprogramma's voor het bewaken van een webtoepassing voor prestaties en het gebruik.

De belangrijkste aspecten van dit proces is en diagnose van diagnostische gegevens. Als de toepassing mislukt, is bedrijven wordt verbroken. De primaire rol van een framework voor bewaking is daarom het detecteren van fouten op betrouwbare wijze, onmiddellijk een melding en aan te bieden u de informatie die nodig is om het probleem vast te stellen. Dit is precies wat Application Insights doet.

### <a name="where-do-bugs-come-from"></a>Waar komen bugs vandaan?
Fouten in web-systemen wordt meestal veroorzaakt door configuratieproblemen of ongeldige interacties tussen hun veel onderdelen. De eerste taak bij het toepassen van een incident live site is daarom het identificeren van de locus van het probleem: welk onderdeel of relatie is de oorzaak?

Sommige van onze, die met grijze haar, kan een eenvoudigere tijdperk waarin een programma is uitgevoerd op een computer onthouden. De ontwikkelaars zou testen aandachtig door voordat u levert. en zelden hebben verzonden, wilt bekijken of denken over het opnieuw. De gebruikers hoeft te plaatsen om met de resterende bugs jaren. 

Wat zijn nu dus heel anders. Uw app heeft tal van verschillende apparaten uit te voeren op en het kan lastig zijn om te waarborgen van de exacte hetzelfde gedrag op elk. Apps in de cloud hosten betekent bugs snel kunnen worden opgelost, maar het betekent ook dat continu competitie en de verwachting van nieuwe functies met regelmatige tussenpozen. 

In deze gevallen is de enige manier om een bedrijf bepalen op basis van het aantal fout geautomatiseerde Eenheidstesten. Het is onmogelijk is om handmatig opnieuw test alles op elke levering. Test jednotky is nu een veelvuldig onderdeel van het bouwproces. Hulpprogramma's, zoals de Xamarin Test Cloud u helpen met geautomatiseerde UI testen op meerdere browserversies. Deze tests regelingen kunnen we hopen dat de frequentie van fouten die zijn gevonden in een app kan worden bewaard tot een minimum beperkt.

Typische webtoepassingen hebben veel live onderdelen. Naast de client (in een browser of apparaat-app) en de webserver is er waarschijnlijk aanzienlijke back endverwerking. De back-end is wellicht een pijplijn van onderdelen of een lossere verzameling samenwerkende onderdelen. En veel van deze zich niet in uw beheer - ze externe services waarvan u afhankelijk bent.

In de configuraties zoals deze, kan het zijn moeilijk en uneconomical wilt testen of voorzien, elke mogelijke fout-modus, andere dan in het live systeem zelf. 

### <a name="questions-"></a>Vragen...
Enkele vragen die we vragen wanneer er wordt gewerkt aan een websysteem:

* Loopt vast mijn app? 
* Wat precies is er gebeurd? -Als een aanvraag is mislukt, die ik wil weten hoe het er gekomen. We hebben nodig een trace van gebeurtenissen...
* Mijn app snel genoeg is? Hoe lang duurt het om te reageren op aanvragen voor typische?
* De server wordt overbelast? Als de frequentie van aanvragen toeneemt, de reactietijd houdt onveranderlijk?
* Hoe snel wordt mijn afhankelijkheden - de REST API's, databases en andere onderdelen die door mijn app roept. In het bijzonder als het systeem traag is, is het mijn onderdeel of krijg ik trage reacties van iemand anders?
* Is mijn app omhoog of omlaag? Kan het worden gezien van over de hele wereld? Laat het me weten als deze wordt gestopt...
* Wat de hoofdoorzaak is? De fout in mijn-onderdeel of een afhankelijkheid is? Is het een communicatieprobleem met de?
* Hoeveel gebruikers zijn beïnvloed? Als ik meer dan één probleem om aan te pakken, dit de belangrijkste is?

## <a name="what-is-application-insights"></a>Wat is Application Insights?
![Basiswerkstroom van Application Insights](./media/app-insights-devops/020.png)

1. Application Insights instrumenten van uw app en telemetrie over deze verzendt terwijl de app wordt uitgevoerd. U kunt u de Application Insights SDK inbouwen in de app of u kunt instrumentatie toepassen tijdens runtime. De eerste methode is flexibeler, zoals u uw eigen telemetrie aan de modules die regelmatig toevoegen kunt.
2. De telemetrie wordt verzonden naar de Application Insights-portal, waar ze worden opgeslagen en verwerkt. (Hoewel Application Insights wordt gehost in Microsoft Azure, het kan een web-apps controleren - niet alleen Azure-apps.)
3. De telemetrie vindt u in de vorm van diagrammen en tabellen van gebeurtenissen.

Er zijn twee typen telemetrie: geaggregeerde en raw-instanties. 

* Instantiegegevens bevat bijvoorbeeld een rapport van een aanvraag die is ontvangen door uw web-app. U kunt zoeken en controleren van de details van een aanvraag met het hulpprogramma zoeken in de Application Insights-portal. Het exemplaar omvat gegevens zoals hoe lang de app duurde om te reageren op de aanvraag, evenals de aangevraagde URL, bij benadering de locatie van de client en andere gegevens.
* Cumulatieve gegevens bevat aantallen gebeurtenissen per tijdseenheid, zodat u de frequentie van aanvragen met de reactietijden kunt vergelijken. Dit omvat ook gemiddelden van metrische gegevens zoals de reactietijden van de aanvraag.

De belangrijkste categorieën van de gegevens zijn:

* Aanvragen aan uw app (meestal HTTP-aanvragen), met de gegevens op de URL, reactietijd, en slagen of mislukken.
* Afhankelijkheden - REST en SQL aanroepen van uw app, ook met de URI, reactietijden en geslaagd
* Uitzonderingen, met inbegrip van de stack-traces.
* Pagina weergavegegevens die afkomstig van de browsers van de gebruikers zijn.
* Metrische gegevens zoals prestatiemeteritems, evenals metrische gegevens die u zelf schrijft. 
* Aangepaste gebeurtenissen die u kunt u zakelijke gebeurtenissen bijhouden
* Logboektraceringen gebruikt voor foutopsporing.

## <a name="case-study-real-madrid-fc"></a>Casestudy: Real Madrid F.C.
De webservice van [Real Madrid Football Club](http://www.realmadrid.com/) bedient ongeveer 450 miljoen fans over de hele wereld. Fans toegang zowel via webbrowsers en mobiele apps van de Club. Fans kunnen niet alleen het adresboek van tickets, maar ook toegang tot gegevens en videoclips op toekomstige games, spelers en resultaten. Ze kunnen zoeken met filters zoals aantallen doelstellingen beoordeeld. Er zijn ook koppelingen naar sociale media. De gebruikerservaring is uiterst persoonlijke en is bedoeld als een communicatie in twee richtingen contact opnemen met fans.

De oplossing [is een systeem van services en toepassingen op Microsoft Azure](https://www.microsoft.com/inculture/sports/real-madrid/). Schaalbaarheid is een basisvereiste: verkeer variabele en tijdens de en rond komt overeen met zeer hoge volumes kunt bereiken.

Voor Real Madrid, is het essentieel is voor het bewaken van prestaties van het systeem. Azure Application Insights biedt een uitgebreide weergave in het systeem ervoor te zorgen dat een betrouwbare en hoog niveau van de service. 

De Club wordt ook diepgaand inzicht in de fans: waar ze zijn (slechts 3% zijn in Spanje), welke belang dat ze hebben in spelers, historische resultaten en toekomstige games en hoe ze reageren zodat deze overeenkomt met de resultaten.

De meeste van deze telemetrische gegevens worden automatisch verzameld zonder dat er extra code, die de oplossing vereenvoudigd en de operationele complexiteit wordt verminderd.  Voor Real Madrid, Application Insights behandelt 3,8 miljard telemetrie punten per maand.

De Power BI-module real Madrid gebruikt om de telemetrie weer te geven.

![Power BI-weergave van Application Insights-telemetrie](./media/app-insights-devops/080.png)

## <a name="smart-detection"></a>Slimme detectie
[Proactieve diagnostische gegevens](app-insights-proactive-diagnostics.md) is een recente functie. Zonder speciale configuratie door u, Application Insights automatisch detecteert en waarschuwt u over ongebruikelijke pieken in foutpercentages in uw app. Is het verstandig om te negeren een achtergrond van incidentele fouten optreden, en ook de pieken die zijn gewoon verhouding staan tot een toename van aanvragen. Bijvoorbeeld werkt als er een storing in een van de services die u afhankelijk bent of als de nieuwe bouwen die u zojuist hebt geïmplementeerd niet zo goed, dan u erover weet Als u kijken naar uw e-mailadres. (En er zijn webhooks, zodat u kunt andere apps activeren.)

Een ander aspect van deze functie voert een dagelijkse diepgaande analyse van uw telemetrie, zoeken naar ongebruikelijke patronen van de prestaties die moeilijk te detecteren. Bijvoorbeeld, vindt er trage prestaties die zijn gekoppeld met een bepaald geografisch gebied of met een bepaalde browser-versie.

In beide gevallen wordt de waarschuwing u niet alleen de symptomen wordt gedetecteerd, maar biedt ook gegevens die u nodig hebt om het probleem, zoals relevante uitzonderingenrapporten vast te stellen.

![E-mail van proactieve diagnostische gegevens](./media/app-insights-devops/030.png)

Klant Samtec zei: "We hebben gevonden tijdens een recente functie cutover, een onder maten database die is de resource-limiet hebt bereikt en time-outs veroorzaakt. De Proactive detectiewaarschuwingen is geleverd via letterlijk als we het probleem, zeer bijna realtime zijn uitsorteren geadverteerd. Deze waarschuwing in combinatie met de Azure-platform waarschuwingen geholpen bij ons vrijwel onmiddellijk los het probleem. Totaal aantal minuten downtime < 10 minuten."

## <a name="live-metrics-stream"></a>Live Metrics Stream
Implementatie van de nieuwste build, kan een zorgen ervaring zijn. Als er problemen zijn, wilt u weten over deze meteen, zodat u kunt back-ups uit als nodig. Live Metrics Stream kunt u belangrijke metrische gegevens met een latentie van ongeveer één seconde.

![Live metrics Stream](./media/app-insights-devops/040.png)

En kunt u direct een voorbeeld van eventuele fouten of uitzonderingen te inspecteren.

![Live foutgebeurtenissen](./media/app-insights-devops/live-stream-failures.png)

## <a name="application-map"></a>Toepassingskaart
Overzicht van de toepassing detecteert automatisch de Toepassingstopologie van uw, waarin de informatie over de prestaties daarboven, zodat u eenvoudig bepalen knelpunten en problematische stromen in uw gedistribueerde omgeving. Hiermee kunt u voor het detecteren van afhankelijkheden voor toepassingen op Azure-Services. U kunt het probleem beoordelen door te begrijpen als deze met betrekking tot code of afhankelijkheid die betrekking hebben en ervaring van een enkele locatie Zoom in op gerelateerde diagnostische gegevens. Uw toepassing kan bijvoorbeeld worden mislukken vanwege een verslechtering van prestaties optreedt in de SQL-laag. Met overzicht van de toepassing, kunt u direct bekijken en Zoom in op de SQL Index Advisor of Queryinzichten optreden.

![Toepassingskaart](./media/app-insights-devops/050.png)

## <a name="application-insights-analytics"></a>Application Insights Analytics
Met [Analytics](app-insights-analytics.md), u kunt een willekeurige query's schrijven in een krachtige SQL-achtige taal.  Diagnose in de hele app-stack wordt het gemakkelijk verschillende perspectieven verbinding en u kunt de juiste vragen serviceprestaties correleren met zakelijke metrische gegevens en ervaring van de klant. 

U kunt uw telemetrie-instantie en metrische onbewerkte gegevens die zijn opgeslagen in de portal een query. De taal bevat filter, join, aggregatie en andere bewerkingen. U kunt velden berekenen en statistische analyses uitvoeren. Er zijn in tabelvorm en grafische visualisaties.

![Grafiek met gebruiksanalyses query en de resultaten](./media/app-insights-devops/025.png)

Bijvoorbeeld, is het eenvoudig:

* Gegevens van de prestaties van uw toepassing aanvragen segmenteren op lagen van de klant om te begrijpen van hun ervaring.
* Zoeken naar specifieke foutcodes of namen van aangepaste gebeurtenis tijdens de live site onderzoeken.
* Zoom in het app-gebruik van specifieke klanten om te begrijpen hoe de functies zijn verkregen en vastgesteld.
* Bijhouden van sessies en reactietijden voor specifieke gebruikers om in te schakelen, ondersteuning en uitvoerende teams instant klantenondersteuning te bieden.
* Bepaal functies veelgebruikte Apps om functie prioriteitsaanduiding vragen te beantwoorden.

Klant DNN zei: "Application Insights is opgegeven met ons op via het ontbrekende onderdeel van de vergelijking voor kunnen te combineren, sorteren en filteren van gegevens naar behoefte. Zodat ons team van hun eigen draaien en de ervaring gebruiken om te vinden is, gegevens met een krachtige querytaal hadden we inzichten en oplossen van problemen we niet weten zelfs niet hadden we. Een groot aantal interessante antwoorden afkomstig zijn van de vragen die beginnen met *' ik wonder als...'.*"

## <a name="development-tools-integration"></a>Integratie van de hulpprogramma's voor ontwikkeling
### <a name="configuring-application-insights"></a>Application Insights configureren
Visual Studio en Eclipse hebt hulpprogramma's voor het configureren van het juiste SDK-pakketten voor het project dat u ontwikkelt. Er is een opdracht toe te voegen van Application Insights.

Als u per ongeluk worden met behulp van een framework voor logboekregistratie van trace zoals Log4N, NLog of System.Diagnostics.trace werkt, klikt u vervolgens krijgt u de mogelijkheid de logboeken verzenden naar Application Insights, samen met de andere telemetrie, zodat u eenvoudig de traceringen met aanvragen, afhankelijkheid correleren kunt aanroepen en uitzonderingen.

### <a name="search-telemetry-in-visual-studio"></a>Zoeken in telemetrie in Visual Studio
Bij de ontwikkeling en foutopsporing in een functie, kunt u bekijken en zoeken van de telemetrie rechtstreeks in Visual Studio, met behulp van de dezelfde search-voorzieningen zoals in de webportal.

En wanneer Application Insights zich een uitzondering, kunt u weergeven van het gegevenspunt in Visual Studio en gaat u rechtstreeks naar de relevante code.

![Visual Studio zoeken](./media/app-insights-devops/060.png)

Tijdens de foutopsporing, hebt u de optie voor het behouden van de telemetrie op uw ontwikkelcomputer bekijken in Visual Studio, maar zonder dat deze worden verzonden naar de portal. Deze optie lokale voorkomt met een combinatie van foutopsporing met telemetrie voor productie.

### <a name="build-annotations"></a>Aantekeningen maken
Als u Azure DevOps om te bouwen en implementeren van uw app, weergegeven implementatie-aantekeningen op de grafieken in de portal. Als uw meest recente versie van invloed zijn op de metrische gegevens is, wordt het duidelijk.

![Aantekeningen maken](./media/app-insights-devops/070.png)

### <a name="work-items"></a>Werkitems
Wanneer een waarschuwing wordt gegenereerd, kan Application Insights automatisch een werkitem maken in uw werk volgsysteem.

## <a name="but-what-about"></a>Maar hoe zit...?
* [Privacy- en storage](app-insights-data-retention-privacy.md) -uw telemetrie wordt bewaard in Azure beveiligde servers.
* Prestaties - de impact is zeer laag. Telemetrie in batch wordt verwerkt.
* [Prijzen](app-insights-pricing.md) : U kunt aan de slag gratis, en die blijft terwijl u in laag volume bent.


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Volgende stappen
Aan de slag met Application Insights is eenvoudig. De belangrijkste opties zijn:

* Instrumenteer een web-app al actief. Dit biedt u alle telemetrie van de ingebouwde prestatiecontrole. Het is beschikbaar voor [Java](app-insights-java-live.md) en [IIS-servers](app-insights-monitor-performance-live-website-now.md), en ook voor [Azure web-apps](app-insights-overview.md).
* Instrumenteer uw project tijdens de ontwikkeling. U kunt dit doen voor [ASP.NET](app-insights-asp-net.md) of [Java](app-insights-java-get-started.md) apps, evenals [Node.js](app-insights-nodejs.md) en een groot aantal [andere typen](app-insights-platforms.md). 
* Instrument [elke webpagina](app-insights-javascript.md) door een korte codefragment toe te voegen.

