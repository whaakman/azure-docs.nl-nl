---
title: Fouten opsporen in uw toepassing in Visual Studio | Microsoft Docs
description: De betrouwbaarheid en prestaties van uw services verbeteren door te ontwikkelen en ze op een lokaal ontwikkelcluster foutopsporing in Visual Studio.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: vturecek;mikhegn
ms.openlocfilehash: 2459025899a7f5ffebf44fa104ed112c0eb99dfa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Fouten opsporen in uw Service Fabric-toepassing met behulp van Visual Studio
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a>Fouten opsporen in een lokale Service Fabric-toepassing
U kunt tijd en geld besparen door te implementeren en foutopsporing van uw Azure Service Fabric-toepassing in een cluster van de ontwikkeling van lokale computer. Visual Studio 2017 of Visual Studio 2015 kunt implementeren van de toepassing met het lokale cluster en het foutopsporingsprogramma wordt automatisch verbinding maken met alle exemplaren van uw toepassing.

1. Een lokaal ontwikkelcluster starten door de stappen in [instellen van uw ontwikkelomgeving Service Fabric](service-fabric-get-started.md).
2. Druk op **F5** of klik op **Debug** > **foutopsporing starten**.
   
    ![Foutopsporing van een toepassing starten][startdebugging]
3. Onderbrekingspunten instellen in uw code en de stap door de toepassing door te klikken op de opdrachten in de **Debug** menu.
   
   > [!NOTE]
   > Visual Studio koppelt aan alle exemplaren van uw toepassing. Terwijl u de code hebt doorlopen, kunnen onderbrekingspunten ophalen geraakt door meerdere processen, wat resulteert in gelijktijdige sessies. Probeer de onderbrekingspunten uitschakelen nadat ze zijn bereikt door middel van elke onderbrekingspunt afhankelijk van de thread-ID of met behulp van diagnostische gebeurtenissen.
   > 
   > 
4. De **diagnostische gebeurtenissen** venster wordt automatisch geopend zodat u de diagnostische gebeurtenissen in realtime kunt bekijken.
   
    ![Diagnostische gebeurtenissen in realtime weergeven][diagnosticevents]
5. U kunt ook openen de **diagnostische gebeurtenissen** venster in de Cloud Explorer.  Onder **Service Fabric**, met de rechtermuisknop op een willekeurig knooppunt en kiest u **weergave Streaming traceringen**.
   
    ![Open het venster diagnostische gebeurtenissen][viewdiagnosticevents]
   
    Als u filteren van uw traceringen voor een bepaalde service of toepassing wilt, schakelt u gewoon streaming traces op die specifieke service of toepassing.
6. De diagnostische gebeurtenissen kunnen worden weergegeven in de automatisch gegenereerde **ServiceEventSource.cs** bestand en worden aangeroepen vanuit de toepassingscode.
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. De **diagnostische gebeurtenissen** venster biedt ondersteuning voor filteren, onderbreken en gebeurtenissen in realtime te bekijken.  Het filter is een eenvoudige tekenreeks zoeken in het gebeurtenisbericht, met inbegrip van de inhoud ervan.
   
    ![Filteren, onderbreken en hervatten of gebeurtenissen in realtime controleren][diagnosticeventsactions]
8. Services-foutopsporing is vergelijkbaar met een andere toepassing voor foutopsporing. U wordt normaal gesproken onderbrekingspunten met Visual Studio voor het opsporen van eenvoudige instellen. Hoewel betrouwbare verzamelingen over meerdere knooppunten repliceren, ze nog steeds IEnumerable is geïmplementeerd. Dit betekent dat kunt u het resultaatvenster in Visual Studio tijdens foutopsporing om te zien wat u hebt opgeslagen in. Gewoon een onderbrekingspunt instellen overal in uw code.
   
    ![Foutopsporing van een toepassing starten][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Fouten opsporen in een externe Service Fabric-toepassing
Als uw Service Fabric-toepassingen worden uitgevoerd op een Service Fabric-cluster in Azure, zich u op afstand fouten opsporen in deze rechtstreeks vanuit Visual Studio.

> [!NOTE]
> Vereist de functie [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) en [Azure SDK voor .NET 2.9](https://azure.microsoft.com/downloads/).    
> 
> 

<!-- -->
> [!WARNING]
> Foutopsporing op afstand is bedoeld voor ontwikkel-/ Testscenario's en niet moet worden gebruikt in een productieomgeving vanwege de gevolgen voor de actieve toepassingen.
> 
> 

1. Navigeer naar uw cluster in **Cloud Explorer**, klik met de rechtermuisknop en kies **foutopsporing inschakelen**
   
    ![Foutopsporing op afstand inschakelen][enableremotedebugging]
   
    Hiermee wordt het proces voor het inschakelen van de externe foutopsporing extensie op uw clusterknooppunten, evenals de vereiste netwerkconfiguraties starten.
2. Met de rechtermuisknop op het clusterknooppunt in **Cloud Explorer**, en kies **foutopsporingsprogramma koppelen**
   
    ![Foutopsporingsprogramma koppelen][attachdebugger]
3. In de **koppelen aan proces** dialoogvenster, kiest u het proces dat u wilt opsporen in en klik op **koppelen**
   
    ![Kies proces][chooseprocess]
   
    De naam van het proces dat u koppelen wilt, is gelijk aan de naam van het assembly-naam van uw service-project.
   
    Het foutopsporingsprogramma zal worden gekoppeld aan alle knooppunten in het proces wordt uitgevoerd.
   
   * In het geval waar u fouten een stateless service opspoort, uitmaken alle exemplaren van de service op alle knooppunten deel van de foutopsporingssessie.
   * Als u fouten een stateful service opspoort, worden alleen de primaire replica van een partitie actief en daarom bijgewerkt door het foutopsporingsprogramma. Als de primaire replica tijdens de foutopsporingssessie wordt, wordt de verwerking van die replica nog steeds deel uitmaken van de foutopsporingssessie.
   * Om te kunnen vangen alleen relevante partities of verschillende exemplaren van een bepaalde service, kunt u voorwaardelijke onderbrekingspunten om alleen een specifieke partitie of het exemplaar.
     
     ![Voorwaardelijke onderbrekingspunt][conditionalbreakpoint]
     
     > [!NOTE]
     > We bieden momenteel geen ondersteuning foutopsporing van een Service Fabric-cluster met meerdere exemplaren van dezelfde uitvoerbare servicenaam.
     > 
     > 
4. Nadat u fouten opspoort in uw toepassing hebt, kunt u de externe foutopsporing uitbreiding uitschakelen met de rechtermuisknop op het cluster in **Cloud Explorer** en kies **foutopsporing uitschakelen**
   
    ![Foutopsporing op afstand uitschakelen][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Streaming traceringen vanaf een externe clusterknooppunt
U staat ook kunnen stroom traceringen rechtstreeks vanaf een externe clusterknooppunt als Visual Studio. Deze functie kunt u stroom ETW-traceringsgebeurtenissen, op het knooppunt van een Service Fabric-cluster geproduceerd.

> [!NOTE]
> Dit onderdeel vereist [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) en [Azure SDK voor .NET 2.9](https://azure.microsoft.com/downloads/).
> Deze functie ondersteunt alleen clusters die worden uitgevoerd in Azure.
> 
> 

<!-- -->
> [!WARNING]
> Streaming traceringen is bedoeld voor ontwikkel-/ Testscenario's en niet moet worden gebruikt in een productieomgeving vanwege de gevolgen voor de actieve toepassingen.
> In een productiescenario verstandig doorsturen van gebeurtenissen met behulp van Azure Diagnostics.
> 
> 

1. Navigeer naar uw cluster in **Cloud Explorer**, klik met de rechtermuisknop en kies **Streaming traceringen inschakelen**
   
    ![Externe streaming traceringen inschakelen][enablestreamingtraces]
   
    Hiermee wordt het proces voor het inschakelen van de streaming traceringen-extensie op uw clusterknooppunten, evenals de vereiste netwerkconfiguraties starten.
2. Vouw de **knooppunten** -element in **Cloud Explorer**, met de rechtermuisknop op het knooppunt dat u wilt streamen traceringen uit en kies **weergave Streaming traceringen**
   
    ![Externe streaming traceringen weergeven][viewremotestreamingtraces]
   
    Herhaal stap 2 voor zoveel knooppunten als u wilt zien van traceringen uit. Elke stroom knooppunten wordt in een speciale venster weergegeven.
   
    U bent nu kunnen zien de traceringen verzonden door het Service Fabric en uw services. Als u filteren van de gebeurtenissen wilt zodat alleen een specifieke toepassing worden weergegeven, gewoon Typ de naam van de toepassing in het filter.
   
    ![Streaming traceringen weergeven][viewingstreamingtraces]
3. Wanneer u klaar bent streaming traceringen van uw cluster, kunt u externe streaming traceringen, uitschakelen met de rechtermuisknop op het cluster in **Cloud Explorer** en kies **Streaming traceringen uitschakelen**
   
    ![Externe streaming traceringen uitschakelen][disablestreamingtraces]

## <a name="next-steps"></a>Volgende stappen
* [Een Service Fabric-service testen](service-fabric-testability-overview.md).
* [Beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
