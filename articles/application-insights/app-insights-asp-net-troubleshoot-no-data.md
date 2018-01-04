---
title: Problemen met ontbrekende gegevens oplossen - Application Insights voor .NET
description: Gegevens in Azure Application Insights niet zien? Probeer hier.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: e231569f-1b38-48f8-a744-6329f41d91d3
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: 951a3217d795df6360cd3cfa2d47db08c11f978e
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Problemen met ontbrekende gegevens oplossen - Application Insights voor .NET
## <a name="some-of-my-telemetry-is-missing"></a>Sommige van mijn telemetrie ontbreekt
*In Application Insights Zie ik slechts een deel van de gebeurtenissen die worden gegenereerd door de app.*

* Als u de dezelfde fractie consistent ziet, is dit waarschijnlijk vanwege adaptieve [steekproeven](app-insights-sampling.md). U kunt dit controleren zoeken openen (van de overzichtsblade) en zoek naar een exemplaar van een aanvraag of een andere gebeurtenis. Klik op '...' voor volledige informatie aan de onderkant van de sectie met eigenschappen. Als aanvragen Count > 1 en vervolgens steekproeven wordt uitgevoerd. 
* Anders is het mogelijk dat u bent kunt u door een [limiet](app-insights-pricing.md#limits-summary) voor uw prijscategorie. Deze beperkingen worden per minuut toegepast.

## <a name="no-data-from-my-server"></a>Er zijn geen gegevens van mijn server
*Ik mijn app hebt geïnstalleerd op de webserver en nu alle telemetrie van het niet wordt weergegeven. Deze OK op mijn computer Developer gewerkt.*

* Waarschijnlijk een firewallprobleem met. [Instellen van firewalluitzonderingen voor Application Insights om gegevens te verzenden](app-insights-ip-addresses.md).
* IIS-Server mogelijk ontbreken sommige vereiste onderdelen: .NET-Extensibility 4.5 en ASP.NET 4.5.

*Ik [geïnstalleerd Status Monitor](app-insights-monitor-performance-live-website-now.md) op de webserver voor het bewaken van bestaande apps. Geen resultaten wordt niet weergegeven.*

* Zie [probleemoplossing statuscontrole](app-insights-monitor-performance-live-website-now.md#troubleshooting-runtime-configuration-of-application-insights). 

## <a name="q01"></a>Geen optie 'Application Insights toevoegen' in Visual Studio
*Wanneer ik een bestaand project in Solution Explorer met de rechtermuisknop, wordt geen Application Insights-opties niet weergegeven.*

* Niet alle soorten .NET project worden ondersteund door de hulpprogramma's. Web- en WCF-projecten worden ondersteund. U kunt ook voor andere projecttypen zoals toepassingen voor desktop- of [handmatig een Application Insights-SDK toevoegen aan uw project](app-insights-windows-desktop.md).
* Zorg ervoor dat u hebt [Visual Studio 2013 Update 3 of hoger](http://go.microsoft.com/fwlink/?LinkId=397827). Het is voorgeïnstalleerd met ontwikkelaars hulpprogramma's voor webanalyse, waarmee de Application Insights-SDK.
* Selecteer **extra**, **uitbreidingen en Updates** en controleert u of **Analytics hulpprogramma's voor ontwikkelaars** is geïnstalleerd en ingeschakeld. Als dit het geval is, klikt u op **Updates** om te zien of er een update beschikbaar is.
* Open het dialoogvenster New Project en kiest u ASP.NET-webtoepassing. Als u de optie van de Application Insights ziet, worden de hulpprogramma's geïnstalleerd. Als dat niet het geval is, probeer te verwijderen en vervolgens opnieuw de Application Insights Tools te installeren.

## <a name="q02"></a>Application Insights toevoegen is mislukt
*Wanneer ik probeer Application Insights toevoegen aan een bestaand project, er een foutbericht weergegeven.*

Mogelijke oorzaken:

* Communicatie met de Application Insights-portal is mislukt; of
* Er is een probleem met uw Azure-account;
* U hoeft alleen [leestoegang tot het abonnement of de groep waar u probeert te maken van de nieuwe resource](app-insights-resources-roles-access-control.md).

FIX:

* Controleer of u aanmeldingsreferenties voor het recht Azure-account opgegeven. 
* Controleer in uw browser dat u toegang tot hebben de [Azure-portal](https://portal.azure.com). Open instellingen en zien of er beperking.
* [Application Insights toevoegen aan een bestaand project](app-insights-asp-net.md): In Solution Explorer, klik met de rechtermuisknop op uw project en kies 'Application Insights toevoegen'.
* Als deze nog steeds niet werkt, voert u de [handmatige procedure](app-insights-windows-services.md) een resource toevoegt in de portal en vervolgens de SDK toevoegen aan uw project. 

## <a name="emptykey"></a>Een fout ophalen 'instrumentatiesleutel mag niet leeg zijn'
Lijkt erop dat is iets verkeerd gegaan tijdens de installatie Application Insights of mogelijk een adapter logboekregistratie.

Klik in Solution Explorer met de rechtermuisknop op uw project en kies **Application Insights > Application Insights configureren**. U krijgt een dialoogvenster met een uitnodiging aan te melden bij Azure en maak een Application Insights-resource opnieuw of gebruik een bestaande.

## <a name="NuGetBuild"></a>"NuGet-pakketten ontbreken' op mijn buildserver
*Alles bouwt OK wanneer ik ben foutopsporing op de ontwikkelcomputer, maar er een NuGet-fout op de buildserver verschijnt.*

Zie [NuGet-pakket herstellen](http://docs.nuget.org/Consume/Package-Restore) en [automatische pakket herstellen](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Ontbrekende menuopdracht Application Insights openen vanuit Visual Studio
*Wanneer ik mijn project Solution Explorer met de rechtermuisknop, zie ik geen Application Insights-opdrachten of een Open Application Insights-opdracht niet weergegeven.*

Mogelijke oorzaken:

* Als u de Application Insights-resource die handmatig zijn gemaakt of als het project is van een type dat niet wordt ondersteund door de Application Insights-hulpprogramma's.
* De ontwikkelaar Analytics-hulpprogramma's zijn uitgeschakeld in uw Visual Studio. 
* Uw Visual Studio is ouder dan 2013 Update 3.

FIX:

* Zorg ervoor dat uw versie van Visual Studio 2013 update 3 of hoger.
* Selecteer **extra**, **uitbreidingen en Updates** en controleert u of **hulpprogramma's voor ontwikkelaars webanalyse** is geïnstalleerd en ingeschakeld. Als dit het geval is, klikt u op **Updates** om te zien of er een update beschikbaar is.
* Met de rechtermuisknop op het project in Solution Explorer. Als u de opdracht ziet **Application Insights > Application Insights configureren**, verbinding maken met uw project de resource in de Application Insights-service gebruiken.

Anders dat het projecttype van uw direct wordt niet ondersteund door de Application Insights-hulpprogramma's. Als u wilt uw telemetrie weergeven, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com), Application Insights kiezen op de navigatiebalk en selecteert u uw toepassing.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>'Toegang geweigerd' Application Insights wordt geopend vanuit Visual Studio
*De menuopdracht 'Open Application Insights', ga ik aan de Azure portal, maar er verschijnt een 'toegang geweigerd'-fout.*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

De aanmeldingspagina voor Microsoft die het laatst op de standaardbrowser gebruikte heeft geen toegang tot [de resource die is gemaakt bij de Application Insights is toegevoegd aan deze app](app-insights-asp-net.md). Er zijn twee waarschijnlijke oorzaken: 

* Hebt u meer dan een Microsoft-account - mogelijk een werk- en een persoonlijk Microsoft-account? De aanmelding die het laatst op de standaardbrowser gebruikte is voor een ander account dan degene die toegang tot heeft [Application Insights toevoegen aan het project](app-insights-asp-net.md). 
  
  * FIX: Klik op de naam van uw op bovenste rechts van het browservenster en meld u af. Meld u aan met van het account dat toegang heeft. Klik op de linkernavigatiebalk op Application Insights en selecteer van uw app.
* Iemand anders Application Insights hebt toegevoegd aan het project en ze vergeten om u te bieden [toegang tot de resourcegroep](app-insights-resources-roles-access-control.md) waarin deze is gemaakt. 
  
  * Oplossing: Als ze een organisatie-account gebruikt, ze kunnen u toevoegen aan het team; of ze kunnen u afzonderlijke toegang verlenen aan de resourcegroep.

## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>'Asset niet gevonden' Application Insights wordt geopend vanuit Visual Studio
*De menuopdracht 'Open Application Insights', ga ik aan de Azure portal, maar ik krijg een foutbericht 'asset niet gevonden'.*

Mogelijke oorzaken:

* De Application Insights-resource voor de toepassing is verwijderd; of
* De instrumentatiesleutel is ingesteld of gewijzigd in ApplicationInsights.config door direct bewerken zonder het projectbestand bijwerken. 

De instrumentatiesleutel in ApplicationInsights.config besturingselementen waarbij de telemetrie wordt verzonden. Een regel in het projectbestand bepaalt welke resource wordt geopend wanneer u de opdracht in Visual Studio gebruiken. 

FIX:

* Klik in Solution Explorer met de rechtermuisknop op het project en kies Application Insights, Application Insights configureren. U kunt in het dialoogvenster ofwel verzenden van telemetrie naar een bestaande resource of een nieuwe maken. Of:
* Open de resource direct. Aanmelden bij [de Azure-portal](https://portal.azure.com), klik met de Application Insights op de linkernavigatiebalk en selecteer vervolgens uw app.

## <a name="where-do-i-find-my-telemetry"></a>Waar vind ik mijn telemetrie?
*Ik aangemeld bij de [Microsoft Azure-portal](https://portal.azure.com), en ik zoek op de Azure Startdashboard. Dus waar vind ik mijn gegevens Application Insights?*

* Klik op de linkernavigatiebalk Application Insights en vervolgens de appnaam van uw. Als u geen projecten er hebt, moet u [toevoegen of configureren van Application Insights in uw webproject](app-insights-asp-net.md).
  
    U ziet er sommige samenvatting grafieken. U kunt klikken via ze meer details.
* Wanneer u fouten uw app opspoort, klikt u in Visual Studio de knop Application Insights.

## <a name="q03"></a>Er is geen server-gegevens (of geen gegevens op alle)
*Ik heb mijn app wordt uitgevoerd en de Application Insights-service vervolgens opent in Microsoft Azure, maar alle grafieken weergeven 'Meer informatie over het verzamelen van...' of 'Niet geconfigureerd'.* Of, *alleen paginaweergave-en gebruikersgegevens, maar er zijn geen servergegevens.*

* Voer uw toepassing in de foutopsporingsmodus in Visual Studio (F5). Gebruik de toepassing om telemetrie te genereren. Controleer of u de gebeurtenissen die zijn vastgelegd in het uitvoervenster Visual Studio kunt zien. 
  
    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)
* Open in de Application Insights-portal [diagnostische gegevens doorzoeken](app-insights-diagnostic-search.md). Gegevens weergegeven meestal hier eerst.
* Klik op de knop vernieuwen. De blade automatisch wordt vernieuwd periodiek, maar u kunt ook dit handmatig doen. Het interval voor vernieuwen is verlengd voor grotere tijdsbereik.
* Controleer dat de instrumentatiesleutels overeenkomen. Op de hoofdblade voor uw app in de Application Insights-portal in de **Essentials** vervolgkeuzelijst, kijkt u naar **instrumentatiesleutel**. In het project in Visual Studio, open vervolgens ApplicationInsights.config en zoek de `<instrumentationkey>`. Controleer of de twee sleutels gelijk zijn. Als dat niet het:
  
  * In de portal klikt u op de Application Insights en zoekt u naar de app-resource met de juiste sleutel; of
  * In Visual Studio Solution Explorer met de rechtermuisknop op het project en kies Application Insights configureren. De app voor het verzenden van telemetrie naar de juiste bron instellen.
  * Als u de overeenkomende sleutels niet kunt vinden, controleer dan of u dezelfde referenties aanmelden in Visual Studio als in de portal.
    
    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
* In de [Microsoft Azure-Startdashboard](https://portal.azure.com), bekijk de servicestatus-kaart. Als er een waarschuwing aanwijzingen zijn, wacht u totdat ze zijn geretourneerd op OK en klik vervolgens sluiten en opnieuw de blade van het Application Insights-toepassing openen.
* Controleer ook [ons blog status](http://blogs.msdn.com/b/applicationinsights-status/).
* Hebt u een code te schrijven voor de [serverzijde SDK](app-insights-api-custom-events-metrics.md) die mogelijk de instrumentatiesleutel in wijzigen `TelemetryClient` exemplaren of in `TelemetryContext`? Of hebt u schrijft een [filter of steekproeven configuratie](app-insights-api-filtering-sampling.md) die het filter worden mogelijk te veel?
* Als u ApplicationInsights.config hebt bewerkt, Controleer zorgvuldig de configuratie van [TelemetryInitializers en TelemetryProcessors](app-insights-api-filtering-sampling.md). Een verkeerde naam type of de parameter kan leiden tot de SDK geen gegevens worden verzonden.

## <a name="q04"></a>Er zijn geen gegevens over het gebruik van de Browsers, paginaweergaven
*Gegevens in serverreactietijd en serveraanvragen grafieken, maar er zijn geen gegevens in de weergave paginalaadtijd of weergegeven in de Browser of gebruik blades.*

De gegevens afkomstig van scripts in de webpagina's. 

* Als u Application Insights hebt toegevoegd aan een bestaand webproject [u moet de scripts handmatig toevoegen](app-insights-javascript.md).
* Controleer of dat Internet Explorer uw site wordt niet weergegeven in de compatibiliteitsmodus.
* De browser foutopsporing functie (F12 op sommige browsers, kies netwerk) gebruiken om te controleren dat gegevens worden verzonden naar `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Er zijn geen gegevens afhankelijkheid of uitzondering
Zie [afhankelijkheidstelemetrie](app-insights-asp-net-dependencies.md) en [uitzonderingstelemetrie](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>Er is geen prestatiegegevens
Prestatiegegevens (CPU, i/o-snelheid, enzovoort) beschikbaar is voor [Java-web-services](app-insights-java-collectd.md), [Windows desktop-apps](app-insights-windows-desktop.md), [IIS web-apps en services als u de statusmonitor installeren](app-insights-monitor-performance-live-website-now.md), en [Azure-Cloudservices](app-insights-azure.md). u vindt deze onder instellingen voor Servers.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>Er zijn geen gegevens (server) omdat ik de app gepubliceerd naar Mijn server
* Controleer of u daadwerkelijk alle Microsoft gekopieerd. ApplicationInsights dll-bestanden naar de server, samen met Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll
* In de firewall u wellicht [enkele TCP-poorten](app-insights-ip-addresses.md).
* Als u een proxyserver gebruikt voor het verzenden van buiten uw bedrijfsnetwerk, stelt u [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) in Web.config
* WindowsServer 2008: Zorg ervoor dat u de volgende updates zijn geïnstalleerd: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).

## <a name="i-used-to-see-data-but-it-has-stopped"></a>Ik gebruikt om gegevens te bekijken, maar deze is gestopt
* Controleer de [status blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Hebt u uw maandelijkse quotum van gegevenspunten bereikt? Open de instellingen/quotum en prijzen om erachter te komen. Als dit het geval is, kunt u uw abonnement upgraden of betalen voor extra capaciteit. Zie de [prijzen schema](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="i-dont-see-all-the-data-im-expecting"></a>Ik zie niet alle gegevens die ik ben verwacht
Als uw toepassing grote hoeveelheden gegevens verzendt en u de Application Insights-SDK voor ASP.NET-versie 2.0.0-beta3 of hoger, gebruikt de [adaptieve steekproeven](app-insights-sampling.md) functie kan werken en slechts een percentage van uw telemetrie verzenden. 

U kunt deze uitschakelen, maar dit wordt niet aanbevolen. Steekproeven is zodanig ontworpen dat gerelateerde telemetrie correct wordt verzonden, voor diagnostische doeleinden. 

## <a name="wrong-geographical-data-in-user-telemetry"></a>Onjuiste geografische gegevens in de telemetrie van de gebruiker
De regio, plaats en land-dimensies zijn afgeleid van IP-adressen en niet altijd nauwkeurig.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Uitzondering Methode niet gevonden bij het uitvoeren van uw app in Azure Cloud Services
Hebt u uw app ontwikkeld voor .NET 4.6? 4.6 wordt niet automatisch ondersteund in Azure Cloud Services-rollen. [Installeer 4.6 voor elke rol](../cloud-services/cloud-services-dotnet-install-dotnet.md) voordat u uw app uitvoert.

## <a name="still-not-working"></a>Nog steeds werkt niet...
* [Application Insights-forum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

