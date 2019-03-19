---
title: Problemen met prestatievermindering - Azure App Service oplossen | Microsoft Docs
description: Dit artikel helpt u problemen met trage app-prestaties oplossen in Azure App Service.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: top-support-issue
keywords: Web app-prestaties, trage app app is langzaam
ms.assetid: b8783c10-3a4a-4dd6-af8c-856baafbdde5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 2d17991854f13f889c4e8c3a8c6f18e933655546
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57884219"
---
# <a name="troubleshoot-slow-app-performance-issues-in-azure-app-service"></a>Problemen met trage app-prestaties oplossen in Azure App Service
Dit artikel helpt u problemen met trage app-prestaties oplossen in [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714).

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op [het Azure MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning voor Azure site](https://azure.microsoft.com/support/options/) en klikt u op **ontvang ondersteuning**.

## <a name="symptom"></a>Symptoom
Wanneer u de app, het laden van pagina's traag bladeren en soms time-out.

## <a name="cause"></a>Oorzaak
Dit probleem wordt vaak veroorzaakt door problemen met toepassingen niveau, zoals:

* netwerkaanvragen duurt lang
* toepassing code of database-query's wordt inefficiënt
* toepassing met behulp van hoge geheugen/CPU
* toepassing is gecrasht vanwege een uitzondering

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing
Oplossen van problemen kan worden onderverdeeld in drie verschillende taken, in opeenvolgende volgorde:

1. [Bekijk en werking van de toepassing controleren](#observe)
2. [Gegevens verzamelen](#collect)
3. [Het probleem](#mitigate)

[App Service](overview.md) biedt verschillende opties bij elke stap.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Bekijk en werking van de toepassing controleren
#### <a name="track-service-health"></a>Servicestatus bijhouden
Microsoft Azure publicizes telkens wanneer er een onderbreking of prestaties verslechtering van de service is. U kunt de status van de service volgen op de [Azure-portal](https://portal.azure.com/). Zie voor meer informatie, [servicestatus bijhouden](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-app"></a>Uw app bewaken
Deze optie kunt u achterhalen als uw toepassing problemen ondervindt. In de blade van uw app, klikt u op de **aanvragen en fouten** tegel. De **Metric** blade ziet u alle metrische gegevens die u kunt toevoegen.

Sommige van de metrische gegevens die u mogelijk wilt controleren voor uw app

* Gemiddeld geheugenwerkset
* Gemiddelde reactietijd
* CPU-tijd
* Geheugenwerkset
* Aanvragen

![app-prestaties bewaken](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Zie voor meer informatie:

* [Apps bewaken in Azure App Service](web-sites-monitor.md)
* [Waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Web-eindpunt controleren
Als u uw app uitvoert de **Standard** App Service prijscategorie, kunt u twee eindpunten van drie verschillende geografische locaties te controleren.

Eindpuntbewaking webtests van geografisch gedistribueerde locaties die reactietijd en uptime van web-URL's testen geconfigureerd. De test voert een HTTP GET-bewerking op de web-URL om te bepalen van de reactietijd en uptime vanaf elke locatie. Elke geconfigureerde locatie wordt een test om de vijf minuten uitgevoerd.

Actieve tijdsduur wordt bewaakt met behulp van HTTP-responscodes en reactietijd wordt gemeten in milliseconden. Een controle test mislukt als de HTTP-responscode groter dan of gelijk is aan 400 is of als het antwoord meer dan 30 seconden heeft. Een eindpunt wordt aangemerkt als beschikbaar als de bewakingstests van de opgegeven locaties slagen.

Om in te stellen, Zie [apps bewaken in Azure App Service](web-sites-monitor.md).

Zie ook [Azure-websites te houden van plus Eindpuntbewaking - met Stefan Schackow](https://channel9.msdn.com/Shows/Azure-Friday/Keeping-Azure-Web-Sites-up-plus-Endpoint-Monitoring-with-Stefan-Schackow) voor een video over het bewaken van het eindpunt.

#### <a name="application-performance-monitoring-using-extensions"></a>Toepassingsprestaties bewaken met extensies
U kunt ook de toepassingsprestaties van uw bewaken met behulp van een *site-extensie*.

Elke App Service-app biedt een uitbreidbaar management-eindpunt waarmee u kunt het gebruiken van een krachtige set hulpprogramma's die zijn geïmplementeerd als site-uitbreidingen. Extensies zijn onder andere: 

- Broncode-editors, zoals [Azure DevOps](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx). 
- Beheerhulpprogramma's voor verbonden bronnen, zoals een MySQL-database verbonden met een app.

[Azure Application Insights](https://azure.microsoft.com/services/application-insights/) is een site-extensie die is ook beschikbaar voor prestatiebewaking. Opnieuw opbouwen uw code met een SDK voor het gebruik van Application Insights. U kunt ook een extensie die toegang tot aanvullende gegevens biedt installeren. De SDK kunt u code schrijven om te controleren van het gebruik en prestaties van uw app in meer detail. Zie voor meer informatie, [prestaties in webtoepassingen bewaken](../azure-monitor/app/web-monitor-performance.md).

<a name="collect" />

### <a name="2-collect-data"></a>2. Gegevens verzamelen
App Service biedt diagnosefunctionaliteit voor waardevolle informatie uit de webserver en de web-App. De informatie is onderverdeeld in de web server diagnostics en application diagnostics.

#### <a name="enable-web-server-diagnostics"></a>Web server diagnostische gegevens inschakelen
U kunt inschakelen of uitschakelen van de volgende soorten logboeken:

* **Gedetailleerde fout logboekregistratie** -gedetailleerde foutinformatie voor HTTP-statuscodes die duiden op een fout (statuscode 400 of hoger). Dit kan bevatten informatie die kan helpen te bepalen waarom de foutcode in de server heeft geretourneerd.
* **Kan geen aanvraag tracering** -gedetailleerde informatie over mislukte aanvragen, met inbegrip van een tracering van de IIS-componenten gebruikt voor het verwerken van de aanvraag en de tijd die in elk onderdeel. Dit is handig als u probeert te verbeteren van de app of isolatie van wat de oorzaak is van een specifieke HTTP-fout.
* **Web Server-logboekregistratie** -informatie over HTTP-transacties met behulp van de uitgebreide indeling W3C. Dit is handig bij het bepalen van de algemene app-metrische gegevens, zoals het aantal aanvragen dat is verwerkt of het aantal aanvragen afkomstig zijn van een specifiek IP-adres.

#### <a name="enable-application-diagnostics"></a>Toepassing diagnostische gegevens inschakelen
Er zijn verschillende opties voor het verzamelen van prestatiegegevens van toepassingen van App Service, profiel voor uw toepassing live vanuit Visual Studio of wijzig de code van uw toepassing om te melden voor meer informatie en traceringen. U kunt de opties op basis van hoeveel u toegang tot de toepassing en wat dat zich heeft voorgedaan hebt van de bewaking-hulpprogramma's.

##### <a name="use-application-insights-profiler"></a>Application Insights Profiler gebruiken
U kunt de Application Insights Profiler om te beginnen met het vastleggen van gedetailleerde prestatietraces inschakelen. U kunt openen traceringen vastgelegd maximaal vijf dagen geleden wanneer u nodig hebt om problemen te onderzoeken in het verleden is er gebeurd. U kunt deze optie als u toegang tot de Application Insights-resource van de app in Azure portal hebt.

Application Insights Profiler voorziet in statistieken op de reactietijd voor elke webaanroep van de- en traceringen die aangeeft welke coderegel veroorzaakt de trage reacties. Soms de App Service-app is traag, omdat bepaalde code niet biedt u een praktische geschreven manier. Voorbeelden zijn opeenvolgende code die in de database voor parallelle en ongewenst vergrendeling contentions kan worden uitgevoerd. Bezig met verwijderen van deze knelpunten in de code van de app-prestaties verhoogt, maar ze zijn moeilijk te detecteren zonder in te stellen uitgebreide traces en Logboeken. De traceringen verzameld door Application Insights Profiler kunt identificeren van de regels met code waarmee de toepassing wordt vertraagd en strijden tegen deze uitdaging voor App Service-apps.

 Zie voor meer informatie, [live-apps in Azure App Service met Application Insights-profilering](../azure-monitor/app/profiler.md).

##### <a name="use-remote-profiling"></a>Gebruik extern profileren
In Azure App Service WebApps en API apps, mobiele back-ends en WebJobs kunnen op afstand worden geprofileerd. Selecteer deze optie als u toegang tot de resource-app hebt en u weet hoe u het probleem te reproduceren of het prestatieprobleem gebeurt er als u bekend bent met de exacte tijdsinterval.

Externe Profiling is handig als het CPU-gebruik van het proces hoog is en het proces is langzamer dan verwacht actief of de latentie van HTTP-aanvragen zijn hoger dan normaal, kunt u op afstand profileren van het proces en ophalen van de CPU-stacks steekproeven aanroep voor het analyseren van het proces activiteit en de code ' hot ' paden.

Zie voor meer informatie, [ondersteuning voor externe profilering in Azure App Service](https://azure.microsoft.com/blog/remote-profiling-support-in-azure-app-service).

##### <a name="set-up-diagnostic-traces-manually"></a>Diagnostische traceringen handmatig instellen
Als u toegang tot de broncode van de web-toepassing hebt, wordt er Application diagnostics kunt u voor het vastleggen van gegevens die worden geproduceerd door een web-App. ASP.NET-toepassingen kunnen gebruikmaken van de `System.Diagnostics.Trace` klasse om informatie te registreren in het toepassingslogboek van diagnostische gegevens. Echter, moet u de code te wijzigen en opnieuw implementeren van uw toepassing. Deze methode wordt aanbevolen als uw app wordt uitgevoerd in een testomgeving.

Zie voor gedetailleerde instructies over het configureren van uw toepassing voor logboekregistratie [Diagnostische logboekregistratie inschakelen voor apps in Azure App Service](troubleshoot-diagnostic-logs.md).

#### <a name="use-the-diagnostics-tool"></a>Het hulpprogramma diagnostics gebruiken
App Service biedt een intelligente en interactieve ervaring om u te helpen bij het oplossen van uw app geen configuratie vereist. Als u problemen met uw app uitvoert, wordt het hulpprogramma diagnostics wijs wat er mis om u te helpen de juiste informatie snel en eenvoudig kunt oplossen en los het probleem is.

Voor toegang tot App Service-diagnostics, gaat u naar uw App Service-app of een App Service-omgeving in de [Azure-portal](https://portal.azure.com). Klik in het linkernavigatievenster op **vaststellen en oplossen van problemen met**.

#### <a name="use-the-kudu-debug-console"></a>Gebruik de Kudu-Console voor foutopsporing
App Service wordt geleverd met een console voor foutopsporing die u gebruiken kunt voor foutopsporing, verkennen, het uploaden van bestanden, evenals de JSON-eindpunten voor het ophalen van gegevens over uw omgeving. Deze console heet de *Kudu-Console* of de *SCM-Dashboard* voor uw app.

U kunt dit dashboard openen door te gaan naar de koppeling **https://&lt;uw app-naam >.scm.azurewebsites.net/**.

Enkele van de dingen die Kudu biedt, zijn:

* omgevingsinstellingen voor uw toepassing
* logboekstream
* diagnostische dump
* fouten opsporen in een console waarin u Powershell-cmdlets en basic DOS-opdrachten kunt uitvoeren.

Een andere handige functie van Kudu is dat, als uw toepassing de eerste kans uitzonderingen genereren is, kunt u Kudu en dumpbestanden voor de SysInternals-hulpprogramma Procdump geheugen maken. Deze geheugendumps zijn momentopnamen van het proces en vaak kunt u meer gecompliceerde problemen oplossen met uw app.

Zie voor meer informatie over functies die beschikbaar zijn in Kudu [Azure DevOps-hulpprogramma's die u moet weten over](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Het probleem
#### <a name="scale-the-app"></a>De app schalen
In Azure App Service, kunt voor betere prestaties en doorvoer, u de schaal waarmee u uw toepassing uitvoert aanpassen. Omhoog schalen van een app bestaat uit twee gerelateerde acties: uw App Service-plan in een hogere prijscategorie te wijzigen, en bepaalde instellingen te configureren nadat u hebt overgeschakeld naar de hogere prijscategorie.

Zie voor meer informatie over het omhoog schalen [schalen van een app in Azure App Service](web-sites-scale.md).

Bovendien kunt u uw toepassing uitvoeren op meer dan één exemplaar. Uitschalen wordt niet alleen biedt u meer mogelijkheden voor verwerking, maar er wordt een biedt ook een zekere mate van fouttolerantie. Als het proces uitgeschakeld op één exemplaar wordt, blijven de andere exemplaren aanvragen verwerken.

U kunt instellen om het handmatig of automatisch worden geschaald.

#### <a name="use-autoheal"></a>Gebruikmaken van AutoHeal
AutoHeal wordt gerecycled het werkproces voor uw app op basis van de instellingen die u (zoals wijzigingen in de configuratie, aanvragen, de limieten voor geheugen of de tijd die nodig is kiest voor het uitvoeren van een aanvraag). De meeste van de tijd is recyclen van het proces de snelste manier om te herstellen van een probleem. Hoewel u altijd opnieuw de app rechtstreeks in Azure portal Start kunt, doet AutoHeal het allemaal automatisch voor u. U hoeft alleen is bepaalde triggers toevoegen in de hoofdmap web.config voor uw app. Deze instellingen zou moeten werken op dezelfde manier, zelfs als uw toepassing niet een .NET-app is.

Zie voor meer informatie, [automatisch herstel Azure Web Sites](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-the-app"></a>De app opnieuw starten
Opnieuw starten is vaak de eenvoudigste manier om eenmalige problemen te verhelpen. Op de [Azure-portal](https://portal.azure.com/), op de blade van uw app, hebt u de opties om te stoppen of opnieuw starten van uw app.

 ![app voor het oplossen van prestatieproblemen met opnieuw starten](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

U kunt ook uw app met behulp van Azure Powershell beheren. Zie [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md) voor meer informatie.
