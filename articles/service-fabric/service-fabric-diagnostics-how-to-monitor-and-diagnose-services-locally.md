---
title: Fouten opsporen in Azure Service Fabric-apps in Windows | Microsoft Docs
description: Informatie over het bewaken en diagnoses uitvoeren van de services die zijn geschreven met behulp van Microsoft Azure Service Fabric op een lokale ontwikkelcomputer.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: edcc0631-ed2d-45a3-851d-2c4fa0f4a326
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 740864276b8d4d7a40a263a0d6d2e09c3534dbd6
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190901"
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Controle en diagnose van services in een lokale machine development setup
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
> 
> 

Bewaking, detecteren, onderzoeken en oplossen van problemen kunt voor diensten om door te gaan met minimale onderbreking van de gebruikerservaring. Controle en diagnose zijn essentieel belang is in een werkelijke geïmplementeerde productie-omgeving, afhankelijk de efficiëntie overstappen op een vergelijkbare model tijdens de ontwikkeling van services om te controleren of dat ze werken wanneer u naar de installatie van een echte verplaatst. Service Fabric kunt eenvoudig service waarmee ontwikkelaars voor het implementeren van diagnostische gegevens die naadloos kan worden gebruikt voor zowel lokale ontwikkeling van één machine-instellingen en instellingen voor real-world productie-cluster.

## <a name="event-tracing-for-windows"></a>Event Tracing voor Windows
[Event Tracing voor Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) is de aanbevolen technologie voor berichten voor tracering in Service Fabric. Er zijn enkele voordelen van het gebruik van ETW:

* **ETW is snel.** Het is ontwikkeld als een tracering-technologie met minimale impact op de uitvoertijd van code.
* **ETW-tracering werkt naadloos met lokale ontwikkelomgevingen en ook instellingen voor real-world-cluster.** Dit betekent dat u hoeft te herschrijven van uw code tracering wanneer u klaar bent voor uw code implementeren in een echt cluster.
* **Service Fabric-systeemcode maakt ook gebruik van ETW voor interne tracering.** Hiermee kunt u om uw toepassingstraces interleaved met Service Fabric-systeem traceringen weer te geven. Ook kunt u eenvoudig inzicht krijgen in de reeksen en relaties tussen uw toepassingscode en gebeurtenissen in het onderliggende systeem.
* **Er is een ingebouwde ondersteuning in Service Fabric-Visual Studio-hulpprogramma's om ETW-gebeurtenissen weer te geven.** ETW-gebeurtenissen worden weergegeven in de weergave van diagnostische gebeurtenissen van Visual Studio als Visual Studio correct is geconfigureerd met Service Fabric. 

## <a name="view-service-fabric-system-events-in-visual-studio"></a>Bekijk systeemgebeurtenissen voor Service Fabric in Visual Studio
Service Fabric verzendt ETW-gebeurtenissen om te begrijpen wat er gebeurt in het platform voor ontwikkelaars van toepassingen. Als u dit nog niet hebt gedaan, gaat u verder en volg de stappen in [maken van uw eerste toepassing in Visual Studio](service-fabric-tutorial-create-dotnet-app.md). Deze informatie krijgt u een toepassing actief en werkend krijgen met de Viewer voor diagnostische gebeurtenissen van de traceringsberichten.

1. Als de diagnostische gegevens over het venster gebeurtenissen niet automatisch wordt weergegeven, gaat u naar de **weergave** tabblad in Visual Studio, kiest u **andere Windows** en vervolgens **Viewer voor diagnostische gebeurtenissen**.
2. Elke gebeurtenis is standaard metagegevens die u leest het knooppunt, toepassing en service afkomstig is van de gebeurtenis. U kunt ook de lijst met gebeurtenissen filteren met behulp van de **gebeurtenissen filteren** vak aan de bovenkant van het venster gebeurtenissen. U kunt bijvoorbeeld filteren op **knooppuntnaam** of **servicenaam.** En als u geïnteresseerd bent in de details van gebeurtenis, kunt u ook onderbreken met behulp van de **onderbreken** knop aan de bovenkant van het venster gebeurtenissen en later hervatten zonder verlies van gebeurtenissen.
   
   ![Visual Studio diagnostische logboeken met gebeurtenissen weer](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/DiagEventsExamples2.png)

## <a name="add-your-own-custom-traces-to-the-application-code"></a>Uw eigen aangepaste traceringen in de toepassingscode toevoegen
De Service Fabric-Visual Studio-projectsjablonen bevatten voorbeelden van code. De code laat zien hoe aangepaste toepassing code ETW-traceringen die worden weergegeven in de Visual Studio ETW-viewer naast system traceringen van Service Fabric toevoegen. Het voordeel van deze methode is dat metagegevens wordt automatisch toegevoegd aan traceringen, en de Visual Studio Viewer voor diagnostische gebeurtenissen al is geconfigureerd om deze weer te geven.

Voor projecten die zijn gemaakt op basis van de **servicesjablonen** (stateless of stateful) zoeken voor de `RunAsync` implementatie:

1. De aanroep van `ServiceEventSource.Current.ServiceMessage` in de `RunAsync` methode toont een voorbeeld van een aangepaste ETW-tracering van de toepassingscode.
2. In de **ServiceEventSource.cs** -bestand, vindt u een overbelasting van de `ServiceEventSource.ServiceMessage` methode die moet worden gebruikt voor high-frequency gebeurtenissen vanwege prestatieredenen.

Voor projecten die zijn gemaakt op basis van de **actor sjablonen** (stateless of stateful):

1. Open de **"ProjectName".cs** waar het bestand *ProjectName* is de naam die u hebt gekozen voor Visual Studio-project.  
2. Zoek de code `ActorEventSource.Current.ActorMessage(this, "Doing Work");` in de *DoWorkAsync* methode.  Dit is een voorbeeld van een aangepaste ETW-tracering van toepassingscode worden geschreven.  
3. V souboru **ActorEventSource.cs**, vindt u een overbelasting van de `ActorEventSource.ActorMessage` methode die moet worden gebruikt voor high-frequency gebeurtenissen vanwege prestatieredenen.

Na het toevoegen van aangepaste ETW-tracering in de servicecode van uw, kunt u bouwen, implementeren en uitvoeren van de toepassing opnieuw om te zien van uw gebeurtenis(sen) in de Viewer voor diagnostische gebeurtenissen. Als u fouten opsporen in de toepassing met **F5**, de Viewer voor diagnostische gebeurtenissen wordt automatisch geopend.

## <a name="next-steps"></a>Volgende stappen
Dezelfde tracering code die u hebt toegevoegd aan uw toepassing hierboven voor lokale diagnostische gegevens werken met hulpprogramma's die u gebruiken kunt om deze gebeurtenissen bij het uitvoeren van uw toepassing op een Azure-cluster weer te geven. Lees deze artikelen die de verschillende opties voor de hulpprogramma's beschreven en wordt beschreven hoe u ze kunt instellen om.

* [Over het verzamelen van logboeken met diagnostische gegevens van Azure](service-fabric-diagnostics-how-to-setup-wad.md)
* [Aggregatie van de gebeurtenis en verzameling met EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md)

