---
title: Besturingssysteem-functionaliteit op Azure App Service
description: Meer informatie over de OS-functionaliteit beschikbaar voor web-apps, back-ends voor mobiele Apps en API apps in Azure App Service
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: 39d5514f-0139-453a-b52e-4a1c06d8d914
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: cephalin
ms.openlocfilehash: a5f022eca8f901388c9cf003f3320db1b9c49e6a
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="operating-system-functionality-on-azure-app-service"></a>Besturingssysteem-functionaliteit op Azure App Service
In dit artikel wordt de algemene basislijn besturingssysteem functionaliteit beschreven die beschikbaar is voor alle apps die worden uitgevoerd op [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Deze functionaliteit omvat bestand, netwerk, en toegang tot het register en logboeken met diagnostische gegevens en gebeurtenissen. 

<a id="tiers"></a>

## <a name="app-service-plan-tiers"></a>App Service plan lagen
App Service kan apps van de klant in een multitenant-hostomgeving uitgevoerd. Apps die zijn geïmplementeerd de **vrije** en **gedeelde** lagen uitvoeren in werkprocessen op gedeelde virtuele machines tijdens de implementatie van apps in de **standaard** en **Premium** lagen worden uitgevoerd op speciale specifiek voor de apps die zijn gekoppeld aan één klant horen.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Omdat App Service biedt ondersteuning voor een naadloze ervaring voor vergroten/verkleinen tussen verschillende lagen, wordt de configuratie van de beveiliging afgedwongen voor App Service-apps blijft hetzelfde. Dit zorgt ervoor dat apps niet plotseling zich anders gedragen, onverwachte manieren mislukt bij het App Service-abonnement van één laag naar de andere overschakelt.

<a id="developmentframeworks"></a>

## <a name="development-frameworks"></a>Ontwikkelingsframeworks
App Service-Prijscategorieën beheren het bedrag van rekenresources (CPU, schijfruimte, geheugen en netwerk uitgaande) beschikbaar om apps te. De breedte van de framework-functionaliteit is beschikbaar voor apps blijft echter hetzelfde, ongeacht de vergroten/verkleinen lagen.

App Service ondersteunt een groot aantal ontwikkelingsframeworks, met inbegrip van ASP.NET, klassiek ASP, node.js, PHP en Python - die allemaal als-extensies in IIS uitvoeren. Om te vereenvoudigen en normaliseren beveiligingsconfiguratie, App Service-apps gewoonlijk de verschillende ontwikkelingsframeworks uitgevoerd met de standaardinstellingen. Een aanpak voor het configureren van apps kan zijn voor het aanpassen van het oppervlak van de API en de functionaliteit voor elke afzonderlijke ontwikkelframework. App Service duurt in plaats daarvan een meer algemene benadering doordat een gemeenschappelijke basislijn van de functionaliteit van besturingssysteem ongeacht ontwikkelframework voor een app.

De volgende secties geven een overzicht van de algemene typen besturingssysteemfunctionaliteit beschikbaar zijn voor App Service-apps.

<a id="FileAccess"></a>

## <a name="file-access"></a>Toegang tot het bestand
Er bestaan verschillende schijven in App Service, waaronder lokale stations- en netwerkstations.

<a id="LocalDrives"></a>

### <a name="local-drives"></a>Lokale stations
De kern is App Service een service die wordt uitgevoerd op de infrastructuur van Azure PaaS (platform als een service). Als gevolg hiervan zijn de lokale stations die zijn 'gekoppeld' aan een virtuele machine dezelfde stationstypen beschikbaar zijn voor alle worker-rol in Azure wordt uitgevoerd. Dit omvat een systeemstation (het station D:\), een toepassing station waarop Azure cspkg pakketbestanden gebruikt uitsluitend door App Service (en niet toegankelijk is voor klanten) en een 'gebruiker' station (het station C:\), waarvan de grootte afhankelijk van de grootte van de virtuele machine varieert.

<a id="NetworkDrives"></a>

### <a name="network-drives-aka-unc-shares"></a>Netwerkstations (aka UNC shares)
Een van de unieke aspecten van App-Service waarmee app-implementatie en het onderhoud duidelijk is dat alle gebruikersinhoud is opgeslagen op een reeks UNC-shares. Dit model wordt zeer mooi toegewezen aan het algemene patroon van inhoudopslag die wordt gebruikt door lokale webhosting-omgevingen met meerdere servers met gelijke taakverdeling. 

In App Service, er zijn van een UNC-shares in elke Datacenter wordt gemaakt. Een percentage van de gebruikersinhoud voor alle klanten in elk datacenter is toegewezen aan elke UNC-share. Bovendien delen alle van het bestand dat de inhoud van het abonnement van één klant altijd op de dezelfde UNC geplaatst wordt. 

Houd er rekening mee dat als gevolg van hoe cloud services-werk, wordt de specifieke virtuele machine die verantwoordelijk is voor het hosten van een UNC-share gewijzigd gedurende een bepaalde periode. Het kan worden gegarandeerd dat UNC-shares gekoppeld door andere virtuele machines als omhoog en omlaag worden gebracht in de normale loop van de bewerkingen van de cloud. Apps moet daarom nooit vastgelegde veronderstellingen dat de gegevens in een UNC-pad van de machine stabiel gedurende een bepaalde periode blijft. In plaats daarvan ze moeten gebruiken de handige *faux* absoluut pad **D:\home\site** die voorziet in App Service. Deze faux absoluut pad biedt een draagbare, app en gebruiker-networkdirect-methode voor het verwijzen naar de eigen app. Met behulp van **D:\home\site**, een bestanden kan overdragen gedeelde app app zonder een nieuwe absoluut pad configureren voor elke overdracht.

<a id="TypesOfFileAccess"></a>

### <a name="types-of-file-access-granted-to-an-app"></a>Typen bestandstoegang te krijgen tot een app
Elke klant-abonnement heeft een gereserveerde mapstructuur op een specifieke UNC-share binnen een datacenter. Een klant kan meerdere apps die zijn gemaakt binnen een specifieke datacentrum, zodat alle directory's die horen bij een abonnement van één klant zijn gemaakt op de dezelfde UNC delen hebben. De share kan mappen zoals die voor inhoud, fout en diagnostische logboeken en eerdere versies van de app die is gemaakt door het besturingselement bevatten. Zoals verwacht, is een klant app mappen zijn beschikbaar voor lees- en schrijftoegang tijdens runtime door de app-toepassingscode.

App Service gereserveerd op de lokale stations die zijn gekoppeld aan de virtuele machine die een app uitvoert, een deel van de ruimte op het station C:\ voor app-specifiek tijdelijke lokale opslag. Hoewel een app volledige lezen/schrijven toegang tot de eigen lokale tijdelijke opslag heeft, is die opslag echt is niet bedoeld om rechtstreeks worden gebruikt door de toepassingscode. In plaats daarvan is de bedoeling om het tijdelijke bestandsopslag bieden voor IIS en web-App-frameworks. App Service wordt ook beperkt de hoeveelheid tijdelijke lokale opslag beschikbaar voor elke app om te voorkomen dat afzonderlijke apps buitensporige hoeveelheden lokale bestandsopslag verbruikt.

Twee voorbeelden van hoe App Service gebruikt voor lokale tijdelijke zijn de map voor tijdelijke bestanden voor ASP.NET en gecomprimeerde bestanden van de map voor IIS. De ASP.NET-compilatie-systeem wordt de map 'Temporary ASP.NET Files' als een tijdelijke compilatie cachelocatie. IIS maakt gebruik van de ' IIS gecomprimeerde ' map met tijdelijke bestanden voor het opslaan van gecomprimeerde antwoorduitvoer. Beide typen van bestand gebruik (evenals anderen) worden toegewezen in App Service aan de lokale tijdelijke per-app. Deze opnieuw toe te wijzen, zorgt u ervoor dat de functionaliteit blijft zoals verwacht.

Elke app in App Service wordt uitgevoerd als de identiteit van een willekeurig uniek beperkte bevoegdheden werkproces aangeroepen 'id van de toepassingsgroep', verder hier beschreven: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities](http://www.iis.net/learn/manage/configuring-security/application-pool-identities). Toepassingscode gebruikt deze identiteit voor basic alleen-lezen toegang tot het besturingssysteemstation (het station D:\). Dit betekent dat toepassingscode kan algemene mapstructuren weergeven en lezen algemene bestanden op het systeemstation. Hoewel dit een enigszins algemeen niveau van toegang, hetzelfde mappen en bestanden zijn toegankelijk lijken wanneer u inrichten wordt een werkrol in een Azure gehoste service en de inhoud van het station lezen. 

<a name="multipleinstances"></a>

### <a name="file-access-across-multiple-instances"></a>Toegang tot het bestand voor meerdere exemplaren
De basismap bevat een app-inhoud en toepassingscode kan schrijven. Als een app wordt uitgevoerd op meerdere exemplaren, wordt de basismap gedeeld door alle exemplaren, zodat alle exemplaren dezelfde directory zien. Ja, bijvoorbeeld als een app geüploade bestanden naar de basismap opslaat, die bestanden zijn onmiddellijk beschikbaar in alle exemplaren. 

<a id="NetworkAccess"></a>

## <a name="network-access"></a>Netwerktoegang
Toepassingscode kan TCP/IP gebruiken en op basis van UDP-protocollen maken uitgaande netwerkverbindingen naar Internet toegankelijk eindpunten die externe services. Apps kunnen deze protocollen dezelfde verbinding maken met services in Azure & #151, bijvoorbeeld door het opstellen van HTTPS-verbindingen met SQL-Database gebruiken.

Er is ook een beperkte capaciteit voor apps voor een lokale loopback-verbinding tot stand brengen en een app luisteren op die lokale loopback-socket hebben. Deze functie bestaat voornamelijk om apps te schakelen die op de lokale loopback-sockets als onderdeel van hun functionaliteit luisteren. Houd er rekening mee dat elke app een 'persoonlijke' loopback-verbinding ziet. App "A" kan niet luisteren naar een lokale loopback-socket tot stand gebracht door app "B".

Named pipes worden ook ondersteund als een mechanisme voor communicatie tussen processen (IPC) tussen verschillende processen die gezamenlijk een app worden uitgevoerd. Bijvoorbeeld: de IIS FastCGI-module is gebaseerd op named pipes voor het coördineren van de afzonderlijke processen die PHP-pagina's worden uitgevoerd.

<a id="Code"></a>

## <a name="code-execution-processes-and-memory"></a>De uitvoering van code, processen en geheugen
Zoals eerder opgemerkt, apps worden uitgevoerd binnen werkprocessen met beperkte bevoegdheden met een willekeurige toepassingsgroepidentiteit. Toepassingscode heeft toegang tot de geheugenruimte die is gekoppeld aan het werkproces, evenals de onderliggende processen die mogelijk worden geïnitieerd door de CGI-processen of andere toepassingen. Maar één app heeft geen toegang tot het geheugen of de gegevens van een andere app zelfs als deze zich op dezelfde virtuele machine.

Apps kunnen scripts of pagina's die zijn geschreven met ondersteunde web ontwikkelingsframeworks worden uitgevoerd. App Service configureren niet alle instellingen voor web-framework voor meer beperkte modi. Bijvoorbeeld: ASP.NET apps die worden uitgevoerd op App Service uitvoeren in 'volledige' vertrouwen in plaats van een meer beperkte vertrouwensrelatie-modus. Web-frameworks, met inbegrip van zowel klassieke ASP- en ASP.NET, kunnen aanroepen COM-onderdelen in-process (maar niet out-of-process COM-onderdelen) zoals ADO (ActiveX Data Objects), die standaard op de Windows-besturingssysteem zijn geregistreerd.

Apps kunnen starten en willekeurige code uitvoeren. Het is toegestaan voor een app handelingen zoals vermenigvuldigen een opdrachtshell of een PowerShell-script uitvoeren. Hoewel arbitraire code en processen kunnen worden geïnitieerd uit een app, zijn uitvoerbare programma's en scripts echter nog steeds beperkt tot de machtigingen te krijgen tot de bovenliggende groep van toepassingen. Bijvoorbeeld kunt een app een uitvoerbaar bestand dat u een uitgaande HTTP-aanroep maakt starten maar dat hetzelfde uitvoerbare bestand kan niet proberen te Hef de binding van het IP-adres van een virtuele machine van de NIC. Een oproep uitgaande netwerk met beperkte bevoegdheden code is toegestaan, maar probeert opnieuw configureren van netwerkinstellingen op een virtuele machine vereist beheerdersbevoegdheden.

<a id="Diagnostics"></a>

## <a name="diagnostics-logs-and-events"></a>Logboeken met diagnostische gegevens en gebeurtenissen
Informatie in het foutenlogboek is een andere set gegevens die een aantal apps proberen te krijgen. De soorten logboekgegevens die beschikbaar zijn voor de code die wordt uitgevoerd in App Service bevat diagnostische en meld u informatie die wordt gegenereerd door een app die ook eenvoudig toegankelijk is voor de app. 

W3C-HTTP-logboeken die worden gegenereerd door een actieve app zijn bijvoorbeeld beschikbaar zijn op een logboekmap in de netwerklocatie van de share gemaakt voor de app of in de blob-opslag beschikbaar als een klant W3C-logboekregistratie naar de opslag is ingesteld. De laatste optie kunt grote hoeveelheden logboeken worden verzameld zonder het risico van de file storage limieten die zijn gekoppeld aan een netwerkshare.

In een vergelijkbare vein realtime diagnostische gegevens van .NET-toepassingen kan ook worden vastgelegd met behulp van de .NET-tracering en diagnostische gegevens infrastructuur, met opties voor het schrijven van de trace-informatie naar de netwerkshare van de app, of u kunt ook naar een blob-opslaglocatie.

Gebieden van diagnostische logboekregistratie en tracering die niet beschikbaar voor apps zijn Windows ETW-gebeurtenissen en algemene Windows-gebeurtenislogboeken (bijvoorbeeld systeem, toepassing en beveiliging gebeurtenislogboeken). Omdat informatie voor ETW-tracering worden bekeken alle computers (met de juiste ACL's) mogelijk zijn kan, zijn de lees- en schrijftoegang tot ETW-gebeurtenissen worden geblokkeerd. Ontwikkelaars wellicht opgevallen dat API-aanroepen te lezen en schrijven ETW-gebeurtenissen en algemene Windows-gebeurtenislogboeken weergegeven om te werken, maar dat is omdat App Service is 'faking' aanroepen van de zodat deze worden weergegeven om u te laten slagen. In werkelijkheid heeft de toepassingscode geen toegang tot de gegevens van deze gebeurtenissen.

<a id="RegistryAccess"></a>

## <a name="registry-access"></a>Toegang tot het register
Apps alleen-lezen toegang hebben tot veel (hoewel niet alle) van het register van de virtuele machine die ze worden uitgevoerd. Dit betekent de registersleutels die alleen-lezen toegang tot de lokale groep gebruikers geven toegankelijk zijn voor apps uit te voeren in de praktijk. Een gedeelte van het register die momenteel niet wordt ondersteund voor lees- of schrijftoegang is de HKEY\_huidige\_gebruikerscomponent.

Schrijftoegang tot het register wordt geblokkeerd, inclusief toegang tot de registersleutels per gebruiker. Vanuit het perspectief van de app, schrijftoegang tot het register moet nooit worden aangehaald in de Azure-omgeving omdat apps kunnen (en) ophalen gemigreerd in verschillende virtuele machines. De alleen permanente beschrijfbare opslag die kan worden afhankelijk is van door een app is de per-app-inhoud mapstructuur opgeslagen op de App Service-UNC-shares. 

## <a name="more-information"></a>Meer informatie

[Azure-Web-App sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) -de meest actuele informatie over de uitvoeringsomgeving van App Service. Deze pagina wordt beheerd rechtstreeks door het ontwikkelingsteam App Service.

> [!NOTE]
> Als u aan de slag wilt met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](https://azure.microsoft.com/try/app-service/). Hier kunt u direct een tijdelijke web-app maken in App Service. U hebt geen creditcard nodig en u gaat geen verplichtingen aan.
> 
> 


