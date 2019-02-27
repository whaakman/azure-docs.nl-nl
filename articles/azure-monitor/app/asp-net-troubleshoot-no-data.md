---
title: Problemen met ontbrekende gegevens oplossen - Application Insights voor .NET
description: Niet al gegevens in Azure Application Insights? Probeer het hier.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: e231569f-1b38-48f8-a744-6329f41d91d3
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/23/2018
ms.author: mbullwin
ms.openlocfilehash: 532121f4e9a3c658e544a8d322ac0f17fa22a2b0
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56875902"
---
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Problemen met ontbrekende gegevens oplossen - Application Insights voor .NET
## <a name="some-of-my-telemetry-is-missing"></a>Aantal van mijn telemetrie ontbreekt
*In Application Insights Zie ik slechts een fractie van de gebeurtenissen die worden gegenereerd door mijn app.*

* Als u het gedeelte van het dezelfde consistent ziet, is het waarschijnlijk vanwege adaptieve [steekproeven](../../azure-monitor/app/sampling.md). U kunt dit controleren zoeken (op de overzichtsblade) openen en zoek naar een exemplaar van een aanvraag of een andere gebeurtenis. Aan de onderkant van de sectie met eigenschappen klikt u op '...' voor volledige informatie. Als aantal > 1 aanvragen en vervolgens steekproeven worden uitgevoerd. 
* Anders is het mogelijk dat u ondervindt een [limiet](../../azure-monitor/app/pricing.md#limits-summary) voor uw prijsplan. Deze limieten gelden per minuut.

## <a name="no-data-from-my-server"></a>Er zijn geen gegevens van mijn server
*Ik mijn app hebt geïnstalleerd op de webserver en nu kan ik geen telemetriegegevens van deze niet ziet. Het is gegaan OK op mijn dev-machine.*

* Waarschijnlijk een probleem met firewall. [Instellen van firewalluitzonderingen voor Application Insights om gegevens te verzenden](../../azure-monitor/app/ip-addresses.md).
* IIS-Server mogelijk ontbreken bepaalde vereisten: .NET-Extensibility 4.5 en ASP.NET 4.5.

*Ik [geïnstalleerd Status Monitor](../../azure-monitor/app/monitor-performance-live-website-now.md) op de webserver voor het bewaken van bestaande apps. Geen resultaten weergegeven niet.*

* Zie [statusmonitor oplossen](../../azure-monitor/app/monitor-performance-live-website-now.md#troubleshoot). 

## <a name="q01"></a>Geen optie 'Application Insights toevoegen' in Visual Studio
*Wanneer ik met de rechtermuisknop op een bestaand project in Solution Explorer, weergegeven niet alle opties voor Application Insights.*

* Niet alle typen .NET-project worden ondersteund door de hulpprogramma's. Web- en WCF-projecten worden ondersteund. Voor andere projecttypen zoals toepassingen voor desktop- of service, kunt u nog steeds [een Application Insights SDK handmatig toevoegen aan uw project](../../azure-monitor/app/windows-desktop.md).
* Zorg ervoor dat u hebt [Visual Studio 2013 Update 3 of hoger](https://docs.microsoft.com/visualstudio/releasenotes/vs2013-update3-rtm-vs). Dit is vooraf geïnstalleerd met analyses voor ontwikkelaars-hulpprogramma's, waardoor de Application Insights-SDK.
* Selecteer **extra**, **extensies en Updates** en controleert u of **Developer Analytics Tools** is geïnstalleerd en ingeschakeld. Als dit het geval is, klikt u op **Updates** om te zien of er een update beschikbaar is.
* Open het dialoogvenster Nieuw Project en kies ASP.NET-webtoepassing. Als u de optie van de Application Insights ziet, klikt u vervolgens de hulpprogramma's geïnstalleerd. Als dat niet het geval is, probeert te verwijderen en vervolgens opnieuw de Application Insights-hulpprogramma's te installeren.

## <a name="q02"></a>Application Insights toevoegen is mislukt
*Wanneer ik Application Insights toevoegen aan een bestaand project, zie ik een foutbericht weergegeven.*

Mogelijke oorzaken:

* Communicatie met de Application Insights-portal is mislukt; of
* Er is een probleem met uw Azure-account;
* U hebt alleen [leestoegang tot het abonnement of de groep waar u probeert te maken van de nieuwe resource](../../azure-monitor/app/resources-roles-access-control.md).

FIX:

* Controleer dat u hebt opgegeven aanmeldingsreferenties voor de juiste Azure-account. 
* In uw browser, Controleer of u toegang tot hebt de [Azure-portal](https://portal.azure.com). Instellingen openen en kijk of er verder geen beperkingen.
* [Application Insights toevoegen aan een bestaand project](../../azure-monitor/app/asp-net.md): Klik met de rechtermuisknop op uw project in Solution Explorer en kies 'Add Application Insights'.
* Als deze nog steeds niet werkt, volgt u de [handmatige procedure](../../azure-monitor/app/windows-services.md) toevoegen van een resource in de portal en vervolgens de SDK toevoegen aan uw project. 

## <a name="emptykey"></a>Ik krijg de foutmelding 'instrumentatiesleutel mag niet leeg zijn'
Er is iets fout gegaan tijdens installatie Application Insights of misschien een adapter voor logboekregistratie.

Klik in Solution Explorer met de rechtermuisknop op uw project en kies **Application Insights > Application Insights configureren**. U krijgt een dialoogvenster waarin u zich aanmeldt bij Azure uitnodigt en maak een Application Insights-resource, of gebruik een bestaande resourcegroep.

## <a name="NuGetBuild"></a> "NuGet-pakketten ontbreken' op mijn buildserver
*Alles bouwt OK wanneer ik ben foutopsporing op de ontwikkelcomputer, maar ik krijg een NuGet-fout op de buildserver.*

Raadpleeg [NuGet-pakket herstellen](https://docs.nuget.org/Consume/Package-Restore) en [automatisch herstellen van pakket](https://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Ontbrekende opdracht Application Insights openen vanuit Visual Studio
*Wanneer ik mijn project Solution Explorer met de rechtermuisknop, zie ik geen Application Insights-opdrachten of ik een Open Application Insights-opdracht niet wordt weergegeven.*

Mogelijke oorzaken:

* Als u de Application Insights-resource die handmatig zijn gemaakt, of als het project is van een type dat wordt niet ondersteund door de Application Insights-hulpprogramma's.
* De hulpprogramma's voor analyses voor ontwikkelaars zijn uitgeschakeld in uw Visual Studio. 
* Uw Visual Studio is ouder dan 2013 Update 3.

FIX:

* Zorg ervoor dat uw versie van Visual Studio 2013 update 3 of hoger.
* Selecteer **extra**, **extensies en Updates** en controleert u of **Developer Analytics tools** is geïnstalleerd en ingeschakeld. Als dit het geval is, klikt u op **Updates** om te zien of er een update beschikbaar is.
* Met de rechtermuisknop op uw project in Solution Explorer. Als u de opdracht ziet **Application Insights > Application Insights configureren**, gebruiken om te verbinden met uw project de resource in de Application Insights-service.

Anders, dat het projecttype rechtstreeks wordt niet ondersteund door de Application Insights-hulpprogramma's. Als u wilt uw telemetrie weergeven, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com), kiest u de Application Insights in de linker navigatiebalk en selecteer uw toepassing.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>'Toegang geweigerd' op de Application Insights openen vanuit Visual Studio
*De opdracht 'Open Application Insights' kom ik bij de Azure-portal, maar ik krijg de foutmelding 'toegang geweigerd'.*

De aanmelding voor Microsoft die u hebt voor het laatst op uw standaardbrowser gebruikt heeft geen toegang tot [de resource die is gemaakt bij de Application Insights is toegevoegd aan deze app](../../azure-monitor/app/asp-net.md). Er zijn twee waarschijnlijke oorzaken: 

* U hebt meer dan één Microsoft-account - misschien een werk- en een persoonlijk Microsoft-account? De aanmelding die u hebt voor het laatst op uw standaardbrowser gebruikt is voor een ander account dan het account dat toegang tot heeft [Application Insights toevoegen aan het project](../../azure-monitor/app/asp-net.md). 
  
  * FIX: Klik op de naam op van uw rechtsboven in het browservenster en meld u af. Meld u vervolgens aan met het account dat toegang heeft. Klik vervolgens op de linker navigatiebalk op Application Insights en selecteer uw app.
* Iemand anders Application Insights hebt toegevoegd aan het project en ze bent vergeten om u te bieden [toegang tot de resourcegroep](../../azure-monitor/app/resources-roles-access-control.md) waarin deze is gemaakt. 
  
  * FIX: Als ze een organisatie-account gebruikt, kunnen ze u toevoegen aan het team; of ze kunnen u afzonderlijke toegang verlenen tot de resourcegroep.

## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>'Asset niet gevonden' op de Application Insights openen vanuit Visual Studio
*De opdracht 'Open Application Insights' kom ik bij de Azure-portal, maar ik krijg de foutmelding 'asset is niet gevonden'.*

Mogelijke oorzaken:

* De Application Insights-resource voor uw toepassing is verwijderd; of
* De instrumentatiesleutel is ingesteld of gewijzigd in ApplicationInsights.config door rechtstreeks te bewerken zonder het projectbestand worden bijgewerkt. 

De instrumentatiesleutel in ApplicationInsights.config besturingselementen waarbij de telemetrie wordt verzonden. Een regel in het projectbestand bepaalt welke resource wordt geopend wanneer u de opdracht in Visual Studio gebruiken. 

FIX:

* Klik in Solution Explorer met de rechtermuisknop op het project en kies Application Insights, Application Insights configureren. U kunt in het dialoogvenster, ofwel telemetrie wordt verzonden naar een bestaande resource of een nieuwe maken. Of:
* Open de resource direct. Aanmelden bij [de Azure-portal](https://portal.azure.com), klikt u op Application Insights in de linker navigatiebalk en selecteer vervolgens de app.

## <a name="where-do-i-find-my-telemetry"></a>Waar vind ik mijn telemetrie?
*Ik aangemeld bij de [Microsoft Azure portal](https://portal.azure.com), en ik ben kijken naar de Azure home-dashboard. Dus waar vind ik mijn gegevens Application Insights?*

* Klik in de linker navigatiebalk op Application Insights en vervolgens de appnaam van uw. Als u alle projecten er geen hebt, moet u [toevoegen of configureren van Application Insights in uw webproject](../../azure-monitor/app/asp-net.md).
  
    Daar ziet u enkele samenvatting grafieken. U kunt klikken voor meer details door.
* Klik op de knop Application Insights in Visual Studio, tijdens de foutopsporing van uw app.

## <a name="q03"></a> Er is geen server-gegevens (of geen gegevens op alle)
*Kan ik mijn app wordt uitgevoerd en vervolgens de Application Insights-service opent in Microsoft Azure, maar alle grafieken weergeven 'Meer informatie over het verzamelen van...' of 'Niet geconfigureerd.'* Of, *alleen paginaweergave en gebruikersgegevens, maar geen servergegevens.*

* Voer uw toepassing uit in de foutopsporingsmodus in Visual Studio (F5). Gebruik de toepassing om telemetrie te genereren. Controleer of u gebeurtenissen vastgelegd in het uitvoervenster van Visual Studio kunt zien. 
  
    ![](./media/asp-net-troubleshoot-no-data/output-window.png)
* Open in de Application Insights-webportal [diagnostische gegevens doorzoeken](../../azure-monitor/app/diagnostic-search.md). Gegevens weergegeven meestal hier eerst.
* Klik op de knop vernieuwen. De blade wordt vernieuwd periodiek, maar u kunt ook dit handmatig doen. Het interval voor vernieuwen is langer voor grotere tijdsbereik.
* Controleer dat de instrumentatiesleutels overeenkomen. Op de hoofdblade voor uw app in de Application Insights-portal, in de **Essentials** vervolgkeuzelijst, kijkt u naar **instrumentatiesleutel**. Vervolgens in uw project in Visual Studio, opent u ApplicationInsights.config en zoek de `<instrumentationkey>`. Controleer of de twee sleutels gelijk zijn. Als dit niet het:
  
  * In de portal, klikt u op de Application Insights en zoek naar de app-resource met de juiste sleutel; of
  * In Visual Studio Solution Explorer met de rechtermuisknop op het project en kies Application Insights kunt configureren. Opnieuw instellen van de app om telemetrie te verzenden naar de juiste bron.
  * Als u de bijbehorende sleutels niet kunt vinden, controleer dan of u dezelfde referenties aanmelden in Visual Studio als in de portal.
    
* In de [home Microsoft Azure-dashboard](https://portal.azure.com), bekijk de servicestatus-kaart. Als er enkele waarschuwingen indicaties zijn, wacht u totdat ze weer terug bent op OK en vervolgens sluiten en uw Application Insights-toepassing-blade opnieuw open.
* Controleer ook [onze blog status](https://blogs.msdn.microsoft.com/servicemap-status/).
* Hebt u een code te schrijven voor de [server-side SDK](../../azure-monitor/app/api-custom-events-metrics.md) die veranderen de instrumentatiesleutel in `TelemetryClient` exemplaren of in `TelemetryContext`? Of hebt u schrijft een [filter of steekproeven](../../azure-monitor/app/api-filtering-sampling.md) die kan worden gefilterd te veel?
* Als u ApplicationInsights.config hebt bewerkt, zorgvuldig controleert u de configuratie van [TelemetryInitializers en TelemetryProcessors](../../azure-monitor/app/api-filtering-sampling.md). Een verkeerde naam type of de parameter kan leiden tot de SDK om geen gegevens te verzenden.

## <a name="q04"></a>Er zijn geen gegevens over het gebruik van paginaweergaven, Browsers,
*Ik zie de gegevens in serverreactietijd en serveraanvragen grafieken, maar er zijn geen gegevens in de weergave paginalaadtijd of op de blades Browser en het gebruik.*

De gegevens afkomstig van scripts in de webpagina's. 

* Als u Application Insights hebt toegevoegd aan een bestaand webproject [hebt tot de scripts handmatig toevoegen](../../azure-monitor/app/javascript.md).
* Zorg ervoor dat Internet Explorer uw site wordt niet weergegeven in de compatibiliteitsmodus.
* Functie voor foutopsporing van de browser (F12 op sommige browsers, kies vervolgens netwerk) gebruiken om te controleren of dat gegevens worden verzonden naar `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Er zijn geen gegevens afhankelijkheid of uitzondering
Zie [afhankelijkheidstelemetrie](../../azure-monitor/app/asp-net-dependencies.md) en [uitzonderingstelemetrie](asp-net-exceptions.md).

## <a name="no-performance-data"></a>Er zijn geen prestatiegegevens
Prestatiegegevens (CPU, IO-snelheid, enzovoort) is beschikbaar voor [Java-web-services](../../azure-monitor/app/java-collectd.md), [Windows-bureaublad-apps](../../azure-monitor/app/windows-desktop.md), [IIS web-apps en services als u statusmonitor installeert](../../azure-monitor/app/monitor-performance-live-website-now.md), en [Azure Cloudservices](../../azure-monitor/app/app-insights-overview.md). u vindt deze onder instellingen voor Servers.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>Er zijn geen gegevens (server) sinds ik de app is gepubliceerd naar Mijn server
* Controleer dat u daadwerkelijk alle Microsoft gekopieerd. Application Insights-dll-bestanden naar de server, samen met Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll
* In de firewall, u mogelijk [bepaalde TCP-poorten openen](../../azure-monitor/app/ip-addresses.md).
* Hebt u een proxyserver gebruikt voor het verzenden van buiten uw bedrijfsnetwerk, stelt u de [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) in Web.config
* Windows Server 2008: Zorg ervoor dat u de volgende updates hebt geïnstalleerd: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).

## <a name="i-used-to-see-data-but-it-has-stopped"></a>Ik gebruikt om gegevens te bekijken, maar deze is gestopt
* Controleer de [status blog](https://blogs.msdn.com/b/applicationinsights-status/).
* Hebt u uw maandelijkse quotum van gegevenspunten bereikt? Open de instellingen/quotum en prijzen om erachter te komen. Als dit het geval is, kunt u uw abonnement upgraden of betalen voor extra capaciteit. Zie de [prijzen schema](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="i-dont-see-all-the-data-im-expecting"></a>Ik zie niet alle gegevens die ik verwacht
Als uw toepassing grote hoeveelheden gegevens verzendt en u de Application Insights-SDK voor ASP.NET-versie 2.0.0-beta3 of hoger, gebruikt de [adaptieve steekproeven](../../azure-monitor/app/sampling.md) functie kan werken en slechts een percentage van uw telemetrie te verzenden. 

U kunt deze uitschakelen, maar dit wordt niet aanbevolen. Steekproeven is zo ontworpen dat gerelateerde telemetrie correct wordt verzonden, voor diagnostische doeleinden. 

## <a name="client-ip-address-is-0000"></a>Client-IP-adres is 0.0.0.0

Op 5 februari 2018 hebben we aangekondigd dat we het vastleggen van de Client-IP-adres verwijderd. Dit heeft geen invloed op geografische locatie.

> [!NOTE]
> Als u de eerste 3 octetten van het IP-adres nodig hebt, kunt u een [telemetrische initializer](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer) een aangepast kenmerk toe te voegen.
> Dit heeft geen invloed op gegevens die zijn verzameld vóór 5 februari 2018.


## <a name="wrong-geographical-data-in-user-telemetry"></a>Onjuiste geografische gegevens in de telemetrie van de gebruiker
De plaats, regio en land dimensies zijn afgeleid van IP-adressen en niet altijd nauwkeurig. Deze IP-adressen zijn voor de locatie als eerste verwerkt en vervolgens worden gewijzigd op 0.0.0.0 worden opgeslagen.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Uitzondering Methode niet gevonden bij het uitvoeren van uw app in Azure Cloud Services
Hebt u uw app ontwikkeld voor .NET 4.6? 4.6 wordt niet automatisch ondersteund in Azure Cloud Services-rollen. [Installeer 4.6 voor elke rol](../../cloud-services/cloud-services-dotnet-install-dotnet.md) voordat u uw app uitvoert.

## <a name="troubleshooting-logs"></a>Logboeken met probleemoplossing

Volg deze instructies voor het vastleggen van logboeken met probleemoplossing voor uw framework.

### <a name="net-framework"></a>.Net Framework

1. Installeer de [Microsoft.AspNet.ApplicationInsights.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNet.ApplicationInsights.HostingStartup) pakket van NuGet. De versie die u installeert, moet overeenkomen met de huidige geïnstalleerde versie van `Microsoft.ApplicationInsighs`

2. Wijzig het bestand applicationinsights.config zodanig dat het volgende:

   ```xml
   <TelemetryModules>
      <Add Type="Microsoft.ApplicationInsights.Extensibility.HostingStartup.FileDiagnosticsTelemetryModule, Microsoft.AspNet.ApplicationInsights.HostingStartup">
        <Severity>Verbose</Severity>
        <LogFileName>mylog.txt</LogFileName>
        <LogFilePath>C:\\SDKLOGS</LogFilePath>
      </Add>
   </TelemetryModules>
   ```
   Uw toepassing moet schrijfmachtigingen hebben op de geconfigureerde locatie
 
 3. Het proces opnieuw starten zodat deze nieuwe instellingen worden bemerkt door SDK
 
 4. Wanneer u klaar bent, kunt u deze wijzigingen ongedaan maken.
  
### <a name="net-core"></a>.NET core

1. Installeer de [Microsoft.AspNetCore.ApplicationInsights.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.ApplicationInsights.HostingStartup) pakket van NuGet. De versie die u installeert, moet overeenkomen met de huidige geïnstalleerde versie van `Microsoft.ApplicationInsights`

2. Wijzigen `ConfigureServices` methode in uw `Startup.cs` klasse.:

    ```csharp
    services.AddSingleton<ITelemetryModule, FileDiagnosticsTelemetryModule>();
    services.ConfigureTelemetryModule<FileDiagnosticsTelemetryModule>( (module, options) => {
        module.LogFilePath = "C:\\SDKLOGS";
        module.LogFileName = "mylog.txt";
        module.Severity = "Verbose";
    } );
    ```
   Uw toepassing moet schrijfmachtigingen hebben op de geconfigureerde locatie
 
 3. Het proces opnieuw starten zodat deze nieuwe instellingen worden bemerkt door SDK
 
 4. Wanneer u klaar bent, kunt u deze wijzigingen ongedaan maken.
  

## <a name="still-not-working"></a>Nog steeds werkt niet...
* [Application Insights-forum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

