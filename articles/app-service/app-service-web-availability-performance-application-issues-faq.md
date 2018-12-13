---
title: Toepassingsprestaties Veelgestelde vragen - Azure App Service | Microsoft Docs
description: Krijg antwoorden op veelgestelde vragen over beschikbaarheid, prestaties en toepassingsproblemen in de functie Web Apps van Azure App Service.
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.custom: seodec18
ms.openlocfilehash: 62039d379861e9981eb1bf556a78e85fc64e2478
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53255190"
---
# <a name="application-performance-faqs-for-web-apps-in-azure"></a>Toepassingsprestaties Veelgestelde vragen voor Web-Apps in Azure

In dit artikel vindt u antwoorden op veelgestelde vragen over toepassing prestatieproblemen voor de [functie van Azure App Service Web Apps](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-is-my-app-slow"></a>Waarom is mijn app is langzaam?

Langzame prestaties van de app mogelijk meerdere factoren bijdragen. Zie voor gedetailleerde stappen voor probleemoplossing, [prestaties voor problemen met trage web-apps](app-service-web-troubleshoot-performance-degradation.md).

## <a name="how-do-i-troubleshoot-a-high-cpu-consumption-scenario"></a>Hoe kan ik een hoog CPU-verbruik scenario oplossen?

Uw app kunnen in sommige hoog CPU-verbruik-scenario's echt vereist meer bronnen. In dat geval kunt u overwegen naar een hogere servicelaag te schalen, zodat de toepassing krijgt alle resources die nodig is. In andere gevallen kan hoog CPU-verbruik worden veroorzaakt door een ongeldige lus of door code uit veiligheidsoverwegingen. Om inzicht te krijgen in wat hogere CPU-verbruik Triggert is een tweedelige-proces. Maak eerst een dump verwerken, en vervolgens analyseren de Procesdump. Zie voor meer informatie, [vastleggen en analyseren van een dumpbestand voor hoge CPU-verbruik voor Web-Apps](https://blogs.msdn.microsoft.com/asiatech/2016/01/20/how-to-capture-dump-when-intermittent-high-cpu-happens-on-azure-web-app/).

## <a name="how-do-i-troubleshoot-a-high-memory-consumption-scenario"></a>Hoe kan ik een hoog geheugenverbruik scenario oplossen?

Uw app kunnen in bepaalde situaties hoog geheugenverbruik, echt vereist meer bronnen. In dat geval kunt u overwegen naar een hogere servicelaag te schalen, zodat de toepassing krijgt alle resources die nodig is. In andere gevallen een fout in de code kan leiden tot een geheugenlek. Uit veiligheidsoverwegingen codering mogelijk ook geheugenverbruik verhogen. Om inzicht te krijgen in wat veel geheugen verbruik is een tweedelige-proces wordt geactiveerd. Maak eerst een dump verwerken, en vervolgens analyseren de Procesdump. Crash Diagnoser uit de galerie met Azure Site-extensie kunt efficiënt beide stappen uitvoeren. Zie voor meer informatie, [vastleggen en analyseren van een dumpbestand voor onregelmatige hoge geheugen voor Web-Apps](https://blogs.msdn.microsoft.com/asiatech/2016/02/02/how-to-capture-and-analyze-dump-for-intermittent-high-memory-on-azure-web-app/).

## <a name="how-do-i-automate-app-service-web-apps-by-using-powershell"></a>Hoe Automatiseer ik App Service WebApps met behulp van PowerShell?

U kunt PowerShell-cmdlets gebruiken om te beheren en onderhouden van App Service Webapps. In onze blogpost [automatiseren webtoepassingen die worden gehost in Azure App Service met behulp van PowerShell](https://blogs.msdn.microsoft.com/puneetgupta/2016/03/21/automating-webapps-hosted-in-azure-app-service-through-powershell-arm-way/), wordt beschreven hoe u op basis van een Azure Resource Manager PowerShell-cmdlets gebruiken om algemene taken te automatiseren. Het blogbericht: heeft ook voorbeeldcode voor verschillende beheertaken voor web-apps. Zie voor beschrijvingen en syntaxis voor alle App Service web apps-cmdlets voor [azurerm.websites gebruikt](https://docs.microsoft.com/powershell/module/azurerm.websites/?view=azurermps-4.0.0).

## <a name="how-do-i-view-my-web-apps-event-logs"></a>Hoe kan ik mijn web-app-gebeurtenislogboeken bekijken?

Logboeken voor uw web-app weergeven:

1. Aanmelden bij uw [Kudu website](https://*yourwebsitename*.scm.azurewebsites.net).
2. Selecteer in het menu **Foutopsporingsconsole** > **CMD**.
3. Selecteer de **logboekbestanden** map.
4. U wilt weergeven van gebeurtenislogboeken, selecteer het potloodpictogram naast **eventlog.xml**.
5. Voor het downloaden van de logboeken, voert u de PowerShell-cmdlet `Save-AzureWebSiteLog -Name webappname`.

## <a name="how-do-i-capture-a-user-mode-memory-dump-of-my-web-app"></a>Hoe ik een geheugendump gebruiker-modus van mijn web-app opnemen?

Om vast te leggen een geheugendump gebruiker-modus van uw web-app:

1. Aanmelden bij uw [Kudu website](https://*yourwebsitename*.scm.azurewebsites.net).
2. Selecteer de **Procesverkenner** menu.
3. Met de rechtermuisknop op de **w3wp.exe** proces of de webtaak wordt uitgevoerd.
4. Selecteer **geheugendump downloaden** > **volledige Dump**.

## <a name="how-do-i-view-process-level-info-for-my-web-app"></a>Hoe geef ik procesniveau informatie weer voor mijn web-app?

U hebt twee opties voor het weergeven van procesniveau informatie voor uw web-app:

*   In Azure Portal:
    1. Open de **Procesverkenner** voor de web-app.
    2. De details, selecteer de **w3wp.exe** proces.
*   In de Kudu-console:
    1. Aanmelden bij uw [Kudu website](https://*yourwebsitename*.scm.azurewebsites.net).
    2. Selecteer de **Procesverkenner** menu.
    3. Voor de **w3wp.exe** proces, selecteer **eigenschappen**.

## <a name="when-i-browse-to-my-app-i-see-error-403---this-web-app-is-stopped-how-do-i-resolve-this"></a>Wanneer ik naar Mijn app bladert, zie ik "Fout 403 - deze web-app is gestopt." Hoe kan ik dit oplossen?

Drie voorwaarden kunnen leiden tot deze fout:

* De web-app heeft een factureringslimiet bereikt en uw site is uitgeschakeld.
* De web-app is gestopt in de portal.
* De web-app heeft een limiet voor het resourcequotum die mogelijk van toepassing zijn op een gratis of gedeeld schaal service-plan bereikt.

Om te zien wat de oorzaak is van de fout en het probleem kunt oplossen, volg de stappen in [Web-Apps: 'Fout 403-deze web-app is gestopt'](https://blogs.msdn.microsoft.com/waws/2016/01/05/azure-web-apps-error-403-this-web-app-is-stopped/).

## <a name="where-can-i-learn-more-about-quotas-and-limits-for-various-app-service-plans"></a>Waar vind ik meer informatie over quota en limieten voor verschillende App Service-plannen?

Zie voor meer informatie over quota en limieten [limieten voor App Service](../azure-subscription-service-limits.md#app-service-limits). 

## <a name="how-do-i-decrease-the-response-time-for-the-first-request-after-idle-time"></a>Hoe ik de reactietijd voor de eerste aanvraag na niet-actieve tijd verlagen?

Standaard worden web-apps uit het geheugen verwijderd als ze gedurende een bepaalde tijd niet actief zijn. Op deze manier het systeem kunt besparen. Het nadeel is dat het antwoord op de eerste aanvraag nadat de web-app verwijderd wordt langer om de web-app te laden en start de afhandeling van reacties. In de serviceabonnementen basis en standaard, kunt u inschakelen de **Always On** instelling in om te voorkomen dat de app altijd geladen. Hierdoor is er meer laadtijden nadat de app actief is. Wijzigen van de **Always On** instelling:

1. In de Azure-portal, gaat u naar uw web-app.
2. Selecteer **toepassingsinstellingen**.
3. Voor **Always On**, selecteer **op**.

## <a name="how-do-i-turn-on-failed-request-tracing"></a>Hoe kan ik tracering van mislukte aanvragen inschakelen?

Tracering van mislukte aanvragen inschakelen:

1. In de Azure-portal, gaat u naar uw web-app.
3. Selecteer **alle instellingen** > **diagnoselogboeken**.
4. Voor **tracering van mislukte aanvragen**, selecteer **op**.
5. Selecteer **Opslaan**.
6. Selecteer op de blade web-app **extra**.
7. Selecteer **Visual Studio Online**.
8. Als de instelling niet is **op**, selecteer **op**.
9. Selecteer **gaat**.
10. Selecteer **Web.config**.
11. System.webServer, voeg toe aan deze configuratie (voor het vastleggen van een specifieke URL):

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
12. Voor het oplossen van problemen met trage prestaties, moet u deze configuratie toevoegen (als de aanvraag voor vastgelegde langer dan 30 seconden duurt):
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
13. Voor het downloaden van de traceringen van mislukte aanvragen, in de [portal](https://portal.azure.com), gaat u naar uw website.
15. Selecteer **extra** > **Kudu** > **gaat**.
18. Selecteer in het menu **Foutopsporingsconsole** > **CMD**.
19. Selecteer de **logboekbestanden** map en selecteer vervolgens de map met de naam die met begint **W3SVC**.
20. Als u wilt zien van het XML-bestand, selecteer het potloodpictogram.

## <a name="i-see-the-message-worker-process-requested-recycle-due-to-percent-memory-limit-how-do-i-address-this-issue"></a>Het bericht ziet: 'Werkproces aangevraagd gerecycled vanwege een limiet voor 'Percentage geheugen'.' Hoe ik dit probleem oplossen?

De maximaal beschikbare hoeveelheid geheugen voor een 32-bits proces (zelfs op een 64-bits besturingssysteem) is 2 GB. Het werkproces is standaard ingesteld op 32-bits in App Service (voor compatibiliteit met oudere web-apps).

Overweeg over te schakelen op 64-bits processen, zodat u van de extra geheugen die beschikbaar zijn in uw Web-werkrol profiteren kunt. Dit activeert een web-app opnieuw opstarten, dus dienovereenkomstig plannen.

Houd er ook rekening mee dat een 64-bits-omgeving is een Basic- of Standard-serviceplan vereist. Gratis en gedeelde abonnementen worden altijd uitgevoerd in een 32-bits-omgeving.

Zie voor meer informatie, [web-apps in App Service configureren](web-sites-configure.md).

## <a name="why-does-my-request-time-out-after-230-seconds"></a>Waarom wordt mijn time-out aanvraag na 230 seconden?

Azure Load Balancer heeft een standaardinstelling voor de time-out voor inactiviteit van vier minuten. Dit is doorgaans een redelijke antwoord tijdslimiet voor een webaanvraag. Als uw web-app is vereist voor verwerking op de achtergrond, raden wij met behulp van Azure WebJobs. De Azure-web-app WebJobs kunt aanroepen en een melding wanneer de verwerking op de achtergrond is voltooid. U kunt kiezen uit meerdere methoden voor het gebruik van WebJobs, met inbegrip van wachtrijen en -triggers.

WebJobs is ontworpen voor verwerking op de achtergrond. U kunt doen zoveel achtergrond verwerken als u in een webtaak wilt. Zie voor meer informatie over WebJobs [achtergrondtaken uitvoeren met WebJobs](web-sites-create-web-jobs.md).

## <a name="aspnet-core-applications-that-are-hosted-in-app-service-sometimes-stop-responding-how-do-i-fix-this-issue"></a>ASP.NET Core-toepassingen die worden gehost in App Service soms reageert niet. Hoe kan ik dit probleem oplossen?

Een bekend probleem met een eerdere [Kestrel versie](https://github.com/aspnet/KestrelHttpServer/issues/1182) kan leiden tot een ASP.NET Core 1.0-app die wordt gehost in App Service tijdelijk niet meer reageert. U kunt ook dit bericht ziet: "De opgegeven CGI-toepassing is een fout opgetreden en de server het proces beëindigd."

Dit probleem is opgelost in versie 1.0.2 Kestrel. Deze versie is opgenomen in de ASP.NET Core 1.0.3-update. U lost dit probleem, moet dat u uw app-afhankelijkheden voor het gebruik van Kestrel 1.0.2 bijwerken. U kunt ook kunt u een van de twee oplossingen die worden beschreven in het blogbericht [problemen met trage prestaties van ASP.NET Core 1.0 in App Service WebApps](https://blogs.msdn.microsoft.com/waws/2016/12/11/asp-net-core-slow-perf-issues-on-azure-websites).


## <a name="i-cant-find-my-log-files-in-the-file-structure-of-my-web-app-how-can-i-find-them"></a>Ik kan mijn logboekbestanden vinden in de structuur van het bestand van mijn web-app. Hoe kan ik ze vinden?

Als u de lokale Cache-functie van App Service gebruikt, wordt de mappenstructuur van de mappen logboekbestanden en de gegevens voor uw App Service-exemplaar worden beïnvloed. Wanneer de lokale Cache wordt gebruikt, worden in de mappen van de gegevens en opslag logboekbestanden submappen gemaakt. De submappen gebruiken de naamgevingsconventie patroon 'de unieke id"+ tijdstempel. Elke submap komt overeen met een VM-instantie waarin de web-app wordt uitgevoerd of is uitgevoerd.

Om te bepalen of u lokale Cache gebruiken, Controleer uw App Service **toepassingsinstellingen** tabblad. Als de lokale Cache wordt gebruikt, de app-instelling `WEBSITE_LOCAL_CACHE_OPTION` is ingesteld op `Always`.

Als u geen van lokale Cache gebruikmaakt en dit probleem ondervindt, kunt u een ondersteuningsaanvraag indienen.

## <a name="i-see-the-message-an-attempt-was-made-to-access-a-socket-in-a-way-forbidden-by-its-access-permissions-how-do-i-resolve-this"></a>Het bericht ziet: "Er is een poging gedaan te krijgen tot een socket op een manier die is niet toegestaan volgens de toegangsmachtigingen." Hoe kan ik dit oplossen?

Deze fout treedt meestal op als de uitgaande TCP-verbindingen op het VM-exemplaar worden verbruikt. In App Service, limieten gelden voor het maximum aantal uitgaande verbindingen die kunnen worden gemaakt voor elk VM-exemplaar. Zie voor meer informatie, [numerieke Cross-VM-limieten](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#cross-vm-numerical-limits).

Deze fout kan ook optreden als u probeert te krijgen tot een lokaal adres van uw toepassing. Zie voor meer informatie, [aanvragen lokaal adres](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#local-address-requests).

Zie voor meer informatie over uitgaande verbindingen in uw web-app, het blogbericht over [uitgaande verbindingen naar Azure websites](http://www.freekpaans.nl/2015/08/starving-outgoing-connections-on-windows-azure-web-sites/).

## <a name="how-do-i-use-visual-studio-to-remote-debug-my-app-service-web-app"></a>Hoe gebruik ik Visual Studio op afstand fouten opsporen in mijn App Service-web-app?

Zie voor een gedetailleerd overzicht waarin wordt uitgelegd hoe u fouten opsporen in uw web-app met behulp van Visual Studio, [extern fouten opsporen in uw App Service-web-app](https://blogs.msdn.microsoft.com/benjaminperkins/2016/09/22/remote-debug-your-azure-app-service-web-app/).
