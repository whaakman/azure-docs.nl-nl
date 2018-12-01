---
title: Problemen oplossen met Azure Application Insights Profiler | Microsoft Docs
description: Deze pagina bevat de stappen en informatie voor ontwikkelaars die problemen ondervindt inschakelen of met behulp van Application Insights profiler het oplossen van problemen.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 7b7aad2cb8aa9b4faeada795f20c818995f62fb6
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52720436"
---
# <a name="troubleshoot-problems-enabling-or-viewing-application-insights-profiler"></a>Problemen oplossen in- of Application Insights Profiler weergeven

## <a id="troubleshooting"></a>Algemene probleemoplossing

### <a name="profiles-are-only-uploaded-if-there-are-requests-to-your-application-while-the-profiler-is-running"></a>Profielen worden alleen geüpload als er aanvragen voor uw toepassing terwijl de profiler wordt uitgevoerd
Application Insights Profiler profiler worden gegevens verzameld voor elk uur, van twee minuten of wanneer de [ **profiel nu** ](app-insights-profiler-settings.md?toc=/azure/azure-monitor/toc.json) wordt geklikt op de **configureren van Application Insights Profiler**pagina. Maar de profileringsgegevens die alleen wordt geüpload wanneer deze kan worden gekoppeld aan een aanvraag die hebben plaatsgevonden terwijl de profiler is uitgevoerd. 

De profiler schrijft traceringsberichten en aangepaste gebeurtenissen naar uw application insights-resource. U kunt deze gebeurtenissen gebruiken om te zien hoe de profiler is uitgevoerd. Als u denkt dat de profiler moet worden uitgevoerd en traceringen vastleggen, maar u ze op de pagina van de prestaties niet ziet, kunt u controleren hoe de profiler wordt uitgevoerd:

1. Zoeken naar traceringsberichten en aangepaste gebeurtenissen die worden verzonden door de profiler naar uw Application Insights-resource. U kunt deze zoekreeks gebruiken om de relevante gegevens te vinden:

    ```
    stopprofiler OR startprofiler OR upload OR ServiceProfilerSample
    ```
    Hier volgt een voorbeeld van twee zoekopdrachten uit twee verschillende AI-resources in de onderstaande schermafbeelding. 
    
    * De gebeurtenis aan de linkerkant wordt van een toepassing die wordt niet van aanvragen ophalen terwijl de profiler wordt uitgevoerd. Hier ziet u het bericht dat de upload is geannuleerd omdat er geen activiteit. 

    * In het voorbeeld aan de rechterkant ziet u dat de profiler is gestart en verzonden van aangepaste gebeurtenissen gedetecteerd aanvragen die hebben plaatsgevonden terwijl de profiler is uitgevoerd. Als u het aangepaste gebeurtenis ServiceProfilerSample ziet, betekent dit de profiler een tracering gekoppeld aan een aanvraag en kunt u de tracering van de pagina van de prestaties van Application Insights bekijken.

    * Als u geen telemetriegegevens helemaal niet ziet, wordt klikt u vervolgens de profiler niet uitgevoerd. U moet mogelijk de secties voor probleemoplossing voor uw specifieke app-type voor dit document hieronder lezen.  

     ![Profiler zoeken in telemetrie][profiler-search-telemetry]

1. Als er aanvragen worden tijdens de periode rond de profiler is uitgevoerd, zorg ervoor dat aanvragen worden verwerkt door het gedeelte van uw toepassing met de profiler is ingeschakeld. Soms toepassingen bestaan uit meerdere onderdelen, maar Profiler is alleen beschikbaar voor sommige, niet alle onderdelen. De pagina configureren van Application Insights Profiler ziet u de onderdelen die traceringen hebt geüpload.

### <a name="other-things-to-check"></a>Andere dingen om te controleren:
* Uw app wordt uitgevoerd op .NET Framework 4.6.
* Als uw web-app een ASP.NET Core-toepassing is, service moet actief zijn ten minste ASP.NET Core 2.0.
* Als de gegevens die u probeert weer te geven ouder dan een paar weken is, kunt u uw tijdfilter beperken en probeer het opnieuw. Traceringen worden na zeven dagen verwijderd.
* Zorg ervoor dat proxy's of een firewall niet hebben toegang tot geblokkeerd https://gateway.azureserviceprofiler.net.

### <a id="double-counting"></a>Dubbele parallelle threads tellen

In sommige gevallen kan is de totale tijd metrische gegevens in de stack-viewer hoger dan de duur van de aanvraag.

Deze situatie kan optreden wanneer twee of meer threads gekoppeld aan een aanvraag zijn, en ze parallel worden uitgevoerd. In dat geval wordt is de tijd van de totale thread hoger dan de verstreken tijd. Één thread kan worden wacht op de andere worden uitgevoerd. De viewer probeert deze situatie detecteren en de interesse wachten wordt weggelaten, maar deze oplossen aan de zijkant te veel informatie weergeven in plaats van weglaten wat mogelijk belangrijke informatie.

Wanneer u parallelle threads in uw traceringen zien, moet u bepalen welke threads wachten, zodat u kunt het kritieke pad voor de aanvraag bekijken. In de meeste gevallen is gewoon de thread die snel een wachtstatus krijgt wacht op de andere threads. Zich concentreren op de andere threads en de tijd in de wachtrij-threads te negeren.

### <a name="error-report-in-the-profile-viewer"></a>Foutrapport kan worden opgenomen in de profiel-viewer
Verzend een ondersteuningsticket in de portal. Moet u de correlatie-ID van het foutbericht opnemen.

## <a name="troubleshooting-profiler-on-app-services"></a>Het oplossen van problemen Profiler in App Services
### <a name="for-the-profiler-to-work-properly"></a>Voor de profiler goed te laten werken:
* Uw web-app service-plan moet Basic-laag of hoger.
* Uw web-app moet Application Insights is ingeschakeld.
* Uw web-app moet de **APPINSIGHTS_INSTRUMENTATIONKEY** app-instelling is geconfigureerd met de dezelfde instrumentatiesleutel die wordt gebruikt door de Application Insights-SDK.
* Uw web-app moet de **APPINSIGHTS_PROFILERFEATURE_VERSION** app-instelling gedefinieerd en ingesteld op 1.0.0.
* Uw web-app moet de **DiagnosticServices_EXTENSION_VERSION** app-instelling gedefinieerd en stel de waarde voor ~ 3.
* De **ApplicationInsightsProfiler3** webtaak moet worden uitgevoerd. U kunt de webtaak controleren door te gaan naar [Kudu](https://blogs.msdn.microsoft.com/cdndevs/2015/04/01/the-kudu-debug-console-azure-websites-best-kept-secret/), en openen van de **WebJobs-Dashboard** onder het menu Extra. Zoals u in de schermafbeeldingen hieronder ziet door te klikken op de koppeling ApplicationInsightsProfiler2, ziet u details van de webtaak, met inbegrip van het logboek.

    Dit is de koppeling die u moet klikken op om de details van de webtaak te zien: 

    ![Profiler-webtaak]

    Hier wordt de pagina met details. U kunt downloaden van het logboek en deze verzenden naar ons team als u weet niet waarom de profiler niet voor u werkt.
    
    ![Profiler-webtaak-logboek]

### <a name="manual-installation"></a>Handmatige installatie

Als u Profiler configureert, zijn er updates naar de web-app-instellingen. U kunt de updates handmatig toepassen als uw omgeving vereist. Een voorbeeld is mogelijk dat uw toepassing wordt uitgevoerd in een omgeving met Web-Apps voor PowerApps.

1. In de **Web App-beheer** geopend deelvenster **instellingen**.
1. Stel **.Net Framework-versie** naar **v4.6**.
1. Stel **AlwaysOn** naar **op**.
1. Voeg de **APPINSIGHTS_INSTRUMENTATIONKEY** app instellen en stel de waarde in op de dezelfde instrumentatiesleutel die wordt gebruikt door de SDK.
1. Voeg de **APPINSIGHTS_PROFILERFEATURE_VERSION** app-instelling en stel de waarde in op 1.0.0.
1. Voeg de **DiagnosticServices_EXTENSION_VERSION** app-instelling en stel de waarde in op ~ 3.

### <a name="too-many-active-profiling-sessions"></a>Te veel actieve profilering sessies

U kunt op dit moment Profiler inschakelen op maximaal vier Azure-web-apps en implementatiesites gebruiken die worden uitgevoerd in de dezelfde service-plan. Als u meer web-apps dan die wordt uitgevoerd in een app service-abonnement hebt, ziet u mogelijk een Microsoft.ServiceProfiler.Exceptions.TooManyETWSessionException gegenereerd door de profiler. De profiler afzonderlijk voor elke web-app wordt uitgevoerd en probeert een ETW-sessie voor elke app te starten. Maar er zijn een beperkt aantal ETW-sessies die in één keer actief kan zijn. Als de webtaak Profiler te veel actieve profilering sessies rapporteren is, verplaatst u enkele web-apps naar een andere service-plan.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Implementatiefout: Directory niet leeg ' D:\\home\\site\\wwwroot\\App_Data\\taken

Als u opnieuw wilt uw web-app aan een Web-Apps-resource met de Profiler is ingeschakeld implementeren, ziet u mogelijk het volgende bericht:

*Map niet leeg ' D:\\home\\site\\wwwroot\\App_Data\\taken*

Deze fout treedt op als u Web Deploy van scripts of van de pijplijn met Azure DevOps-implementatie uitvoeren. De oplossing is de volgende aanvullende implementatie-parameters toevoegen aan de Web Deploy-taak:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Deze parameters Verwijder de map die wordt gebruikt door Application Insights Profiler en blokkering van het proces opnieuw implementeren. Ze hebben geen invloed op de Profiler-exemplaar dat momenteel wordt uitgevoerd.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Hoe bepaal ik of Application Insights Profiler wordt uitgevoerd?

Profiler wordt uitgevoerd als een doorlopende webtaak in de web-app. U kunt openen de resource voor de web-app in de [Azure-portal](https://portal.azure.com). In de **WebJobs** deelvenster, Controleer de status van **ApplicationInsightsProfiler**. Als deze niet wordt uitgevoerd, opent u **logboeken** voor meer informatie.

## <a name="troubleshooting-problems-with-profiler-and-wad"></a>Oplossen van problemen met de Profiler en WAD

Er zijn drie dingen om te controleren of de profiler juist is geconfigureerd met WAD. Eerst moet u controleren of de inhoud van de configuratie van de WAD die zijn geïmplementeerd zijn wat u verwacht. Ten tweede moet u controleren dat WAD geeft de juiste iKey op de profiler-opdrachtregel. Derde, kunt u controleren in het logboekbestand profiler om te zien als profiler is uitgevoerd, maar wordt er een fout optreedt. 

Om te controleren of de instellingen die zijn gebruikt voor het configureren van WAD, moet u zich aanmelden bij de virtuele machine en opent u het logboekbestand op deze locatie: 
```
c:\logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.11.3.12\DiagnosticsPlugin.logs  
```
U kunt zoeken naar de tekenreeks "WadCfg" en vindt u de instellingen die zijn doorgegeven aan de virtuele machine configureren WAD in dat bestand. U kunt controleren dat die worden gebruikt door de profiler-sink iKey juist is.

Ten tweede kunt u de opdrachtregel die wordt gebruikt om te beginnen de profiler controleren. Het volgende bestand bevat de argumenten gebruikt voor het starten van de profiler.
```
D:\ProgramData\ApplicationInsightsProfiler\config.json
```
Controleer of de ikey op de opdrachtregel profiler juist is. 

Derde, met behulp van het pad gevonden in het bovenstaande config.json-bestand, Controleer het logboekbestand profiler. U ziet gegevens voor foutopsporing die de instellingen voor die de profiler wordt gebruikt en de status en foutberichten van de profiler aangeeft. Als de profiler wordt uitgevoerd terwijl de toepassing is ontvangen van aanvragen, ziet u dit bericht: activiteit gedetecteerd van de iKey. Wanneer de tracering is geüpload, ziet u dit bericht: Start met uploaden van de tracering. 


[profiler-search-telemetry]:./media/app-insights-profiler/Profiler-Search-Telemetry.png
[Profiler-webtaak]:./media/app-insights-profiler/Profiler-webjob.png
[Profiler-webtaak-logboek]:./media/app-insights-profiler/Profiler-webjob-log.png








