---
title: Overzicht van Azure App Service lokale Cache | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt inschakelen, vergroten of verkleinen en de status van de lokale Cache in Azure App Service-functie opvragen
services: app-service
documentationcenter: app-service
author: SyntaxC4
manager: yochayk
editor: 
tags: optional
keywords: 
ms.assetid: e34d405e-c5d4-46ad-9b26-2a1eda86ce80
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/04/2016
ms.author: cfowler
ms.openlocfilehash: 75f2dcb80514105ed663ba1fe5f7adccc05af1fc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-app-service-local-cache-overview"></a>Overzicht van Azure App Service lokale Cache
Azure-web-app-inhoud is opgeslagen op Azure Storage en omhoog in een duurzame manier als share met de inhoud is opgehaald. Dit ontwerp is bedoeld voor gebruik met tal van apps en heeft de volgende kenmerken:  

* De inhoud wordt gedeeld door meerdere exemplaren van de virtuele machine (VM) van de web-app.
* De inhoud duurzaam en kan worden gewijzigd door het uitvoeren van web-apps.
* Logboekbestanden en diagnostische gegevensbestanden zijn beschikbaar onder de map met dezelfde gedeelde inhoud.
* De map met inhoud publiceren van nieuwe inhoud rechtstreeks worden bijgewerkt. U kunt direct dezelfde inhoud via de SCM-website en de actieve web-app weergeven (doorgaans bij sommige technologieën zoals ASP.NET een web-app opnieuw opstarten initiëren op sommige bestandswijzigingen ophalen van de meest recente inhoud).

Bij veel WebApps gebruik maken van een of meer van deze functies, zijn sommige web-apps hoeft alleen maar een hoge prestaties, alleen-lezen store voor inhoud die ze vanuit met hoge beschikbaarheid uitvoeren kunnen. Deze apps kunnen profiteren van een VM-exemplaar van een specifieke lokale cache.

De lokale Cache in Azure App Service-functie biedt een webweergave van de rol van uw inhoud. Deze inhoud is een cache voor write-maar-negeren inhoud van de opslag die asynchroon ter plaatse opstarten wordt gemaakt. Wanneer de cache klaar is, wordt de site om te worden uitgevoerd op basis van de inhoud in cache ingeschakeld. Web-apps die worden uitgevoerd op de lokale Cache hebben de volgende voordelen:

* Ze zijn immuun voor latentie die optreden wanneer ze toegang krijgen inhoud op Azure Storage tot.
* Ze zijn gebaseerd op de geplande upgrades of ongeplande downtime en eventuele andere storingen met Azure Storage die plaatsvinden op servers die de inhoudsshare.
* Ze hebben minder app opnieuw wordt opgestart als gevolg van wijzigingen voor storage-share.

## <a name="how-local-cache-changes-the-behavior-of-app-service"></a>Hoe lokale Cache verandert het gedrag van App Service
* De lokale cache is een kopie van de mappen KCC/site en /siteextensions van de web-app. Het is gemaakt op het lokale exemplaar van de VM op web-app starten. De grootte van de lokale cache per web-app is standaard beperkt tot 300 MB, maar u maximaal 2 GB kunt verhogen.
* De lokale cache is alleen-lezen. Eventuele wijzigingen worden echter genegeerd wanneer de web-app kunt u virtuele machines verplaatsen of opnieuw wordt opgestart. Gebruik geen lokale Cache voor apps die bedrijfskritieke gegevens opslaan in de store.
* Web-apps kunnen doorgaan met het schrijven van logboekbestanden en diagnostische gegevens net zo uit als op dit moment. Logboekbestanden en gegevens, maar worden lokaal opgeslagen op de virtuele machine. Vervolgens worden ze gekopieerd via periodiek naar de gedeelde inhoud store. Het kopiëren naar de gedeelde inhoud store is een gunstigste inspanning--schrijven achterzijde verbroken vanwege een onverwachte vastlopen van een VM-instantie worden kunnen.
* Er is een wijziging in de mapstructuur van de logboekbestanden en de gegevens mappen voor WebApps die gebruikmaken van de lokale Cache. Er zijn nu submappen in de opslag logboekbestanden en de gegevens de mappen die het naamgevingspatroon van 'unieke id' + tijdstempel volgen. Elk van de submappen komt overeen met een VM-instantie waar de web-app wordt uitgevoerd of is uitgevoerd.  
* Publishing wijzigingen in de web-app via een van de publicatie mechanismen publiceren naar de gedeelde inhoud store. Dit is standaard Aangezien we de gepubliceerde inhoud om te kunnen gaan. Als u wilt vernieuwen van de lokale cache van de web-app, moet deze opnieuw worden opgestart. Dit een uitzonderlijk groot stap lijkt? Als u de levenscyclus van naadloze, Zie de informatie later in dit artikel.
* D:\Home verwijst naar de lokale cache. D:\Local blijven verwijzen naar de tijdelijke opslag van de VM-specifieke.
* De standaardweergave van de site SCM blijft die de shared content store.

## <a name="enable-local-cache-in-app-service"></a>Lokale Cache in App Service inschakelen
U configureren lokale Cache met behulp van een combinatie van gereserveerde app-instellingen. U kunt deze app-instellingen configureren met behulp van de volgende methoden:

* [Azure Portal](#Configure-Local-Cache-Portal)
* [Azure Resource Manager](#Configure-Local-Cache-ARM)

### <a name="configure-local-cache-by-using-the-azure-portal"></a>Lokale Cache configureren met behulp van de Azure-portal
<a name="Configure-Local-Cache-Portal"></a>

U kunt lokale Cache inschakelen op basis van de per-web-app met behulp van deze appinstelling:`WEBSITE_LOCAL_CACHE_OPTION` = `Always`  

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

## <a name="change-the-size-setting-in-local-cache"></a>Wijzig de instelling van de grootte in lokale Cache
Grootte van de lokale cache is standaard **300 MB**. Dit omvat de KCC/site en /siteextensions-mappen die zijn gekopieerd uit de store voor inhoud, evenals lokaal gemaakte mappen logboeken en gegevens. U kunt deze limiet verhogen door de appinstelling gebruiken `WEBSITE_LOCAL_CACHE_SIZEINMB`. U kunt de grootte tot vergroten **2 GB** (2000 MB) per web-app.

## <a name="best-practices-for-using-app-service-local-cache"></a>Aanbevolen procedures voor het gebruik van lokale App Service-Cache
Het is raadzaam dat u de lokale Cache in combinatie met gebruiken de [Faseringsomgevingen](../app-service/web-sites-staged-publishing.md) functie.

* Voeg de *een tijdelijke* app-instelling `WEBSITE_LOCAL_CACHE_OPTION` met de waarde `Always` naar uw **productie** sleuf. Als u `WEBSITE_LOCAL_CACHE_SIZEINMB`, ook toevoegen als een tijdelijke instelling naar de productiesite.
* Maak een **fasering** sleuf en publiceren naar de Staging-site. De faseringssleuf gewisseld naar de lokale Cache gebruiken om in te schakelen van de levensduur van een naadloze build-implementeren-test voor fasering als u de voordelen van de lokale Cache voor de productiesite krijgt doorgaans niet wordt ingesteld.
* Test uw site op basis van de Staging-site.  
* Wanneer u klaar bent, geven een [wisselen](../app-service/web-sites-staged-publishing.md#Swap) sleuven tussen uw fasering en productie.  
* Een tijdelijke instellingen omvatten de naam en een tijdelijke in een sleuf. Dus wanneer de Staging-sleuf opgehaald gewisseld naar de productie, neemt de instellingen van de lokale Cache-app. De zojuist gewisseld productiesite wordt uitgevoerd aan de lokale cache na een paar minuten en wordt opgewarmd als onderdeel van sleuf opwarmtijd na wisselen. Dus wanneer de wisseling van sleuven voltooid is, wordt de productiesite werkt met de lokale cache.

## <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen
### <a name="how-can-i-tell-if-local-cache-applies-to-my-web-app"></a>Hoe kan ik zien als een lokale Cache van toepassing op mijn web-app?
Als uw web-app een krachtige, betrouwbare inhoudsarchief moet, de content store geen gebruikt kritieke gegevens tijdens runtime te schrijven en minder dan 2 GB in de totale grootte is, is het antwoord 'Ja'! Als u de totale grootte van de mappen KCC/site en /siteextensions, kunt u de site-uitbreiding "Azure Web Apps gebruik van de schijf."

### <a name="how-can-i-tell-if-my-site-has-switched-to-using-local-cache"></a>Hoe kan ik zien of Mijn site is overgeschakeld naar het gebruik van de lokale Cache
Als u de functie van de lokale Cache met Faseringsomgevingen gebruikt, wordt de wisselbewerking niet voltooid tot lokale Cache is opgewarmd. U kunt de omgevingsvariabele voor worker-proces controleren om te controleren of uw site wordt uitgevoerd op basis van de lokale Cache, `WEBSITE_LOCALCACHE_READY`. Volg de instructies op de [worker proces omgevingsvariabele](https://github.com/projectkudu/kudu/wiki/Process-Threads-list-and-minidump-gcdump-diagsession#process-environment-variable) pagina voor toegang tot de omgevingsvariabele worker-proces op meerdere exemplaren.  

### <a name="i-just-published-new-changes-but-my-web-app-does-not-seem-to-have-them-why"></a>Ik alleen nieuwe wijzigingen hebt gepubliceerd, maar mijn web-app niet lijkt te laten. Hoe komt dat?
Als uw web-app gebruikmaakt van lokale Cache, moet u opnieuw opstarten van uw site om de meest recente wijzigingen. Wil je je niet wijzigingen publiceren naar een productiesite? Zie de sleuf opties in de vorige sectie met best practices.

### <a name="where-are-my-logs"></a>Waar bevinden zich mijn Logboeken?
Met de lokale Cache zien uw logboeken en gegevensmappen er iets anders. De structuur van uw submappen blijft echter hetzelfde, behalve dat de submappen onder een submap met de indeling 'unieke id van VM-' + tijdstempel zijn surfen.

### <a name="i-have-local-cache-enabled-but-my-web-app-still-gets-restarted-why-is-that-i-thought-local-cache-helped-with-frequent-app-restarts"></a>Ik heb lokale Cache ingeschakeld, maar nog steeds mijn web-app wordt gestart. Waarom is dat? Ik beschouwd in dat lokale Cache heeft geholpen frequente app opnieuw wordt opgestart.
Lokale Cache voorkomen dat opslag-gerelateerde web-app opnieuw wordt opgestart. Uw web-app kan echter nog steeds opnieuw wordt opgestart ondergaan tijdens geplande infrastructuur upgrades van de virtuele machine. De algehele app opnieuw wordt opgestart die met de lokale Cache ingeschakeld optreden moet minder zijn.

### <a name="does-local-cache-exclude-any-directories-from-being-copied-to-the-faster-local-drive"></a>Lokale Cache alle mappen uitsluiten wordt gekopieerd naar het lokale station sneller
Als onderdeel van de stap die de inhoud van de opslag kopieert, is een map met de naam opslagplaats uitgesloten. Dit helpt met scenario's waar de inhoud van uw site een resourcebeheerbibliotheek die niet nodig in dagelijkse bewerking van de web-app zijn kan bevatten. 
