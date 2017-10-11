---
title: Bewaren van gegevens en opslag in Azure Application Insights | Microsoft Docs
description: Beleidsverklaring bewaren en privacy
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: a6268811-c8df-42b5-8b1b-1d5a7e94cbca
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: bwren
ms.openlocfilehash: ddb9fa516da66da0484619439848583a29e1f5c1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="data-collection-retention-and-storage-in-application-insights"></a>Verzameling, retentie en opslag van gegevens in Application Insights


Wanneer u installeert [Azure Application Insights] [ start] SDK in uw app verzendt telemetrie over uw app naar de Cloud. Natuurlijk willen verantwoordelijk weten precies welke gegevens worden verzonden, wat gebeurt er met de gegevens en hoe ze nu de controle kunnen houden. In het bijzonder kan gevoelige gegevens worden verzonden, waarbij is opgeslagen, en hoe veilig is het? 

Eerste, het korte antwoord:

* De standaard telemetrie-modules die worden uitgevoerd 'out of het selectievakje' zijn onwaarschijnlijk dat gevoelige gegevens verzenden naar de service. De telemetrie heeft betrekking op de belasting, prestaties en gebruik metrische gegevens, uitzonderingenrapporten en andere diagnostische gegevens. De hoofdgebruiker van zichtbaar in de rapporten van diagnostische gegevens zijn URL's; maar uw app in ieder geval gevoelige gegevens in tekst zonder opmaak in een URL mag niet plaatsen.
* U kunt de code die aanvullende aangepaste telemetrie verzendt naar het uitvoeren van diagnostische gegevens en gebruiksgegevens bewaking schrijven. (Deze uitbreidbaarheid is een uitstekende functie van Application Insights.) Het is mogelijk, per ongeluk deze code te schrijven, zodat het persoonlijke en andere gevoelige gegevens bevat. Als uw toepassing met dergelijke gegevens werkt, moet u een grondig processen toepassen op alle code die u schrijft.
* Tijdens het ontwikkelen en testen van uw app, is het gemakkelijk om te controleren wat wordt verzonden door de SDK. De gegevens worden weergegeven in het windows-foutopsporing uitvoer van de IDE en de browser. 
* De gegevens worden bewaard [Microsoft Azure](http://azure.com) servers in de Verenigde Staten of Europa. (Maar overal kan worden uitgevoerd door uw app.) Azure heeft [sterke beveiliging verwerkt en voldoet aan een breed scala aan nalevingsstandaards](https://azure.microsoft.com/support/trust-center/). Alleen u en uw team aangewezen moet u toegang hebben tot uw gegevens. Medewerkers van Microsoft kunt beperkte toegang hebben tot beperkt tot specifieke beperkte omstandigheden met uw medeweten. Het versleuteld in doorvoer, maar niet in de servers.

De rest van dit artikel meer volledig wordt ingegaan op wordt deze antwoorden. Het is ontworpen om te worden ingesloten, zodat u deze aan collega's die geen deel uitmaken van uw team kunt weergeven.

## <a name="what-is-application-insights"></a>Wat is Application Insights?
[Azure Application Insights] [ start] is een service van Microsoft die u helpt de prestaties en bruikbaarheid van uw live-toepassing verbeteren. Het bewaakt uw toepassing de tijd die zijn uitgevoerd, zowel tijdens het testen en nadat u hebt gepubliceerd of geïmplementeerd. Application Insights maakt grafieken en tabellen die u hebt, bijvoorbeeld weergeven, welke tijdstippen krijgt u de meeste gebruikers, hoe responsief de app is en hoe goed worden geleverd door een externe services die afhankelijk zijn van. Als er crashes, fouten of prestatieproblemen, kunt u via de telemetriegegevens in detail om de oorzaak van de oorzaak te zoeken. En de service verzendt u e-mailberichten als er wijzigingen in de beschikbaarheid en prestaties van uw app.

Om deze functionaliteit, kunt u een Application Insights-SDK installeren in uw toepassing, dat deel van de code uitmaakt. Wanneer uw app wordt uitgevoerd, wordt de SDK de werking ervan bewaakt en telemetrie verzendt naar de Application Insights-service. Dit is een cloudservice wordt gehost door [Microsoft Azure](http://azure.com). (Maar Application Insights werkt voor alle toepassingen, niet alleen met toepassingen die worden gehost in Azure.)

![De SDK in uw app verzendt telemetrie naar de Application Insights-service.](./media/app-insights-data-retention-privacy/01-scheme.png)

De Application Insights-service worden opgeslagen en analyseert de telemetrie. Overzicht van de analyse of zoekopdracht in de opgeslagen telemetrie, u aanmelden bij uw Azure-account en opent u de Application Insights-resource voor uw toepassing. U kunt ook toegang tot de gegevens delen met andere leden van uw team of met de opgegeven Azure-abonnees.

U kunt de gegevens die zijn geëxporteerd uit de Application Insights-service, bijvoorbeeld naar een database of naar externe hulpprogramma's hebben. U kunt elk hulpprogramma opgeven met een speciale sleutel die u van de service verkrijgen. De sleutel kan worden ingetrokken, indien nodig. 

Application Insights-SDK's zijn beschikbaar voor een bereik van toepassingstypen: webservices die worden gehost in uw eigen servers J2EE of ASP.NET of in Azure. web-clients - dat wil zeggen, de code die wordt uitgevoerd in een webpagina. Desktop-apps en services; apparaat-apps, zoals Windows Phone, iOS en Android. Alle verzenden telemetrie naar dezelfde service.

## <a name="what-data-does-it-collect"></a>Welke gegevens verzamelt deze?
### <a name="how-is-the-data-is-collected"></a>Hoe wordt de gegevens worden verzameld?
Er zijn drie gegevensbronnen:

* De SDK, die u met uw app ofwel integreert [in ontwikkeling](app-insights-asp-net.md) of [tijdens runtime](app-insights-monitor-performance-live-website-now.md). Er zijn verschillende SDK's voor verschillende toepassingstypen. Er is ook een [SDK voor webpagina's](app-insights-javascript.md), die in de browser van de eindgebruiker samen met de pagina wordt geladen.
  
  * Elke SDK heeft een aantal [modules](app-insights-configuration-with-applicationinsights-config.md), welke andere technieken gebruiken voor het verzamelen van verschillende typen telemetrie.
  * Als u de SDK in ontwikkeling installeren, kunt u de API voor het verzenden van uw eigen telemetrie, naast de standaard-modules. Deze aangepaste telemetrie kunt u wilt verzenden gegevens bevatten.
* In sommige webservers zijn er ook agents die samen met de app worden uitgevoerd en het verzenden van telemetrie over CPU, geheugen en het netwerk bezetting. Bijvoorbeeld, virtuele Azure-machines, Docker-hosts en [J2EE-servers](app-insights-java-agent.md) dergelijke agents kan hebben.
* [Beschikbaarheidstests](app-insights-monitor-web-app-availability.md) processen worden uitgevoerd door Microsoft die aanvragen verzenden naar uw web-app met regelmatige tussenpozen. De resultaten worden verzonden naar de Application Insights-service.

### <a name="what-kinds-of-data-are-collected"></a>Welke typen gegevens worden verzameld?
De belangrijkste categorieën zijn:

* [Web servertelemetrie](app-insights-asp-net.md) -HTTP-aanvragen.  URI, tijd verwerking van de aanvraag, antwoordcode, client-IP-adres. Sessie-id.
* [Webpagina's](app-insights-javascript.md) -pagina-, gebruikers- en sessiegegevens aantallen. Laadtijden voor pagina's. Uitzonderingen. AJAX-aanroepen.
* Prestaties tellers - geheugen, CPU, i/o, netwerk-bezetting.
* Client en server context - OS, landinstellingen, apparaattype, browser, schermresolutie.
* [Uitzonderingen](app-insights-asp-net-exceptions.md) en crashes - **dumpbestanden stack**, CPU-type-id te bouwen. 
* [Afhankelijkheden](app-insights-asp-net-dependencies.md) -aanroepen naar externe services zoals REST, SQL, AJAX. URI of verbindingstekenreeks, duur, geslaagd, opdracht.
* [Beschikbaarheidstests](app-insights-monitor-web-app-availability.md) -duur van de test en stappen, antwoorden.
* [Traceerlogboeken](app-insights-asp-net-trace-logs.md) en [aangepaste telemetrie](app-insights-api-custom-events-metrics.md) - **alles wat u de code in de logboeken of de telemetrie**.

[Meer details](#data-sent-by-application-insights).

## <a name="how-can-i-verify-whats-being-collected"></a>Hoe kan ik controleren wat worden verzameld?
Als u de app met Visual Studio ontwikkelt, moet u de app uitvoeren in de foutopsporingsmodus (F5). De telemetrie wordt weergegeven in het uitvoervenster. U kunt van daaruit kopiëren en formatteert u het als JSON voor eenvoudig inspectie. 

![](./media/app-insights-data-retention-privacy/06-vs.png)

Er is ook een beter leesbare weergave in de Diagnostics-venster.

Open uw browservenster foutopsporing voor webpagina's.

![Druk op F12 en open het tabblad netwerk.](./media/app-insights-data-retention-privacy/08-browser.png)

### <a name="can-i-write-code-to-filter-the-telemetry-before-it-is-sent"></a>Kan ik code schrijven om te filteren van de telemetrie voordat deze wordt verzonden?
Door te schrijven mogelijk zou dit een [telemetrie processor invoegtoepassing](app-insights-api-filtering-sampling.md).

## <a name="how-long-is-the-data-kept"></a>Hoe lang de gegevens blijft?
Onbewerkte gegevenspunten (items die u kunt een query in Analytics en controleren in de zoekopdracht) tot maximaal 90 dagen bewaard. Als u moet langer zijn dan die gegevens op te slaan, kunt u [continue export](app-insights-export-telemetry.md) om deze te kopiëren naar een opslagaccount.

Cumulatieve gegevens (dat wil zeggen, aantallen, gemiddelden en andere statistische gegevens die u in de metriek Explorer ziet) worden op een interval van 1 minuut gedurende 90 dagen bewaard.

## <a name="who-can-access-the-data"></a>Wie heeft er toegang tot de gegevens?
De gegevens zijn zichtbaar voor u en hebt u een organisatieaccount, leden van uw team. 

Het door u en uw teamleden worden geëxporteerd en kan worden gekopieerd naar andere locaties en doorgegeven aan andere gebruikers.

#### <a name="what-does-microsoft-do-with-the-information-my-app-sends-to-application-insights"></a>Wat doet Microsoft met de informatie die mijn app naar Application Insights verzendt?
Microsoft gebruikt de gegevens alleen om te kunnen bieden u de service.

## <a name="where-is-the-data-held"></a>Waar is de gegevens opgeslagen?
* In de Verenigde Staten of Europa. U kunt de locatie selecteren wanneer u een nieuwe Application Insights-resource maakt. 


#### <a name="does-that-mean-my-app-has-to-be-hosted-in-the-usa-or-europe"></a>Betekent die worden gehost in de Verenigde Staten of Europa is mijn app?
* Nee. Uw toepassing overal kan worden uitgevoerd, in uw eigen lokale hosts of in de Cloud.

## <a name="how-secure-is-my-data"></a>Hoe veilig is mijn gegevens?
Application Insights is een Azure-Service. Beveiligingsbeleid worden beschreven in de [whitepaper Azure-beveiliging, Privacy en naleving](http://go.microsoft.com/fwlink/?linkid=392408).

De gegevens worden opgeslagen in Microsoft Azure-servers. Voor accounts in Azure Portal accountbeperkingen worden beschreven in de [Azure-beveiliging, Privacy en naleving document](http://go.microsoft.com/fwlink/?linkid=392408).

Toegang tot uw gegevens door medewerkers van Microsoft is beperkt. We toegang tot uw gegevens alleen met uw toestemming en als het is nodig voor de ondersteuning van uw gebruik van Application Insights. 

Gegevens in een statistische functie in alle onze klanten toepassingen (zoals gegevenssnelheden en de gemiddelde grootte van het traceren van) wordt gebruikt voor het verbeteren van Application Insights.

#### <a name="could-someone-elses-telemetry-interfere-with-my-application-insights-data"></a>Kan de telemetrie van iemand anders verstoren mijn gegevens Application Insights?
Ze kunnen extra telemetrie verzenden naar uw account met behulp van de instrumentatiesleutel die u in de code van uw webpagina's vinden kunt. Met voldoende extra gegevens zou metrische gegevens over uw niet exact overeen met de prestaties en het gebruik van uw app.

Als u code met andere projecten deelt, moet uw instrumentatiesleutel verwijderen.

## <a name="is-the-data-encrypted"></a>Worden de gegevens versleuteld?
Niet binnen de servers op dit moment.

Alle gegevens worden versleuteld doorloopt datacenters.

#### <a name="is-the-data-encrypted-in-transit-from-my-application-to-application-insights-servers"></a>Worden de gegevens versleuteld tijdens de overdracht van mijn toepassing op servers met Application Insights?
Ja, we https gebruiken om gegevens te verzenden naar de portal van bijna alle SDK's, inclusief webservers, apparaten en HTTPS-webpagina's. De enige uitzondering hierop zijn gegevens verzonden van gewone HTTP-webpagina's. 

## <a name="personally-identifiable-information"></a>Persoonlijke gegevens
#### <a name="could-personally-identifiable-information-pii-be-sent-to-application-insights"></a>Kan persoonsgegevens (PII) worden verzonden naar Application Insights?
Ja, is het mogelijk. 

Algemene richtlijnen:

* Meest standaard telemetrie (dat wil zeggen, telemetrie verzonden zonder dat u code schrijven) bevat geen expliciete PII. Echter, het is mogelijk om u te identificeren personen met Deductie uit een verzameling van gebeurtenissen.
* Uitzondering en tracering berichten kunnen PII bevatten
* Aangepaste telemetrie - aanroepen zoals TrackEvent die u schrijft in code met behulp van de API- of logboekbestand traceringen - kunt u gegevens bevatten.

De tabel aan het einde van dit document bevat meer gedetailleerde beschrijvingen van de verzamelde gegevens.

#### <a name="am-i-responsible-for-complying-with-laws-and-regulations-in-regard-to-pii"></a>Kan ik verantwoordelijk is voor die voldoet aan de wet- en regelgeving met betrekking tot PII?
Ja. Het is uw verantwoordelijkheid om ervoor te zorgen dat het verzamelen en gebruiken van de gegevens in overeenstemming met wet- en regelgeving en met de voorwaarden in Microsoft Online Services.

U moet uw klanten op de juiste wijze informeren over de gegevens die uw toepassing worden verzameld en hoe de gegevens worden gebruikt.

#### <a name="can-my-users-turn-off-application-insights"></a>Kunnen mijn gebruikers uitschakelen Application Insights?
Niet rechtstreeks. Geen bieden we een netwerkswitch die uw gebruikers werken kunnen Application Insights uitschakelen.

U kunt deze functie echter implementeren in uw toepassing. De SDK's bevatten een API-instelling uitgeschakeld telemetrie-verzameling wordt. 

#### <a name="my-application-is-unintentionally-collecting-sensitive-information-can-application-insights-scrub-this-data-so-it-isnt-retained"></a>Mijn toepassing verzamelen per ongeluk gevoelige informatie. Kan Application Insights deze gegevens wissen zodat deze wordt niet behouden
Application Insights niet filteren of verwijderen van uw gegevens. U moet de gegevens op de juiste wijze te beheren en te voorkomen dat dergelijke gegevens te verzenden naar Application Insights.

## <a name="data-sent-by-application-insights"></a>Gegevens die worden verzonden door de Application Insights
De SDK's variëren tussen platforms en er zijn verschillende onderdelen die u kunt installeren. (Raadpleeg [Application Insights - overzicht][start].) Elk onderdeel verzendt verschillende gegevens.

#### <a name="classes-of-data-sent-in-different-scenarios"></a>Klassen van gegevens die worden verzonden in verschillende scenario 's
| De actie | Gegevensklassen verzameld (Zie de volgende tabel) |
| --- | --- |
| [Application Insights-SDK toevoegen aan een .NET-webproject][greenbrown] |ServerContext<br/>Afgeleid<br/>-Prestatiemeteritems<br/>Aanvragen<br/>**Uitzonderingen**<br/>Sessie<br/>gebruikers |
| [Statusmonitor installeren op IIS][redfield] |Afhankelijkheden<br/>ServerContext<br/>Afgeleid<br/>-Prestatiemeteritems |
| [Application Insights-SDK toevoegen aan een Java-web-app][java] |ServerContext<br/>Afgeleid<br/>Aanvraag<br/>Sessie<br/>gebruikers |
| [JavaScript-SDK toevoegen aan webpagina 's][client] |ClientContext <br/>Afgeleid<br/>Pagina<br/>ClientPerf<br/>AJAX |
| [Standaard-eigenschappen definiëren][apiproperties] |**Eigenschappen** op alle standaard- en aangepaste gebeurtenissen |
| [Aanroep TrackMetric][api] |Numerieke waarden<br/>**Eigenschappen** |
| [Aanroep bijhouden *][api] |De naam van gebeurtenis<br/>**Eigenschappen** |
| [Aanroep TrackException][api] |**Uitzonderingen**<br/>Stackdump<br/>**Eigenschappen** |
| SDK kan geen gegevens verzamelen. Bijvoorbeeld: <br/> -heeft geen toegang tot prestatiemeteritems<br/> -uitzondering in telemetrie initialisatiefunctie |SDK diagnostische gegevens |

Voor [SDK's voor andere platforms][platforms], Zie hun documenten.

#### <a name="the-classes-of-collected-data"></a>De klassen van de verzamelde gegevens
| Klasse van de verzamelde gegevens | (Geen uitputtende lijst) bevat |
| --- | --- |
| **Eigenschappen** |**Een gegevens - bepaald door uw code** |
| DeviceContext |ID, IP-, landinstellingen, Apparaatmodel, netwerk, netwerktype, OEM-naam, schermresolutie Rolinstantie, Role-naam, Type apparaat |
| ClientContext |OS landinstellingen, taal, netwerk, venster resolutie |
| Sessie |Sessie-id |
| ServerContext |Naam van de computer, landinstellingen, besturingssysteem, apparaat, gebruikerssessie, gebruikerscontext, bewerking |
| Afgeleid |geografische locatie van IP-adres, timestamp, besturingssysteem, browser |
| Metrische gegevens |Metrische naam en waarde |
| Gebeurtenissen |Gebeurtenisnaam en waarde |
| PageViews |URL- en de naam of scherm |
| Client perf |Naam van de URL/pagina, laadtijd van de browser |
| AJAX |HTTP-aanroepen vanuit een webpagina met server |
| Aanvragen |De URL, duur, antwoordcode |
| Afhankelijkheden |Type (SQL, HTTP,...), de verbindingsreeks of de URI, sync/async, duur, geslaagd, SQL-instructie (met Status Monitor) |
| **Uitzonderingen** |Type **bericht**, stacks aanroepen, bron- en regelnummer number, thread-id |
| Crashes |Proces-id, proces-id van bovenliggende, crashes thread-id; toepassingspatch, -id, build;  uitzonderingstype, adres, reden; verborgen symbolen en registers binaire begin- en -adressen, binaire naam en pad, cpu-type |
| Tracering |**Bericht** en ernst |
| -Prestatiemeteritems |Processortijd, beschikbaar geheugen, frequentie van aanvragen, uitzondering snelheid, proces eigen bytes, i/o-snelheid, duur van de aanvraag, lengte van aanvraagwachtrij |
| Beschikbaarheid |Web-test antwoord-code, de duur van elke stap, naam van test, timestamp, geslaagd, reactietijden, testlocatie |
| SDK diagnostische gegevens |Trace-bericht of uitzondering |

U kunt [enkele van de gegevens door te bewerken ApplicationInsights.config uitschakelen][config]

## <a name="credits"></a>Tegoed
Dit product bevat GeoLite2 gegevens die zijn gemaakt door MaxMind beschikbaar is via [http://www.maxmind.com](http://www.maxmind.com).



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[platforms]: app-insights-platforms.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

