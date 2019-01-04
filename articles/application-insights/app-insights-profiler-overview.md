---
title: Profileren van productie-apps in Azure met Application Insights Profiler | Microsoft Docs
description: Het dynamische pad in de code van de web-server met een lage footprint profiler identificeren.
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
ms.openlocfilehash: c7266d840a8d60275a40cf31291d3a8f524131f9
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54016736"
---
# <a name="profile-production-applications-in-azure-with-application-insights"></a>Profiel productie-apps in Azure met Application Insights
## <a name="enable-profiler-for-your-application"></a>Profiler inschakelen voor uw toepassing

Application Insights Profiler biedt prestatietraces voor toepassingen die in productie in Azure worden uitgevoerd. De gegevens automatisch op schaal worden vastgelegd zonder negatieve gevolgen voor uw eindgebruikers. Profiler kunt u het 'hot' codepad dat de meeste tijd wanneer het verwerken van een bepaalde website aanvragen vergt identificeren. 

De profiler werkt met .net-toepassingen die zijn geïmplementeerd op de volgende Azure-services. Specifieke instructies voor het inschakelen van de profiler voor elk servicetype zijn in de onderstaande koppelingen.

* [App Services](../azure-monitor/app/profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric-toepassingen](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtuele Machines en virtuele machine Scalesets](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Als u Profiler is ingeschakeld, maar traceringen niet ziet, controleert u onze [Troubleshooting Guide.](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json)

Profiler dat on-premises wordt niet officieel ondersteund, maar we hebben enkele [instructies die u kunt proberen.](https://docs.microsoft.com/azure/application-insights/enable-profiler-compute#enable-profiler-on-on-premises-servers)

## <a name="view-profiler-data"></a>Profiler-gegevens weergeven

Opdat de profiler voor het uploaden van traceringen, aanvragen uw toepassing actief afhandeling van. Als u een experiment uitvoert, kunt u aanvragen genereren voor uw web-app via [Application Insights-prestatietests](https://docs.microsoft.com/vsts/load-test/app-service-web-app-performance-test). Als u Profiler zojuist hebt ingeschakeld, kunt u een korte belastingtest uitvoeren. Terwijl de belastingstest wordt uitgevoerd, drukt u op de **profiel nu** knop in de [ **Profiler instellingenpagina**](app-insights-profiler-settings.md#profiler-settings-page). Zodra profiler wordt uitgevoerd, wordt het willekeurig profiel over één keer per uur en voor een periode van twee minuten. Als uw toepassing een constante stroom van aanvragen afhandelt is, uploadt Profiler traceringen elk uur.

Nadat uw toepassing een deel van netwerkverkeer ontvangt en de profiler tijd voor het uploaden van de trances heeft gehad, hebt u traceringen om weer te geven. Dit proces kan 5 tot 10 minuten duren. Als u wilt weergeven traceringen, gaat u naar de **prestaties** venster **acties ondernemen** profiler-traceringen weergeven en selecteer vervolgens de **Profiler-traceringen** knop.

![Application Insights-prestaties deelvenster preview Profiler-traceringen][performance-blade]

Selecteer een voorbeeld om een uitsplitsing van de code-niveau van de tijd besteed aan het uitvoeren van de aanvraag weer te geven.

![Application Insights trace explorer][trace-explorer]

De trace-explorer bevat de volgende informatie:

* **Hot Path tonen**: Wordt geopend de hoogste blad knooppunt of ten minste iets sluiten. In de meeste gevallen is dit knooppunt in de buurt van het prestatieknelpunt.
* **Label**: De naam van de functie of de gebeurtenis. De structuur wordt een combinatie van code en gebeurtenissen die hebben plaatsgevonden (zoals SQL- en HTTP-gebeurtenissen) weergegeven. De eerste gebeurtenis vertegenwoordigt de totale aanvraagduur.
* **Verstreken**: Het tijdsinterval tussen het begin van de bewerking en het einde van de bewerking.
* **Wanneer**: De tijd waarop de functie of het evenement werd uitgevoerd ten opzichte van andere functies.

## <a name="how-to-read-performance-data"></a>Het lezen van prestatiegegevens

De service-profiler van Microsoft maakt gebruik van een combinatie van steekproeven methoden en instrumentation voor het analyseren van de prestaties van uw toepassing. Wanneer uitgebreide verzameling uitgevoerd wordt, voorbeelden de profiler service de aanwijzer instructie van de CPU van de machine elke milliseconde. Elk voorbeeld bevat de volledige aanroepstack van de thread die momenteel wordt uitgevoerd. Het biedt gedetailleerde informatie over deze thread is uitgevoerd, zowel op hoog niveau en op het niveau van een laag van abstractie. De service profiler verzamelt ook andere gebeurtenissen om correlatie van activiteit en oorzakelijke, inclusief de context schakelen tussen gebeurtenissen, taak parallelle bibliotheek (TPL) gebeurtenissen en thread pool te houden.

De aanroepstack die wordt weergegeven in de tijdlijnweergave is het resultaat van de lijnen en -instrumentatie. Omdat elk voorbeeld de volledige aanroepstack van de thread bevat, bevat deze code van Microsoft .NET Framework en van andere frameworks waarnaar u verwijst.

### <a id="jitnewobj"></a>Object-toewijzing (clr! JIT\_nieuw of clr! JIT\_Newarr1)

**CLR! JIT\_nieuw** en **clr! JIT\_Newarr1** Help-functies in .NET Framework die geheugen van een beheerde opslagruimte toewijzen zijn. **CLR! JIT\_nieuw** wordt aangeroepen wanneer een object wordt toegewezen. **CLR! JIT\_Newarr1** wordt aangeroepen wanneer een object array wordt toegewezen. Deze twee functies zijn meestal snel en relatief kleine hoeveelheden tijd duren. Als u ziet **clr! JIT\_nieuw** of **clr! JIT\_Newarr1** nemen van een aanzienlijke hoeveelheid tijd in de tijdlijn, betekent dit dat de code kan worden bij het toewijzen van veel objecten en gebruiken van grote hoeveelheden geheugen.

### <a id="theprestub"></a>Het laden van code (clr! ThePreStub)

**CLR! ThePreStub** is van een Help-functie in .NET Framework dat de code uit te voeren voor de eerste keer wordt voorbereid. Dit meestal bevat, maar is niet beperkt tot de compilatie van just-in-time (JIT). Voor elke C#-methode **clr! ThePreStub** maximaal één keer tijdens de levensduur van een proces moet worden aangeroepen.

Als **clr! ThePreStub** neemt veel tijd voor een aanvraag, dit geeft aan de aanvraag is de eerste record die wordt uitgevoerd die methode. De tijd voor de .NET Framework runtime laden van de eerste methode is van belang. Kunt u overwegen een opwarmtijd-proces dat gedeelte van de code wordt uitgevoerd voordat uw gebruikers toegang tot, of breng Native Image Generator (ngen.exe) uitgevoerd op uw assembly's gebruiken.

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

## <a name="next-steps"></a>Volgende stappen
Application Insights Profiler inschakelen voor uw Azure-toepassing
* [App Services](../azure-monitor/app/profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric-toepassingen](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtuele Machines en virtuele machine Scalesets](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)


[performance-blade]: ./media/app-insights-profiler-overview/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler-overview/trace-explorer.png
