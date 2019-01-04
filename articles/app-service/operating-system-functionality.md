---
title: De functionaliteit van het besturingssysteem in App Service - Azure
description: Meer informatie over de OS-functionaliteit beschikbaar voor web-apps, back-ends voor mobiele Apps en API apps in Azure App Service
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: 39d5514f-0139-453a-b52e-4a1c06d8d914
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: ad27a7eaf88ae57f730609e2b0f43a2f5ea182a1
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2018
ms.locfileid: "53653506"
---
# <a name="operating-system-functionality-on-azure-app-service"></a>De functionaliteit van het besturingssysteem op Azure App Service
In dit artikel beschrijft de functionaliteit van algemene basislijn besturingssysteem dat beschikbaar is voor alle Windows-apps die worden uitgevoerd op [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). Deze functionaliteit bevat de bestands-, netwerk, en toegang tot het register en logboeken met diagnostische gegevens en gebeurtenissen. 

> [!NOTE] 
> [Linux-apps](containers/app-service-linux-intro.md) in App Service uitvoeren in hun eigen containers. Geen toegang tot het hostbesturingssysteem is toegestaan, hebt u toegang tot de hoofdmap voor de container. Op dezelfde manier voor [apps die worden uitgevoerd in Windows-containers](app-service-web-get-started-windows-container.md), hebt u beheerderstoegang tot de container, maar geen toegang tot het hostbesturingssysteem. 
>

<a id="tiers"></a>

## <a name="app-service-plan-tiers"></a>App Service-planlagen
App Service wordt uitgevoerd van klanten-apps in een omgeving met meerdere tenants host. Apps die zijn geïmplementeerd de **gratis** en **gedeelde** lagen in werkprocessen op gedeelde virtuele machines uitvoeren, terwijl apps die zijn geïmplementeerd de **Standard** en **Premium**  lagen worden uitgevoerd op virtuele machines die specifiek voor de apps die zijn gekoppeld aan één klant toegewezen.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Omdat App Service biedt ondersteuning voor een naadloze ervaring voor vergroten/verkleinen tussen verschillende lagen, wordt de configuratie van de beveiliging afgedwongen voor App Service-apps blijft hetzelfde. Dit zorgt ervoor dat apps niet plotseling zich anders gedragen, onverwacht, mislukken wanneer App Service-plan van één laag naar een andere overschakelt.

<a id="developmentframeworks"></a>

## <a name="development-frameworks"></a>Ontwikkelingsframeworks
App Service-Prijscategorieën bepaalt de hoeveelheid rekenresources (CPU, disk-opslag, geheugen en uitgaand netwerkverkeer) beschikbaar voor apps. De breedte van de framework-functionaliteit is beschikbaar voor apps blijft echter hetzelfde, ongeacht de vergroten/verkleinen lagen.

App Service ondersteuning biedt voor tal van ontwikkelingsframeworks voor, met inbegrip van ASP.NET, klassieke ASP, node.js, PHP en Python - die allemaal als-extensies in IIS uitvoeren. Om te vereenvoudigen en beveiligingsconfiguratie normaliseren, uitvoeren App Service-apps doorgaans de verschillende ontwikkelingsframeworks met de standaardinstellingen. Eén mogelijke benadering voor het configureren van apps kan zijn om aan te passen van de API-gebied en functionaliteit voor elke afzonderlijke webontwikkelingsframework. App Service wordt in plaats daarvan een algemenere benadering door in te schakelen van een algemene basislijn van de functionaliteit van het besturingssysteem, ongeacht het raamwerk voor ontwikkelaars van een app.

De volgende secties geven een overzicht van de algemene typen besturingssysteemfunctionaliteit die beschikbaar is in App Service-apps.

<a id="FileAccess"></a>

## <a name="file-access"></a>Toegang tot het bestand
Er bestaan verschillende schijven in App Service, met inbegrip van lokale stations en netwerkstations.

<a id="LocalDrives"></a>

### <a name="local-drives"></a>Lokale stations
App Service is in de kern, een service die wordt uitgevoerd boven op de infrastructuur van Azure PaaS (platform als een service). Als gevolg hiervan zijn de lokale stations die zijn 'gekoppeld' aan een virtuele machine dezelfde stationstypen beschikbaar zijn voor alle worker-rol die worden uitgevoerd in Azure. Dit omvat:

- Een systeemstation (het station D:\)
- Een toepassing station waarop Azure cspkg pakketbestanden gebruikt uitsluitend door App Service (en niet toegankelijk is voor klanten)
- De schijf van een 'gebruiker' (het station C:\), waarvan de grootte, afhankelijk van de grootte van de virtuele machine varieert. 

Het is belangrijk voor het bewaken van uw schijfgebruik wanneer uw toepassing groeit. Als de schijf is bereikt, kan dit negatieve gevolgen heeft voor uw toepassing hebben. Bijvoorbeeld: 

- De app mogelijk een foutbericht dat aangeeft niet genoeg ruimte op de schijf genereert.
- Mogelijk ziet u schijffouten optreden bij het bladeren naar de Kudu-console.
- Implementatie van VSTS of Visual Studio kan mislukken met `ERROR_NOT_ENOUGH_DISK_SPACE: Web deployment task failed. (Web Deploy detected insufficient space on disk)`.
- Trage prestaties kan worden beïnvloed door uw app.

<a id="NetworkDrives"></a>

### <a name="network-drives-aka-unc-shares"></a>Netwerkstations (ook wel UNC shares)
Een van de unieke aspecten van App Service voor app-implementatie en onderhoud duidelijk is dat alle gebruikersinhoud is opgeslagen op een set met UNC-shares. Dit model wordt toegewezen en aan het algemene patroon van inhoud opslag die wordt gebruikt door on-premises-omgevingen met meerdere servers met load balancing voor webhosting. 

Er is een aantal UNC-shares in elke Datacenter hebt gemaakt in App Service. Een percentage van de gebruikersinhoud voor alle klanten in elk datacenter is toegewezen aan elke UNC-share. Bovendien delen alle van de inhoud van het abonnement van één klant altijd wordt geplaatst op de dezelfde UNC bestand. 

Vanwege de manier waarop Azure werken, wordt de specifieke virtuele machine die verantwoordelijk is voor het hosten van een UNC-share verloop van tijd veranderen. Het kan worden gegarandeerd dat UNC-shares zal worden gekoppeld door verschillende virtuele machines als ze omhoog en omlaag de normale loop van de Azure-bewerkingen worden gebracht. Om deze reden moeten apps nooit veronderstellingen vastgelegd dat de gegevens in een UNC-pad van de machine stabiel na verloop van tijd blijft. Gebruik in plaats daarvan, ze moeten de handige *faux* absoluut pad **D:\home\site** die App Service biedt. Deze faux absoluut pad biedt een methode draagbare, app-en-gebruiker-agnostische voor verwijzen naar eigen app. Met behulp van **D:\home\site**, een bestanden kan overdragen gedeelde app app zonder een nieuwe absoluut pad voor elke overdracht configureren.

<a id="TypesOfFileAccess"></a>

### <a name="types-of-file-access-granted-to-an-app"></a>Toegang tot het bestand verleend aan een app-typen
Elke klant-abonnement heeft een gereserveerde mapstructuur op een specifieke UNC-share binnen een datacenter. Een klant kan meerdere apps die zijn gemaakt binnen een specifieke Datacenter, zodat alle van de mappen die behoren tot een abonnement één klant zijn gemaakt op de dezelfde UNC delen hebben. De share kan include-mappen, zoals die voor inhoud, fout en diagnostische logboeken en eerdere versies van de app gemaakt door het besturingselement. Zoals verwacht, is van een klant app mappen zijn beschikbaar voor lezen en schrijven tijdens runtime door de toepassingscode van de app.

Op de lokale stations die zijn gekoppeld aan de virtuele machine die een app uitvoert, behoudt App Service een segment van de ruimte op het station C:\ voor de tijdelijke lokale opslag van de app-specifiek. Hoewel een app volledige lees-/ schrijftoegang tot een eigen lokale tijdelijke opslag heeft, is dat de opslagruimte echt is niet bedoeld voor gebruik rechtstreeks door de toepassingscode. In plaats daarvan is de bedoeling voor opslag van tijdelijke bestanden voor IIS en web App-frameworks. Het bedrag van de tijdelijke lokale opslag beschikbaar voor elke app om te voorkomen dat afzonderlijke apps verbruikt buitensporige hoeveelheden lokale bestandsopslag ook limieten voor App Service.

Twee voorbeelden van hoe App Service maakt gebruik van tijdelijke lokale opslag zijn de map voor de tijdelijke ASP.NET-bestanden en de map voor IIS gecomprimeerde bestanden. De ASP.NET-compilatie-systeem gebruikt de map 'Tijdelijke ASP.NET Files' als een tijdelijke compilatie cachelocatie. IIS maakt gebruik van de ' IIS gecomprimeerde "map met tijdelijke bestanden voor het opslaan van de uitvoer van de gecomprimeerde antwoord. Beide van deze typen bestand gebruik (evenals andere) opnieuw worden toegewezen in App Service naar de tijdelijke lokale opslag per app. Dit opnieuw toe te wijzen, zorgt u ervoor dat de functionaliteit blijft zoals verwacht.

Elke app in App Service wordt uitgevoerd als de identiteit van een willekeurige unieke met beperkte bevoegdheden werkproces met de naam 'id van de toepassingsgroep', verder die hier worden beschreven: [ https://www.iis.net/learn/manage/configuring-security/application-pool-identities ](https://www.iis.net/learn/manage/configuring-security/application-pool-identities). Toepassingscode gebruikt deze identiteit voor basic alleen-lezen toegang tot het besturingssysteemstation (het station D:\). Dit betekent toepassingscode kan lijst met algemene mapstructuren en algemene bestanden lezen op het systeemstation. Hoewel dit lijkt te zijn van een enigszins algemeen niveau van toegang, de dezelfde mappen en bestanden zijn toegankelijk wanneer u inricht wordt een werkrol in Azure gehoste service en de station-inhoud lezen. 

<a name="multipleinstances"></a>

### <a name="file-access-across-multiple-instances"></a>Toegang tot het bestand over meerdere instanties
De basismap bevat de inhoud van een app en toepassingscode daarnaartoe kan schrijven. Als een app wordt uitgevoerd op meerdere exemplaren, wordt de basismap wordt gedeeld tussen alle exemplaren zodat alle instanties dezelfde map zien. Dus, bijvoorbeeld, als een app geüploade bestanden naar de basismap opslaat, die bestanden zijn onmiddellijk beschikbaar voor alle exemplaren. 

<a id="NetworkAccess"></a>

## <a name="network-access"></a>Netwerktoegang
Toepassingscode kunt TCP/IP- en UDP gebaseerde protocollen gebruiken om uitgaande netwerkverbindingen op Internet toegankelijke eindpunten die beschikbaar maken van externe services te maken. Apps deze dezelfde protocollen verbinding maken met services in Azure kunnen gebruiken&#151;bijvoorbeeld door het HTTPS-verbindingen met SQL Database tot stand brengen.

Er is ook een beperkte capaciteit voor apps in een lokale loopback-verbinding tot stand brengen en een app op die lokale loopback-socket luisteren. Deze functie bestaat voornamelijk om in te schakelen van apps die op de lokale loopback-sockets als onderdeel van hun functionaliteit luisteren. Elke app ziet een 'persoonlijke' loopback-verbinding. App 'A' kan niet luisteren naar een lokale loopback-socket tot stand gebracht door app "B".

Named pipes worden ook ondersteund als een mechanisme voor communicatie tussen processen (IPC) tussen verschillende processen die gezamenlijk een app uitvoeren. De IIS FastCGI-module is bijvoorbeeld afhankelijk van named pipes voor de coördinatie van de afzonderlijke processen die worden uitgevoerd van PHP-pagina's.

<a id="Code"></a>

## <a name="code-execution-processes-and-memory"></a>Uitvoering van code, processen en geheugen
Als u eerder hebt genoteerd, apps worden uitgevoerd binnen de werkprocessen met beperkte bevoegdheden met behulp van een willekeurige identiteit van groep. Toepassingscode heeft toegang tot de geheugenruimte die is gekoppeld aan het werkproces, evenals eventuele onderliggende processen die kunnen worden geïnitieerd door de CGI-processen of andere toepassingen. Maar één app heeft geen toegang tot het geheugen of de gegevens van een andere app, zelfs als deze zich op dezelfde virtuele machine.

Apps kunnen scripts of pagina's die zijn geschreven met ondersteunde webontwikkelingsframeworks worden uitgevoerd. App Service configureren niet alle instellingen van het framework web naar meer beperkte modi. Bijvoorbeeld: de ASP.NET-apps in App Service in 'volledig' vertrouwen in plaats van een meer beperkte vertrouwensmodus uitgevoerd. Webframeworks, waaronder zowel klassieke ASP- en ASP.NET, kunnen worden aangeroepen COM-onderdelen in-process (maar niet out-of proces COM-onderdelen), zoals ADO (ActiveX Data Objects) die standaard op het Windows-besturingssysteem zijn geregistreerd.

Apps kunnen produceren en arbitraire code wordt uitgevoerd. Het is toegestaan voor een app voor handelingen zoals vermenigvuldigen een opdrachtshell of een PowerShell-script uit te voeren. Hoewel arbitraire code en processen kunnen worden gestart vanuit een app, zijn uitvoerbare programma's en scripts echter nog steeds beperkt tot de bevoegdheden verleend aan de bovenliggende groep van toepassingen. Een app kan bijvoorbeeld een uitvoerbaar bestand dat u een uitgaande HTTP-aanroep maakt, produceren, maar dat hetzelfde uitvoerbare binding ongedaan maken van het IP-adres van een virtuele machine van de NIC kan niet proberen Een oproep uitgaand netwerkverkeer is toegestaan in de code met beperkte bevoegdheden, maar er wordt geprobeerd om de netwerkinstellingen op een virtuele machine opnieuw te beheerdersbevoegdheden vereist.

<a id="Diagnostics"></a>

## <a name="diagnostics-logs-and-events"></a>Logboeken met diagnostische gegevens en gebeurtenissen
Logboekgegevens is een andere set gegevens die bepaalde apps proberen te krijgen. De soorten logboekgegevens die beschikbaar zijn voor code die wordt uitgevoerd in App Service bevat diagnostische en logboekgegevens die zijn gegenereerd door een app die ook eenvoudig toegankelijk is voor de app. 

W3C-HTTP-logboeken die worden gegenereerd door een actieve app zijn bijvoorbeeld beschikbaar op een logboekmap op de netwerklocatie van de share gemaakt voor de app, of ze zijn beschikbaar in blob-opslag als een klant van het W3C-logboekregistratie naar de opslag is ingesteld. De laatste optie kunt grote hoeveelheden logboeken worden verzameld zonder het risico van meer dan de maximale opslag van bestand dat is gekoppeld aan een netwerkshare.

In een vergelijkbaar vein realtime diagnostische gegevens vanuit .NET-apps kan ook worden geregistreerd met behulp van de .NET-tracering en diagnostische gegevens over infrastructuur, met opties voor het schrijven van de traceringsinformatie naar de netwerkshare van de app, of u kunt ook naar de locatie van een blob storage.

Gebieden van diagnostische logboekregistratie en tracering die niet beschikbaar voor apps zijn Windows ETW-gebeurtenissen en algemene Windows-gebeurtenislogboeken (bijvoorbeeld, systeem, toepassing en beveiliging gebeurtenislogboeken). Aangezien ETW trace-informatie bekeken-alle computers (met de juiste ACL's) mogelijk zijn kan, zijn de lees- en schrijftoegang tot ETW-gebeurtenissen worden geblokkeerd. Ontwikkelaars hebben gemerkt dat API-aanroepen te lezen en schrijven van ETW-gebeurtenissen en algemene Windows-gebeurtenislogboeken weergegeven om te werken, maar dat komt doordat het App Service is 'faking' aanroepen van de zodat deze worden weergegeven te voltooien. In werkelijkheid is de code van de toepassing geen toegang tot de gegevens van deze gebeurtenis.

<a id="RegistryAccess"></a>

## <a name="registry-access"></a>Toegang tot het register
Apps hebben alleen-lezen toegang tot veel (hoewel niet alle) van het register van de virtuele machine waarop ze worden uitgevoerd. Dit betekent de registersleutels die alleen-lezen toegang tot de lokale groep gebruikers zijn ook toegankelijk via apps uit te voeren in de praktijk. Een gedeelte van het register dat wordt momenteel niet ondersteund voor lees- of schrijftoegang is de HKEY\_huidige\_gebruiker hive.

Schrijftoegang tot het register wordt geblokkeerd, inclusief de toegang tot alle registersleutels per gebruiker. Vanuit het perspectief van de app, van toegang voor schrijven naar het register moet nooit worden gebruikt in de Azure-omgeving omdat apps kunnen (en) voor verschillende virtuele machines gemigreerd. De alleen permanente beschrijfbare opslag die kan worden afhankelijk is van door een app is de per-app-inhoud directory-structuur die zijn opgeslagen op de App Service-UNC-shares. 

## <a name="more-information"></a>Meer informatie

[Azure App Service-sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) -de meest actuele informatie over de uitvoeringsomgeving van App Service. Deze pagina wordt beheerd rechtstreeks door het ontwikkelingsteam van App Service.

