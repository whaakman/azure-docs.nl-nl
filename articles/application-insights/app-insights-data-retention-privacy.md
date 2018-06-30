---
title: Bewaren van gegevens en opslag in Azure Application Insights | Microsoft Docs
description: Beleidsverklaring bewaren en privacy
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: a6268811-c8df-42b5-8b1b-1d5a7e94cbca
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/29/2018
ms.author: mbullwin
ms.openlocfilehash: 897671ef592ac691402a4e452f7a0baa04aa228a
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129054"
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
* In de Verenigde Staten, Europa of Zuidoost-Azië. U kunt de locatie selecteren wanneer u een nieuwe Application Insights-resource maakt. 


#### <a name="does-that-mean-my-app-has-to-be-hosted-in-the-usa-europe-or-southeast-asia"></a>Betekent die worden gehost in de Verenigde Staten, Europa of Zuidoost-Azië is mijn app?
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

## <a name="how-do-i-send-data-to-application-insights-using-tls-12"></a>Hoe kan ik verzenden gegevens naar Application Insights met TLS 1.2?

Als u wilt zorgen dat de beveiliging van gegevens tijdens verzending naar de Application Insights-eindpunten, we raden klanten hun toepassing configureren voor gebruik ten minste Transport Layer Security (TLS) 1.2. Oudere versies van TLS/Secure Sockets Layer (SSL) kwetsbaar zijn gevonden en ze werken op dit moment nog steeds zodat achterwaartse compatibiliteit, zijn ze **niet aanbevolen**, en de branche snel is verplaatst naar het afbreken van ondersteuning voor deze oudere protocollen. 

De [PCI Security Standards Council](https://www.pcisecuritystandards.org/) heeft een [deadline van 30 juni 2018](https://www.pcisecuritystandards.org/pdfs/PCI_SSC_Migrating_from_SSL_and_Early_TLS_Resource_Guide.pdf) oudere versies van TLS/SSL en upgrade op meer veilige protocollen uitschakelen. Wanneer Azure ondersteuning voor oudere, zakt als uw toepassing/clients via ten minste communiceren kunnen TLS 1.2 is het zou niet mogelijk om gegevens te verzenden naar Application Insights. De aanpak die u ondernemen om te testen en valideren van uw toepassing TLS-ondersteuning is afhankelijk van het besturingssysteem/platform, evenals het taalframework/maakt gebruik van uw toepassing.

We raden niet expliciet instellen van uw toepassing alleen gebruik van TLS 1.2 tenzij dit echt nodig omdat dit platform beveiliging op rijniveau functies waarmee u automatisch detecteren en te profiteren van de nieuwere veiliger protocollen zodra deze kunt verbreken beschikbaar zoals TLS 1.3. Het is raadzaam om het uitvoeren van een uitgebreide controle van uw toepassingscode om te controleren op hardcoderen van specifieke versies van TLS/SSL.

### <a name="platformlanguage-specific-guidance"></a>Specifieke hulp platform/taal

|Platform/taal | Ondersteuning | Meer informatie |
| --- | --- | --- |
| Azure App Services  | Ondersteund, zijn configuratie vereist. | Ondersteuning is in April 2018 aangekondigd. Lezen van de aankondiging voor [configuratiedetails](https://blogs.msdn.microsoft.com/appserviceteam/2018/04/17/app-service-and-functions-hosted-apps-can-now-update-tls-versions/).  |
| Azure-functie Apps | Ondersteund, zijn configuratie vereist. | Ondersteuning is in April 2018 aangekondigd. Lezen van de aankondiging voor [configuratiedetails](https://blogs.msdn.microsoft.com/appserviceteam/2018/04/17/app-service-and-functions-hosted-apps-can-now-update-tls-versions/). |
|.NET | Ondersteund, varieert configuratie per versie. | Raadpleeg voor gedetailleerde configuratie-informatie voor .NET 4.7 en eerdere versies [deze instructies](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls#support-for-tls-12).  |
|Statuscontrole | Ondersteunde, vereiste configuratie | Statusmonitor is afhankelijk van [configuratie van het besturingssysteem](https://docs.microsoft.com/en-us/windows-server/security/tls/tls-registry-settings) + [configuratie .NET](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls#support-for-tls-12) voor ondersteuning van TLS 1.2.
|Node.js |  Ondersteund in v10.5.0, zijn configuratie vereist. | Gebruik de [officiële Node.js TLS/SSL-documentatie](https://nodejs.org/api/tls.html) voor elke specifieke configuratie van de toepassing. |
|Java | Ondersteund, JDK-ondersteuning voor TLS 1.2 is toegevoegd in [JDK 6 update 121](http://www.oracle.com/technetwork/java/javase/overview-156328.html#R160_121) en [JDK 7](http://www.oracle.com/technetwork/java/javase/7u131-relnotes-3338543.html). | Maakt gebruik van 8 JDK [TLS 1.2 standaard](https://blogs.oracle.com/java-platform-group/jdk-8-will-use-tls-12-as-default).  |
|Linux | Linux-distributies meestal afhankelijk zijn van [OpenSSL](https://www.openssl.org) voor ondersteuning van TLS 1.2.  | Controleer de [OpenSSL Changelog](https://www.openssl.org/news/changelog.html) om te bevestigen dat de versie van OpenSSL wordt ondersteund.|
| Windows 8.0 10 | Ondersteunde en standaard ingeschakeld. | Om te bevestigen dat u nog steeds de [standaardinstellingen](https://docs.microsoft.com/en-us/windows-server/security/tls/tls-registry-settings).  |
| WindowsServer 2012-2016 | Ondersteunde en standaard ingeschakeld. | Om te bevestigen dat u nog steeds de [standaardinstellingen](https://docs.microsoft.com/en-us/windows-server/security/tls/tls-registry-settings) |
| Windows 7 SP1 en Windows Server 2008 R2 SP1 | Ondersteund, maar niet standaard ingeschakeld. | Zie de [Transport Layer Security (TLS) registerinstellingen](https://docs.microsoft.com/en-us/windows-server/security/tls/tls-registry-settings) pagina voor meer informatie over het inschakelen.  |
| Windows Server 2008 SP2 | Ondersteuning voor TLS 1.2 moet worden bijgewerkt. | Zie [Update voor het toevoegen van ondersteuning voor TLS 1.2](https://support.microsoft.com/help/4019276/update-to-add-support-for-tls-1-1-and-tls-1-2-in-windows-server-2008-s) in Windows Server 2008 SP2. |
|Windows Vista | Niet ondersteund. | N/A

### <a name="check-what-version-of-openssl-your-linux-distribution-is-running"></a>Controleer welke versie van OpenSSL uw Linux-distributie wordt uitgevoerd.

Als u wilt controleren welke versie van OpenSSL die u hebt geïnstalleerd, open de terminal en voer:

```terminal
openssl version -a
```

### <a name="run-a-test-tls-12-transaction-on-linux"></a>Een test met TLS 1.2 transactie uitvoert op Linux

Uitvoeren van een voorlopige basistest om te zien als uw Linux-systeem via TLS 1.2 communiceren kan. Open de terminal en voer:

```terminal
openssl s_client -connect bing.com:443 -tls1_2
```

## <a name="personal-data-stored-in-application-insights"></a>Persoonlijke gegevens die zijn opgeslagen in de Application Insights

Onze [Application Insights persoonsgegevens artikel](app-insights-customer-data.md) wordt dit probleem gedetailleerd beschreven.

#### <a name="can-my-users-turn-off-application-insights"></a>Kunnen mijn gebruikers uitschakelen Application Insights?
Niet rechtstreeks. Geen bieden we een netwerkswitch die uw gebruikers werken kunnen Application Insights uitschakelen.

U kunt deze functie echter implementeren in uw toepassing. De SDK's bevatten een API-instelling uitgeschakeld telemetrie-verzameling wordt. 

## <a name="data-sent-by-application-insights"></a>Gegevens die worden verzonden door de Application Insights
De SDK's variëren tussen platforms en er zijn verschillende onderdelen die u kunt installeren. (Raadpleeg [Application Insights - overzicht][start].) Elk onderdeel verzendt verschillende gegevens.

#### <a name="classes-of-data-sent-in-different-scenarios"></a>Klassen van gegevens die worden verzonden in verschillende scenario 's
| De actie | Gegevensklassen verzameld (Zie de volgende tabel) |
| --- | --- |
| [Application Insights-SDK toevoegen aan een .NET-webproject][greenbrown] |ServerContext<br/>Afgeleid<br/>Prestatiemeteritems<br/>Aanvragen<br/>**Uitzonderingen**<br/>Sessie<br/>gebruikers |
| [Statusmonitor installeren op IIS][redfield] |Afhankelijkheden<br/>ServerContext<br/>Afgeleid<br/>Prestatiemeteritems |
| [Application Insights-SDK toevoegen aan een Java-web-app][java] |ServerContext<br/>Afgeleid<br/>Aanvraag<br/>Sessie<br/>gebruikers |
| [JavaScript-SDK toevoegen aan webpagina 's][client] |ClientContext <br/>Afgeleid<br/>Pagina<br/>ClientPerf<br/>Ajax |
| [Standaard-eigenschappen definiëren][apiproperties] |**Eigenschappen** op alle standaard- en aangepaste gebeurtenissen |
| [Aanroep TrackMetric][api] |Numerieke waarden<br/>**Eigenschappen** |
| [Aanroep bijhouden *][api] |Gebeurtenisnaam<br/>**Eigenschappen** |
| [Aanroep TrackException][api] |**Uitzonderingen**<br/>Stackdump<br/>**Eigenschappen** |
| SDK kan geen gegevens verzamelen. Bijvoorbeeld: <br/> -heeft geen toegang tot prestatiemeteritems<br/> -uitzondering in telemetrie initialisatiefunctie |SDK diagnostische gegevens |

Voor [SDK's voor andere platforms][platforms], Zie hun documenten.

#### <a name="the-classes-of-collected-data"></a>De klassen van de verzamelde gegevens
| Klasse van de verzamelde gegevens | (Geen uitputtende lijst) bevat |
| --- | --- |
| **Eigenschappen** |**Een gegevens - bepaald door uw code** |
| DeviceContext |ID, IP-, landinstellingen, Apparaatmodel, netwerk, netwerktype, OEM-naam, schermresolutie Rolinstantie, Role-naam, Type apparaat |
| ClientContext |OS landinstellingen, taal, netwerk, venster resolutie |
| Sessie |sessie-id |
| ServerContext |Naam van de computer, landinstellingen, besturingssysteem, apparaat, gebruikerssessie, gebruikerscontext, bewerking |
| Afgeleid |geografische locatie van IP-adres, timestamp, besturingssysteem, browser |
| Metrische gegevens |Metrische naam en waarde |
| Gebeurtenissen |Gebeurtenisnaam en waarde |
| PageViews |URL- en de naam of scherm |
| Client perf |Naam van de URL/pagina, laadtijd van de browser |
| Ajax |HTTP-aanroepen vanuit een webpagina met server |
| Aanvragen |De URL, duur, antwoordcode |
| Afhankelijkheden |Type (SQL, HTTP,...), de verbindingsreeks of de URI, sync/async, duur, geslaagd, SQL-instructie (met Status Monitor) |
| **Uitzonderingen** |Type **bericht**, stacks aanroepen, bron- en regelnummer number, thread-id |
| Crashes |Proces-id, proces-id van bovenliggende, crashes thread-id; toepassingspatch, -id, build;  uitzonderingstype, adres, reden; verborgen symbolen en registers binaire begin- en -adressen, binaire naam en pad, cpu-type |
| Tracering |**Bericht** en ernst |
| Prestatiemeteritems |Processortijd, beschikbaar geheugen, frequentie van aanvragen, uitzondering snelheid, proces eigen bytes, i/o-snelheid, duur van de aanvraag, lengte van aanvraagwachtrij |
| Beschikbaarheid |Web-test antwoord-code, de duur van elke stap, naam van test, timestamp, geslaagd, reactietijden, testlocatie |
| SDK diagnostische gegevens |Trace-bericht of uitzondering |

U kunt [enkele van de gegevens door te bewerken ApplicationInsights.config uitschakelen][config]

## <a name="credits"></a>Credits
Dit product bevat GeoLite2 gegevens die zijn gemaakt door MaxMind beschikbaar is via [ http://www.maxmind.com ](http://www.maxmind.com).



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

