---
title: Langzame prestaties van de web-app in App Service | Microsoft Docs
description: In dit artikel helpt u problemen met trage web app prestaties in Azure App Service.
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: top-support-issue
keywords: Web-appprestaties, trage app de app is langzaam
ms.assetid: b8783c10-3a4a-4dd6-af8c-856baafbdde5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: cephalin
ms.openlocfilehash: 1cfe7ec37ad8b24a8bd9ab2bf67e95675a57b675
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Oplossen van prestatieproblemen van trage web-app in Azure App Service
Dit artikel helpt u problemen met trage web app prestaties in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op [de Azure MSDN en de Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook kunt u ook een incident voor ondersteuning van Azure bestand. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en klik op **Get Support**.

## <a name="symptom"></a>Symptoom
Wanneer u de web-app, het laden van pagina's traag bladert en soms time-out.

## <a name="cause"></a>Oorzaak
Dit probleem wordt vaak veroorzaakt door problemen met toepassingen niveau, zoals:

* netwerkaanvragen duurt lang
* toepassing code of database-query's wordt inefficiënt
* toepassing met hoge geheugen/CPU
* toepassing is gecrasht vanwege een uitzondering

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing
Het oplossen van problemen kan worden onderverdeeld in drie verschillende taken in opeenvolgende volgorde:

1. [Bekijken en controleren van gedrag van toepassingen](#observe)
2. [Gegevens verzamelen](#collect)
3. [Het probleem te verhelpen](#mitigate)

[App Service Web Apps](/services/app-service/web/) biedt verschillende opties bij elke stap.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Bekijken en controleren van gedrag van toepassingen
#### <a name="track-service-health"></a>Servicestatus bijhouden
Microsoft Azure publicizes telkens wanneer er een service wordt onderbroken of de prestaties nadelig beïnvloeden is. U kunt de status van de service bijhouden op het [Azure-portal](https://portal.azure.com/). Zie voor meer informatie [bijhouden servicestatus](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Uw web-app bewaken
Deze optie kunt u achterhalen als uw toepassing problemen ondervindt. Klik op de blade van uw web-app de **aanvragen en fouten** tegel. De **metriek** blade ziet u alle metrische gegevens die u kunt toevoegen.

Sommige van de metrische gegevens die u voor uw web-app kunt bewaken

* Gemiddelde geheugen werkset
* Gemiddelde reactietijd
* CPU-tijd
* Geheugen-werkset
* Aanvragen

![web-app-prestaties bewaken](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Zie voor meer informatie:

* [Web-Apps bewaken in Azure App Service](web-sites-monitor.md)
* [Waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Web-Eindpuntstatus controleren
Als u uw web-app uitvoert de **standaard** Web-Apps prijscategorie, kunt u twee eindpunten van drie verschillende geografische locaties bewaken.

Eindpuntcontrole configureert webtests van geografisch verspreide locaties met reactietijden en uptime van web-URL's testen. De test voert een HTTP GET-bewerking op de web-URL voor het bepalen van de reactietijd en bedrijfstijd vanaf elke locatie. Elke geconfigureerde locatie wordt een test om de vijf minuten uitgevoerd.

Actieve tijdsduur wordt bewaakt met HTTP-antwoordcodes en reactietijd wordt gemeten in milliseconden. Een bewakingstest mislukt als de HTTP-antwoordcode groter dan of gelijk zijn aan 400 is of als het antwoord langer dan 30 seconden duurt. Een eindpunt wordt aangemerkt als beschikbaar als de bewakingstests van de opgegeven locaties slagen.

Om in te stellen, Zie [in Azure App Service-apps bewaken](web-sites-monitor.md).

Zie ook [Azure-websites te houden van plus Eindpuntcontrole - met Stefan Schackow](https://channel9.msdn.com/Shows/Azure-Friday/Keeping-Azure-Web-Sites-up-plus-Endpoint-Monitoring-with-Stefan-Schackow) voor een video over eindpuntcontrole.

#### <a name="application-performance-monitoring-using-extensions"></a>APM-extensies gebruiken
U kunt ook de toepassingsprestaties van uw bewaken met behulp van *site-uitbreidingen*.

Elke App Service-web-app biedt een uitbreidbare management-eindpunt waarmee u een krachtige reeks hulpprogramma's die zijn geïmplementeerd als site-uitbreidingen te gebruiken. Extensies zijn onder andere: 

- Bron code-editors, zoals [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx). 
- Beheerhulpprogramma's voor verbonden resources, zoals een MySQL-database is verbonden met een web-app.

[Azure Application Insights](/services/application-insights/) en [New Relic](/marketplace/partners/newrelic/newrelic/) zijn twee van de site-uitbreidingen die beschikbaar zijn voor prestatiebewaking. Voor het gebruik van New Relic door een agent tijdens runtime te installeren. Voor het gebruik van Azure Application Insights bouwen van uw code bij een SDK en u kunt ook een uitbreiding die toegang tot aanvullende gegevens biedt installeren. De SDK kunt u code schrijven voor het gebruik en prestaties van uw app in meer detail controleren.

Zie voor het gebruik van Application Insights [prestaties van webtoepassingen bewaken](../application-insights/app-insights-web-monitor-performance.md).

Zie voor het gebruik van New Relic [nieuwe Relic prestaties Toepassingsbeheer op Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />

### <a name="2-collect-data"></a>2. Gegevens verzamelen
De Web-Apps-omgeving biedt diagnostische functionaliteit voor logboekgegevens van de webserver en de webtoepassing. De informatie is in de web server diagnostics en application diagnostics verdeeld.

#### <a name="enable-web-server-diagnostics"></a>Web server diagnostische gegevens inschakelen
U kunt in- of uitschakelen van de volgende soorten logboeken:

* **Gedetailleerde fout logboekregistratie** -gedetailleerde informatie over de fout voor HTTP-statuscodes die wijzen op mislukte (statuscode 400 of hoger). Deze kan informatie om te bepalen waarom de server heeft geretourneerd met de foutcode bevatten.
* **Kan geen aanvraag voor het traceren** -gedetailleerde informatie over de mislukte aanvragen, met inbegrip van een tracering van de IIS-onderdelen gebruikt voor het verwerken van de aanvraag en de tijd die in elk onderdeel. Dit is handig als u probeert te verbeteren de prestaties van web-app of komen wat een specifieke HTTP-fout wordt veroorzaakt.
* **Web Server-logboekregistratie** -informatie over HTTP transacties met behulp van de W3C extended logboekbestandsindeling. Dit is handig bij het bepalen van de algemene web app metrische gegevens, zoals het aantal aanvragen dat is verwerkt of hoeveel aanvragen van een specifiek IP-adres.

#### <a name="enable-application-diagnostics"></a>Toepassing diagnostische gegevens inschakelen
Er zijn verschillende opties voor het verzamelen van prestatiegegevens van toepassingen van Web-Apps, profiel uw toepassing live vanuit Visual Studio of wijzigen van uw toepassingscode voor meer informatie en traceringen logboekregistratie. U kunt de opties op basis van hoeveel u toegang tot de toepassing en u waargenomen hebt van de bewaking van hulpprogramma's voor.

##### <a name="use-application-insights-profiler"></a>Gebruik Application Insights Profiler
U kunt de Application Insights Profiler beginnen met gedetailleerde prestaties traceringen opnemen inschakelen. U toegang hebt tot traceringen vastgelegd maximaal vijf dagen geleden als u wilt onderzoeken van problemen is opgetreden in het verleden. U kunt deze optie als u toegang tot Application Insights-resource voor de web-app op Azure-portal hebben.

Application Insights Profiler voorziet in statistieken op de reactietijd voor elke aanroep van de web- en traceringen die aangeeft welke coderegel veroorzaakt trage reacties. Soms de App Service-app is langzaam, omdat bepaalde code niet in een zodat geschreven wordt manier. Voorbeelden zijn opeenvolgende code die in de database voor parallelle en ongewenste vergrendeling contentions kan worden uitgevoerd. Verwijderen van deze knelpunten in de code verhoogt de prestaties van de app, maar ze zijn moeilijk te detecteren zonder uitgebreide traceringen en logboeken te stellen. De traceringen verzameld door Application Insights Profiler kunt identificeren van de regels code waarmee de toepassing wordt vertraagd en overwinnen deze uitdaging voor App Service-apps.

 Zie voor meer informatie [Profiling live Azure-web-apps met Application Insights](../application-insights/app-insights-profiler.md).

##### <a name="use-remote-profiling"></a>Profileren van externe gebruiken
In Azure App Service Web Apps, API-Apps en WebJobs kunnen op afstand profiling wordt gestart. Selecteer deze optie als u toegang tot de resource voor de web-app hebt en u weet hoe u het probleem te reproduceren of het prestatieprobleem gebeurt er als u weet dat het exacte tijdsinterval.

Externe Profiling is handig als het CPU-gebruik van het proces hoog is en uw proces trager dan verwacht wordt uitgevoerd of de latentie van HTTP-aanvragen zijn hoger dan normaal, u kunt op afstand uw proces bekijken en ophalen van de CPU steekproeven stacks voor het analyseren van het proces activiteit en code hot paden.

Zie voor meer informatie [afstand profileren ondersteuning in Azure App Service](https://azure.microsoft.com/blog/remote-profiling-support-in-azure-app-service).

##### <a name="set-up-diagnostic-traces-manually"></a>Diagnostische traceringen handmatig instellen
Als u toegang tot de broncode van de web-toepassing hebt, met Application diagnostics kunt u informatie die wordt geproduceerd door een webtoepassing vastleggen. ASP.NET-toepassingen kunnen gebruikmaken van de `System.Diagnostics.Trace` klasse om informatie te registreren in het toepassingslogboek van de diagnostische gegevens. Echter, moet u de code wijzigen en implementeren van uw toepassing. Deze methode wordt aanbevolen als uw app wordt uitgevoerd op een testomgeving.

Zie voor gedetailleerde instructies over het configureren van uw toepassing voor logboekregistratie [logboekregistratie van diagnostische gegevens van web-apps in Azure App Service](web-sites-enable-diagnostic-log.md).

#### <a name="use-the-azure-app-service-support-portal"></a>Gebruik de portal van Azure App Service-ondersteuning
Web Apps biedt u de mogelijkheid om op te lossen problemen met betrekking tot uw web-app door te kijken HTTP Logboeken, gebeurtenislogboeken proces dumpbestanden en meer. U toegang hebt tot deze informatie met onze portal ondersteuning op **http://&lt;uw app-naam >.scm.azurewebsites.net/Support**

De portal van de ondersteuning van Azure App Service biedt drie afzonderlijke tabbladen ter ondersteuning van de drie stappen van een algemeen scenario voor het oplossen van problemen:

1. Huidige gedrag observeren
2. Door het verzamelen van diagnostische gegevens en uitvoeren van de ingebouwde analyzers analyseren
3. Beperken

Als het probleem nu voordoet zich, klikt u op **analyseren** > **Diagnostics** > **onderzoeken nu** voor het maken van een diagnostische sessie voor u, verzamelt HTTP zich aanmeldt, Logboeken, geheugen dumpbestanden, PHP-foutenlogboek en PHP-proces rapport.

Nadat de gegevens worden verzameld, wordt de portal ondersteuning wordt een analyse uitgevoerd op de gegevens en biedt u een HTML-rapport.

Als u downloaden van de gegevens standaard wilt, zou deze opgeslagen in de map D:\home\data\DaaS.

Zie voor meer informatie over de ondersteuning van Azure App Service portal [nieuwe Updates van Site-uitbreiding ondersteuning voor Azure Websites](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Gebruik de Kudu-Console voor foutopsporing
Web-Apps wordt geleverd met een Foutopsporingsconsole die u gebruiken kunt voor foutopsporing, verkennen, het uploaden van bestanden, evenals de JSON-eindpunten voor het ophalen van informatie over uw omgeving. Deze console heet de *Kudu-Console* of de *SCM Dashboard* voor uw web-app.

U kunt dit dashboard openen door op de koppeling **https://&lt;uw app-naam >.scm.azurewebsites.net/**.

Enkele van de Kudu verschaft zaken zijn:

* omgevingsinstellingen voor uw toepassing
* logboekstream
* diagnostische dump
* fouten opsporen console waarin u Powershell-cmdlets en basic DOS-opdrachten kunt uitvoeren.

Een andere handige functie van Kudu is dat, als uw toepassing die eerste kans uitzonderingen, kunt u Kudu en dumpbestanden voor het SysInternals-hulpprogramma Procdump geheugen maken. Deze geheugendumps momentopnamen van het proces zijn en vaak kunt u meer gecompliceerde problemen oplossen met uw web-app.

Zie voor meer informatie over functies die beschikbaar zijn in Kudu [Azure Websites Team Services-hulpprogramma's die u moet weten over](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Het probleem te verhelpen
#### <a name="scale-the-web-app"></a>De web-app schalen
In Azure App Service kunt voor betere prestaties en doorvoer, u de schaal waarmee u uw toepassing uitvoert aanpassen. Schalen van een web-app omvat twee gerelateerde acties: uw App Service-abonnement wijzigen in een hogere prijscategorie en bepaalde instellingen configureren nadat u hebt overgeschakeld naar de hogere prijscategorie.

Zie voor meer informatie over het schalen [schalen van een web-app in Azure App Service](web-sites-scale.md).

U kunt bovendien uw toepassing uitvoeren in meer dan één exemplaar. Uitschalen niet alleen biedt u meer verwerkingscapaciteit maar kunt u ook bepaalde hoeveelheid fouttolerantie. Als het proces uitgeschakeld op één exemplaar wordt, blijven de overige exemplaren aanvragen verwerken.

U kunt instellen om de schaal worden handmatig of automatisch.

#### <a name="use-autoheal"></a>Gebruik AutoHeal
AutoHeal wordt gerecycled het werkproces voor uw app op basis van de instellingen die u (zoals wijzigingen in de configuratie, aanvragen, limieten op basis van geheugen of de benodigde tijd kiest voor het uitvoeren van een aanvraag). De meeste gevallen, is recyclen van het proces de snelste manier om te herstellen van een probleem. Hoewel u altijd opnieuw van de web-app rechtstreeks in de Azure-portal opstarten kunt, doet AutoHeal dit automatisch voor u. Hoeft u sommige triggers toevoegen in web.config voor de hoofdmap voor uw web-app is. Deze instellingen zou op dezelfde manier werken, zelfs als uw toepassing niet een .net-app is.

Zie voor meer informatie [Azure websites automatisch herstel](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-the-web-app"></a>Opnieuw opstarten van de web-app
Opnieuw starten is vaak de eenvoudigste manier om eenmalige problemen herstellen. Op de [Azure-portal](https://portal.azure.com/), op de blade van uw web-app, hebt u de opties te stoppen of opnieuw opstarten van uw app.

 ![opnieuw opstarten van web-app voor het oplossen van prestatieproblemen met](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

U kunt ook uw web-app met Azure Powershell beheren. Zie [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md) voor meer informatie.
