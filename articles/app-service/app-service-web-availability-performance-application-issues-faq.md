---
title: Toepassingsprestaties Veelgestelde vragen voor Azure-web-apps | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over de beschikbaarheid, prestaties en problemen met toepassingen in de functie Web Apps van Azure App Service.
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 926005eace0acde27e26619a2787368ba0954351
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="application-performance-faqs-for-web-apps-in-azure"></a>Toepassingsprestaties Veelgestelde vragen voor Web-Apps in Azure

In dit artikel bevat antwoorden op veelgestelde vragen (FAQ's) over toepassing prestatieproblemen voor de [functie van Azure App Service Web Apps](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-is-my-app-slow"></a>Waarom wordt mijn trage app?

Langzame appprestaties kan bijdragen tot meerdere factoren. Zie voor gedetailleerde stappen voor probleemoplossing, [problemen met trage web appprestaties](app-service-web-troubleshoot-performance-degradation.md).

## <a name="how-do-i-troubleshoot-a-high-cpu-consumption-scenario"></a>Hoe kan ik een hoog CPU-verbruik scenario oplossen?

Uw app kunnen in sommige gevallen hoog CPU-verbruik echt vereist meer bronnen. Overweeg in dat geval schalen met een hogere servicetier zodat de toepassing krijgt alle resources die nodig is. Andere keren kan hoog CPU-verbruik worden veroorzaakt door een onjuiste lus of door de code uit veiligheidsoverwegingen. Om inzicht te krijgen in wat meer CPU-verbruik wordt activering is een tweedelige-proces. Eerst een Procesdump maken en vervolgens de Procesdump analyseren. Zie voor meer informatie [vastleggen en analyseren van een dumpbestand voor hoog CPU-verbruik voor Web-Apps](https://blogs.msdn.microsoft.com/asiatech/2016/01/20/how-to-capture-dump-when-intermittent-high-cpu-happens-on-azure-web-app/).

## <a name="how-do-i-troubleshoot-a-high-memory-consumption-scenario"></a>Hoe kan ik een scenario hoog geheugengebruik oplossen?

Uw app kunnen in sommige gevallen hoog geheugengebruik echt vereist meer bronnen. Overweeg in dat geval schalen met een hogere servicetier zodat de toepassing krijgt alle resources die nodig is. Andere tijden kan een fout in de code een geheugenlek veroorzaken. Code uit veiligheidsoverwegingen mogelijk ook geheugenverbruik verhogen. Om inzicht te krijgen in wat wordt activering van veel geheugen verbruik bestaat uit twee fasen. Eerst een Procesdump maken en vervolgens de Procesdump analyseren. Crash Diagnoser uit de galerie van Azure Site-uitbreiding kunt efficiënt beide deze stappen uitvoeren. Zie voor meer informatie [vastleggen en analyseren van een dumpbestand voor onregelmatige veel geheugen voor Web-Apps](https://blogs.msdn.microsoft.com/asiatech/2016/02/02/how-to-capture-and-analyze-dump-for-intermittent-high-memory-on-azure-web-app/).

## <a name="how-do-i-automate-app-service-web-apps-by-using-powershell"></a>Hoe ik App Service-web-apps met behulp van PowerShell automatiseren?

U kunt de PowerShell-cmdlets gebruiken om te beheren en onderhouden van App Service-web-apps. In onze blogbericht [web-apps die worden gehost in Azure App Service met behulp van PowerShell automatiseren](https://blogs.msdn.microsoft.com/puneetgupta/2016/03/21/automating-webapps-hosted-in-azure-app-service-through-powershell-arm-way/), er wordt beschreven hoe u op basis van Azure Resource Manager PowerShell-cmdlets gebruiken om algemene taken te automatiseren. Het blogbericht heeft ook een voorbeeld van code voor verschillende beheertaken voor web-apps. Zie voor beschrijvingen en de syntaxis voor alle cmdlets van App Service web apps [AzureRM.Websites](https://docs.microsoft.com/powershell/module/azurerm.websites/?view=azurermps-4.0.0).

## <a name="how-do-i-view-my-web-apps-event-logs"></a>Hoe kan ik mijn web-app-gebeurtenislogboeken bekijken?

Uw web-app-gebeurtenislogboeken weergeven:

1. Aanmelden bij uw [Kudu website](https://*yourwebsitename*.scm.azurewebsites.net).
2. Selecteer in het menu **Console voor foutopsporing** > **CMD**.
3. Selecteer de **logboekbestanden** map.
4. Wilt u gebeurtenislogboeken weergeven, selecteert u het potloodpictogram naast **eventlog.xml**.
5. Voer de PowerShell-cmdlet voor het downloaden van de logboeken `Save-AzureWebSiteLog -Name webappname`.

## <a name="how-do-i-capture-a-user-mode-memory-dump-of-my-web-app"></a>Hoe ik een geheugendump gebruikersmodus van mijn web-app vastleggen?

Voor het vastleggen van een geheugendump gebruikersmodus van uw web-app:

1. Aanmelden bij uw [Kudu website](https://*yourwebsitename*.scm.azurewebsites.net).
2. Selecteer de **Procesverkenner** menu.
3. Met de rechtermuisknop op de **w3wp.exe** proces of de webtaak wordt uitgevoerd.
4. Selecteer **geheugendump downloaden** > **Dump volledige**.

## <a name="how-do-i-view-process-level-info-for-my-web-app"></a>Hoe kan ik voor mijn web-app procesniveau info bekijken?

U hebt twee opties voor het weergeven van procesniveau informatie voor uw web-app:

*   In Azure Portal:
    1. Open de **Procesverkenner** voor de web-app.
    2. Voor de details, selecteer de **w3wp.exe** proces.
*   In de Kudu-console:
    1. Aanmelden bij uw [Kudu website](https://*yourwebsitename*.scm.azurewebsites.net).
    2. Selecteer de **Procesverkenner** menu.
    3. Voor de **w3wp.exe** proces, selecteer **eigenschappen**.

## <a name="when-i-browse-to-my-app-i-see-error-403---this-web-app-is-stopped-how-do-i-resolve-this"></a>Wanneer ik mijn app bladert, verschijnt "Fout 403 - deze web-app is gestopt." Hoe kan ik dit oplossen?

Drie voorwaarden kunnen leiden tot deze fout:

* De web-app een facturering limiet heeft bereikt en uw site is uitgeschakeld.
* De web-app is in de portal gestopt.
* De web-app heeft een resource quotumlimiet die mogelijk van toepassing zijn op een vrij of gedeeld serviceplan schaal bereikt.

Om te zien wat de oorzaak van de fout en los het probleem, volg de stappen in [Web-Apps: 'Fout 403 – deze web-app is gestopt'](https://blogs.msdn.microsoft.com/waws/2016/01/05/azure-web-apps-error-403-this-web-app-is-stopped/).

## <a name="where-can-i-learn-more-about-quotas-and-limits-for-various-app-service-plans"></a>Waar vind ik meer informatie over quota en limieten voor verschillende App Service-abonnementen?

Zie voor meer informatie over quota en limieten [App Service-beperkingen](../azure-subscription-service-limits.md#app-service-limits). 

## <a name="how-do-i-decrease-the-response-time-for-the-first-request-after-idle-time"></a>Hoe ik de reactietijd voor de eerste aanvraag na niet-actieve tijd verkleinen?

Standaard worden web-apps uit het geheugen verwijderd als ze inactief gedurende een bepaalde tijd zijn. Op deze manier het systeem kunt besparen. Het nadeel is dat het antwoord op de eerste aanvraag nadat de web-app verwijderd wordt langer om toe te staan van de web-app te laden en start de afhandeling van reacties. In het service-plan, Basic en Standard, kunt u op de **altijd op** instelling te houden van de app altijd geladen. Hierdoor langer laadtijden nadat de app actief is. Wijzigen van de **altijd op** instelling:

1. Ga naar uw web-app in de Azure portal.
2. Selecteer **toepassingsinstellingen**.
3. Voor **altijd op**, selecteer **op**.

## <a name="how-do-i-turn-on-failed-request-tracing"></a>Hoe kan ik tracering van mislukte aanvragen inschakelen?

Tracering van mislukte aanvragen inschakelen:

1. Ga naar uw web-app in de Azure portal.
3. Selecteer **alle instellingen** > **diagnostische logboeken**.
4. Voor **tracering van mislukte aanvragen**, selecteer **op**.
5. Selecteer **Opslaan**.
6. Selecteer op de blade web-app **extra**.
7. Selecteer **Visual Studio Online**.
8. Als de instelling niet is **op**, selecteer **op**.
9. Selecteer **gaat**.
10. Selecteer **Web.config**.
11. In system.webServer, voegt u deze configuratie (voor het vastleggen van een specifieke URL):

    ```
    <system.webServer>
    <tracing> <traceFailedRequests>
    <remove path="*api*" />
    <add path="*api*">
    <traceAreas>
    <add provider="ASP" verbosity="Verbose" />
    <add provider="ASPNET" areas="Infrastructure,Module,Page,AppServices" verbosity="Verbose" />
    <add provider="ISAPI Extension" verbosity="Verbose" />
    <add provider="WWW Server" areas="Authentication,Security,Filter,StaticFile,CGI,Compression, Cache,RequestNotifications,Module,FastCGI" verbosity="Verbose" />
    </traceAreas>
    <failureDefinitions statusCodes="200-999" />
    </add> </traceFailedRequests>
    </tracing>
    ```
12. Voor het oplossen van problemen met trage prestaties moet u deze configuratie toevoegen (als de vastleggen aanvraag langer dan 30 seconden duurt):
    ```
    <system.webServer>
    <tracing> <traceFailedRequests>
    <remove path="*" />
    <add path="*">
    <traceAreas> <add provider="ASP" verbosity="Verbose" />
    <add provider="ASPNET" areas="Infrastructure,Module,Page,AppServices" verbosity="Verbose" />
    <add provider="ISAPI Extension" verbosity="Verbose" />
    <add provider="WWW Server" areas="Authentication,Security,Filter,StaticFile,CGI,Compression, Cache,RequestNotifications,Module,FastCGI" verbosity="Verbose" />
    </traceAreas>
    <failureDefinitions timeTaken="00:00:30" statusCodes="200-999" />
    </add> </traceFailedRequests>
    </tracing>
    ```
13. Voor het downloaden van de traceringen van mislukte aanvragen de [portal](https://portal.azure.com), gaat u naar uw website.
15. Selecteer **extra** > **Kudu** > **gaat**.
18. Selecteer in het menu **Console voor foutopsporing** > **CMD**.
19. Selecteer de **logboekbestanden** map en selecteer vervolgens de map met een naam die met begint **W3SVC**.
20. Overzicht van het XML-bestand, selecteer het potloodpictogram.

## <a name="i-see-the-message-worker-process-requested-recycle-due-to-percent-memory-limit-how-do-i-address-this-issue"></a>Het bericht ziet: "Werkproces recycling aangevraagd als gevolg ' percentage ' geheugenlimiet." Hoe ik dit probleem op te lossen?

De maximaal beschikbare hoeveelheid geheugen voor een 32-bits proces (zelfs op een 64-bits besturingssysteem) is 2 GB. Het werkproces is standaard ingesteld op 32-bits in App Service (voor compatibiliteit met oudere toepassingen).

Overweeg over te schakelen op 64-bits processen, zodat u van extra geheugen in uw Web-werkrol profiteren kunt. Dit activeert een web-app opnieuw starten, dus plannen dienovereenkomstig.

Let ook op een 64-bits-omgeving hebt u een Basic- of Standard service-abonnement nodig. Gratis en plannen van gedeelde altijd uitgevoerd in een 32-bits-omgeving.

Zie voor meer informatie [configureren van web-apps in App Service](web-sites-configure.md).

## <a name="why-does-my-request-time-out-after-240-seconds"></a>Waarom wordt mijn time-out aanvraag na 240 seconden?

Azure Load Balancer heeft een time-out voor inactiviteit standaardinstelling van vier minuten. Doorgaans is dit een redelijke antwoord tijdslimiet voor een webaanvraag. Als uw web-app achtergrondverwerking vereist, wordt u aangeraden Azure WebJobs. De Azure-web-app kunt aanroepen WebJobs en een melding krijgen wanneer achtergrondverwerking is voltooid. U kunt kiezen uit meerdere methoden voor het gebruik van WebJobs, met inbegrip van wachtrijen en triggers.

WebJobs is ontworpen voor de achtergrond wordt verwerkt. U kunt doen zoveel achtergrond verwerkt als u in een webtaak wilt. Zie voor meer informatie over WebJobs [achtergrondtaken uitvoeren met WebJobs](web-sites-create-web-jobs.md).

## <a name="aspnet-core-applications-that-are-hosted-in-app-service-sometimes-stop-responding-how-do-i-fix-this-issue"></a>ASP.NET Core toepassingen die worden gehost in App Service soms reageert. Hoe kan ik dit probleem oplossen?

Een bekend probleem met een eerdere [Kestrel versie](https://github.com/aspnet/KestrelHttpServer/issues/1182) kan ertoe leiden dat een ASP.NET Core 1.0-app die wordt gehost in App Service wel en soms niet reageert. Ook ziet u dit bericht: 'de opgegeven CGI-toepassing is een fout opgetreden en het proces is beëindigd.'

Dit probleem is opgelost in Kestrel versie 1.0.2. Deze versie is opgenomen in de update van ASP.NET Core 1.0.3. U lost dit probleem, moet dat u uw app-afhankelijkheden voor het gebruik van Kestrel 1.0.2 bijwerken. U kunt ook kunt u een van twee oplossingen die worden beschreven in de blogbericht [trage prestaties van ASP.NET Core 1.0 problemen in App Service WebApps](https://blogs.msdn.microsoft.com/waws/2016/12/11/asp-net-core-slow-perf-issues-on-azure-websites).


## <a name="i-cant-find-my-log-files-in-the-file-structure-of-my-web-app-how-can-i-find-them"></a>Ik kan mijn logboekbestanden in de structuur van mijn web-app niet vinden. Hoe kan ik ze vinden?

Als u de lokale cachefunctie van App Service gebruikt, geldt de mappenstructuur van de logboekbestanden en de gegevens mappen voor uw App Service-exemplaar. Wanneer lokale Cache wordt gebruikt, worden de submappen gemaakt in de opslag logboekbestanden en de gegevensmappen. De submappen gebruiken de naamgevingsconventie patroon 'unieke id' + tijdstempel. Elke submap komt overeen met een VM-exemplaar waarin de web-app wordt uitgevoerd of is uitgevoerd.

Controleer uw App Service om te bepalen of u van lokale Cache gebruikmaakt, **toepassingsinstellingen** tabblad. Als de lokale Cache wordt gebruikt, de app instellen `WEBSITE_LOCAL_CACHE_OPTION` is ingesteld op `Always`.

Als u geen van lokale Cache gebruikmaakt en dit probleem zich voordoet, indienen ondersteuning aan te vragen.

## <a name="i-see-the-message-an-attempt-was-made-to-access-a-socket-in-a-way-forbidden-by-its-access-permissions-how-do-i-resolve-this"></a>Het bericht ziet: "Er is een poging gedaan te krijgen tot een socket op een manier is niet toegestaan volgens de toegangsmachtigingen." Hoe kan ik dit oplossen?

Deze fout treedt meestal op als de uitgaande TCP-verbindingen op de VM-exemplaar zijn uitgeput. In App Service gelden limieten voor het maximum aantal uitgaande verbindingen die kunnen worden gemaakt voor elke VM-instantie. Zie voor meer informatie [Cross-VM numerieke limieten](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#cross-vm-numerical-limits).

Deze fout kan ook optreden als u probeert te krijgen tot een lokaal adres van uw toepassing. Zie voor meer informatie [aanvragen van lokaal adres](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#local-address-requests).

Zie voor meer informatie over uitgaande verbindingen in uw web-app, het blogbericht over [uitgaande verbindingen naar Azure websites](http://www.freekpaans.nl/2015/08/starving-outgoing-connections-on-windows-azure-web-sites/).

## <a name="how-do-i-use-visual-studio-to-remote-debug-my-app-service-web-app"></a>Hoe gebruik ik Visual Studio op afstand fouten opsporen in mijn App Service-web-app?

Zie voor een gedetailleerd overzicht waarin u fouten opsporen in uw web-app met behulp van Visual Studio, [afstand fouten opsporen in uw App Service-web-app](https://blogs.msdn.microsoft.com/benjaminperkins/2016/09/22/remote-debug-your-azure-app-service-web-app/).
