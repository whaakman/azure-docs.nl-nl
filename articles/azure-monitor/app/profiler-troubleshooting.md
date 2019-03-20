---
title: Problemen oplossen met Azure Application Insights Profiler | Microsoft Docs
description: In dit artikel geeft stappen en informatie voor ontwikkelaars die problemen ondervindt inschakelen of met behulp van Application Insights Profiler het oplossen van problemen.
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: 6c96b7139787a3863b3f7a47949d9cdf20cc5021
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57855670"
---
# <a name="troubleshoot-problems-enabling-or-viewing-application-insights-profiler"></a>Problemen oplossen in- of Application Insights Profiler weergeven

## <a id="troubleshooting"></a>Algemene probleemoplossing

### <a name="profiles-are-uploaded-only-if-there-are-requests-to-your-application-while-profiler-is-running"></a>Profielen worden geüpload alleen als er aanvragen worden ingediend om uw toepassing terwijl Profiler wordt uitgevoerd

Azure Application Insights Profiler verzamelt gegevens voor elk uur van twee minuten. De service verzamelt ook gegevens wanneer u selecteert de **profiel nu** knop in de **voor Application Insights Profiler configureren** deelvenster. Maar de profileringsgegevens die is geüpload, alleen wanneer deze kan worden gekoppeld aan een aanvraag die hebben plaatsgevonden terwijl Profiler is uitgevoerd. 

Profiler schrijft traceringsberichten en aangepaste gebeurtenissen naar uw Application Insights-resource. U kunt deze gebeurtenissen gebruiken om te zien hoe de Profiler is uitgevoerd. Als u denkt dat Profiler moet worden uitgevoerd en traceringen vastleggen, maar ze worden niet weergegeven in de **prestaties** deelvenster kunt u controleren hoe de Profiler wordt uitgevoerd:

1. Zoeken naar traceringsberichten en aangepaste gebeurtenissen die door de Profiler wordt verzonden naar uw Application Insights-resource. U kunt deze zoekreeks gebruiken om de relevante gegevens te vinden:

    ```
    stopprofiler OR startprofiler OR upload OR ServiceProfilerSample
    ```
    De volgende afbeelding geeft twee voorbeelden van zoekopdrachten uit twee AI-resources: 
    
   * Aan de linkerkant is niet de toepassing aanvragen ontvangen terwijl Profiler wordt uitgevoerd. Het bericht wordt uitgelegd dat het uploaden is geannuleerd omdat er geen activiteit. 

   * Aan de rechterkant Profiler gestart en aangepaste gebeurtenissen gedetecteerd aanvragen die hebben plaatsgevonden terwijl Profiler werd uitgevoerd verzonden. Als het aangepaste gebeurtenis ServiceProfilerSample wordt weergegeven, betekent dit dat Profiler een tracering gekoppeld aan een aanvraag en vindt u de tracering in de **Insights de prestaties van toepassingen** deelvenster.

     Als er geen telemetrie wordt weergegeven, worden Profiler wordt niet uitgevoerd. Zie de secties voor probleemoplossing voor uw specifieke app-type verderop in dit artikel om op te lossen.  

     ![Profiler telemetrie zoeken][profiler-search-telemetry]

1. Als er aanvragen zijn het Profiler is uitgevoerd, ervoor zorgen dat de aanvragen worden verwerkt door het gedeelte van uw toepassing met de Profiler is ingeschakeld. Hoewel toepassingen wordt soms bestaan uit meerdere onderdelen, worden Profiler is ingeschakeld voor slechts enkele van de onderdelen. De **voor Application Insights Profiler configureren** in ziet u de onderdelen die traceringen hebt geüpload.

### <a name="other-things-to-check"></a>Andere dingen om te controleren
* Zorg ervoor dat uw app wordt uitgevoerd op .NET Framework 4.6.
* Als uw web-app een ASP.NET Core-toepassing is, service moet actief zijn ten minste ASP.NET Core 2.0.
* Als de gegevens die u probeert om weer te geven ouder dan een paar weken is, kunt u uw tijdfilter beperken en probeer het opnieuw. Traceringen worden na zeven dagen verwijderd.
* Zorg ervoor dat proxy of firewall toegang tot niet geblokkeerd https://gateway.azureserviceprofiler.net.

### <a id="double-counting"></a>Dubbele parallelle threads tellen

In sommige gevallen kan is de totale tijd metrische gegevens in de stack-viewer hoger dan de duur van de aanvraag.

Deze situatie kan optreden wanneer twee of meer threads gekoppeld aan een aanvraag zijn en ze parallel worden uitgevoerd. In dat geval wordt is de tijd van de totale thread hoger dan de verstreken tijd. Één thread kan worden wacht op de andere worden uitgevoerd. De viewer probeert deze situatie detecteren en oninteressant wachten wordt weggelaten. In dat geval, het oplossen aan de zijkant te veel informatie weergeven in plaats van weglaten wat mogelijk belangrijke informatie.

Wanneer u parallelle threads in uw traceringen zien, moet u bepalen welke threads wachten zodat u kunt het kritieke pad voor de aanvraag bekijken. De thread die snel een wachtstatus krijgt is meestal gewoon wacht op de andere threads. Zich concentreren op de andere threads en de tijd in de wachtrij-threads te negeren.

### <a name="error-report-in-the-profile-viewer"></a>Foutrapport kan worden opgenomen in de profiel-viewer
Verzend een ondersteuningsticket in de portal. Moet u de correlatie-ID van het foutbericht opnemen.

## <a name="troubleshoot-profiler-on-azure-app-service"></a>Profiler in Azure App Service oplossen
Voor de Profiler goed te laten werken:
* Uw web-app service-plan moet Basic-laag of hoger.
* Uw web-app moet Application Insights is ingeschakeld.
* Uw web-app moet de **APPINSIGHTS_INSTRUMENTATIONKEY** app-instelling is geconfigureerd met de dezelfde instrumentatiesleutel die wordt gebruikt door de Application Insights-SDK.
* Uw web-app moet de **APPINSIGHTS_PROFILERFEATURE_VERSION** app-instelling gedefinieerd en ingesteld op 1.0.0.
* Uw web-app moet de **DiagnosticServices_EXTENSION_VERSION** appinstelling die is gedefinieerd en de waarde die is ingesteld op ~ 3.
* De **ApplicationInsightsProfiler3** webtaak moet worden uitgevoerd. Om te controleren of de webtaak:
   1. Ga naar [Kudu](https://blogs.msdn.microsoft.com/cdndevs/2015/04/01/the-kudu-debug-console-azure-websites-best-kept-secret/).
   1. In de **extra** in het menu **WebJobs-Dashboard**.  
      De **WebJobs** deelvenster wordt geopend. 
   
      ![profiler-webjob]   
   
   1. Selecteer om de details van de webtaak, met inbegrip van het logboek weer te geven de **ApplicationInsightsProfiler2** koppeling.  
     De **continue WebJob Details** deelvenster wordt geopend.

      ![profiler-webjob-log]

Als u weet niet waarom Profiler niet voor u werkt, kunt u het logboek downloaden en deze verzenden naar ons team voor ondersteuning. 
    
### <a name="manual-installation"></a>Handmatige installatie

Als u Profiler configureert, zijn er updates naar de web-app-instellingen. Als uw omgeving nodig is, kunt u de updates handmatig toepassen. Een voorbeeld is mogelijk dat uw toepassing wordt uitgevoerd in een omgeving met Web-Apps voor PowerApps. Als u wilt toepassen updates handmatig, door het volgende te doen:

1. In de **Web App-beheer** geopend deelvenster **instellingen**.

1. Stel **.NET Framework-versie** naar **v4.6**.

1. Stel **AlwaysOn** naar **op**.

1. Voeg de **APPINSIGHTS_INSTRUMENTATIONKEY** app instellen en stel de waarde in op de dezelfde instrumentatiesleutel die wordt gebruikt door de SDK.

1. Voeg de **APPINSIGHTS_PROFILERFEATURE_VERSION** app-instelling en stel de waarde in op 1.0.0.

1. Voeg de **DiagnosticServices_EXTENSION_VERSION** app-instelling en stel de waarde in op ~ 3.

### <a name="too-many-active-profiling-sessions"></a>Te veel actieve profilering sessies

U kunt op dit moment Profiler inschakelen op maximaal vier Azure-web-apps en implementatiesites gebruiken die worden uitgevoerd in de dezelfde service-plan. Als u meer dan vier web-apps die worden uitgevoerd in een app service-abonnement hebt, Profiler mogelijk genereert een *Microsoft.ServiceProfiler.Exceptions.TooManyETWSessionException*. Profiler afzonderlijk voor elke web-app wordt uitgevoerd en probeert om een sessie Event Tracing voor Windows (ETW) voor elke app te starten. Maar een beperkt aantal ETW-sessies in één keer actief kan zijn. Als de webtaak Profiler-te veel actieve profilering sessies rapporten, verplaatst u enkele web-apps naar een andere service-plan.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Implementatiefout: Map niet leeg ' D:\\home\\site\\wwwroot\\App_Data\\taken

Als u uw web-app aan een Web-Apps-resource opnieuw wilt met de Profiler is ingeschakeld implementeren bent, ziet u mogelijk het volgende bericht:

*Map niet leeg ' D:\\home\\site\\wwwroot\\App_Data\\taken*

Deze fout treedt op als u Web Deploy van scripts of van de pijplijn van het Azure DevOps-implementatie uitvoeren. De oplossing is de volgende aanvullende implementatie-parameters toevoegen aan de Web Deploy-taak:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Deze parameters Verwijder de map die wordt gebruikt door Application Insights Profiler en blokkering van het proces opnieuw implementeren. Ze hebben geen invloed op de Profiler-exemplaar dat momenteel wordt uitgevoerd.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Hoe bepaal ik of Application Insights Profiler wordt uitgevoerd?

Profiler wordt uitgevoerd als een continue webjob in de web-app. U kunt openen de resource voor de web-app in de [Azure-portal](https://portal.azure.com). In de **WebJobs** deelvenster, Controleer de status van **ApplicationInsightsProfiler**. Als deze niet wordt uitgevoerd, opent u **logboeken** voor meer informatie.

## <a name="troubleshoot-problems-with-profiler-and-azure-diagnostics"></a>Oplossen van problemen met de Profiler en Azure Diagnostics

  >**Er is een fout in de profiler dat wordt meegeleverd met de nieuwste versie van WAD voor Cloud Services.** Als u wilt profiler gebruiken met een cloudservice, ondersteunt deze alleen AI SDK maximaal versie 2.7.2. Als u een nieuwere versie van de AI-SDK gebruikt, hebt u terugkeren naar 2.7.2 als u wilt de profiler gebruiken. Als u Visual Studio te downgraden van de versie van de App Insights-SDK, krijgt u mogelijk een binding omleidings-fout tijdens runtime. Dit komt doordat de "newVersion' in het bestand web.config voor Microsoft.ApplicationInsights moet worden ingesteld op '2.7.2.0' nadat downgraden van de AI-SDK, maar deze niet automatisch bijgewerkt.

Als u wilt zien of de Profiler juist is geconfigureerd door Azure Diagnostics, moet u de volgende drie dingen doen: 
1. Controleer eerst of de inhoud van de Azure Diagnostics-configuratie die zijn geïmplementeerd zijn wat u verwacht. 

1. Controleer vervolgens of dat Azure Diagnostics wordt doorgegeven voor de juiste iKey op de Profiler vanaf de opdrachtregel. 

1. Derde, Controleer het logboekbestand Profiler om te zien of Profiler is uitgevoerd, maar heeft een fout geretourneerd. 

Om te controleren of de instellingen die zijn gebruikt voor het configureren van Azure Diagnostics:

1. Meld u aan de virtuele machine (VM) en open vervolgens het logboekbestand op deze locatie. (Het station c: kan zijn of d: en de versie van de invoegtoepassing kunnen afwijken.)

    ```
    c:\logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.11.3.12\DiagnosticsPlugin.log  
    ```
    of
    ```
    c:\WindowsAzure\logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.11.3.12\DiagnosticsPlugin.log
    ```

1. In het bestand, kunt u zoeken naar de tekenreeks **WadCfg** te vinden van de instellingen die zijn doorgegeven aan de virtuele machine naar Azure Diagnostics configureren. U kunt controleren of de iKey die worden gebruikt door de sink Profiler juist is.

1. Controleer de opdrachtregel die wordt gebruikt voor het starten van Profiler. De argumenten die worden gebruikt voor het starten van Profiler zijn in het volgende bestand. (Het station kan worden c: of d:)

    ```
    D:\ProgramData\ApplicationInsightsProfiler\config.json
    ```

1. Zorg ervoor dat de iKey op de opdrachtregel Profiler juist is. 

1. Via het pad gevonden in de voorgaande *config.json* bestand, controleert u het logboekbestand Profiler. De gegevens voor foutopsporing die aangeeft van de instellingen die van Profiler gebruikmaakt wordt weergegeven. Status- en foutberichten van Profiler worden ook weergegeven.  

    Als Profiler wordt uitgevoerd terwijl de toepassing aanvragen ontvangt, wordt het volgende bericht weergegeven: *Activiteit gedetecteerd van de iKey*. 

    Wanneer de tracering is geüpload, wordt het volgende bericht weergegeven: *Start met uploaden van trace*. 


[profiler-search-telemetry]:./media/profiler-troubleshooting/Profiler-Search-Telemetry.png
[profiler-webjob]:./media/profiler-troubleshooting/Profiler-webjob.png
[profiler-webjob-log]:./media/profiler-troubleshooting/Profiler-webjob-log.png








