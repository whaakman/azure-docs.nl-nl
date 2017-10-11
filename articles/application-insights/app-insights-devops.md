---
title: Web application performance monitoring - Azure Application Insights | Microsoft Docs
description: Hoe Application Insights in de cyclus devOps past
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 479522a9-ff5c-471e-a405-b8fa221aedb3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: de94633cabaa7a1562a5a4839a8a8924da91a283
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="deep-diagnostics-for-web-apps-and-services-with-application-insights"></a>Diepe diagnostische gegevens voor web-apps en services met Application Insights
## <a name="why-do-i-need-application-insights"></a>Waarom moet ik Application Insights?
Application Insights wordt uw actieve web-app bewaakt. Het vertelt u over de fouten en prestatieproblemen en helpt u bij het analyseren hoe klanten gebruiken voor uw app. Het werkt voor apps die worden uitgevoerd op verschillende platforms (ASP.NET, J2EE, Node.js,...) en wordt gehost in de Cloud of on-premises. 

![Aspecten van de complexiteit van het leveren van web-apps](./media/app-insights-devops/010.png)

Het is essentieel voor het bewaken van een moderne toepassingen terwijl deze wordt uitgevoerd. Het belangrijkste is dat u fouten opsporen voordat de meeste van uw klanten. U wilt detecteren en oplossen van prestatieproblemen die, terwijl niet onherstelbare ook, dingen mogelijk vertragen of ongemak kan veroorzaken voor uw gebruikers. En wanneer het systeem naar wens uitvoert, wilt u weten wat gebruikers doen met het: ze de meest recente functie gebruikt? Slaagt ze ermee?

Moderne webtoepassingen zijn ontwikkeld in een cyclus van continue levering: een nieuwe functie of verbetering; vrijgeven Houd rekening met hoe goed werkt voor gebruikers. plan de volgende verhoging van de ontwikkeling op basis van deze kennis van. Een belangrijk onderdeel van deze cyclus is de fase observatie. Application Insights biedt de hulpprogramma's voor het bewaken van een webtoepassing voor prestaties en het gebruik.

De belangrijkste aspecten van dit proces is en diagnose van diagnostische gegevens. Als de toepassing mislukt, is business wordt verbroken. De primaire rol van een framework voor controle is daarom om fouten op betrouwbare wijze detecteren, onmiddellijk een melding en aan te bieden u de informatie die nodig is het probleem op te sporen. Dit is precies wat Application Insights doet.

### <a name="where-do-bugs-come-from"></a>Waar komen bugs vandaan?
Fouten in de web-systemen worden doorgaans veroorzaakt door problemen met de configuratie of ongeldige interacties tussen hun veel onderdelen. De eerste taak bij het aanpakken van een incident live site is daarom het identificeren van de locus van het probleem: welk onderdeel of relatie is de oorzaak?

Sommige van ons die met grijze haar, kan een eenvoudigere era waarin een computer wordt uitgevoerd in één computer onthouden. De ontwikkelaars zou testen zorgvuldig door voordat u levert. en zelden hebben verzonden, zou Zie of denken. De gebruikers zou moeten verpakt met de resterende bugs jaren. 

Dingen zijn nu zo heel verschillend. Uw app heeft tal van verschillende apparaten uit te voeren op kan, en moeilijk exact hetzelfde gedrag op elk garanderen. Apps in de cloud hosten betekent fouten kunnen snel worden opgelost, maar het betekent ook dat continue concurrentie en de verwachting van nieuwe functies met regelmatige tussenpozen. 

In deze voorwaarden is de enige manier om een vast besturingselement op het aantal fout bewaren geautomatiseerde eenheid testen. Het is onmogelijk om handmatig opnieuw test alles op elke levering. Testen van eenheden is nu een heel gebruikelijk deel van deze procedure. Hulpprogramma's, zoals de Xamarin-Testcloud u helpen met geautomatiseerde UI testen op meerdere browserversies. Deze test regelingen kunnen we hopen dat de frequentie van fouten die zijn gevonden in een app kan worden bewaard tot een minimum te beperken.

Typische webtoepassingen hebben veel live onderdelen. Naast de client (in een browser of een apparaat-app) en de webserver is er waarschijnlijk aanzienlijke back-end-verwerking. De back-end is mogelijk een pijplijn van onderdelen of een verzameling lossere samenwerkende onderdelen. Veel van deze niet in uw beheer- en - ze zijn externe services waarvan u afhankelijk zijn.

Het kan in configuraties dergelijke moeilijk en uneconomical wilt testen of voorzien, elke modus mislukken, anders dan in het live systeem zelf zijn. 

### <a name="questions-"></a>Vragen...
Enkele vragen we vragen we bij het ontwikkelen van een web-systeem:

* Mijn app vastloopt? 
* Wat is precies? -Als een aanvraag is mislukt, wil ik weten hoe het er gekomen. Er moet een tracering van gebeurtenissen...
* Mijn app snel genoeg is? Hoe lang duurt het reageren op aanvragen voor typische?
* De server overbelast? Als de frequentie van aanvragen stijgt, de reactietijd hebben constante?
* Hoe responsief zijn mijn afhankelijkheden - de REST-API's, databases en andere onderdelen die mijn app aanroept. In het bijzonder als het systeem traag is, is het mijn onderdeel of krijg ik trage reacties van iemand anders?
* Mijn app is omhoog of omlaag? Kan het worden afgeleid uit de hele wereld? Informeer mij als deze wordt gestopt...
* Wat is de hoofdoorzaak? De fout in mijn-onderdeel of een afhankelijkheid is? Is het een communicatieprobleem?
* Hoeveel gebruikers ondervinden gevolgen? Als er meer dan één probleem om aan te pakken, het belangrijkste is?

## <a name="what-is-application-insights"></a>Wat is Application Insights?
![Basiswerkstroom van Application Insights](./media/app-insights-devops/020.png)

1. Application Insights instrumenten van uw app en telemetrie over deze verzendt terwijl de app wordt uitgevoerd. U kunt u de Application Insights-SDK in de app of u kunt toepassen instrumentation tijdens runtime. De eerste methode is flexibeler, zoals u uw eigen telemetrie aan de reguliere modules toevoegen kunt.
2. De telemetrie wordt verzonden naar de Application Insights-portal, waar ze worden opgeslagen en verwerkt. (Hoewel Application Insights wordt gehost in Microsoft Azure, te controleren alle web-apps - niet alleen Azure apps.)
3. De telemetrie vindt u in de vorm van grafieken en tabellen van gebeurtenissen.

Er zijn twee typen telemetrie: cumulatieve en raw-exemplaren. 

* Exemplaargegevens bevat bijvoorbeeld een rapport van een aanvraag die is ontvangen door uw web-app. U kunt vinden voor en controleer de details van een aanvraag met het hulpprogramma zoeken in de Application Insights-portal. Het exemplaar omvat gegevens zoals hoe lang uw app duurde om te reageren op de aanvraag, evenals de aangevraagde URL, bij benadering de locatie van de client en andere gegevens.
* Cumulatieve gegevens omvat het aantal gebeurtenissen per tijdseenheid, zodat u kunt de frequentie van aanvragen met de reactietijden vergelijken. Dit omvat ook gemiddelden van metrische gegevens zoals reactietijden van de aanvraag.

De belangrijkste categorieën van gegevens zijn:

* Aanvragen aan uw app (meestal HTTP-aanvragen) met de gegevens op de URL, reactietijden, en slagen of mislukken.
* Afhankelijkheden - REST en SQL aanroepen van uw app, ook met URI, responstijden en geslaagd
* Uitzonderingen, met inbegrip van stack-traces.
* Pagina weergavegegevens die afkomstig van de gebruikers browsers zijn.
* Metrische gegevens zoals prestatiemeteritems, evenals metrische gegevens die u zelf schrijven. 
* Aangepaste gebeurtenissen kunt u zakelijke gebeurtenissen bijhouden
* Logboektraceringen ten behoeve van foutopsporing.

## <a name="case-study-real-madrid-fc"></a>Casestudy: Echte Madrid F.C.
De webservice van [echte Madrid Football vereniging](http://www.realmadrid.com/) ongeveer 450 miljoen ventilatoren fungeert de hele wereld. Ventilatoren toegang hebt tot het zowel via webbrowsers en mobiele apps van de vereniging. Ventilatoren kunnen niet alleen tickets boek, maar ook toegang tot informatie en videoclips op resultaten, spelers en toekomstige games. Ze kunnen zoeken met filters zoals getallen van doelstellingen berekend. Er zijn ook koppelingen naar sociale media. De gebruikerservaring is uiterst persoonlijke en is bedoeld als een communicatie in twee richtingen ventilatoren bezighouden.

De oplossing [is een systeem van services en toepassingen op Microsoft Azure](https://www.microsoft.com/en-us/enterprise/microsoftcloud/realmadrid.aspx). Schaalbaarheid is een belangrijke vereiste: verkeer variabele en zeer hoge volumes kunnen bereiken tijdens en rond overeenkomsten.

Voor echte Madrid, is het essentieel is voor het bewaken van de prestaties van het systeem. Azure Application Insights biedt een uitgebreide weergave in het systeem een betrouwbare en hoge serviceniveau gezorgd. 

De vereniging wordt ook diepgaand inzicht in de ventilatoren: waar ze zijn (slechts 3% zijn in Spanje), welke interesse hebben spelers, historische resultaten, en toekomstige games en hoe ze reageren zodat deze overeenkomen met resultaten.

De meeste van deze telemetriegegevens automatisch worden verzameld door geen toegevoegde code waarmee de oplossing is vereenvoudigd en operationele complexiteit verlaagd.  Voor echte Madrid, Application Insights behandelt 3,8 miljard telemetrie punten elke maand.

De Power BI-module echte Madrid gebruikt om de telemetrie weer te geven.

![Power BI-weergave van telemetrie in Application Insights](./media/app-insights-devops/080.png)

## <a name="smart-detection"></a>Slimme detectie
[Proactieve diagnoses](app-insights-proactive-diagnostics.md) is een recente functie. Zonder speciale configuratie door u, Application Insights automatisch detecteert en waarschuwt u over ongebruikelijke verhogingen van uitvalpercentage in uw app. Is het verstandig voor het negeren van een achtergrond van incidentele fouten en verhogingen die zijn gewoon verhouding staat tot een toename van aanvragen. Dus bijvoorbeeld, werkt als er is een fout in een van de services die u afhankelijk zijn, of als de nieuwe bouwen die u zojuist hebt geïmplementeerd niet zo goed en u over deze weet Als u uw e-mailadres bekijkt. (En er zijn webhooks, zodat u andere apps kunt activeren.)

Een ander aspect van deze functie wordt uitgevoerd voor een dagelijkse gedetailleerde analyse van uw telemetrie, zoeken naar ongebruikelijke patronen van prestaties die moeilijk te detecteren. Bijvoorbeeld, vindt het trage prestaties die hoort bij een bepaald geografisch gebied of met een bepaalde browser-versie.

In beide gevallen wordt de waarschuwing niet geeft alleen aan de symptomen wordt gedetecteerd, maar kunt u ook gegevens die u nodig hebt om u te helpen bij het analyseren van het probleem, zoals relevante uitzonderingenrapporten.

![E-mail van proactieve diagnostische gegevens](./media/app-insights-devops/030.png)

Klant Samtec gezegd: 'we gevonden tijdens een recente functie cutover, een onder uitgebreide database die is roept de limieten en time-outs veroorzaakt. Waarschuwingen met proactieve detectie is geleverd door letterlijk we het probleem, zeer bijna realtime zijn gesorteerd geadverteerd. Deze waarschuwing samen met de Azure-platform-waarschuwingen helpen ons bijna onmiddellijk los het probleem. Totale downtime < 10 minuten."

## <a name="live-metrics-stream"></a>Metrische gegevens livestream
De laatste build te implementeren, kan een WENSENDE ervaring zijn. Als er problemen zijn, wilt u weten over deze meteen, zodat u kunt back uit, indien nodig. Metrische gegevens livestream biedt u de belangrijkste metrische gegevens met een latentie van ongeveer 1 seconde.

![Live metrische gegevens](./media/app-insights-devops/040.png)

En kunt u direct een steekproef van eventuele fouten en uitzonderingen te controleren.

![Live foutgebeurtenissen](./media/app-insights-devops/live-stream-failures.png)

## <a name="application-map"></a>Toepassingskaart
De toepassingstoewijzing detecteert automatisch de Toepassingstopologie van uw waarin de informatie over de prestaties toe, zodat u eenvoudig identificeren van knelpunten en problematisch stromen in uw hele gedistribueerde omgeving. Hiermee kunt u voor het detecteren van afhankelijkheden voor toepassingen op Azure-Services. U kunt het probleem door understanding sorteren als deze code-gerelateerde of afhankelijkheid gerelateerd en ervaring van een enkele locatie lager niveau gerelateerde diagnostische gegevens. Uw toepassing is bijvoorbeeld mogelijk beschadigd als gevolg van vertragingen in SQL-laag. Met de toepassingstoewijzing, kunt u direct bekijken en inzoomen op de SQL-Index Advisor of Query Insights optreden.

![Toepassingskaart](./media/app-insights-devops/050.png)

## <a name="application-insights-analytics"></a>Application Insights Analytics
Met [Analytics](app-insights-analytics.md), kunt u een willekeurige query's schrijven in een krachtige SQL-achtige taal.  Diagnose van over de hele app-stack wordt eenvoudig verschillende perspectieven verbinding en u kunt de juiste vragen serviceprestaties correleren met zakelijke metrische gegevens en de ervaring van de klant. 

U kunt query alle telemetrie-exemplaar en metrische onbewerkte gegevens die zijn opgeslagen in de portal. De taal bevat filter, join, aggregatie en andere bewerkingen. U kunt velden berekenen en uitvoeren van statistische analyse. Er zijn in tabelvorm en grafische visualisaties.

![Query's en resultaten grafiek](./media/app-insights-devops/025.png)

Bijvoorbeeld, is het eenvoudig:

* Gegevens van de prestaties van uw toepassing aanvragen segmenteren door lagen van de klant om te begrijpen hun ervaring.
* Zoeken naar specifieke foutcodes of namen van aangepaste gebeurtenis tijdens live site onderzoeken.
* De app-gebruik van specifieke klanten om te begrijpen hoe de functies zijn aangeschaft en vastgesteld Zoom.
* Bijhouden sessies en reactietijden voor specifieke gebruikers willen inschakelen van ondersteuning en bewerkingen teams instant klantenondersteuning te bieden.
* Bepaal functies veelgebruikte Apps functie prioriteitsaanduiding vragen te beantwoorden.

Klant DNN gezegd: 'Application Insights ons heeft geleverd het ontbrekende deel van de vergelijking voor wordt kunnen te combineren, sorteren en filteren van gegevens naar behoefte. Ons team hun eigen draaien en ervaring te vinden van gegevens met een krachtige querytaal kunnen we insights zoeken en oplossen van problemen gebruiken zodat we niet wist dat we hebben gehad. Een groot aantal interessante antwoorden afkomstig zijn van de vragen die beginnen met *' ik wonder als...'.*'

## <a name="development-tools-integration"></a>Development tools-integratie
### <a name="configuring-application-insights"></a>Application Insights configureren
Visual Studio en Eclipse hebben hulpprogramma's voor het configureren van de juiste SDK-pakketten voor het project dat u ontwikkelt. Er is een menuopdracht Application Insights toevoegen.

Als u per ongeluk een traceerlogboekregistratie framework zoals Log4N, NLog of System.Diagnostics.Trace gebruikt, vervolgens krijgt u de optie voor de logboeken naar Application Insights samen met de andere telemetrie verzenden zodat u eenvoudig de traceringen met aanvragen, afhankelijkheidsaanroepen en uitzonderingen correleren kunt.

### <a name="search-telemetry-in-visual-studio"></a>Zoeken telemetrie in Visual Studio
U kunt tijdens de ontwikkeling en foutopsporing van een functie, weergeven en zoeken van de telemetrie direct in Visual Studio, met behulp van de dezelfde search-voorzieningen zoals in de web-portal.

En wanneer Application Insights zich een uitzondering aanmeldt, kunt u bekijken van het gegevenspunt in Visual Studio en Ga door naar de relevante code.

![Visual Studio zoeken](./media/app-insights-devops/060.png)

Tijdens de foutopsporing, hebt u de optie voor het behouden van de telemetrie in uw ontwikkelcomputer bekijken in Visual Studio, maar zonder het te verzenden naar de portal. Deze optie voor lokale voorkomt de combinatie van foutopsporing met telemetrie voor productie.

### <a name="build-annotations"></a>Aantekeningen maken
Als u Visual Studio Team Services gebruikt om te bouwen en implementeren van uw app, weergegeven implementatie aantekeningen in de grafieken in de portal. Als uw meest recente versie had invloed zijn op de metrische gegevens, wordt het duidelijk.

![Aantekeningen maken](./media/app-insights-devops/070.png)

### <a name="work-items"></a>Werkitems
Wanneer een waarschuwing wordt gegenereerd, maken Application Insights automatisch een werkitem in uw werk traceringssysteem.

## <a name="but-what-about"></a>Maar hoe zit...?
* [Privacy- en](app-insights-data-retention-privacy.md) -uw telemetrie op Azure beveiligde servers wordt gehouden.
* Prestaties: het effect is zeer weinig. Telemetrie in batch wordt verwerkt.
* [Prijzen](app-insights-pricing.md) : U kunt aan de slag gratis en die blijft terwijl u in de laag volume bent.


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Volgende stappen
Aan de slag met Application Insights is eenvoudig. De belangrijkste opties zijn:

* Softwareontwikkelaars een web-app al actief. Dit biedt u de ingebouwde prestatietelemetrie. Is beschikbaar voor [Java](app-insights-java-live.md) en [IIS-servers](app-insights-monitor-performance-live-website-now.md), en ook voor [Azure-web-apps](app-insights-azure.md).
* Uw project instrumenteren tijdens de ontwikkeling. U kunt dit doen voor [ASP.NET](app-insights-asp-net.md) of [Java](app-insights-java-get-started.md) apps, evenals [Node.js](app-insights-nodejs.md) en een groot aantal [andere typen](app-insights-platforms.md). 
* Instrument [elke webpagina](app-insights-javascript.md) door een korte codefragment toe te voegen.

