---
title: Live web-apps in Azure met Application Insights Profiler profiel | Microsoft Docs
description: Identificeer de hot pad in uw web server-code met een lage footprint profiler.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: mbullwin
ms.openlocfilehash: c65ef9141898369b8fcadd4c52972b767aca7cfe
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2018
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Profiel live Azure-web-apps met Application Insights

*Deze functie van Azure Application Insights is algemeen beschikbaar is voor de functie Web Apps van Azure App Service en is Preview-versie voor Azure-rekenresources.*

In dit artikel worden de hoeveelheid tijd die nodig om in elke methode van uw live-webtoepassing wanneer u [Application Insights](app-insights-overview.md). Het hulpprogramma Application Insights Profiler geeft gedetailleerde profielen van live-aanvragen die door uw app zijn behandeld. Profiler markeert de *hot pad* die gebruikmaakt van de meeste tijd. Aanvragen met verschillende reactietijden zijn profiel op basis van steekproeven. Met behulp van tal van technieken minimaliseert u de overhead die is gekoppeld aan de toepassing.

Profiler werkt momenteel voor ASP.NET- en ASP.NET Core web-apps die worden uitgevoerd op de Web-Apps. Het Basic servicelaag of hoger is vereist voor het gebruik van de Profiler.

## <a id="installation"></a> Profiler voor uw web-app voor Web-Apps inschakelen
Als u al hebben tot de toepassing naar een web-app gepubliceerd, maar in de broncode voor het gebruik van Application Insights niet hebt gedaan, het volgende doen:
1. Ga naar de **App Services** deelvenster in de Azure-portal.
2. Onder **bewaking**, selecteer **Application Insights**, en vervolgens ofwel Volg de instructies in het deelvenster te maken van een nieuwe resource of een bestaande Application Insights-resource voor het bewaken van uw web-selecteren App.

   ![App inzicht in de portal App Services inschakelen][appinsights-in-appservices]

3. Als u toegang tot de broncode van uw project hebt [Installeer Application Insights-](app-insights-asp-net.md).  
   Als deze al geïnstalleerd, zorg er dan voor dat u de meest recente versie hebt. Om te controleren of de nieuwste versie, klikt u in Solution Explorer met de rechtermuisknop op uw project en selecteer vervolgens **beheren NuGet-pakketten** > **Updates** > **alle bijwerken pakketten**. Vervolgens implementeert u uw app.

ASP.NET Core toepassingen moet de installatie van de 2.1.0-beta6 Microsoft.ApplicationInsights.AspNetCore NuGet-pakket of later om te werken met Profiler. Eerdere versies worden niet ondersteund vanaf 27 juni 2017.

1. In [de Azure-portal](https://portal.azure.com), opent u de Application Insights-resource voor uw web-app. 
2. Selecteer **prestaties** > **Application Insights Profiler inschakelen**.

   ![Selecteer de banner van de profiler inschakelen][enable-profiler-banner]

3. U kunt ook selecteren de **Profiler** configuratie van de status bekijken en in- of uitschakelen van profileringsfunctie.

   ![Configuratie van de Profiler selecteren][performance-blade]

   Web-apps die zijn geconfigureerd met Application Insights worden vermeld in de **Profiler** deelvenster configuratie. Als u de voorgaande stappen hebt gevolgd, kan de Profiler-agent moet worden geïnstalleerd. 

4. In de **Profiler** deelvenster configuratie, selecteer **Profiler inschakelen**.

5. Volg de instructies voor het installeren van de agent Profiler indien nodig. Als er geen web-apps zijn geconfigureerd met Application Insights, selecteert u **gekoppelde Apps toevoegen**.

   ![Opties configureren][linked app services]

In tegenstelling tot web-apps die worden gehost door middel van Web-Apps plannen, toepassingen die worden gehost in Azure-rekenresources (bijvoorbeeld Azure Virtual Machines, virtuele-machineschaalsets, Azure Service Fabric of Azure Cloud Services) niet rechtstreeks worden beheerd door Azure. Er is in dit geval geen web-app wilt koppelen. In plaats van koppelt aan een app, selecteer de **Profiler inschakelen** knop.

### <a name="enable-profiler-for-azure-compute-resources-preview"></a>Profiler inschakelen voor Azure-rekenresources (preview)

Zie voor meer informatie, de [preview-versie van de Profiler voor Azure-rekenresources](https://go.microsoft.com/fwlink/?linkid=848155).

## <a name="view-profiler-data"></a>Gegevens van de profiler weergeven

Zorg ervoor dat uw toepassing verkeer ontvangt. Als u een experiment doet, kunt u aanvragen genereren naar uw app met [Application Insights-prestatietests](https://docs.microsoft.com/en-us/vsts/load-test/app-service-web-app-performance-test). Als u hebt zojuist Profiler ingeschakeld, kunt u een korte belastingstest ongeveer 15 minuten, die moeten worden gegenereerd profiler traceringen uitvoeren. Als u al ingeschakeld voor een tijdje Profiler had, is Houd er rekening mee dat de Profiler wordt uitgevoerd willekeurig twee keer om het uur en gedurende een periode van twee minuten elke keer dat deze wordt uitgevoerd. Het is raadzaam om de eerste uitvoering van de load-test voor één uur om ervoor te zorgen dat u voorbeeld profiler traceringen.

Nadat u uw toepassing verkeer ontvangt, gaat u naar de **prestaties** deelvenster **acties ondernemen** profiler traceringen weergeven en selecteer vervolgens de **Profiler traceringen** knop.

![Application Insights-prestaties deelvenster preview Profiler wordt getraceerd][performance-blade-v2-examples]

Selecteer een voorbeeld om weer te geven van een code-niveau verdeling van tijd besteed aan de aanvraag wordt uitgevoerd.

![Application Insights trace explorer][trace-explorer]

De tracering explorer bevat de volgende informatie:

* **Hot pad weergeven**: wordt geopend de grootste blad knooppunt of ten minste iets sluit. In de meeste gevallen is dit knooppunt naast het prestatieknelpunt.
* **Label**: de naam van de functie of de gebeurtenis. Deze structuur ziet u een mengeling van code en gebeurtenissen die hebben plaatsgevonden (zoals SQL- en HTTP-gebeurtenissen). De eerste gebeurtenis vertegenwoordigt de totale duur van de aanvraag.
* **Verstreken**: het tijdsinterval tussen het begin van de bewerking en het einde van de bewerking.
* **Wanneer**: de tijd waarop de functie of gebeurtenis werd uitgevoerd in de relatie aan andere functies.

## <a name="how-to-read-performance-data"></a>Het lezen van prestatiegegevens

De service Microsoft profiler wordt een combinatie van steekproeven methoden en instrumentation voor het analyseren van de prestaties van uw toepassing. Als gedetailleerde verzameling uitgevoerd wordt, voorbeelden de serviceprofiler voor de instructie aanwijzer van elke machine CPU elke milliseconde. Elk voorbeeld bevat de volledige aanroepstack van de thread die momenteel wordt uitgevoerd. Dit biedt gedetailleerde informatie over die thread werd uitgevoerd, zowel op hoog niveau en op een laag niveau van abstractie. De serviceprofiler verzamelt ook andere gebeurtenissen om correlatie van activiteit en oorzakelijk verband, inclusief de context overschakelen van gebeurtenissen, taak parallelle bibliotheek (TPL) gebeurtenissen en thread pool te houden.

De aanroepstack die wordt weergegeven in de tijdlijnweergave is het resultaat van de steekproef en instrumentation. Omdat elk voorbeeld de volledige aanroepstack van de thread bevat, bevat deze een code van Microsoft .NET Framework en van andere frameworks waarnaar u verwijst.

### <a id="jitnewobj"></a>Object-toewijzing (clr! JIT\_nieuw of clr! JIT\_Newarr1)
**CLR! JIT\_nieuw** en **clr! JIT\_Newarr1** hulpfuncties in .NET Framework die geheugen van een beheerde opslagruimte toewijzen zijn. **CLR! JIT\_nieuw** wordt aangeroepen wanneer een object wordt toegewezen. **CLR! JIT\_Newarr1** wordt aangeroepen wanneer een objectmatrix is toegewezen. Deze twee functies zijn meestal snel en relatief kleine hoeveelheden tijd duren. Als u ziet **clr! JIT\_nieuw** of **clr! JIT\_Newarr1** een aanzienlijke hoeveelheid tijd kosten in de tijdlijn, betekent dit dat de code kan worden veel objecten toewijzen en gebruiken van grote hoeveelheden geheugen.

### <a id="theprestub"></a>Bij het laden van code (clr! ThePreStub)
**CLR! ThePreStub** is een Help-functie in .NET Framework dat de code uit te voeren voor de eerste keer wordt voorbereid. Dit meestal omvat, maar is niet beperkt tot compilatie just in time (Just in time). Voor elke methode C# **clr! ThePreStub** mag maximaal één keer tijdens de levensduur van een proces worden aangeroepen.

Als **clr! ThePreStub** neemt veel tijd voor een aanvraag, betekent dit dat de aanvraag het eerste beheerpunt waarmee deze methode wordt uitgevoerd. De tijd voor de .NET Framework runtime laden van de eerste methode is van belang. U kunt met behulp van een opwarmtijd-proces dat gedeelte van de code wordt uitgevoerd voordat uw gebruikers toegang dit tot of overweeg Native Image Generator (ngen.exe) uitgevoerd op uw assembly's.

### <a id="lockcontention"></a>Vergrendelingsconflicten (clr! JITutil\_MonContention of clr! JITutil\_MonEnterWorker)
**CLR! JITutil\_MonContention** of **clr! JITutil\_MonEnterWorker** geeft aan dat de huidige thread wacht een vergrendeling worden vrijgegeven. Deze tekst wordt doorgaans weergegeven wanneer u een C# **vergrendeling** instructie bij het aanroepen van de **Monitor.Enter** methode, of bij het aanroepen van een methode met de **MethodImplOptions.Synchronized** kenmerk. Vergrendelingsconflicten treedt meestal op wanneer thread _A_ verkrijgt van een vergrendeling en thread _B_ probeert de dezelfde vergrendeling voordat thread _A_ vrijgegeven.

### <a id="ngencold"></a>Bij het laden van code ([koude])
Als de naam van de methode bevat **[koude]**, zoals **mscorlib.ni! [ COLD]System.Reflection.CustomAttribute.IsDefined**, het uitvoeren van de .NET Framework runtime is code voor de eerste keer dat niet is geoptimaliseerd door <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">optimalisatie profiel begeleide</a>. Voor elke methode moet die worden weergegeven maximaal één keer tijdens de levensduur van het proces.

Als een aanzienlijke hoeveelheid tijd voor een aanvraag in beslag tijdens het laden van code neemt, betekent dit dat de aanvraag de eerste is uit te voeren van het niet geoptimaliseerde gedeelte van de methode. Overweeg het gebruik van een opwarmtijd-proces dat wordt uitgevoerd dat deel van de code voordat uw gebruikers toegang krijgen deze tot.

### <a id="httpclientsend"></a>HTTP-aanvraag verzenden
Methoden zoals **HttpClient.Send** aangeven dat de code nog een HTTP-aanvraag te voltooien.

### <a id="sqlcommand"></a>Databasebewerking
Methoden zoals **SqlCommand.Execute** aangeven dat de code wordt gewacht aan een databasebewerking te voltooien.

### <a id="await"></a>Wachten op (AWAIT\_tijd)
**AWAIT\_tijd** geeft aan dat de code nog een andere taak te voltooien. Dit gebeurt meestal met C# **AWAIT** instructie. Wanneer de code doet een C# **AWAIT**, de thread wordt afgewikkeld en wordt de besturing aan de threadgroep en er is geen thread die is geblokkeerd wachten op de **AWAIT** te voltooien. Echter, logisch, de thread die heeft de **AWAIT** 'geblokkeerd' en wacht tot de bewerking te voltooien. De **AWAIT\_tijd** instructie geeft de geblokkeerde tijd wachten op de taak te voltooien.

### <a id="block"></a>Geblokkeerde tijd
**BLOCKED_TIME** geeft aan dat de code door een andere bron beschikbaar wordt gewacht. Het kan bijvoorbeeld worden wachten voor een synchronisatieobject, een thread beschikbaar of een aanvraag te voltooien.

### <a id="cpu"></a>CPU-tijd
De CPU is bezig met uitvoeren van de instructies.

### <a id="disk"></a>Schijftijd
De toepassing uitvoert schijfbewerkingen.

### <a id="network"></a>Netwerk tijd
De toepassing met het uitvoeren van bewerkingen in het netwerk.

### <a id="when"></a>Wanneer kolom
De **wanneer** kolom is een visualisatie van hoe de inclusief voorbeelden die worden verzameld voor een knooppunt gedurende een bepaalde periode variëren. Het totale bereik van de aanvraag is onderverdeeld in 32 tijd buckets. Inclusief voorbeelden voor dat knooppunt wordt in deze buckets 32 geteld. Elke bucket wordt weergegeven als een balk. De hoogte van de balk vertegenwoordigt een uitgebreid waarde. Voor knooppunten die zijn gemarkeerd **CPU_TIME** of **BLOCKED_TIME**, of wanneer er een duidelijke relatie voor de toepassingen van een bron (bijvoorbeeld een CPU, schijf of thread), de balk staat voor het verbruik van een van de bronnen die tijdens de periode die bucket. Voor deze metrische gegevens is het mogelijk dat een waarde van meer dan 100 procent door meerdere resources verbruikt. Als u, op gemiddelde twee CPU's tijdens een interval, krijgt u bijvoorbeeld 200 procent.

## <a name="limitations"></a>Beperkingen

De bewaartermijn voor gegevens is vijf dagen. De maximale gegevens die wordt ingenomen per dag is 10 GB.

Er zijn geen kosten voor het gebruik van de Profiler-service. Voordat u kunt het gebruik van de Profiler-service, uw web-app moet worden gehost in ten minste de basisstaffel van Web-Apps.

## <a name="overhead-and-sampling-algorithm"></a>Overhead en steekproeven algoritme

Profiler wordt willekeurig twee minuten elk uur uitgevoerd op elke virtuele machine die als host fungeert voor de toepassing die ingeschakeld is voor het vastleggen van traces Profiler. Wanneer de Profiler wordt uitgevoerd, wordt deze toegevoegd van 5 procent aan 15 procent CPU-overhead op de server.

De meer servers die zijn beschikbaar voor het hosten van de toepassing de minder invloed Profiler heeft op de algehele prestaties. Dit is omdat het algoritme steekproeven resulteert in slechts 5 procent van de servers waarop op elk gewenst moment Profiler. Meer servers zijn beschikbaar voor webaanvragen naar offset van de overhead van de server veroorzaakt door het uitvoeren van de Profiler fungeren.

## <a name="disable-profiler"></a>Profiler uitschakelen
Profiler onder voor een afzonderlijke web apps-exemplaar starten of stoppen **webtaken**, gaat u naar de Web-Apps-resource. Als u wilt verwijderen Profiler, gaat u naar **extensies**.

![Profiler voor een web-taak uitschakelen][disable-profiler-webjob]

Het is raadzaam dat u ingeschakeld op alle web-apps hebt voor het detecteren van eventuele prestatieproblemen zo spoedig mogelijk Profiler.

Als u Web Deploy te implementeren wijzigingen aan uw webtoepassing hebt gebruikt, zorg ervoor dat u de map App_Data uitsluiten worden verwijderd tijdens de implementatie. De Profiler-extensie-bestanden verwijderd anders de volgende keer dat u de webtoepassing naar Azure implementeert.


## <a id="troubleshooting"></a>Problemen oplossen

### <a name="too-many-active-profiling-sessions"></a>Te veel sessies actief profilering

U kunt op dit moment Profiler inschakelen op maximaal vier Azure-web-apps en implementatiesites die worden uitgevoerd in dezelfde service-abonnement. Als de Profiler web-taak is te veel sessies actief profilering rapportage, verplaatst u een web-apps op een andere service-abonnement.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Hoe bepaal of de Application Insights Profiler wordt uitgevoerd?

Profiler wordt uitgevoerd als een continue web taak in de web-app. U kunt openen de resource voor de web-app in de [Azure-portal](https://portal.azure.com). In de **WebJobs** deelvenster, Controleer de status van **ApplicationInsightsProfiler**. Als deze niet wordt uitgevoerd, opent u **logboeken** voor meer informatie.

### <a name="why-cant-i-find-any-stack-examples-even-though-profiler-is-running"></a>Waarom kan ik geen voorbeelden stack vinden ook al Profiler wordt uitgevoerd?

Hier volgen enkele dingen die u kunt controleren:

* Zorg ervoor dat uw web-app service-abonnement basisstaffel of hoger.
* Zorg ervoor dat uw web-app Application Insights SDK 2.2 Beta heeft of later worden ingeschakeld.
* Zorg ervoor dat uw web-app heeft de **APPINSIGHTS_INSTRUMENTATIONKEY** instelling geconfigureerd met dezelfde instrumentatiesleutel die wordt gebruikt door de Application Insights-SDK.
* Zorg ervoor dat uw web-app wordt uitgevoerd op .NET Framework 4.6.
* Als uw web-app een toepassing ASP.NET Core is, controleert u [de vereiste afhankelijkheden](#aspnetcore).

Nadat de Profiler wordt gestart, is er een korte opwarmtijd gedurende welke door Profiler actief verschillende prestaties traceringen worden verzameld. Daarna Profiler traceringen worden verzameld prestaties twee minuten om het uur.

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Ik werd Azure Service profiler gebruikt. Wat is er gebeurd met het?

Wanneer u Application Insights Profiler inschakelt, wordt de agent van de profiler Azure Service uitgeschakeld.

### <a id="double-counting"></a>Dubbele parallelle threads tellen

In sommige gevallen kan is de totale tijd metrische gegevens in de stack-viewer hoger dan de duur van de aanvraag.

Deze situatie kan zich voordoen wanneer twee of meer threads gekoppeld aan een aanvraag zijn, en ze parallel werken. In dat geval wordt is de tijd van de totale thread hoger dan de verstreken tijd. Één thread kan worden wacht op de andere worden voltooid. De viewer wordt gezocht naar dit en de niet-interessante wachttijd wordt weggelaten, maar het errs aan de kant van te veel gegevens om weer te geven in plaats van weglaten mogelijk essentiële informatie.

Als u parallelle threads in uw traceringen zien, kunt u bepalen welke threads wachten zodat u het kritieke pad voor de aanvraag kunt gaan. In de meeste gevallen is gewoon de thread die probeert het snel de status van een wait wacht op de andere threads. Richten op de andere threads en de tijd in de wachtende threads te negeren.

### <a id="issue-loading-trace-in-viewer"></a>Er is geen profileringsgegevens

Hier volgen enkele dingen die u kunt controleren:

* Als de gegevens die u probeert weer te geven ouder dan een paar weken is, kunt u uw tijdfilter beperken en probeer het opnieuw.
* Zorg ervoor dat proxy of firewall niet hebben toegang tot https://gateway.azureserviceprofiler.net is geblokkeerd.
* Zorg ervoor dat de Application Insights-instrumentatiesleutel die u in uw app werkt hetzelfde als de Application Insights-resource die u voor ingeschakelde profilering gebruikt. De sleutel bevindt zich doorgaans in het bestand ApplicationInsights.config is, maar mogelijk ook in het bestand web.config of app.config.

### <a name="error-report-in-the-profiling-viewer"></a>Foutrapport in de viewer voor profielservices gegeven

Dien een ondersteuningsticket in de portal. Zorg dat de correlatie-id van het foutbericht.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Implementatiefout: Directory niet leeg ' D:\\thuis\\site\\wwwroot\\App_Data\\taken

Als u uw web-app aan een resource-Web-Apps met Profiler ingeschakeld zijn hostproblemen ziet u mogelijk een bericht als volgt:

*Directory niet leeg ' D:\\thuis\\site\\wwwroot\\App_Data\\taken*

Deze fout treedt op als u Web Deploy van scripts of vanuit de Pipeline Visual Studio Team Services implementatie uitvoeren. De oplossing is de volgende extra implementatieparameters toevoegen aan de taak Web Deploy:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Deze parameters Verwijder de map die wordt gebruikt door de Application Insights Profiler en blokkering van het proces opnieuw distribueren. Ze hebben geen invloed op een exemplaar van de Profiler die momenteel wordt uitgevoerd.


## <a name="manual-installation"></a>Handmatige installatie

Wanneer u Profiler configureert, wordt bijgewerkt naar de web-app-instellingen. Als dit vereist is voor uw omgeving, kunt u de updates handmatig toepassen. Een voorbeeld is mogelijk dat uw toepassing wordt uitgevoerd in een omgeving met Web-Apps voor PowerApps.

1. In de **Web App Control** deelvenster open **instellingen**.
2. Stel **.Net Framework-versie** naar **4.6**.
3. Stel **AlwaysOn** naar **op**.
4. Voeg de **APPINSIGHTS_INSTRUMENTATIONKEY** app instellen en stel de waarde in op de dezelfde instrumentatiesleutel die wordt gebruikt door de SDK.
5. Open **geavanceerde Tools**.
6. Selecteer **gaat** de Kudu-website openen.
7. Selecteer op de website Kudu **Site-uitbreidingen**.
8. Installeer **Application Insights** uit de galerie van Azure Web Apps.
9. Start opnieuw op de web-app.

## <a id="profileondemand"></a> Profiler handmatig opnieuw starten
Wanneer we Profiler ontwikkeld, hebben we een opdrachtregelinterface toegevoegd zodat we Profiler op app-services testen kan. Met behulp van deze dezelfde interface, kunnen gebruikers ook aanpassen hoe Profiler wordt gestart. Op een hoog niveau Profiler Web Apps Kudu System gebruikt voor het beheren van profilering op de achtergrond. Wanneer u de Application Insights-extensie installeert, maken we een taak continu web die als host fungeert van de profileringsfunctie. We gebruiken deze dezelfde technologie voor het maken van een nieuwe web-taak die u aanpassen kunt aan uw behoeften.

Deze sectie wordt uitgelegd hoe:

* Een web-taak die Profiler twee minuten met de druk op de knop beginnen kunt maken.
* Maak een web-taak, die u kunt plannen Profiler om uit te voeren.
* Argumenten voor de Profiler instellen.


### <a name="set-up"></a>Instellen
Lees eerst de van de taak van de web-dashboard. Onder **instellingen**, selecteer de **WebJobs** tabblad.

![blade webjobs](./media/app-insights-profiler/webjobs-blade.png)

Zoals u zien kunt, geeft dit dashboard weer alle webtaken die momenteel zijn geïnstalleerd op uw site. Hier ziet u de taak ApplicationInsightsProfiler2 web, waarvoor de Profiler-taak uitgevoerd. Dit is waar we een nieuwe webtaken maken voor handmatige en geplande profilering.

Ophalen van de binaire bestanden die u nodig hebt, doet u het volgende:

1.  Op de Kudu-site op het **ontwikkelingsprogramma's** tabblad de **geavanceerde hulpmiddelen** tabblad met het logo van Kudu en selecteer vervolgens **gaat**.  
   Een nieuwe site wordt geopend en u automatisch bent aangemeld.
2.  Voor het downloaden van de binaire bestanden voor de Profiler, gaat u naar Verkenner via **Console voor foutopsporing** > **CMD**, die aan de bovenkant van de pagina bevindt.
3.  Selecteer **Site** > **wwwroot** > **App_Data** > **taken**  >   **Continue**.  
   Er is een map met de naam *ApplicationInsightsProfiler2*. 
4. Aan de linkerkant van de map, selecteert u de **downloaden** pictogram.  
   Hierdoor downloadt de *ApplicationInsightsProfiler2.zip* bestand. U wordt aangeraden dat u een schone map voor het verplaatsen van deze zip-archief te maken.

### <a name="setting-up-the-web-job-archive"></a>Instellen van de taak webarchief
Wanneer u een nieuwe web-taak niet aan de Azure-website toevoegen, maakt u in feite een zip-archief met een *run.cmd* binnen het bestand. De *run.cmd* bestand geeft het systeem van de taak webpagina wat te doen wanneer u de web-taak uitvoert.

1.  Maak een nieuwe map (bijvoorbeeld *RunProfiler2Minutes*).
2.  Kopieer de bestanden van de uitgepakte *ApplicationInsightProfiler2* map in deze nieuwe map.
3.  Maak een nieuwe *run.cmd* bestand.  
    Voor het gemak kunt u de werkmap openen in Visual Studio Code voordat u begint.
4.  In het bestand, voegt u de opdracht `ApplicationInsightsProfiler.exe start --engine-mode immediate --single --immediate-profiling-duration 120`. De opdrachten worden als volgt beschreven:

    * `start`: Hiermee geeft u de Profiler om te starten.  
    * `--engine-mode immediate`: Hiermee geeft u de Profiler om te beginnen met profilering onmiddellijk.  
    * `--single`: Hiermee geeft u Profiler uitvoeren en vervolgens automatisch stoppen.  
    * `--immediate-profiling-duration 120`: Hiermee geeft u Profiler 120 seconden of 2 minuten uit te voeren.

5.  Sla uw wijzigingen op.
6.  Archiveren van de map met de rechtermuisknop op en selecteert u **verzenden naar** > **gecomprimeerde ZIP-**.  
   Deze actie wordt een ZIP-bestand met de naam van de map gemaakt.

![Profiler opdracht voor starten](./media/app-insights-profiler/start-profiler-command.png)

U hebt nu een webpagina taak ZIP-bestand dat u gebruiken kunt voor het instellen van webtaken in uw site gemaakt.

### <a name="add-a-new-web-job"></a>Een nieuwe web-taak niet toevoegen
In deze sectie voegt u een nieuwe web-taak op uw site. Het volgende voorbeeld laat zien hoe een taak handmatig triggered web toevoegen. Nadat u de taak handmatig triggered web hebt toegevoegd, wordt het proces is bijna hetzelfde voor een geplande web-taak.

1.  Ga naar de **Web-taken** dashboard.
2.  Selecteer op de werkbalk **toevoegen**.
3.  Uw web-job een naam geven.  
    Ter verduidelijking, kan helpen overeenkomen met de naam van uw archief en opent u het voor een groot aantal versies van de *run.cmd* bestand.
4.  In de **uploaden bestand** gebied van het formulier, selecteer de **bestand openen** pictogram en zoek vervolgens naar het ZIP-bestand dat u in de voorgaande sectie hebt gemaakt.

    a.  In de **Type** de optie **Triggered**.  
    b.  In de **Triggers** de optie **handmatige**.

5.  Selecteer **OK**.

![Profiler opdracht voor starten](./media/app-insights-profiler/create-webjob.png)

### <a name="run-profiler"></a>Profiler uitvoeren

Nu dat u een nieuwe web-taak die u handmatig activeren hebt kunt, kunt u proberen uit te voeren door de instructies in deze sectie.

Standaard kunt u hebt slechts één *ApplicationInsightsProfiler.exe* proces dat wordt uitgevoerd op een computer op een bepaald moment. Dus voordat u begint, schakel de *doorlopend* web-taak uit dit dashboard. 
1. Selecteer de rij met het nieuwe web-project en selecteer vervolgens **stoppen**. 
2. Selecteer op de werkbalk **vernieuwen**, en controleer de status geeft aan dat de taak is gestopt.
3. Selecteer de rij met het nieuwe web-project en selecteer vervolgens **uitvoeren**.
4. Met de rij is geselecteerd, klikt u op de werkbalk, selecteer de **logboeken** opdracht.  
    Hiermee een webdashboard taken voor het nieuwe web-project en geeft het de meest recente wordt uitgevoerd en de bijbehorende resultaten.
5. Selecteer het exemplaar van de uitvoeren die u zojuist hebt gestart.  
    Als u de nieuwe web-taak met succes hebt geactiveerd, ziet u enkele logboeken met diagnostische gegevens die afkomstig zijn van de Profiler die controleren of de profilering is gestart.

### <a name="things-to-consider"></a>Aandachtspunten

Hoewel deze methode is betrekkelijk eenvoudig, overweeg dan het volgende:

* Omdat uw web-taak niet wordt beheerd door onze service, hebben we geen manier om bij te werken van de binaire bestanden van de agent voor uw web-taak. Er momenteel geen een stabiele downloadpagina voor onze binaire bestanden, dus de enige manier om de meest recente binaire bestanden op uw toestelnummer bijwerken en grabbing uit is de *doorlopend* map als u in de vorige stappen hebt.

* Omdat dit proces maakt gebruik van opdrachtregelargumenten op die oorspronkelijk zijn ontworpen voor ontwikkelaars in plaats van eindgebruikers, kunnen dat de argumenten in de toekomst worden gewijzigd. Wanneer u een upgrade, worden op de hoogte van mogelijke wijzigingen. Het mag niet veel van een probleem zijn omdat een web-taak niet toevoegen, uitvoeren en testen om ervoor te zorgen dat deze werkt. Uiteindelijk, maakt u een gebruikersinterface voor het afhandelen van dit zonder het handmatige proces.

* De functie webtaken van Web-Apps is uniek. Wanneer deze de web-taak wordt uitgevoerd, zorgt u ervoor dat uw proces heeft de dezelfde omgevingsvariabelen en appinstellingen die uw website. Dit betekent dat u niet wilt de instrumentatiesleutel via de opdrachtregel doorgegeven aan Profiler. Profiler moet de instrumentatiesleutel uit de omgeving kunnen worden opgepikt. Als u wilt de Profiler uitgevoerd op de box dev of op een computer buiten de Web-Apps, moet u een instrumentatiesleutel opgeeft. U kunt dit doen door een argument wordt doorgegeven `--ikey <instrumentation-key>`. Deze waarde moet overeenkomen met de instrumentatiesleutel die uw toepassing wordt gebruikt. De uitvoer van de logboekbestanden van de Profiler leest u welke ikey Profiler met gestart en of er activiteit uit die instrumentatiesleutel gedetecteerd tijdens het we zijn profilering.

* Handmatig triggered webtaken kunnen worden geactiveerd via Web haakje. U kunt deze URL door met de rechtermuisknop op de web-taak op het dashboard en de eigenschappen weer te geven. Of u kunt selecteren in de werkbalk **eigenschappen** nadat u de web-taak hebt geselecteerd in de tabel. Deze aanpak oneindig veel mogelijkheden, zoals de Profiler activatie van uw pijplijn CI/CD (zoals VSTS) of iets zoals Microsoft Flow (https://flow.microsoft.com/en-us/) wordt geopend. Uiteindelijk uw keuze is afhankelijk van hoe complexe dat u wilt maken uw *run.cmd* bestand (dit kan ook worden veroorzaakt een *run.ps1* bestand), maar is er de flexibiliteit.

## <a name="next-steps"></a>Volgende stappen

* [Met Application Insights werken in Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
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
