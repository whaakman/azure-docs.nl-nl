---
title: Overzicht van Azure App Service lokale Cache | Microsoft Docs
description: In dit artikel wordt beschreven hoe u het inschakelen, grootte en het opvragen van de status van de lokale Cache van Azure App Service-functie
services: app-service
documentationcenter: app-service
author: cephalin
manager: jpconnock
editor: ''
tags: optional
keywords: ''
ms.assetid: e34d405e-c5d4-46ad-9b26-2a1eda86ce80
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/04/2016
ms.author: cephalin
ms.openlocfilehash: 6729c87dcc9a85e2e3ccb6b4822213d38e2ba6f7
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666111"
---
# <a name="azure-app-service-local-cache-overview"></a>Overzicht van Azure App Service lokale Cache

> [!NOTE]
> Lokale cache wordt niet ondersteund in de functie-apps of App Service-apps in containers, zoals op [App Service on Linux](containers/app-service-linux-intro.md).


Azure-web-app-inhoud is opgeslagen in Azure Storage en omhoog in een duurzame manier als share met de inhoud is opgehaald. Dit ontwerp is bedoeld om te werken met verschillende apps en bevat de volgende kenmerken:  

* De inhoud wordt gedeeld tussen meerdere exemplaren van de virtuele machine (VM) van de web-app.
* De inhoud, duurzaam en kan worden gewijzigd door het uitvoeren van webtoepassingen.
* Logboekbestanden en diagnostische gegevensbestanden zijn beschikbaar onder de map met dezelfde gedeelde inhoud.
* De map met inhoud publiceren van nieuwe inhoud rechtstreeks worden bijgewerkt. U ziet onmiddellijk de dezelfde inhoud via de SCM-website en de actieve web-app (doorgaans sommige technologieën zoals ASP.NET een web-app opnieuw opstarten initiëren op sommige bestandswijzigingen om op te halen van de meest recente inhoud).

Veel web-apps gebruiken een of meer van deze functies, zijn sommige web-apps hoeft alleen maar een krachtige, alleen-lezen store voor inhoud die ze kunnen worden uitgevoerd vanaf met hoge beschikbaarheid. Deze apps kunnen profiteren van een VM-exemplaar van een specifieke lokale cache.

De functie van de lokale Cache van Azure App Service biedt een webweergave van de rol van uw inhoud. Deze inhoud is een cache voor write-maar-negeren van de inhoud van uw opslag die asynchroon on-site opstarten wordt gemaakt. Als de cache klaar is, wordt de site om te worden uitgevoerd op basis van de inhoud in cache ingeschakeld. WebApps die worden uitgevoerd op de lokale Cache hebben de volgende voordelen:

* Ze zijn immuun voor latentie die optreden wanneer ze toegang inhoud op Azure Storage tot.
* Ze zijn ongevoelig voor de geplande upgrades of niet-geplande storingen en eventuele andere storingen met Azure Storage die zich voordoen op servers die de inhoud delen.
* Ze hebben minder app wordt opnieuw opgestart vanwege wijzigingen voor storage-share.

## <a name="how-the-local-cache-changes-the-behavior-of-app-service"></a>Hoe de lokale cache verandert het gedrag van App Service
* _D:\home_ verwijst naar de lokale cache, die op de VM-exemplaar wordt gemaakt wanneer de app wordt gestart. _D:\Local_ om te verwijzen naar de tijdelijke opslag voor VM-specifieke blijft.
* De lokale cache bevat een momentopname van de _/siteconfiguratie_ en _/siteextensions_ mappen van de gedeelde inhoud store op _D:\home\site_ en _D:\home\ siteextensions_, respectievelijk. De bestanden zijn gekopieerd naar de lokale cache wanneer de app wordt gestart. De grootte van de twee mappen voor elke app is standaard beperkt tot 300 MB, maar u het kunt verhogen, kunt u maximaal 2 GB.
* De lokale cache is alleen-lezen. Een wijziging wordt echter verwijderd wanneer de app wordt verplaatst van virtuele machines of opnieuw wordt opgestart. Gebruik de lokale cache niet voor apps die essentiële gegevens opslaan in de store.
* _D:\home\LogFiles_ en _D:\home\Data_ logboekbestanden en app-gegevens bevatten. De twee submappen worden lokaal opgeslagen op de VM-exemplaar en periodiek worden gekopieerd naar de gedeelde inhoud store. Apps kunnen logboekbestanden en gegevens behouden door ze naar deze mappen te schrijven. Het kopiëren naar de gedeelde inhoud store is echter best-effort, zodat het mogelijk voor logboekbestanden en de gegevens verloren vanwege het vastlopen van een plotselinge van een VM-exemplaar.
* [Logboekstreaming](web-sites-enable-diagnostic-log.md#streamlogs) wordt beïnvloed door het best-effort-exemplaar. U kan zien tot een vertraging van één minuut in de gestreamde Logboeken.
* In de gedeelde inhoud store is een wijziging in de mapstructuur van de _logboekbestanden_ en _gegevens_ mappen voor apps die gebruikmaken van de lokale cache. Er zijn nu submappen in die het naamgevingspatroon van "de unieke id" + tijdstempel volgen. Elk van de submappen komt overeen met een VM-exemplaar waar de app wordt uitgevoerd of is uitgevoerd.
* Andere mappen in _D:\home_ blijven in de lokale cache en zijn niet gekopieerd naar de gedeelde inhoud store.
* App-implementatie via elke ondersteunde methode publiceert rechtstreeks naar de duurzame opslag voor gedeelde inhoud. Om te vernieuwen de _D:\home\site_ en _D:\home\siteextensions_ mappen in de lokale cache, de app moet opnieuw worden opgestart. Als u de levenscyclus van naadloze, Zie de informatie later in dit artikel.
* De standaardweergave van de SCM-site blijft die van de gedeelde opslag van inhoud.

## <a name="enable-local-cache-in-app-service"></a>Lokale Cache in App Service inschakelen
U configureren lokale Cache met behulp van een combinatie van gereserveerde app-instellingen. U kunt deze appinstellingen configureren met behulp van de volgende methoden:

* [Azure Portal](#Configure-Local-Cache-Portal)
* [Azure Resource Manager](#Configure-Local-Cache-ARM)

### <a name="configure-local-cache-by-using-the-azure-portal"></a>Lokale Cache configureren met behulp van de Azure-portal
<a name="Configure-Local-Cache-Portal"></a>

U de lokale Cache inschakelen op basis van de per-web-app met behulp van deze app-instelling: `WEBSITE_LOCAL_CACHE_OPTION` = `Always`  

![Azure portal-app-instellingen: lokale Cache](media/app-service-local-cache-overview/app-service-local-cache-configure-portal.png)

### <a name="configure-local-cache-by-using-azure-resource-manager"></a>Lokale Cache configureren met behulp van Azure Resource Manager
<a name="Configure-Local-Cache-ARM"></a>

```

...

{
    "apiVersion": "2015-08-01",
    "type": "config",
    "name": "appsettings",
    "dependsOn": [
        "[resourceId('Microsoft.Web/sites/', variables('siteName'))]"
    ],

"properties": {
        "WEBSITE_LOCAL_CACHE_OPTION": "Always",
        "WEBSITE_LOCAL_CACHE_SIZEINMB": "300"
    }
}

...
```

## <a name="change-the-size-setting-in-local-cache"></a>De grootte van instelling wijzigen in lokale Cache
Grootte van de lokale cache is standaard **300 MB**. Dit omvat de KCC/site en /siteextensions-mappen die zijn gekopieerd uit de store voor inhoud, evenals de lokaal gemaakte logboeken en gegevens-mappen. Als u wilt deze limiet wilt verhogen, gebruikt u de app-instelling `WEBSITE_LOCAL_CACHE_SIZEINMB`. U kunt de grootte tot vergroten **2 GB** (2000 MB) per web-app.

## <a name="best-practices-for-using-app-service-local-cache"></a>Aanbevolen procedures voor het gebruik van lokale Cache van App Service
Het is raadzaam dat u de lokale Cache in combinatie met gebruiken de [Faseringsomgevingen](../app-service/web-sites-staged-publishing.md) functie.

* Voeg de *sticky* app-instelling `WEBSITE_LOCAL_CACHE_OPTION` met de waarde `Always` aan uw **productie** sleuf. Als u `WEBSITE_LOCAL_CACHE_SIZEINMB`, ook toevoegen als een sticky instelling naar de productiesite.
* Maak een **fasering** sleuf en publiceren naar de Staging-site. Stelt u de staging-site naar de lokale Cache gebruiken voor het inschakelen van de levenscyclus van een naadloze build-implementeren-test voor staging-als u de voordelen van de lokale Cache voor de productiesite meestal niet.
* Test uw site op basis van de Staging-site.  
* Wanneer u klaar bent, een [wisselen](../app-service/web-sites-staged-publishing.md#Swap) sleuven tussen uw fasering en productie.  
* Sticky instellingen omvatten de naam en het vergrendelde aan een site. Dus wanneer de Staging-site als ze in productie komen opgehaald, neemt de instellingen voor de lokale Cache-app. De zojuist verwisseld productiesite na een paar minuten wordt uitgevoerd op de lokale cache en wordt opgewarmd als onderdeel van sleuf opwarmtijd na wisselen. Dus als de wisseling van sleuven voltooid is, wordt uw productiesite uitgevoerd op basis van de lokale cache.

## <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen
### <a name="how-can-i-tell-if-local-cache-applies-to-my-web-app"></a>Hoe kan ik zien als een lokale Cache van toepassing op mijn web-app?
Als uw web-app een krachtige, betrouwbare inhoudsarchief moet, niet de store voor inhoud gebruikt voor het schrijven van kritieke gegevens tijdens runtime en minder dan 2 GB in de totale grootte is, klikt u vervolgens is het antwoord "Ja". Als u de totale grootte van de mappen KCC/site en /siteextensions, kunt u de site-extensie "Azure Web Apps schijfgebruik."

### <a name="how-can-i-tell-if-my-site-has-switched-to-using-local-cache"></a>Hoe kan ik zien als Mijn site is overgeschakeld naar het gebruik van lokale Cache?
Als u de lokale Cache-functie met Faseringsomgevingen, wordt de wisselbewerking niet voltooien totdat de lokale Cache is opgewarmd. Als u wilt controleren of uw site wordt uitgevoerd op basis van de lokale Cache, kunt u de omgevingsvariabele voor worker-proces controleren `WEBSITE_LOCALCACHE_READY`. Volg de instructies op de [worker proces omgevingsvariabele](https://github.com/projectkudu/kudu/wiki/Process-Threads-list-and-minidump-gcdump-diagsession#process-environment-variable) pagina voor toegang tot de worker-proces omgevingsvariabele op meerdere exemplaren.  

### <a name="i-just-published-new-changes-but-my-web-app-does-not-seem-to-have-them-why"></a>Ik heb nieuwe wijzigingen hebt gepubliceerd, maar mijn web-app lijkt niet te laat. Hoe komt dat?
Als uw web-app maakt gebruik van lokale Cache, moet u uw site als u de meest recente wijzigingen opnieuw gestart. Wilt u zich geen wijzigingen aan een productiesite publiceren? Zie de sleuf opties in de vorige sectie met best practices.

### <a name="where-are-my-logs"></a>Waar zijn mijn Logboeken?
Met de lokale Cache uw logboeken en gegevensmappen zien er enigszins anders. De structuur van uw submappen blijft echter hetzelfde, behalve dat de submappen onder een submap met de indeling 'de unieke id van VM-' + tijdstempel zijn surfen.

### <a name="i-have-local-cache-enabled-but-my-web-app-still-gets-restarted-why-is-that-i-thought-local-cache-helped-with-frequent-app-restarts"></a>Ik heb lokale Cache ingeschakeld, maar nog steeds mijn web-app wordt gestart. Waarom is dit? Ik beschouwd in dat lokale Cache geholpen met regelmatige app opnieuw wordt opgestart.
Lokale Cache helpt bij het voorkomen van opslag met betrekking tot web-app wordt opnieuw opgestart. Uw web-app kan echter nog steeds opnieuw wordt opgestart ondergaan tijdens geplande infrastructuur upgrades van de virtuele machine. De algemene app opnieuw wordt opgestart die zich met lokale Cache ingeschakeld voordoen moet minder zijn.

### <a name="does-local-cache-exclude-any-directories-from-being-copied-to-the-faster-local-drive"></a>Lokale Cache alle mappen uitsluiten wordt gekopieerd naar het lokale station sneller
Als onderdeel van de stap waarmee de inhoud van de opslag worden gekopieerd, is een map met de naam opslagplaats uitgesloten. Dit helpt bij de scenario's waarbij de inhoud van uw site een opslagplaats voor bronbeheer die niet nodig in werking van dag tot dag van de web-app is kan bevatten. 
