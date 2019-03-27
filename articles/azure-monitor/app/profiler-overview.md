---
title: Profileren van productie-apps in Azure met Application Insights Profiler | Microsoft Docs
description: Het dynamische pad in de code van de web-server met een lage footprint profiler identificeren.
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
ms.openlocfilehash: c07b325f3de6cd2cf3aaa436736786d2cdc42881
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58498125"
---
# <a name="profile-production-applications-in-azure-with-application-insights"></a>Profiel productie-apps in Azure met Application Insights
## <a name="enable-application-insights-profiler-for-your-application"></a>Application Insights Profiler inschakelen voor uw toepassing

Azure Application Insights Profiler biedt prestatietraces voor toepassingen die worden uitgevoerd in de productie in Azure. Profiler bevat de gegevens op schaal automatisch zonder negatieve gevolgen voor uw gebruikers. Profiler kunt u het 'hot' codepad dat de meeste tijd wanneer er een bepaalde webaanvraag verwerkt vergt identificeren. 

Profiler werkt met .NET-toepassingen die zijn geïmplementeerd op de volgende Azure-services. Specifieke instructies voor het inschakelen van Profiler voor elk servicetype zijn in de onderstaande koppelingen.

* [Azure App Service](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric](profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Azure virtuele Machines en virtuele-machineschaalsets](profiler-vm.md?toc=/azure/azure-monitor/toc.json)
* [**Preview-versie** ASP.NET Core Azure Linux-Web-Apps](profiler-aspnetcore-linux.md?toc=/azure/azure-monitor/toc.json) 

Als u Profiler hebt ingeschakeld, maar traceringen niet ziet, controleert u onze [Troubleshooting guide](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).

## <a name="view-profiler-data"></a>Profiler-gegevens weergeven

Voor de Profiler voor het uploaden van traceringen, moet uw toepassing worden actief afhandeling van aanvragen. Als u een experiment uitvoert, kunt u aanvragen naar uw web-app genereren met behulp van [Application Insights-Prestatietesten](https://docs.microsoft.com/vsts/load-test/app-service-web-app-performance-test). Als u Profiler zojuist hebt ingeschakeld, kunt u een korte belastingtest uitvoeren. Terwijl de belastingstest wordt uitgevoerd, selecteert u de **profiel nu** knop op de [ **Profiler instellingen** deelvenster](profiler-settings.md#profiler-settings-pane). Bij het uitvoeren van Profiler, profilering willekeurig ongeveer één keer per uur en voor een periode van twee minuten. Als uw toepassing een constante stroom van aanvragen afhandelt is, uploads Profiler traceringen elk uur.

Nadat uw toepassing een deel van netwerkverkeer ontvangt en de tijd voor het uploaden van de traceringen van Profiler heeft gehad, hebt u traceringen om weer te geven. Dit proces kan 5 tot 10 minuten duren. Om weer te geven traceringen, in de **prestaties** venster **acties ondernemen**, en selecteer vervolgens de **Profiler-traceringen** knop.

![Application Insights-prestaties deelvenster preview Profiler-traceringen][performance-blade]

Selecteer een voorbeeld om een uitsplitsing van de code-niveau van de tijd besteed aan het uitvoeren van de aanvraag weer te geven.

![Application Insights trace explorer][trace-explorer]

De trace-explorer bevat de volgende informatie:

* **Hot Path tonen**: Wordt geopend de hoogste blad knooppunt of ten minste iets sluiten. In de meeste gevallen is dit knooppunt in de buurt van het prestatieknelpunt.
* **Label**: De naam van de functie of de gebeurtenis. De structuur van weergeven een combinatie van code en gebeurtenissen die hebben plaatsgevonden, zoals SQL en HTTP-gebeurtenissen De eerste gebeurtenis vertegenwoordigt de totale aanvraagduur.
* **Verstreken**: Het tijdsinterval tussen het begin van de bewerking en het einde van de bewerking.
* **Wanneer**: De tijd waarop de functie of het evenement werd uitgevoerd ten opzichte van andere functies.

## <a name="how-to-read-performance-data"></a>Het lezen van prestatiegegevens

De service-profiler van Microsoft maakt gebruik van een combinatie van steekproeven methoden en instrumentation voor het analyseren van de prestaties van uw toepassing. Wanneer uitgebreide verzameling uitgevoerd wordt, voorbeelden de profiler service de aanwijzer instructie van de CPU van de machine elke milliseconde. Elk voorbeeld bevat de volledige aanroepstack van de thread die momenteel wordt uitgevoerd. Het biedt gedetailleerde informatie over die thread is uitgevoerd, op een hoog niveau en in een laag niveau van abstractie. De service profiler verzamelt ook andere gebeurtenissen om correlatie van activiteit en oorzakelijke, inclusief de context schakelen tussen gebeurtenissen, taak parallelle bibliotheek (TPL) gebeurtenissen en thread pool te houden.

De aanroepstack die wordt weergegeven in de tijdlijnweergave is het resultaat van de lijnen en -instrumentatie. Omdat elk voorbeeld de volledige aanroepstack van de thread bevat, bevat deze code van Microsoft .NET Framework en andere frameworks waarnaar u verwijst.

### <a id="jitnewobj"></a>Object-toewijzing (clr! JIT\_nieuw of clr! JIT\_Newarr1)

**CLR! JIT\_nieuw** en **clr! JIT\_Newarr1** Help-functies in .NET Framework die geheugen van een beheerde opslagruimte toewijzen zijn. **CLR! JIT\_nieuw** wordt aangeroepen wanneer een object wordt toegewezen. **CLR! JIT\_Newarr1** wordt aangeroepen wanneer een object array wordt toegewezen. Deze twee functies zijn meestal snel en relatief kleine hoeveelheden tijd duren. Als **clr! JIT\_nieuw** of **clr! JIT\_Newarr1** duurt een veel tijd in de tijdlijn, de code kunnen worden bij het toewijzen van veel objecten en gebruiken van grote hoeveelheden geheugen.

### <a id="theprestub"></a>Het laden van code (clr! ThePreStub)

**CLR! ThePreStub** is van een Help-functie in .NET Framework dat de code uit te voeren voor de eerste keer wordt voorbereid. Deze uitvoering meestal bevat, maar is niet beperkt tot de compilatie van just-in-time (JIT). Voor elk C# methode **clr! ThePreStub** maximaal één keer tijdens een proces moet worden aangeroepen.

Als **clr! ThePreStub** duurt lang voordat een aanvraag, de aanvraag is het eerste item dat methode uit te voeren. De tijd voor .NET Framework runtime laden van de eerste methode is van belang. Kunt u overwegen een opwarmtijd-proces dat gedeelte van de code wordt uitgevoerd voordat uw gebruikers toegang tot, of breng Native Image Generator (ngen.exe) uitgevoerd op uw assembly's gebruiken.

### <a id="lockcontention"></a>Vergrendelingsconflicten (clr! JITutil\_MonContention of clr! JITutil\_MonEnterWorker)

**CLR! JITutil\_MonContention** of **clr! JITutil\_MonEnterWorker** geeft aan dat de huidige thread wordt gewacht tot een vergrendeling worden vrijgegeven. Deze tekst wordt vaak weergegeven wanneer u een C# **vergrendeling** -instructie, aanroepen van de **Monitor.Enter** methode, of aanroepen van een methode met de **MethodImplOptions.Synchronized**kenmerk. Vergrendelingsconflicten treedt meestal op wanneer de thread _A_ verkrijgt een vergrendeling en thread _B_ probeert te verkrijgen van de dezelfde vergrendeling voordat thread _A_ vrijgegeven.

### <a id="ngencold"></a>Het laden van code ([KOUD])

Als de methodenaam van de bevat **[koude]**, zoals **mscorlib.ni! [ COLD]System.Reflection.CustomAttribute.IsDefined**, het uitvoeren van .NET Framework runtime is code voor de eerste keer is dat niet is geoptimaliseerd door [profiel begeleide optimalisatie](/cpp/build/profile-guided-optimizations). Voor elke methode moet die worden weergegeven maximaal één keer tijdens het proces.

Als het laden van de code een aanzienlijke hoeveelheid tijd voor een aanvraag duurt, is de aanvraag voor het eerste item voor het uitvoeren van de niet-geoptimaliseerde gedeelte van de methode. Overweeg het gebruik van een opwarmtijd-proces dat gedeelte van de code wordt uitgevoerd voordat uw gebruikers toegang krijgen deze tot.

### <a id="httpclientsend"></a>HTTP-aanvraag verzenden

Methoden, zoals **HttpClient.Send** aangeven dat de code wordt gewacht tot een HTTP-aanvraag moet worden voltooid.

### <a id="sqlcommand"></a>Database-bewerking

Methoden, zoals **SqlCommand.Execute** aangeven dat de code wordt gewacht tot een database-bewerking te voltooien.

### <a id="await"></a>Wachten op (AWAIT\_tijd)

**AWAIT\_tijd** geeft aan dat de code wordt gewacht tot een andere taak te voltooien. Deze vertraging is meestal gebeurt er met de C# **AWAIT** instructie. Wanneer de code doet een C# **AWAIT**, de thread afgewikkeld en retourneert het besturingselement naar de thread-groep, en er is geen thread geblokkeerd wachten op de **AWAIT** om te voltooien. Echter, logisch, de thread die heeft de **AWAIT** 'geblokkeerd,' en er wordt gewacht tot de bewerking is voltooid. De **AWAIT\_tijd** instructie geeft aan dat de geblokkeerde tijd wachten op de taak is voltooid.

### <a id="block"></a>Geblokkeerde tijd

**BLOCKED_TIME** geeft aan dat de code wordt gewacht tot een andere bron beschikbaar zijn. Het kan bijvoorbeeld worden wachten voor een synchronisatieobject, een thread beschikbaar zijn of een verzoek om te voltooien.

### <a name="unmanaged-async"></a>Niet-beheerd asynchroon

.NET framework verzendt ETW-gebeurtenissen en doorgegeven activiteit-ID's tussen threads zodat asynchrone aanroepen threads kunnen worden gevolgd. Niet-beheerde code (systeemeigen code) en sommige oudere stijlen van asynchrone code ontbreken deze gebeurtenissen en activiteit-ID's, zodat de profiler weet niet welke thread en welke functies worden uitgevoerd op de thread. Dit heet 'Niet-beheerde asynchrone' in de aanroepstack. Als u de ETW-bestand hebt gedownload, kunt u mogelijk kunnen gebruiken [voorbeeld](https://github.com/Microsoft/perfview/blob/master/documentation/Downloading.md) om meer inzicht in wat er gebeurt.

### <a id="cpu"></a>CPU-tijd

De CPU is bezig met uitvoeren van de instructies.

### <a id="disk"></a>Schijftijd

De toepassing uitvoert schijfbewerkingen.

### <a id="network"></a>Netwerktijd

De toepassing met het uitvoeren van bewerkingen op het netwerk.

### <a id="when"></a>Als kolom

De **wanneer** kolom is een visualisatie van hoe de inclusief voorbeelden die worden verzameld voor een knooppunt na verloop van tijd variëren. Het totale bereik van de aanvraag is onderverdeeld in 32 tijd buckets. De inclusief voorbeelden voor dat knooppunt zijn verzameld in deze 32 buckets. Elke bucket wordt weergegeven als een balk. De hoogte van de balk met vertegenwoordigt een schaal waarde. Voor de knooppunten die zijn gemarkeerd als **CPU_TIME** of **BLOCKED_TIME**, of wanneer er een duidelijke relatie met het gebruiken van een bron (bijvoorbeeld een CPU, schijf of thread), de balk staat voor het gebruik van een van de resources tijdens de bucket. Voor deze metrische gegevens is het mogelijk dat een waarde van meer dan 100 procent door meerdere bronnen verbruikt. Als u, Gemiddeld, twee CPU's tijdens een interval gebruikt, krijgt u bijvoorbeeld 200 procent.

## <a name="limitations"></a>Beperkingen

De bewaartermijn voor gegevens is vijf dagen. De maximale gegevens die per dag wordt opgenomen, is 10 GB.

Er zijn geen kosten voor het gebruik van de Profiler-service. U kunt deze gebruiken, uw web-app moet worden gehost in ten minste de basislaag van de functie Web Apps van Azure App Service.

## <a name="overhead-and-sampling-algorithm"></a>Overhead en densitysampling-algoritme

Profiler wordt willekeurig twee minuten elk uur uitgevoerd op elke virtuele machine die als host fungeert voor de toepassing die ingeschakeld is voor het vastleggen van traces Profiler. Wanneer de Profiler wordt uitgevoerd, wordt toegevoegd van 5 naar 15 procent CPU-overhead op de server.

## <a name="next-steps"></a>Volgende stappen
Application Insights Profiler inschakelen voor uw Azure-toepassing. Zie ook:
* [App Services](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric](profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Azure virtuele Machines en virtuele-machineschaalsets](profiler-vm.md?toc=/azure/azure-monitor/toc.json)


[performance-blade]: ./media/profiler-overview/performance-blade-v2-examples.png
[trace-explorer]: ./media/profiler-overview/trace-explorer.png
