---
title: Profileer live web-apps op Azure met Application Insights Profiler | Microsoft Docs
description: Het dynamische pad in de code van de web-server met een lage footprint profiler identificeren.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 07/13/2018
ms.author: mbullwin
ms.openlocfilehash: 52f0e6c90aa9fa0dc439eae3801b2d4ac29387a9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39429677"
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Profiel live Azure-web-apps met Application Insights

Deze functie van Azure Application Insights is algemeen beschikbaar voor de functie Web Apps van Azure App Service en is in Preview-versie van Azure compute-resources. Voor informatie met betrekking tot [on premises gebruik van profiler](https://docs.microsoft.com/azure/application-insights/enable-profiler-compute#enable-profiler-on-on-premises-servers).

Dit artikel worden de hoeveelheid tijd die nodig om in elke methode van uw live webtoepassing wanneer u [Application Insights](app-insights-overview.md). Het hulpprogramma Application Insights Profiler geeft gedetailleerde profielen van live-aanvragen die door uw app zijn behandeld. Profiler markeert de *snelpad* die gebruikmaakt van de meeste tijd. Aanvragen met verschillende reactietijden worden geprofileerd op basis van steekproeven. Met behulp van verschillende technieken, kunt u de overhead die gepaard gaat met de toepassing minimaliseren.

Profiler is op dit moment werkt voor ASP.NET en ASP.NET Core web-apps die worden uitgevoerd op Web-Apps. De basis-servicelaag of hoger is vereist voor het gebruik van Profiler.

## <a id="installation"></a> Profiler voor uw Web-Apps inschakelen

Wanneer u een Web-App hebt geïmplementeerd, ongeacht als u de App Insights-SDK in de broncode opgenomen, het volgende doen:

1. Ga naar de **App Services** deelvenster in de Azure-portal.
1. Navigeer naar **instellingen > controle** deelvenster.

   ![App Insights inschakelen in App Services-portal](./media/app-insights-profiler/AppInsights-AppServices.png)

1. Ofwel de instructies in het deelvenster te maken van een nieuwe resource of Selecteer een bestaande App Insights-resource voor het bewaken van uw web-app. Accepteer alle standaardopties. **Gegevens op codeniveau** is standaard ingeschakeld en kunt u Profiler.

   ![App Insights-site-extensie toevoegen][Enablement UI]

1. Profiler is nu geïnstalleerd met de App Insights-site-extensie en is ingeschakeld met behulp van een App-instelling van de App-Services.

    ![App-instelling voor Profiler][profiler-app-setting]

### <a name="enable-profiler-for-azure-compute-resources-preview"></a>Profiler inschakelen voor Azure-rekenresources (preview)

Zie voor meer informatie, de [preview-versie van Profiler voor Azure-rekenresources](https://go.microsoft.com/fwlink/?linkid=848155).

## <a name="view-profiler-data"></a>Profiler-gegevens weergeven

Zorg ervoor dat uw toepassing verkeer ontvangt. Als u een experiment uitvoert, kunt u aanvragen genereren voor uw web-app via [Application Insights-prestatietests](https://docs.microsoft.com/vsts/load-test/app-service-web-app-performance-test). Als u Profiler zojuist hebt ingeschakeld, kunt u een korte belastingstest ongeveer 15 minuten, die profiler-traceringen moeten genereren uitvoeren. Als u de Profiler is al ingeschakeld voor een tijdje hebt gehad, wordt Houd er rekening mee dat Profiler willekeurig twee keer per uur uitgevoerd en wordt gedurende een periode van twee minuten telkens wanneer deze wordt uitgevoerd. U wordt aangeraden de eerste uitvoering van de load-test voor één uur om er zeker van te zijn dat u een voorbeeld van profiler-traceringen.

Nadat uw toepassing heeft een deel van netwerkverkeer ontvangen, gaat u naar de **prestaties** venster **acties ondernemen** profiler-traceringen weergeven en selecteer vervolgens de **Profiler-traceringen** knop.

![Application Insights-prestaties deelvenster preview Profiler-traceringen][performance-blade-v2-examples]

Selecteer een voorbeeld om een uitsplitsing van de code-niveau van de tijd besteed aan het uitvoeren van de aanvraag weer te geven.

![Application Insights trace explorer][trace-explorer]

De trace-explorer bevat de volgende informatie:

* **Hot Path tonen**: wordt geopend de hoogste blad knooppunt, of ten minste iets sluiten. In de meeste gevallen is dit knooppunt naast een bottleneck in de prestaties.
* **Label**: de naam van de functie of de gebeurtenis. De structuur wordt een combinatie van code en gebeurtenissen die hebben plaatsgevonden (zoals SQL- en HTTP-gebeurtenissen) weergegeven. De eerste gebeurtenis vertegenwoordigt de totale aanvraagduur.
* **Verstreken**: het tijdsinterval tussen het begin van de bewerking en het einde van de bewerking.
* **Wanneer**: de tijd waarop de functie of het evenement werd uitgevoerd ten opzichte van andere functies.

## <a name="how-to-read-performance-data"></a>Het lezen van prestatiegegevens

De service-profiler van Microsoft maakt gebruik van een combinatie van steekproeven methoden en instrumentation voor het analyseren van de prestaties van uw toepassing. Wanneer uitgebreide verzameling uitgevoerd wordt, voorbeelden de profiler service de aanwijzer instructie van de CPU van de machine elke milliseconde. Elk voorbeeld bevat de volledige aanroepstack van de thread die momenteel wordt uitgevoerd. Het biedt gedetailleerde informatie over deze thread is uitgevoerd, zowel op hoog niveau en op het niveau van een laag van abstractie. De service profiler verzamelt ook andere gebeurtenissen om correlatie van activiteit en oorzakelijke, inclusief de context schakelen tussen gebeurtenissen, taak parallelle bibliotheek (TPL) gebeurtenissen en thread pool te houden.

De aanroepstack die wordt weergegeven in de tijdlijnweergave is het resultaat van de lijnen en -instrumentatie. Omdat elk voorbeeld de volledige aanroepstack van de thread bevat, bevat deze code van Microsoft .NET Framework en van andere frameworks waarnaar u verwijst.

### <a id="jitnewobj"></a>Object-toewijzing (clr! JIT\_nieuw of clr! JIT\_Newarr1)

**CLR! JIT\_nieuw** en **clr! JIT\_Newarr1** Help-functies in .NET Framework die geheugen van een beheerde opslagruimte toewijzen zijn. **CLR! JIT\_nieuw** wordt aangeroepen wanneer een object wordt toegewezen. **CLR! JIT\_Newarr1** wordt aangeroepen wanneer een object array wordt toegewezen. Deze twee functies zijn meestal snel en relatief kleine hoeveelheden tijd duren. Als u ziet **clr! JIT\_nieuw** of **clr! JIT\_Newarr1** nemen van een aanzienlijke hoeveelheid tijd in de tijdlijn, betekent dit dat de code kan worden bij het toewijzen van veel objecten en gebruiken van grote hoeveelheden geheugen.

### <a id="theprestub"></a>Het laden van code (clr! ThePreStub)

**CLR! ThePreStub** is van een Help-functie in .NET Framework dat de code uit te voeren voor de eerste keer wordt voorbereid. Dit meestal bevat, maar is niet beperkt tot de compilatie van just-in-time (JIT). Voor elke C#-methode **clr! ThePreStub** maximaal één keer tijdens de levensduur van een proces moet worden aangeroepen.

Als **clr! ThePreStub** duurt een aanzienlijke hoeveelheid tijd voor een aanvraag, betekent dit dat de aanvraag de eerste record die wordt uitgevoerd die methode. De tijd voor de .NET Framework runtime laden van de eerste methode is van belang. Kunt u overwegen een opwarmtijd-proces dat gedeelte van de code wordt uitgevoerd voordat uw gebruikers toegang tot, of breng Native Image Generator (ngen.exe) uitgevoerd op uw assembly's gebruiken.

### <a id="lockcontention"></a>Vergrendelingsconflicten (clr! JITutil\_MonContention of clr! JITutil\_MonEnterWorker)

**CLR! JITutil\_MonContention** of **clr! JITutil\_MonEnterWorker** geeft aan dat de huidige thread wordt gewacht tot een vergrendeling worden vrijgegeven. Deze tekst wordt vaak weergegeven wanneer u een C# **vergrendeling** instructie, bij het aanroepen van de **Monitor.Enter** methode, of bij het aanroepen van een methode met de **MethodImplOptions.Synchronized** kenmerk. Vergrendelingsconflicten treedt meestal op wanneer de thread _A_ verkrijgt een vergrendelen op afstand en thread _B_ probeert te verkrijgen van de dezelfde vergrendeling voordat thread _A_ vrijgegeven.

### <a id="ngencold"></a>Het laden van code ([KOUD])

Als de methodenaam van de bevat **[koude]**, zoals **mscorlib.ni! [ COLD]System.Reflection.CustomAttribute.IsDefined**, het uitvoeren van de .NET Framework runtime is code voor de eerste keer is dat niet is geoptimaliseerd door <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">profiel begeleide optimalisatie</a>. Voor elke methode moet die worden weergegeven maximaal één keer tijdens de levensduur van het proces.

Als het laden van code nodig een aanzienlijke hoeveelheid tijd voor een aanvraag heeft, betekent dit dat de aanvraag voor het eerste item voor het uitvoeren van de niet-geoptimaliseerde gedeelte van de methode is. Overweeg het gebruik van een opwarmtijd-proces dat gedeelte van de code wordt uitgevoerd voordat uw gebruikers toegang krijgen deze tot.

### <a id="httpclientsend"></a>HTTP-aanvraag verzenden

-Methoden zoals **HttpClient.Send** aangeven dat de code wordt gewacht tot een HTTP-aanvraag moet worden voltooid.

### <a id="sqlcommand"></a>Database-bewerking

-Methoden zoals **SqlCommand.Execute** aangeven dat de code wordt gewacht tot een database-bewerking te voltooien.

### <a id="await"></a>Wachten op (AWAIT\_tijd)

**AWAIT\_tijd** geeft aan dat de code wordt gewacht tot een andere taak te voltooien. Dit gebeurt meestal met de C# **AWAIT** instructie. Wanneer de code doet een C# **AWAIT**, de thread afgewikkeld en retourneert het besturingselement naar de thread-groep, en er is geen thread geblokkeerd wachten op de **AWAIT** om te voltooien. Echter, logisch, de thread die heeft de **AWAIT** 'geblokkeerd', en wacht tot de bewerking is voltooid. De **AWAIT\_tijd** instructie geeft aan dat de geblokkeerde tijd wachten op de taak is voltooid.

### <a id="block"></a>Geblokkeerde tijd

**BLOCKED_TIME** geeft aan dat de code wordt gewacht tot een andere bron beschikbaar zijn. Het kan bijvoorbeeld worden wachten voor een synchronisatieobject, een thread beschikbaar zijn of een verzoek om te voltooien.

### <a id="cpu"></a>CPU-tijd

De CPU is bezig met uitvoeren van de instructies.

### <a id="disk"></a>Schijftijd

De toepassing uitvoert schijfbewerkingen.

### <a id="network"></a>Netwerktijd

De toepassing met het uitvoeren van bewerkingen op het netwerk.

### <a id="when"></a>Als kolom

De **wanneer** kolom is een visualisatie van hoe de inclusief voorbeelden die worden verzameld voor een knooppunt na verloop van tijd variëren. Het totale bereik van de aanvraag is onderverdeeld in 32 tijd buckets. De inclusief voorbeelden voor dat knooppunt zijn verzameld in deze 32 buckets. Elke bucket wordt weergegeven als een balk. De hoogte van de balk met vertegenwoordigt een schaal waarde. Voor de knooppunten die zijn gemarkeerd als **CPU_TIME** of **BLOCKED_TIME**, of wanneer er een duidelijke relatie met het gebruiken van een bron (bijvoorbeeld een CPU, schijf of thread), de balk staat voor het gebruik van een van de resources tijdens de periode van die bucket. Voor deze metrische gegevens is het mogelijk dat een waarde van meer dan 100 procent door meerdere bronnen verbruikt. Als u, Gemiddeld, twee CPU's tijdens een interval gebruikt, krijgt u bijvoorbeeld 200 procent.

## <a name="limitations"></a>Beperkingen

De bewaartermijn voor gegevens is vijf dagen. De maximale gegevens die per dag wordt opgenomen, is 10 GB.

Er zijn geen kosten voor het gebruik van de Profiler-service. U kunt de Profiler-service gebruiken, uw web-app moet worden gehost in ten minste de basislaag van de Web-Apps.

## <a name="overhead-and-sampling-algorithm"></a>Overhead en densitysampling-algoritme

Profiler wordt willekeurig twee minuten elk uur uitgevoerd op elke virtuele machine die als host fungeert voor de toepassing die ingeschakeld is voor het vastleggen van traces Profiler. Wanneer de Profiler wordt uitgevoerd, wordt deze toegevoegd van 5 procent aan 15 procent CPU-overhead op de server.

De meer servers die beschikbaar zijn voor het hosten van de toepassing de minder impact Profiler heeft op de algehele prestaties zijn. Dit komt doordat het samplingalgoritme Profiler alleen 5 procent van de servers waarop op elk gewenst moment leidt. Meer servers zijn beschikbaar voor de webaanvragen worden verwerkt om de overhead van de server veroorzaakt door het uitvoeren van Profiler compenseren.

## <a name="disable-profiler"></a>Profiler uitschakelen

Profiler voor een afzonderlijke web-app-exemplaar, onder opnieuw te starten of stoppen **-webtaken**, gaat u naar de Web-Apps-resource. Als u wilt verwijderen van Profiler, gaat u naar **extensies**.

![Profiler uitschakelen voor een webtaak][disable-profiler-webjob]

Het is raadzaam dat u hebt ingeschakeld op al uw WebApps voor het detecteren van eventuele prestatieproblemen zo vroeg mogelijk Profiler.

Als u Web Deploy om te implementeren wijzigingen in uw web-App gebruikt, zorgt u ervoor dat u de map App_Data uitsluiten van wordt verwijderd tijdens de implementatie. De Profiler-extensie bestanden verwijderd anders de volgende keer dat u de web-App in Azure implementeren.


## <a id="troubleshooting"></a>Problemen oplossen

### <a name="too-many-active-profiling-sessions"></a>Te veel actieve profilering sessies

U kunt op dit moment Profiler inschakelen op maximaal vier Azure-web-apps en implementatiesites gebruiken die worden uitgevoerd in de dezelfde service-plan. Als de webtaak Profiler te veel actieve profilering sessies rapporteren is, verplaatst u enkele web-apps naar een andere service-plan.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Hoe bepaal ik of Application Insights Profiler wordt uitgevoerd?

Profiler wordt uitgevoerd als een doorlopende webtaak in de web-app. U kunt openen de resource voor de web-app in de [Azure-portal](https://portal.azure.com). In de **WebJobs** deelvenster, Controleer de status van **ApplicationInsightsProfiler**. Als deze niet wordt uitgevoerd, opent u **logboeken** voor meer informatie.

### <a name="why-cant-i-find-any-stack-examples-even-though-profiler-is-running"></a>Waarom kan ik niet vinden een stack-voorbeelden, hoewel Profiler wordt uitgevoerd?

Hier volgen enkele dingen die u kunt controleren:

* Zorg ervoor dat uw web-app service-plan Basic-laag of hoger.
* Zorg ervoor dat uw web-app Application Insights SDK 2.2 Beta heeft of later worden ingeschakeld.
* Zorg ervoor dat uw web-app heeft de **APPINSIGHTS_INSTRUMENTATIONKEY** instelling die is geconfigureerd met de dezelfde instrumentatiesleutel die wordt gebruikt door de Application Insights-SDK.
* Zorg ervoor dat uw web-app wordt uitgevoerd op .NET Framework 4.6.
* Als uw web-app een ASP.NET Core-toepassing is, controleert u [de vereiste afhankelijkheden](#aspnetcore).

Nadat de Profiler is gestart, is er een korte opwarmtijd gedurende welke door Profiler actief verschillende prestaties traceringen worden verzameld. Hierna verzamelt Profiler prestatietraces voor twee minuten om het uur.

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Ik was met behulp van Azure Service profiler. Wat is er gebeurd met het?

Wanneer u Application Insights Profiler inschakelt, wordt de Azure Service profiler-agent is uitgeschakeld.

### <a id="double-counting"></a>Dubbele parallelle threads tellen

In sommige gevallen kan is de totale tijd metrische gegevens in de stack-viewer hoger dan de duur van de aanvraag.

Deze situatie kan optreden wanneer twee of meer threads gekoppeld aan een aanvraag zijn, en ze parallel worden uitgevoerd. In dat geval wordt is de tijd van de totale thread hoger dan de verstreken tijd. Één thread kan worden wacht op de andere worden uitgevoerd. De viewer wordt gezocht naar dit en oninteressant wachten wordt weggelaten, maar deze oplossen aan de zijkant te veel informatie weergeven in plaats van wat mogelijk belangrijke informatie weglaten.

Wanneer u parallelle threads in uw traceringen zien, moet u bepalen welke threads wachten, zodat u kunt het kritieke pad voor de aanvraag bekijken. In de meeste gevallen is gewoon de thread die snel een wachtstatus krijgt wacht op de andere threads. Zich concentreren op de andere threads en de tijd in de wachtrij-threads te negeren.

### <a id="issue-loading-trace-in-viewer"></a>Er zijn geen gegevens

Hier volgen enkele dingen die u kunt controleren:

* Als de gegevens die u probeert weer te geven ouder dan een paar weken is, kunt u uw tijdfilter beperken en probeer het opnieuw.
* Zorg ervoor dat proxy's of een firewall niet hebben toegang tot geblokkeerd https://gateway.azureserviceprofiler.net.
* Zorg ervoor dat de Application Insights-instrumentatiesleutel die u in uw app gebruikt is hetzelfde als de Application Insights-resource die u hebt gebruikt om in te schakelen profilering. De sleutel bevindt zich doorgaans in het bestand ApplicationInsights.config, maar mogelijk ook in het bestand web.config of app.config.

### <a name="error-report-in-the-profiling-viewer"></a>Foutrapport kan worden opgenomen in de viewer voor profilering

Verzend een ondersteuningsticket in de portal. Moet u de correlatie-ID van het foutbericht opnemen.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Implementatiefout: Directory niet leeg ' D:\\home\\site\\wwwroot\\App_Data\\taken

Als u opnieuw wilt uw web-app aan een Web-Apps-resource met de Profiler is ingeschakeld implementeren, ziet u mogelijk een bericht als volgt uit:

*Map niet leeg ' D:\\home\\site\\wwwroot\\App_Data\\taken*

Deze fout treedt op als u Web Deploy van scripts of van de pijplijn Visual Studio Team Services-implementatie uitvoeren. De oplossing is de volgende aanvullende implementatie-parameters toevoegen aan de Web Deploy-taak:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Deze parameters Verwijder de map die wordt gebruikt door Application Insights Profiler en blokkering van het proces opnieuw implementeren. Ze hebben geen invloed op de Profiler-exemplaar dat momenteel wordt uitgevoerd.

## <a name="manual-installation"></a>Handmatige installatie

Als u Profiler configureert, zijn er updates naar de web-app-instellingen. U kunt de updates handmatig toepassen als uw omgeving vereist. Een voorbeeld is mogelijk dat uw toepassing wordt uitgevoerd in een omgeving met Web-Apps voor PowerApps.

1. In de **Web App-beheer** geopend deelvenster **instellingen**.
1. Stel **.Net Framework-versie** naar **v4.6**.
1. Stel **AlwaysOn** naar **op**.
1. Voeg de **APPINSIGHTS_INSTRUMENTATIONKEY** app instellen en stel de waarde in op de dezelfde instrumentatiesleutel die wordt gebruikt door de SDK.
1. Open **geavanceerde hulpmiddelen**.
1. Selecteer **gaat** openen van de Kudu-website.
1. Selecteer op de website Kudu **Site-extensies**.
1. Installeer **Application Insights** uit de galerie met Azure Web Apps.
1. Start opnieuw op de web-app.

## <a id="profileondemand"></a> Profiler handmatig activeren

Profiler kan handmatig met één klik worden geactiveerd. Stel dat u een test voor de prestaties van webtoepassingen worden uitgevoerd. Moet u traceringen om te begrijpen hoe uw web-app wordt uitgevoerd onder belasting. Controle over wanneer traceringen worden vastgelegd met is cruciaal omdat u weet wanneer belastingstest wordt uitgevoerd, maar het interval van steekproeven mogelijk worden gemist.
De volgende stappen laten zien hoe dit scenario werkt:

### <a name="optional-step-1-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>(Optioneel) Stap 1: Het verkeer naar uw web-app genereren op basis van een WebTest voor prestaties

Als uw web-app is al het inkomende verkeer of als u alleen wilt handmatig genereren verkeer, deze sectie overslaan en doorgaan met stap 2.

Ga naar Application Insights-portal **configureren > prestatietests**. Klik op de knop Nieuw om een nieuwe prestatietest te starten.
![maken van nieuwe prestatietest][create-performance-test]

In de **nieuwe prestatietest** in het deelvenster voor het configureren van de test doel-URL. Accepteer alle standaardinstellingen en start de load-test uitvoert.

![Configureren van load test][configure-performance-test]

U ziet dat de nieuwe test staat in de wachtrij eerst, gevolgd door de status wordt uitgevoerd.

![belastingstest wordt verzonden en in de wachtrij geplaatst][load-test-queued]

![belastingstest wordt uitgevoerd in voortgang][load-test-in-progress]

### <a name="step-2-start-profiler-on-demand"></a>Stap 2: Profiler op aanvraag starten

Zodra de load-test wordt uitgevoerd, kunnen we beginnen profiler voor het vastleggen van traces op de web-app tijdens het laden wordt ontvangen.
Navigeer naar het deelvenster Profiler configureren:

![profiler deelvenster vermelding configureren][configure-profiler-entry]

Op de **Profiler configureren deelvenster**, er is een **profiel nu** knop voor het activeren van profiler op alle instanties van de gekoppelde web-apps. Bovendien zijn ingevoerde zichtbaarheid op wanneer profiler in het verleden werd uitgevoerd.

![Profiler op aanvraag][profiler-on-demand]

U ziet de melding en status op de profiler uitvoeringsstatus wijzigen.

### <a name="step-3-view-traces"></a>Stap 3: Weergave traceringen

Nadat de profiler is voltooid, volg de instructies op de melding om naar de blade en bekijk prestatietraces te gaan.

### <a name="troubleshooting-on-demand-profiler"></a>Profiler voor het oplossen van problemen op aanvraag

U ziet soms Profiler time-out foutbericht weergegeven na een sessie op aanvraag:

![Profiler-time-outfout][profiler-timeout]

Er zijn twee redenen waarom u deze fout ziet:

1. De sessie op aanvraag profiler is voltooid, maar de Application Insights duurde langer om de verzamelde gegevens te verwerken. Als gegevens niet worden verwerkt in 15 minuten is voltooid, wordt een time-out-bericht weergegeven in de portal. Hoewel het na enige tijd, worden Profiler-traceringen weergegeven. Als dit het geval is, alleen kunt negeren het foutbericht voor nu. We werken actief aan een oplossing

1. Uw web-app heeft een oudere versie van Profiler-agent die beschikt niet over de functie op aanvraag. Als u Application Insights-profiel eerder ingeschakeld, is het waarschijnlijk moet u bijwerken van uw Profiler-agent te starten met behulp van de capaciteit op aanvraag.
  
Volg deze stappen om te controleren en installeren van de meest recente Profiler:

1. Ga naar App-instellingen voor App-Services en controleer of de volgende instellingen zijn geconfigureerd:
    * **APPINSIGHTS_INSTRUMENTATIONKEY**: vervangen door de juiste instrumentatiesleutel voor Application Insights.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0 als een van deze instellingen niet zijn ingesteld, gaat u naar het deelvenster voor het inschakelen van Application Insights voor het installeren van de recentste site-extensie.

1. Ga naar Application Insights-venster in App Services-portal.

    ![Application Insights inschakelen vanuit de portal voor App Services][enable-app-insights]

1. Als u een knop 'Update' in de volgende pagina ziet, klikt u erop om bij te werken van Application Insights-extensie voor site die de meest recente Profiler-agent wordt geïnstalleerd.
![Update-site-uitbreiding][update-site-extension]

1. Klik vervolgens op **wijzigen** om te zorgen dat de Profiler is ingeschakeld en selecteer **OK** de wijzigingen op te slaan.

    ![Wijzigen en opslaan van app insights][change-and-save-appinsights]

1. Ga terug naar **App-instellingen** tabblad voor de App Service om te controleren van de volgende items voor de app-instellingen zijn ingesteld:
    * **APPINSIGHTS_INSTRUMENTATIONKEY**: vervangen door de juiste instrumentatiesleutel voor application insights.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0

    ![App-instellingen voor profiler][app-settings-for-profiler]

1. (Optioneel) Controleer de versie van de extensie en ervoor te zorgen dat er is geen update beschikbaar.

    ![selectievakje voor het bijwerken van de extensie][check-for-extension-update]

## <a name="next-steps"></a>Volgende stappen

* [Werken met Application Insights in Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
[Enablement UI]: ./media/app-insights-profiler/Enablement_UI.png
[profiler-app-setting]:./media/app-insights-profiler/profiler-app-setting.png
[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[performance-blade-v2-examples]:./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png
[create-performance-test]: ./media/app-insights-profiler/new-performance-test.png
[configure-performance-test]: ./media/app-insights-profiler/configure-performance-test.png
[load-test-queued]: ./media/app-insights-profiler/load-test-queued.png
[load-test-in-progress]: ./media/app-insights-profiler/load-test-inprogress.png
[profiler-on-demand]: ./media/app-insights-profiler/Profiler-on-demand.png
[configure-profiler-entry]: ./media/app-insights-profiler/configure-profiler-entry.png
[enable-app-insights]: ./media/app-insights-profiler/enable-app-insights-blade-01.png
[update-site-extension]: ./media/app-insights-profiler/update-site-extension-01.png
[change-and-save-appinsights]: ./media/app-insights-profiler/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/app-insights-profiler/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/app-insights-profiler/check-extension-update-01.png
[profiler-timeout]: ./media/app-insights-profiler/profiler-timeout.png
