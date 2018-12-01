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
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: mbullwin
ms.openlocfilehash: 075358ad559806c375d44b6a125e8d2839b6632f
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52726301"
---
# <a name="data-collection-retention-and-storage-in-application-insights"></a>Verzameling, retentie en opslag van gegevens in Application Insights

Wanneer u installeert [Azure Application Insights] [ start] SDK in uw app, verzendt deze telemetrie over uw app naar de Cloud. Op een natuurlijke manier, verantwoordelijk ontwikkelaars meer willen weten precies welke gegevens worden verzonden, wat gebeurt er met de gegevens en hoe ze deze controle kunnen behouden. In het bijzonder, kan gevoelige gegevens worden verzonden, waarbij is opgeslagen, en hoe veilig is het? 

Eerste, de korte antwoord:

* De standaardtelemetrie voor modules die worden uitgevoerd 'buiten het vak' waarschijnlijk geen gevoelige gegevens te verzenden naar de service. De telemetrie is betrokken bij het laden, prestaties en gebruik metrische gegevens, uitzonderingenrapporten en andere diagnostische gegevens. De belangrijkste gegevens zichtbaar zijn in de diagnostische rapporten worden URL's; maar uw app mogen geen gevoelige gegevens in elk geval geplaatst als tekst zonder opmaak in een URL.
* U kunt code schrijven die aanvullende aangepaste telemetrie verzendt naar helpt u met de diagnostische gegevens en bewaking wordt gebruikt. (Deze uitbreiding is een fantastische functie van Application Insights.) Is het mogelijk, per ongeluk deze code schrijven, zodat het persoonlijke en andere gevoelige gegevens bevat. Als uw toepassing met dergelijke gegevens werkt, moet u een grondig processen toepassen op de code die u schrijft.
* Tijdens het ontwikkelen en testen van uw app, is het gemakkelijk om te controleren wat er door de SDK worden verzonden. De gegevens weergegeven in de windows-foutopsporing uitvoer van de IDE en de browser. 
* De gegevens worden bewaard in [Microsoft Azure](http://azure.com) servers in de Verenigde Staten of Europa. (Maar overal kan worden uitgevoerd door uw app.) Azure heeft [sterke beveiliging verwerkt en voldoet aan een breed scala aan standaarden voor compliance](https://azure.microsoft.com/support/trust-center/). Alleen u en uw team aangewezen hebt toegang tot uw gegevens. Medewerkers van Microsoft kunt beperkte toegang toe alleen in bepaalde beperkte omstandigheden met uw kennis. Het versleuteld in doorvoer, maar niet in de servers.

De rest van dit artikel meer volledig wordt ingegaan op wordt deze vragen te beantwoorden. Het is ontworpen om zichzelf, zodat u deze kunt weergeven aan collega's die geen deel uitmaken van uw team.

## <a name="what-is-application-insights"></a>Wat is Application Insights?
[Azure Application Insights] [ start] is een service van Microsoft waarmee u de prestaties en bruikbaarheid van uw live-toepassing verbeteren. Deze bewaakt uw toepassing voortdurend die wordt uitgevoerd, zowel tijdens het testen en nadat u hebt gepubliceerd of geïmplementeerd. Application Insights maakt grafieken en tabellen waarin u u, bijvoorbeeld ziet, welke tijdstippen van de dag krijgt u de meeste gebruikers, hoe snel de app is en hoe goed wordt geleverd door de externe services die afhankelijk van is. Als er crashes, fouten of prestatieproblemen, kunt u zoeken via de telemetrische gegevens in detail om de oorzaak te achterhalen. En de service wordt u e-mails verzenden als er wijzigingen in de beschikbaarheid en prestaties van uw app.

Om deze functionaliteit, kunt u een Application Insights-SDK installeren in uw toepassing, dat deel van de code uitmaakt. Wanneer uw app wordt uitgevoerd, wordt de SDK de werking ervan bewaakt en telemetrie verzendt naar de Application Insights-service. Dit is een cloudservice wordt gehost door [Microsoft Azure](http://azure.com). (Maar Application Insights werkt voor toepassingen, niet alleen de objecten die worden gehost in Azure.)

![De SDK in uw app verzendt telemetrie naar de Application Insights-service.](./media/app-insights-data-retention-privacy/01-scheme.png)

De Application Insights-service worden opgeslagen en de telemetriegegevens te analyseren. Als u wilt zien van de analyse of zoekopdracht in de opgeslagen telemetrie, u aanmelden bij uw Azure-account en de Application Insights-resource openen voor uw toepassing. U kunt ook toegang tot de gegevens delen met andere leden van uw team of met de opgegeven Azure-abonnees.

U kunt gegevens die zijn geëxporteerd uit de Application Insights-service, bijvoorbeeld naar een database of naar externe hulpprogramma's hebben. U kunt elk hulpprogramma opgeven met een speciale sleutel die u via de service aanschaft. De sleutel kan worden ingetrokken, indien nodig. 

Application Insights-SDK's zijn beschikbaar voor een bereik van toepassingstypen: web-services die worden gehost in uw eigen J2EE of ASP.NET-servers, of in Azure. web-clients - dat wil zeggen, de code die wordt uitgevoerd in een webpagina. bureaublad-apps en services. apps op apparaten, zoals Windows Phone, iOS en Android. Alle verzenden telemetrie naar dezelfde service.

## <a name="what-data-does-it-collect"></a>Welke gegevens verzamelt deze?
### <a name="how-is-the-data-is-collected"></a>Hoe worden de gegevens worden verzameld?
Er zijn drie gegevensbronnen:

* De SDK, die u met uw app ofwel integreert [in de ontwikkeling van](app-insights-asp-net.md) of [tijdens runtime](app-insights-monitor-performance-live-website-now.md). Er zijn verschillende SDK's voor verschillende toepassingstypen. Er is ook een [SDK voor webpagina's](app-insights-javascript.md), die in de browser de eindgebruiker, samen met de pagina wordt geladen.
  
  * Elke SDK heeft een aantal [modules](app-insights-configuration-with-applicationinsights-config.md), welke verschillende technieken gebruiken voor het verzamelen van verschillende typen telemetrie.
  * Als u de SDK in de ontwikkeling installeren, kunt u de API voor het verzenden van uw eigen telemetrie, naast de standard-modules. Deze aangepaste telemetrische gegevens kan bevatten alle gegevens die u wilt verzenden.
* In bepaalde webservers zijn er ook agents die samen met de app uitvoeren en telemetrie over de CPU, geheugen en netwerk bezetting verzendt. Bijvoorbeeld, virtuele machines van Azure Docker-hosts en [J2EE-servers](app-insights-java-agent.md) dergelijke agents kan hebben.
* [Beschikbaarheidstests](app-insights-monitor-web-app-availability.md) processen worden uitgevoerd door Microsoft en die aanvragen verzenden naar uw web-app met regelmatige intervallen. De resultaten worden verzonden naar de Application Insights-service.

### <a name="what-kinds-of-data-are-collected"></a>Wat voor soort gegevens worden verzameld?
De belangrijkste categorieën zijn:

* [Web servertelemetrie](app-insights-asp-net.md) -HTTP-aanvragen.  URI, gebruikte tijd voor het verwerken van de aanvraag, responscode, client-IP-adres. Sessie-id.
* [Webpagina's](app-insights-javascript.md) -pagina, gebruikers- en sessieaantallen. Laadtijden voor pagina's. Uitzonderingen. AJAX-aanroepen.
* Prestaties tellers - geheugen, CPU, i/o-, netwerk-bezetting.
* Client en server context - besturingssysteem, landinstelling, apparaattype, browser, schermresolutie.
* [Uitzonderingen](app-insights-asp-net-exceptions.md) en crashes - **dumpbestanden voor foutopsporing stack**, CPU-type-id maken. 
* [Afhankelijkheden](app-insights-asp-net-dependencies.md) -aanroepen naar externe services, zoals REST, SQL, AJAX. URI of verbindingsreeks, duur, geslaagd, opdracht.
* [Beschikbaarheidstests](app-insights-monitor-web-app-availability.md) -duur van test en stappen, antwoorden.
* [Logboeken met traceringen](app-insights-asp-net-trace-logs.md) en [aangepaste telemetrie](app-insights-api-custom-events-metrics.md) - **alles wat u de code in uw logboeken of telemetrie**.

[Meer details](#data-sent-by-application-insights).

## <a name="how-can-i-verify-whats-being-collected"></a>Hoe kan ik controleren wat wordt verzameld?
Als u de app met Visual Studio ontwikkelt, moet u de app uitvoeren in de foutopsporingsmodus (F5). De telemetrie weergegeven in het uitvoervenster weergegeven. U kunt van daaruit kopiëren en deze als JSON-indeling voor gemakkelijke controle. 

![](./media/app-insights-data-retention-privacy/06-vs.png)

Er is ook een beter leesbare weergave in het venster diagnostische gegevens.

Open uw browservenster opsporen van fouten voor webpagina's.

![Druk op F12 en open het tabblad netwerk.](./media/app-insights-data-retention-privacy/08-browser.png)

### <a name="can-i-write-code-to-filter-the-telemetry-before-it-is-sent"></a>Kan ik code schrijven om te filteren van de telemetrie voordat deze wordt verzonden?
Dit mogelijk zou zijn door het schrijven van een [telemetrie processor-invoegtoepassing](app-insights-api-filtering-sampling.md).

## <a name="how-long-is-the-data-kept"></a>Hoe lang worden de gegevens bewaard?
Onbewerkte gegevenspunten (dat wil zeggen, items die u kunt query's uitvoeren in Analytics en controleren in het zoekvak) zijn maximaal 90 dagen bewaard. Als u nodig hebt om gegevens langer duurt dan te houden, kunt u [continue export](app-insights-export-telemetry.md) om deze te kopiëren naar een opslagaccount.

Cumulatieve gegevens (dat wil zeggen, aantal, gemiddelde en andere statistische gegevens die u in Metric Explorer ziet) worden bewaard in een interval van 1 minuut gedurende 90 dagen.

[Fouten opsporen in momentopnamen](app-insights-snapshot-debugger.md) gedurende zeven dagen worden opgeslagen. Deze bewaarbeleid is ingesteld op basis van de per toepassing. Als u nodig hebt om deze waarde te verhogen, kunt u een toename van aanvragen door een ondersteuningsaanvraag opent in de Azure-portal.

## <a name="who-can-access-the-data"></a>Wie heeft er toegang tot de gegevens?
De gegevens zijn zichtbaar voor u en hebt u een organisatieaccount, leden van uw team. 

Worden geëxporteerd door u en uw teamleden kan en kan worden gekopieerd naar andere locaties en doorgegeven aan andere gebruikers.

#### <a name="what-does-microsoft-do-with-the-information-my-app-sends-to-application-insights"></a>Wat doet Microsoft met de informatie verzonden naar Application Insights via Mijn app?
Microsoft gebruikt de gegevens alleen om te kunnen bieden u de service.

## <a name="where-is-the-data-held"></a>Waar worden de gegevens bewaard?
* In de Verenigde Staten, Europa of Zuidoost-Azië. Wanneer u een nieuwe Application Insights-resource maakt, kunt u de locatie selecteren. 


#### <a name="does-that-mean-my-app-has-to-be-hosted-in-the-usa-europe-or-southeast-asia"></a>Houdt dat in mijn app moet worden gehost in de Verenigde Staten, Europa of Zuidoost-Azië?
* Nee. Uw toepassing kan overal worden uitgevoerd in de hosts van uw eigen on-premises of in de Cloud.

## <a name="how-secure-is-my-data"></a>Hoe veilig zijn mijn gegevens?
Application Insights is een Azure-Service. Beleidsregels voor veiligheid zijn beschreven in de [whitepaper voor Azure-beveiliging, Privacy en naleving](https://go.microsoft.com/fwlink/?linkid=392408).

De gegevens worden opgeslagen in Microsoft Azure-servers. Voor accounts in Azure Portal, accountbeperkingen worden beschreven in de [Azure-beveiliging, Privacy en naleving document](https://go.microsoft.com/fwlink/?linkid=392408).

Toegang tot uw gegevens door medewerkers van Microsoft is beperkt. We toegang tot uw gegevens alleen met uw toestemming en als deze nodig voor het ondersteunen van uw gebruik van Application Insights is. 

Gegevens in een statistische functie voor al onze klanten toepassingen (zoals gegevenstarieven en gemiddelde grootte van traceringen) wordt gebruikt voor het verbeteren van Application Insights.

#### <a name="could-someone-elses-telemetry-interfere-with-my-application-insights-data"></a>Kan de telemetrie van iemand anders leiden tot problemen met mijn gegevens Application Insights?
Ze kunnen extra telemetrie verzenden naar uw account met behulp van de instrumentatiesleutel die kan worden gevonden in de code van uw webpagina's. Met voldoende bijkomende gegevens, zou uw metrische gegevens niet exact overeen met de prestaties en het gebruik van uw app.

Als u code met andere projecten delen, vergeet dan niet te verwijderen van de instrumentatiesleutel.

## <a name="is-the-data-encrypted"></a>Worden de gegevens versleuteld?
Niet binnen de servers op dit moment.

Alle gegevens worden versleuteld wanneer deze worden verplaatst tussen datacenters.

#### <a name="is-the-data-encrypted-in-transit-from-my-application-to-application-insights-servers"></a>Worden de gegevens versleuteld tijdens de overdracht van mijn toepassing naar Application Insights-servers?
Ja, we https gebruiken om gegevens te verzenden naar de portal van bijna alle SDK's, zoals webservers, apparaten en HTTPS-webpagina's. De enige uitzondering hierop zijn gegevens verzonden van gewone HTTP-webpagina's.

## <a name="does-the-sdk-create-temporary-local-storage"></a>Maakt de SDK tijdelijk lokale opslag?

Ja, bepaalde kanalen telemetrie blijft actief data lokaal als een eindpunt kan niet worden bereikt. Controleer hieronder om te zien welke frameworks en telemetrie kanalen zijn beïnvloed.


Telemetrie-kanalen die gebruikmaken van lokale opslag maken tijdelijke bestanden in de TEMP of APPDATA mappen die zijn beperkt tot het uitvoeren van uw toepassing specifieke-account. Dit kan gebeuren wanneer een eindpunt tijdelijk niet beschikbaar is of u de bandbreedteregeling limiet bereikt. Zodra dit probleem verholpen is, hervat de telemetrie-kanaal verzenden van alle nieuwe en persistente gegevens.


Deze persistente gegevens **niet versleuteld** en het wordt sterk aanbevolen om te herstructureren van uw beleid voor gegevensverzameling om uit te schakelen van het verzamelen van persoonlijke gegevens. (Zie [exporteren en verwijderen van persoonlijke gegevens](https://docs.microsoft.com/azure/application-insights/app-insights-customer-data#how-to-export-and-delete-private-data) voor meer informatie.)


Als een klant moet deze map configureren met specifieke beveiligingsvereisten kan deze worden geconfigureerd per framework. Zorg ervoor dat het proces uitvoeren van uw toepassing schrijven toegang tot deze map heeft, maar ook voor zorgen dat deze map wordt beveiligd om te voorkomen dat telemetrie wordt gelezen door ongewenste gebruikers.

### <a name="java"></a>Java

`C:\Users\username\AppData\Local\Temp` wordt gebruikt voor het vastleggen van gegevens. Deze locatie niet kunnen worden geconfigureerd in de configuratiemap en de machtigingen voor toegang tot deze map zijn beperkt tot de specifieke gebruiker met de vereiste referenties. (Zie [implementatie](https://github.com/Microsoft/ApplicationInsights-Java/blob/40809cb6857231e572309a5901e1227305c27c1a/core/src/main/java/com/microsoft/applicationinsights/internal/util/LocalFileSystemUtils.java#L48-L72) hier.)

###  <a name="net"></a>.Net

Standaard `ServerTelemetryChannel` maakt gebruik van de huidige gebruiker lokale app-gegevensmap `%localAppData%\Microsoft\ApplicationInsights` of de tijdelijke map `%TMP%`. (Zie [implementatie](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/91e9c91fcea979b1eec4e31ba8e0fc683bf86802/src/ServerTelemetryChannel/Implementation/ApplicationFolderProvider.cs#L54-L84) hier.)


Via het configuratiebestand:
```
<TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel,   Microsoft.AI.ServerTelemetryChannel">
    <StorageFolder>D:\NewTestFolder</StorageFolder>
</TelemetryChannel>
```

Via code:

- ServerTelemetryChannel verwijderen uit het configuratiebestand
- Dit codefragment toevoegen aan uw configuratie:
```
ServerTelemetryChannel channel = new ServerTelemetryChannel();
channel.StorageFolder = @"D:\NewTestFolder";
channel.Initialize(TelemetryConfiguration.Active);
TelemetryConfiguration.Active.TelemetryChannel = channel;
```

### <a name="netcore"></a>NetCore

Standaard `ServerTelemetryChannel` maakt gebruik van de huidige gebruiker lokale app-gegevensmap `%localAppData%\Microsoft\ApplicationInsights` of de tijdelijke map `%TMP%`. (Zie [implementatie](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/91e9c91fcea979b1eec4e31ba8e0fc683bf86802/src/ServerTelemetryChannel/Implementation/ApplicationFolderProvider.cs#L54-L84) hier.) Lokale opslag worden, een Linux-omgeving uitgeschakeld, tenzij een opslagmap is opgegeven.

Het volgende codefragment laat zien hoe u om in te stellen `ServerTelemetryChannel.StorageFolder` in de `ConfigureServices()`  -methode van uw `Startup.cs` klasse:

```
services.AddSingleton(typeof(ITelemetryChannel), new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
```

(Zie [AspNetCore aangepaste configuratie](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) voor meer informatie. )

### <a name="nodejs"></a>Node.js

Standaard `%TEMP%/appInsights-node{INSTRUMENTATION KEY}` wordt gebruikt voor het vastleggen van gegevens. Machtigingen voor toegang tot deze map zijn beperkt tot de huidige gebruiker en beheerders. (Zie [implementatie](https://github.com/Microsoft/ApplicationInsights-node.js/blob/develop/Library/Sender.ts) hier.)

Het voorvoegsel van de map `appInsights-node` kan worden overschreven door het wijzigen van de runtime-waarde van de statische variabele `Sender.TEMPDIR_PREFIX` gevonden in [Sender.ts](https://github.com/Microsoft/ApplicationInsights-node.js/blob/7a1ecb91da5ea0febf5ceab13d6a4bf01a63933d/Library/Sender.ts#L384).



## <a name="how-do-i-send-data-to-application-insights-using-tls-12"></a>Hoe kan ik verzenden gegevens naar Application Insights met behulp van TLS 1.2?

Als u wilt controleren of de beveiliging van gegevens die onderweg zijn met de Application Insights-eindpunten, we raden klanten hun toepassing configureren voor het gebruik van ten minste Transport Layer Security (TLS) 1.2. Oudere versies van TLS/Secure Sockets Layer (SSL) kwetsbaar zijn gevonden en hoewel ze op dit moment nog steeds werken om toe te staan achterwaartse compatibiliteit, zijn ze onderling **niet aanbevolen**, en de branche is snel veranderende te breken ondersteuning voor deze oudere protocollen. 

De [PCI Security Standards Council heeft onlangs](https://www.pcisecuritystandards.org/) heeft een [deadline van 30 juni 2018](https://www.pcisecuritystandards.org/pdfs/PCI_SSC_Migrating_from_SSL_and_Early_TLS_Resource_Guide.pdf) om uit te schakelen van oudere versies van TLS/SSL en upgrade voor de protocollen beter kunt beveiligen. Nadat Azure ondersteuning voor oudere, komt als uw toepassing/clients niet kunnen via ten minste communiceren TLS 1.2 u niet mogelijk zou zijn om gegevens te verzenden naar Application Insights. De methode die u ondernemen om te testen en valideren van de TLS-ondersteuning van uw toepassing varieert afhankelijk van het besturingssysteem/platform, evenals de taal/framework die maakt gebruik van uw toepassing.

We raden niet expliciet instellen van uw toepassing alleen gebruik van TLS 1.2, tenzij dit absoluut noodzakelijk is omdat deze functies waarmee u kunt automatisch detecteren en te profiteren van de nieuwere veiliger protocollen zodra deze van beveiliging op het platform kunt verbreken beschikbaar, zoals TLS 1.3. Het is raadzaam om het uitvoeren van een grondige controle van de code van uw toepassing om te controleren op hardcoderen van specifieke versies van TLS/SSL.

### <a name="platformlanguage-specific-guidance"></a>Servicespecifieke richtlijnen voor platform/taal

|Platform/taal | Ondersteuning | Meer informatie |
| --- | --- | --- |
| Azure App Services  | Ondersteund, zijn configuratie vereist. | Ondersteuning is aangekondigd in April 2018. Lees de aankondiging voor [configuratiedetails](https://blogs.msdn.microsoft.com/appserviceteam/2018/04/17/app-service-and-functions-hosted-apps-can-now-update-tls-versions/).  |
| Azure-functie-Apps | Ondersteund, zijn configuratie vereist. | Ondersteuning is aangekondigd in April 2018. Lees de aankondiging voor [configuratiedetails](https://blogs.msdn.microsoft.com/appserviceteam/2018/04/17/app-service-and-functions-hosted-apps-can-now-update-tls-versions/). |
|.NET | Ondersteund, varieert de configuratie door versie. | Raadpleeg voor gedetailleerde configuratie-informatie voor .NET 4.7 en eerdere versies [deze instructies](https://docs.microsoft.com/dotnet/framework/network-programming/tls#support-for-tls-12).  |
|Status Monitor | Ondersteunde, configuratie is vereist | Afhankelijk van statusmonitor [configuratie van het besturingssysteem](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) + [configuratie .NET](https://docs.microsoft.com/dotnet/framework/network-programming/tls#support-for-tls-12) voor ondersteuning van TLS 1.2.
|Node.js |  Ondersteund in v10.5.0, zijn configuratie vereist. | Gebruik de [officiële Node.js TLS/SSL-documentatie](https://nodejs.org/api/tls.html) voor een specifieke configuratie van de toepassing. |
|Java | Ondersteund, JDK ondersteuning voor TLS 1.2 is toegevoegd in [JDK 6 update 121](http://www.oracle.com/technetwork/java/javase/overview-156328.html#R160_121) en [JDK 7](http://www.oracle.com/technetwork/java/javase/7u131-relnotes-3338543.html). | JDK 8 gebruikt [TLS 1.2 standaard](https://blogs.oracle.com/java-platform-group/jdk-8-will-use-tls-12-as-default).  |
|Linux | Linux-distributies meestal afhankelijk zijn van [OpenSSL](https://www.openssl.org) voor ondersteuning van TLS 1.2.  | Controleer de [OpenSSL Changelog](https://www.openssl.org/news/changelog.html) om te bevestigen van uw versie van OpenSSL wordt ondersteund.|
| Windows 8.0-10 | Ondersteund en standaard ingeschakeld. | Om te bevestigen dat u nog steeds gebruikt de [standaardinstellingen](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings).  |
| WindowsServer 2012-2016 | Ondersteund en standaard ingeschakeld. | Om te bevestigen dat u nog steeds gebruikt de [standaardinstellingen](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) |
| Windows 7 SP1 en Windows Server 2008 R2 SP1 | Ondersteund, maar niet standaard ingeschakeld. | Zie de [registerinstellingen voor Transport Layer Security (TLS)](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) pagina voor meer informatie over het inschakelen.  |
| Windows Server 2008 SP2 | Ondersteuning voor TLS 1.2 is een update vereist. | Zie [Update voor het toevoegen van ondersteuning voor TLS 1.2](https://support.microsoft.com/help/4019276/update-to-add-support-for-tls-1-1-and-tls-1-2-in-windows-server-2008-s) in Windows Server 2008 SP2. |
|Windows Vista | Niet ondersteund. | N/A

### <a name="check-what-version-of-openssl-your-linux-distribution-is-running"></a>Controleren welke versie van OpenSSL uw Linux-distributie wordt uitgevoerd.

Als u wilt controleren welke versie van OpenSSL die u hebt geïnstalleerd, open de terminal en voer:

```terminal
openssl version -a
```

### <a name="run-a-test-tls-12-transaction-on-linux"></a>Een test TLS 1.2-transactie worden uitgevoerd op Linux

Voor het uitvoeren van een eenvoudige inleidende test om te zien als uw Linux-systeem via TLS 1.2 communiceren kan. Open de terminal en voer:

```terminal
openssl s_client -connect bing.com:443 -tls1_2
```

## <a name="personal-data-stored-in-application-insights"></a>Persoonlijke gegevens die zijn opgeslagen in Application Insights

Onze [Application Insights persoonsgegevens artikel](../log-analytics/log-analytics-personal-data-mgmt.md) wordt dit probleem uitgebreid beschreven.

#### <a name="can-my-users-turn-off-application-insights"></a>Kunnen mijn gebruikers uitschakelen Application Insights?
Niet rechtstreeks. Geen bieden we een switch die uw gebruikers werken kunnen aan het uitschakelen van Application Insights.

U kunt echter een dergelijke functie implementeren in uw toepassing. Alle SDK's bevatten een API-instelling uitgeschakeld verzamelen van telemetriegegevens wordt. 

## <a name="data-sent-by-application-insights"></a>Gegevens die worden verzonden door Application Insights
De SDK's variëren tussen platforms en er zijn verschillende onderdelen die u kunt installeren. (Raadpleeg [Application Insights - overzicht][start].) Elk onderdeel verzendt verschillende gegevens.

#### <a name="classes-of-data-sent-in-different-scenarios"></a>Soorten gegevens die in verschillende scenario's worden verzonden
| De actie | Gegevensklassen die worden verzameld (Zie de volgende tabel) |
| --- | --- |
| [Application Insights-SDK toevoegen aan een .NET-web-project][greenbrown] |ServerContext<br/>Afgeleid<br/>Prestatiemeteritems<br/>Aanvragen<br/>**Uitzonderingen**<br/>Sessie<br/>gebruikers |
| [Installeer statusmonitor op IIS][redfield] |Afhankelijkheden<br/>ServerContext<br/>Afgeleid<br/>Prestatiemeteritems |
| [Application Insights SDK toevoegen aan een Java-web-app][java] |ServerContext<br/>Afgeleid<br/>Aanvraag<br/>Sessie<br/>gebruikers |
| [JavaScript SDK toevoegen aan webpagina][client] |ClientContext <br/>Afgeleid<br/>Pagina<br/>ClientPerf<br/>Ajax |
| [Standaard-eigenschappen definiëren][apiproperties] |**Eigenschappen van** op alle standaardentiteiten en aangepaste gebeurtenissen |
| [TrackMetric aanroepen][api] |Numerieke waarden<br/>**Eigenschappen** |
| [Aanroep bijhouden *][api] |De naam van gebeurtenis<br/>**Eigenschappen** |
| [TrackException aanroepen][api] |**Uitzonderingen**<br/>Stackdump<br/>**Eigenschappen** |
| SDK kan geen gegevens verzamelen. Bijvoorbeeld: <br/> -geen toegang tot de prestatiemeteritems<br/> -uitzondering in telemetrische initializer |Diagnostische gegevens van SDK |

Voor [SDK's voor andere platforms][platforms], Zie de documenten.

#### <a name="the-classes-of-collected-data"></a>De klassen van de verzamelde gegevens
| Klasse van de verzamelde gegevens | (Geen uitputtende lijst) bevat |
| --- | --- |
| **Eigenschappen** |**Alle gegevens - bepaald door uw code** |
| DeviceContext |-ID, IP-, lokale, Apparaatmodel, netwerk, netwerktype, OEM-naam, schermresolutie Rolinstantie, rolnaam, apparaattype |
| ClientContext |Besturingssysteem, land, taal, netwerk, venster resolutie |
| Sessie |Sessie-id |
| ServerContext |Naam van de computer, landinstelling, besturingssysteem, apparaat, gebruikerssessie, gebruikerscontext, bewerking |
| Afgeleid |geo-locatie van IP-adres, timestamp, besturingssysteem, browser |
| Metrische gegevens |Naam van de meetwaarde en de waarde |
| Gebeurtenissen |Gebeurtenisnaam en waarde |
| PageViews |URL- en -naam of de schermnaam van het |
| Client voor prestaties |URL-/ pagina-naam, de laadtijd van de browser |
| Ajax |HTTP-aanroepen van de webpagina met server |
| Aanvragen |URL, duur, responscode |
| Afhankelijkheden |Type (SQL, HTTP,...), de verbindingsreeks of de URI, synchronisatie/asynchrone, duur, geslaagd, SQL-instructie (met Status Monitor) |
| **Uitzonderingen** |Type, **bericht**, stacks aanroepen, bestands- en regel number, thread-id van bron |
| Oorzaken van crashes |Proces-id, proces-id van bovenliggende, crashes thread-id; toepassingspatch voor de, -id, build;  uitzonderingstype, adres, reason; verborgen symbolen en wordt geregistreerd, binaire begin- en -adressen, binaire naam en pad, cpu-type |
| Tracering |**Bericht** en ernst |
| Prestatiemeteritems |Tijd van processor, geheugen, snelheid van aanvragen, aantal uitzonderingen, proceseigen bytes, i/o-snelheid, aanvraagduur, lengte van aanvraagwachtrij |
| Beschikbaarheid |Web test responscode, de duur van elke stap, de testnaam van de, timestamp, geslaagd, reactietijd, testlocatie |
| Diagnostische gegevens van SDK |Traceringsbericht of uitzondering |

U kunt [uitschakelen deel van de gegevens door ApplicationInsights.config te bewerken][config]

## <a name="credits"></a>Credits
Dit product bevat GeoLite2 gegevens die zijn gemaakt door MaxMind, beschikbaar is via [ http://www.maxmind.com ](http://www.maxmind.com).



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

