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
ms.date: 05/04/2017
ms.author: mbullwin
ms.openlocfilehash: 5f691fb88c6764309bf012dfc65b561ec87afede
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Profiel live Azure-web-apps met Application Insights

*Deze functie van Application Insights is beschikbaar voor de Azure App Service en is Preview-versie voor Azure-rekenresources.*

Na hoeveel tijd is besteed aan elke methode in uw live-webtoepassing wanneer u weten [Application Insights](app-insights-overview.md). De Application Insights profilering hulpprogramma toont gedetailleerde profielen van live-aanvragen die door uw app zijn behandeld en markeert de *hot pad* die gebruikmaakt van de meeste tijd. Aanvragen met verschillende responstijden zijn profiel op basis van steekproeven. Overhead bij de toepassing wordt geminimaliseerd met behulp van verschillende technieken.

De profiler werkt momenteel voor ASP.NET- en ASP.NET core web-apps in Azure App Service waarop ten minste de **Basic** servicelaag.

## <a id="installation"></a>De profiler voor Web-App van App Services inschakelen
Als u al de toepassing is gepubliceerd naar een App-Services, maar in de broncode voor Application Insights gebruik, gaat u naar het deelvenster van de App-Services op Azure-portal niet hebt gedaan, gaat u naar **controle | Application Insights**, volg de instructies in het deelvenster maken van nieuwe of bestaande Application Insights-resource voor het bewaken van uw Web-App selecteren. Let op: Profiler alleen met werkt **Basic** App Services-abonnement of hoger.

![App inzicht in de portal App Services inschakelen][appinsights-in-appservices]

Als u toegang tot de broncode van uw project hebt [Installeer Application Insights-](app-insights-asp-net.md). Als deze al geïnstalleerd, zorg er dan voor dat u de meest recente versie hebt. Om te controleren of de nieuwste versie, klikt u in Solution Explorer met de rechtermuisknop op uw project en selecteer vervolgens **beheren NuGet-pakketten** > **Updates** > **alle bijwerken pakketten**. Vervolgens implementeert u uw app.

Een toepassing ASP.NET Core moet de 2.1.0-beta6 Microsoft.ApplicationInsights.AspNetCore NuGet-pakket installeren of hoger om te werken met de profiler. Vanaf 27 juni 2017 eerdere versies worden niet ondersteund.

In [de Azure-portal](https://portal.azure.com), opent u de Application Insights-resource voor uw web-app. Selecteer **prestaties** > **Application Insights Profiler inschakelen**.

![Selecteer de banner van de profiler inschakelen][enable-profiler-banner]

U kunt ook selecteren **Profiler** configuratie om de status bekijken en in- of uitschakelen van de profiler.

![Selecteer onder prestaties Profiler configuratie][performance-blade]

Web-apps die zijn geconfigureerd met Application Insights worden vermeld in de **Profiler** deelvenster configuratie. Als u bovenstaande stappen hebt gevolgd, moet al profiler agent worden geïnstalleerd. Selecteer **Profiler inschakelen** in de **Profiler** deelvenster configuratie.

Volg de instructies voor het installeren van de agent profiler indien nodig. Als er geen web-apps zijn geconfigureerd met Application Insights, selecteert u **gekoppelde Apps toevoegen**.

![Opties configureren][linked app services]

In tegenstelling tot web-apps die worden gehost door App Service-abonnementen, toepassingen die worden gehost in Azure-rekenresources (bijvoorbeeld Azure Virtual Machines, virtuele-machineschaalsets, Azure Service Fabric of Azure Cloud Services) niet rechtstreeks worden beheerd door Azure. Er is in dit geval geen web-app wilt koppelen. In plaats van koppelt aan een app, selecteer de **Profiler inschakelen** knop.

### <a name="enable-the-profiler-for-azure-compute-resources-preview"></a>De profiler voor Azure-rekenresources (preview) inschakelen

Informatie ophalen over een [preview-versie van de profiler voor Azure-rekenresources](https://go.microsoft.com/fwlink/?linkid=848155).

## <a name="view-profiler-data"></a>Gegevens van de profiler weergeven

**Zorg ervoor dat uw toepassing traffics ontvangt.** Als u een experiment doet, kunt u aanvragen genereren naar uw Web-App met de [Application Insights-prestatietests](https://docs.microsoft.com/en-us/vsts/load-test/app-service-web-app-performance-test). Als u zojuist de Profiler ingeschakeld, moet u een korte belastingstest ongeveer 15 minuten kunt uitvoeren en krijgt u profiler traceringen. Als u de Profiler die is ingeschakeld voor een tijdje al hebt, houd er rekening mee dat de Profiler willekeurig actief is gedurende twee keer die elk uur van de twee minuten en elke keer dat deze wordt uitgevoerd. Als u wilt uitvoeren van de load-test voor één uur om ervoor te zorgen dat u voorbeeld profiler traceringen voorgesteld.

Wanneer uw toepassing verkeer ontvangen, gaat u naar de **prestaties** blade, gaat u naar de **acties ondernemen** sectie van de pagina om profiler traceringen weer te geven. Selecteer de **Profiler traceringen** knop.

![Application Insights-prestaties deelvenster preview profiler traceringen][performance-blade-v2-examples]

Selecteer een voorbeeld om weer te geven van een code-niveau verdeling van tijd besteed aan de aanvraag wordt uitgevoerd.

![Application Insights trace explorer][trace-explorer]

De tracering explorer bevat de volgende informatie:

* **Hot pad weergeven**: wordt geopend de grootste blad knooppunt of ten minste iets sluit. In de meeste gevallen is dit knooppunt naast het prestatieknelpunt.
* **Label**: de naam van de functie of de gebeurtenis. De structuur bevat een mengeling van code en gebeurtenissen die hebben plaatsgevonden (zoals SQL- en HTTP-gebeurtenissen). De eerste gebeurtenis vertegenwoordigt de totale duur van de aanvraag.
* **Verstreken**: het tijdsinterval tussen het begin van de bewerking en het einde van de bewerking.
* **Wanneer**: wanneer de functie of de gebeurtenis is uitgevoerd in de relatie aan andere functies.

## <a name="how-to-read-performance-data"></a>Het lezen van prestatiegegevens

De service Microsoft profiler wordt een combinatie van steekproeven methoden en instrumentation voor het analyseren van de prestaties van uw toepassing. Wanneer uitgebreide verzameling uitgevoerd wordt, voorbeelden de serviceprofiler de instructie aanwijzer van elk van de machine CPU in elke milliseconde. Elk voorbeeld bevat de volledige aanroepstack van de thread die momenteel wordt uitgevoerd. Dit biedt gedetailleerde en nuttige informatie over die thread werd uitgevoerd, zowel op hoog niveau en op een laag niveau van abstractie. De serviceprofiler verzamelt ook andere gebeurtenissen om correlatie van activiteit en oorzakelijk verband, inclusief de context overschakelen van gebeurtenissen, taak parallelle bibliotheek (TPL) gebeurtenissen en thread pool te houden.

De aanroepstack die wordt weergegeven in de tijdlijnweergave is het resultaat van de steekproef en instrumentation. Omdat elk voorbeeld de volledige aanroepstack van de thread bevat, bevat deze een code van Microsoft .NET Framework en van andere frameworks waarnaar u verwijst.

### <a id="jitnewobj"></a>Object-toewijzing (clr! JIT\_nieuw of clr! JIT\_Newarr1)
**CLR! JIT\_nieuw** en **clr! JIT\_Newarr1** hulpfuncties in .NET Framework die geheugen van een beheerde opslagruimte toewijzen zijn. **CLR! JIT\_nieuw** wordt aangeroepen wanneer een object wordt toegewezen. **CLR! JIT\_Newarr1** wordt aangeroepen wanneer een objectmatrix is toegewezen. Deze twee functies doorgaans zijn snel en relatief kleine hoeveelheden tijd duren. Als u ziet **clr! JIT\_nieuw** of **clr! JIT\_Newarr1** een aanzienlijke hoeveelheid tijd kosten in de tijdlijn, is het een indicatie dat de code kan worden veel objecten toewijzen en gebruiken van grote hoeveelheden geheugen.

### <a id="theprestub"></a>Bij het laden van code (clr! ThePreStub)
**CLR! ThePreStub** is een Help-functie in .NET Framework dat de code uit te voeren voor de eerste keer wordt voorbereid. Dit doorgaans omvat, maar is niet beperkt tot compilatie just in time (Just in time). Voor elke methode C# **clr! ThePreStub** mag maximaal één keer tijdens de levensduur van een proces worden aangeroepen.

Als **clr! ThePreStub** neemt veel tijd voor een aanvraag, betekent dit dat de aanvraag het eerste beheerpunt waarmee deze methode wordt uitgevoerd. De tijd voor de .NET Framework runtime laden van deze methode is van belang. U kunt met behulp van een opwarmtijd-proces dat gedeelte van de code wordt uitgevoerd voordat uw gebruikers toegang dit tot of overweeg Native Image Generator (ngen.exe) uitgevoerd op uw assembly's.

### <a id="lockcontention"></a>Vergrendelingsconflicten (clr! JITutil\_MonContention of clr! JITutil\_MonEnterWorker)
**CLR! JITutil\_MonContention** of **clr! JITutil\_MonEnterWorker** geeft aan dat de huidige thread wacht een vergrendeling worden vrijgegeven. Dit meestal wordt weergegeven bij het uitvoeren van een C# **vergrendeling** instructie bij het aanroepen van de **Monitor.Enter** methode, of bij het aanroepen van een methode met de **MethodImplOptions.Synchronized**kenmerk. Vergrendelingsconflicten treedt meestal op wanneer een thread een vergrendeling verkrijgt en thread B probeert de vergrendeling te verkrijgen dezelfde voordat een thread is vrijgegeven.

### <a id="ngencold"></a>Bij het laden van code ([koude])
Als de naam van de methode bevat **[koude]**, zoals **mscorlib.ni! [ COLD]System.Reflection.CustomAttribute.IsDefined**, het uitvoeren van de .NET Framework runtime is code voor de eerste keer dat niet is geoptimaliseerd door <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">optimalisatie profiel begeleide</a>. Voor elke methode moet worden weergegeven van maximaal één keer tijdens de levensduur van het proces.

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
De **wanneer** kolom is een visualisatie van hoe de inclusief voorbeelden die worden verzameld voor een knooppunt gedurende een bepaalde periode variëren. Het totale bereik van de aanvraag is onderverdeeld in 32 tijd buckets. Inclusief voorbeelden voor dat knooppunt wordt in deze buckets 32 geteld. Elke bucket wordt weergegeven als een balk. De hoogte van de balk vertegenwoordigt een uitgebreid waarde. Voor knooppunten gemarkeerd **CPU_TIME** of **BLOCKED_TIME**, of wanneer er een duidelijke relatie van de consumptie van een resource (CPU, schijf, een thread), de balk staat voor een van deze resources verbruikt voor de periode van het tijdstip dat bucket. Voor deze metrische gegevens is het mogelijk dat een waarde van meer dan 100% door meerdere resources verbruikt. Als u twee CPU's gemiddeld tijdens een interval gebruikt, krijgt u bijvoorbeeld 200%.

## <a name="limitations"></a>Beperkingen

De standaard gegevensretentie is vijf dagen. De maximale gegevens ingenomen per dag is 10 GB.

Er zijn geen kosten voor het gebruik van de profiler-service. Voor het gebruik van de service profiler uw web-app moet worden gehost in ten minste de Basic-App Service-laag.

## <a name="overhead-and-sampling-algorithm"></a>Overhead en steekproeven algoritme

De profiler wordt willekeurig twee minuten elk uur uitgevoerd op elke virtuele machine die als host fungeert voor de toepassing die ingeschakeld is voor het vastleggen van traces profiler. Wanneer de profiler wordt uitgevoerd, wordt toegevoegd tussen 5 en 15% CPU-overhead op de server.
De meer servers die zijn beschikbaar voor het hosten van de toepassing de minder invloed de profiler is op de algehele prestaties. Dit is omdat het algoritme steekproeven resulteert in de profiler die is uitgevoerd op slechts 5 procent van de servers op elk gewenst moment. Meer servers zijn beschikbaar voor webaanvragen naar offset van de overhead van de server veroorzaakt door het uitvoeren van de profiler fungeren.

## <a name="disable-the-profiler"></a>De profiler uitschakelen
Om te stoppen of opnieuw starten van de profiler voor een afzonderlijke App Service-exemplaar onder **webtaken**, gaat u naar de App Service-resource. Als u wilt verwijderen van de profiler, gaat u naar **extensies**.

![Profiler voor een webtaken uitschakelen][disable-profiler-webjob]

We raden u aan de profiler die is ingeschakeld op alle web-apps voor het detecteren van alle prestatieproblemen zo snel mogelijk.

Als u Web Deploy te implementeren wijzigingen aan uw webtoepassing hebt gebruikt, zorg ervoor dat u de map App_Data uitsluiten worden verwijderd tijdens de implementatie. De profiler-extensie-bestanden verwijderd anders de volgende keer dat u de webtoepassing naar Azure implementeert.


## <a id="troubleshooting"></a>Problemen oplossen

### <a name="too-many-active-profiling-sessions"></a>Te veel sessies actief profilering

U kunt op dit moment profiler op maximaal vier Azure-web-apps en implementatiesites die worden uitgevoerd in dezelfde service-abonnement inschakelen. Als de profiler web-taak is te veel sessies actief profilering rapportage, verplaatst u een web-apps op een andere service-abonnement.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Hoe bepaal of de Application Insights Profiler wordt uitgevoerd?

De profiler wordt uitgevoerd als een continue web taak in de web-app. U kunt een resource voor de web-app in openen [de Azure-portal](https://portal.azure.com). In de **WebJobs** deelvenster, Controleer de status van **ApplicationInsightsProfiler**. Als deze niet wordt uitgevoerd, opent u **logboeken** voor meer informatie.

### <a name="why-cant-i-find-any-stack-examples-even-though-the-profiler-is-running"></a>Waarom kan ik geen voorbeelden stack vinden ook al de profiler wordt uitgevoerd?

Hier volgen enkele dingen die u kunt controleren:

* Zorg ervoor dat uw web-app service-abonnement basisstaffel of hoger.
* Zorg ervoor dat uw web-app Application Insights SDK 2.2 Beta heeft of later worden ingeschakeld.
* Zorg ervoor dat uw web-app heeft de **APPINSIGHTS_INSTRUMENTATIONKEY** instelling geconfigureerd met dezelfde instrumentatiesleutel die wordt gebruikt door de Application Insights-SDK.
* Zorg ervoor dat uw web-app wordt uitgevoerd op .NET Framework 4.6.
* Als uw web-app een toepassing ASP.NET Core is, controleert u [de vereiste afhankelijkheden](#aspnetcore).

Nadat de profiler is gestart, is er een korte opwarmtijd gedurende welke door de profiler actief verschillende prestaties traceringen worden verzameld. Hierna is de profiler traceringen worden verzameld prestaties twee minuten in elk uur.  

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Ik werd Azure Service Profiler gebruikt. Wat is er gebeurd met het?  

Wanneer u Application Insights Profiler inschakelt, wordt de Azure Service Profiler-agent is uitgeschakeld.

### <a id="double-counting"></a>Dubbele parallelle threads tellen

In sommige gevallen kan is de totale tijd metrische gegevens in de stack-viewer hoger dan de duur van de aanvraag.

Dit kan gebeuren wanneer er twee of meer threads die zijn gekoppeld aan een aanvraag, en ze parallel werken. In dat geval wordt is de tijd van de totale thread hoger dan de verstreken tijd. Één thread kan worden wacht op de andere worden voltooid. De viewer wordt gezocht naar dit en de niet-interessante wachttijd wordt weggelaten, maar het errs aan de kant van te veel in plaats van als weggelaten mogelijk belangrijke informatie wordt weergegeven.  

Als u parallelle threads in uw traceringen zien, kunt u bepalen welke threads wachten zodat u het kritieke pad voor de aanvraag kunt zien. In de meeste gevallen is gewoon de thread die probeert het snel de status van een wait wacht op de andere threads. Richten op de andere threads en de tijd in de wachtende threads wordt genegeerd.

### <a id="issue-loading-trace-in-viewer"></a>Er is geen profileringsgegevens

Hier volgen enkele dingen die u kunt controleren:

* Als de gegevens die u probeert weer te geven ouder dan een paar weken is, kunt u uw tijdfilter beperken en probeer het opnieuw.
* Controleer dat proxy of firewall hebben geen toegang tot geblokkeerd https://gateway.azureserviceprofiler.net.
* Controleer of de Application Insights-instrumentatiesleutel die u in uw app werkt hetzelfde als de Application Insights-resource die u voor ingeschakelde profilering gebruikt. De sleutel is meestal in ApplicationInsights.config, maar ook zich mogelijk in de web.config of app.config-bestanden.

### <a name="error-report-in-the-profiling-viewer"></a>Foutrapport in de viewer voor profielservices gegeven

Dien een ondersteuningsticket in de portal. Zorg dat de correlatie-id van het foutbericht.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Implementatiefout: Directory niet leeg ' D:\\thuis\\site\\wwwroot\\App_Data\\taken

Als u uw web-app en een App Service-resource met de profiler die ingeschakeld zijn hostproblemen ziet u mogelijk een bericht dat op het volgende lijkt:

Directory niet leeg ' D:\\thuis\\site\\wwwroot\\App_Data\\taken

Deze fout treedt op als u Web Deploy van scripts of van Visual Studio Team Services implementatie pijplijn uitvoeren. De oplossing is de volgende extra implementatieparameters toevoegen aan de taak Web Deploy:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Deze parameters Verwijder de map die wordt gebruikt door de Application Insights Profiler en blokkering van het proces opnieuw distribueren. Ze hebben geen invloed op een exemplaar van de profiler die momenteel wordt uitgevoerd.


## <a name="manual-installation"></a>Handmatige installatie

Wanneer u de profiler configureert, wordt bijgewerkt naar de web-app-instellingen. Als dit vereist is voor uw omgeving, kunt u de updates handmatig toepassen. Als bijvoorbeeld uw toepassing wordt uitgevoerd in App Service-omgeving voor PowerApps.

1. Open in het deelvenster Beheer van web-app **instellingen**.
2. Stel **.Net Framework-versie** naar **4.6**.
3. Stel **AlwaysOn** naar **op**.
4. Voeg de __APPINSIGHTS_INSTRUMENTATIONKEY__ app instellen en stel de waarde in op de dezelfde instrumentatiesleutel die wordt gebruikt door de SDK.
5. Open **geavanceerde Tools**.
6. Selecteer **gaat** de Kudu-website openen.
7. Selecteer op de website Kudu **Site-uitbreidingen**.
8. Installeer __Application Insights__ uit de galerie van Azure Web Apps.
9. Start opnieuw op de web-app.

## <a id="profileondemand"></a>Profiler handmatig opnieuw starten
Wanneer we de profiler ontwikkeld hebben we een opdrachtregelinterface toegevoegd zodat we de profiler op app-services kunnen testen. Met deze gebruikers met dezelfde interface kunt ook aanpassen hoe de profiler wordt gestart. Op hoog niveau gebruikt de profiler van App Service Kudu systeem voor het beheren van profilering op de achtergrond. Wanneer u de Application Insights-extensie installeert, maken we een continue web-taak die als host fungeert voor de profiler. We gebruiken deze dezelfde technologie voor het maken van een nieuwe web-taak die u aanpassen kunt aan uw behoeften voldoet.

Deze sectie wordt uitgelegd hoe:

1.  Een web-taak die u de profiler twee minuten opnieuw met de druk op de knop starten kunt maken.
2.  Maak een web-taak, die u kunt plannen dat de profiler om uit te voeren.
3.  Argumenten voor de profiler instellen.


### <a name="set-up"></a>Instellen
Eerste we raken met de web-taak dashboard. Onder instellingen op het tabblad WebJobs.

![blade webjobs](./media/app-insights-profiler/webjobs-blade.png)

Zoals u dit dashboard ziet ziet u alle webservice-taken die momenteel zijn geïnstalleerd op uw site. U ziet de ApplicationInsightsProfiler2 web-taak, die de actieve taak voor de profiler is. Dit is waar we onze nieuwe webtaken maken voor handmatige en geplande profilering terechtkomen.

Eerst gaan we aan de binaire bestanden die we nodig hebben.

1.  Ga naar de kudu-site. In de ontwikkeling tabblad Hulpmiddelen Klik op het tabblad 'Geavanceerde hulpmiddelen' met het logo van Kudu. Klik op "Ga". Hiermee gaat u naar een nieuwe site en u kunt zich aanmelden automatisch.
2.  Vervolgens moet de profiler binaire bestanden gedownload. Navigeer naar de Verkenner via de Console voor foutopsporing -> CMD zich boven aan de pagina.
3.  Klik op de site -> wwwroot -> App_Data -> taken -> continue. Hier ziet u een map 'ApplicationInsightsProfiler2'. Klik op het downloadpictogram aan de linkerkant van de map. Hiermee wordt een 'ApplicationInsightsProfiler2.zip'-bestand downloaden.
4.  Hiermee worden alle bestanden die u moet downloaden u verder gaat. Ik het beste een nieuwe map te verplaatsen van deze zip-archief op voordat u doorgaat.

### <a name="setting-up-the-web-job-archive"></a>Instellen van de taak webarchief
Wanneer u een nieuwe web-taak aan de azure-website in feite toevoegen maakt u een zip-archief met een run.cmd in. De run.cmd geeft het systeem van de taak webpagina wat te doen wanneer u de web-taak uitvoert. Er zijn andere opties die u in de documentatie van de taak webpagina kunt lezen, maar voor onze doel we hebben iets anders niet nodig.

1.  Om te starten voor het maken van een nieuwe map, ik mij 'RunProfiler2Minutes' met de naam.
2.  Kopieer de bestanden van de uitgepakte ApplicationInsightProfiler2-map in deze nieuwe map.
3.  Maak een nieuwe run.cmd-bestand. (Deze werkmap in geopend tegenover code voordat u begint voor het gemak)
4.  De opdracht toevoegen `ApplicationInsightsProfiler.exe start --engine-mode immediate --single --immediate-profiling-duration 120`, en sla het bestand.
a.  De `start` opdracht vertelt de profiler om te starten.
b.  `--engine-mode immediate`Hiermee geeft u de profiler dat we willen profileren onmiddellijk starten.
c.  `--single`betekent om uit te voeren en vervolgens automatisch d stoppen.  `--immediate-profiling-duration 120`betekent dat de profiler 120 seconden of 2 minuten uitgevoerd.
5.  Sla dit bestand.
6.  Deze map te archiveren, kunt u met de rechtermuisknop op de map en kies verzenden naar gecomprimeerde map ->. Hiermee maakt u een ZIP-bestand met de naam van de map.

![profiler opdracht voor starten](./media/app-insights-profiler/start-profiler-command.png)

We hebben nu een web-taak .zip die we webtaken instellen in onze site kunnen gebruiken.

### <a name="add-a-new-web-job"></a>Een nieuwe web-taak niet toevoegen
Vervolgens wordt een nieuwe web-taak toevoegen in onze site. In dit voorbeeld ziet u hoe u een handmatige triggered web-taak niet toevoegen. Nadat u zich kunt zijn dat is het proces bijna precies hetzelfde voor geplande. U kunt meer lezen over geplande taken op uw eigen worden geactiveerd.

1.  Ga naar het dashboard van web-taken.
2.  Klik op de werkbalk van de Add-opdracht.
3.  Geef een naam op voor uw web-job overeenkomen met de naam van Mijn archief voor de duidelijkheid en open het tot verschillende versies van de run.cmd gekozen.
4.  In het bestand uploaden delen van het formulier klik op het pictogram van het geopende bestand en zoek het ZIP-bestand dat u hierboven hebt gemaakt.
5.  Kies voor het type Triggered.
6.  Kies de handleiding voor de Triggers.
7.  Klik op OK om op te slaan.

![profiler opdracht voor starten](./media/app-insights-profiler/create-webjob.png)

### <a name="run-the-profiler"></a>De profiler uitvoeren

Nu we hebben een nieuwe web-taak die we handmatig kunnen activeren kunt we proberen uit te voeren.

1.  U kunt slechts één ApplicationInsightsProfiler.exe proces dat wordt uitgevoerd op een computer op elk moment per ontwerp hebben. Dus om te beginnen met zorg ervoor dat de taak continu web uit dit dashboard uitschakelen. Klik op de rij en druk op 'Stop'. Vernieuwen op de werkbalk en Bevestig dat de status bevestigt dat de taak is gestopt.
2.  Klik op de rij met de nieuwe web-taak die u hebt toegevoegd en drukt u op uitvoeren.
3.  Met de rij nog steeds geselecteerde Klik op de opdracht logboeken op de werkbalk, wordt dit zorgen voor een webdashboard taken voor deze web-taak die u hebt gestart. Er wordt een lijst met de meest recente worden uitgevoerd en hun resultaat.
4.  Klik op de uitvoeren die u zojuist hebt gestart.
5.  Als alles goed is gegaan ziet u enkele logboeken met diagnostische gegevens die afkomstig zijn van de profiler bevestigen dat we profilering hebt gestart.

### <a name="things-to-consider"></a>Aandachtspunten

Hoewel deze methode betrekkelijk eenvoudig is zijn er enkele overwegingen.

1.  Omdat dit niet wordt beheerd door de service wordt we hebben geen manier om de agent binaire bestanden voor uw web-job bij te werken. We momenteel geen een stabiele downloadpagina voor onze binaire bestanden zodat de enige manier om de meest recente door uw toestelnummer bijwerken en uit de map continue grabbing zoals al eerder is gedaan.
2.  Als dit wordt gebruikt door opdrachtregelargumenten op die oorspronkelijk zijn ontworpen met ontwikkelaars gebruik in plaats van gebruik van de eindgebruiker, deze argumenten wijziging in de toekomst, dus u hoeft alleen mogelijk weten dat bij een upgrade. Het mag niet veel van een probleem zijn, omdat u een web-taak, uitvoeren en test die hierbij worden kunt toevoegen. Uiteindelijk gebruikersinterface om dit te doen zonder het handmatige proces maakt, maar is iets in overweging moet nemen.
3.  De functie Web-taken voor App Services is uniek tijdens het uitvoeren van de taak webpagina het zorgt ervoor dat uw proces heeft de dezelfde omgevingsvariabelen en appinstellingen die uw website uiteindelijk hebben. Dit betekent dat u niet wilt dat de instrumentatiesleutel via de opdrachtregel doorgegeven aan de profiler, deze moet alleen kunnen worden opgepikt de instrumentatiesleutel uit de omgeving. Als u wilt de profiler uitvoeren op de box dev of op een computer buiten App Services moet u echter een instrumentatiesleutel opgeeft. U kunt dit doen door door te geven in een argument `--ikey <instrumentation-key>`. Houd er rekening mee dat deze waarde moet overeenkomen met de instrumentatiesleutel die wordt gebruikt door uw toepassing. In de uitvoer van de logboekbestanden van de profiler profilering dit laat u weten welke de profiler die is gestart met ikey en als er activiteit uit die instrumentatiesleutel terwijl we.
4.  De webtaken handmatig triggered kunnen daadwerkelijk worden geactiveerd via Web haakje. U kunt deze url met de rechtermuisknop op de web-taak in het dashboard en de eigenschappen weer te geven of eigenschappen te kiezen in de werkbalk na het selecteren van de taak webpagina van de tabel. Er zijn veel van de artikelen die u kunt vinden over deze online zodat instellingen niet veel informatie over het gaat, maar dit de mogelijkheid van de activering van de profiler van uw pijplijn CI/CD (zoals VSTS) of iets zoals Microsoft Flow (https://flow.microsoft.com/en-us/) wordt geopend. De mogelijkheden zijn afhankelijk van hoe fraai dat u maken van uw run.cmd die door de manier waarop een run.ps1 wilt, uitgebreid.  

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
