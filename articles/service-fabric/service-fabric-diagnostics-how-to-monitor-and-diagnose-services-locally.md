---
title: Fouten opsporen in Azure microservices in Windows | Microsoft Docs
description: Informatie over het bewaken en onderzoeken van uw services die zijn geschreven met behulp van Microsoft Azure Service Fabric op een lokale ontwikkelcomputer.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: edcc0631-ed2d-45a3-851d-2c4fa0f4a326
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2017
ms.author: dekapur
ms.openlocfilehash: 08998340afb2f242b9a268331607b0d1ddb9b0c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Controle en diagnose van services in de instellingen voor een lokale computer-ontwikkeling
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
> 
> 

Bewaking, detecteren, onderzoeken en het oplossen van problemen kunt services wilt gaan met minimale onderbreking van de gebruikerservaring. Controle en diagnostische gegevens zijn essentieel belang is in een werkelijke geïmplementeerde productie-omgeving, afhankelijk de efficiëntie van een vergelijkbaar model overstap tijdens de ontwikkeling van services zodat ze werken wanneer u naar een echte setup. Service Fabric maakt het eenvoudig voor ontwikkelaars van de service voor het implementeren van diagnostische gegevens die naadloos voor zowel lokale ontwikkeling één machine-instellingen als de echte productie cluster instellingen werkt.

## <a name="event-tracing-for-windows"></a>Gebeurtenistracering voor Windows
[Event Tracing voor Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) is de aanbevolen technologie voor tracering van berichten in Service Fabric. Er zijn enkele voordelen van het gebruik van ETW:

* **ETW is snel.** Het is ontwikkeld als een tracering technologie met minimale gevolgen voor een uitvoeringstijden code.
* **ETW-tracering werkt naadloos samen in lokale ontwikkelomgevingen en ook instellingen echte cluster.** Dit betekent dat u code te herschrijven uw tracering wanneer u klaar bent voor het implementeren van uw code naar een echte cluster geen hebt.
* **Service Fabric systeemcode gebruikt ook ETW voor interne tracering.** Hiermee kunt u uw toepassingstraceringen interleaved met traceringen van Service Fabric-systeem weergeven. Ook kunt u gemakkelijker inzicht in de takenreeksen en relaties tussen uw toepassingscode en gebeurtenissen in het onderliggende systeem.
* **Er is een ingebouwde ondersteuning voor Service Fabric Visual Studio tools ETW-gebeurtenissen weergeven.** ETW-gebeurtenissen worden weergegeven in de weergave van diagnostische gebeurtenissen van Visual Studio als Visual Studio correct is geconfigureerd met Service Fabric. 

## <a name="view-service-fabric-system-events-in-visual-studio"></a>Service Fabric-systeemgebeurtenissen bekijken in Visual Studio
Service Fabric verzendt ETW-gebeurtenissen om te begrijpen wat er gebeurt in het platform voor ontwikkelaars van toepassingen. Als u dit nog niet hebt gedaan, gaat u verder gaan en volg de stappen in [maken van uw eerste toepassing in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md). Deze informatie kunt u een toepassing actief en werkend ophalen met de diagnostische gebeurtenissen Viewer de traceringsberichten weergegeven.

1. Als de diagnostische gegevens gebeurtenissen venster wordt niet automatisch wordt weergegeven, gaat u naar de **weergave** tabblad in Visual Studio, kiest u **overige vensters** en vervolgens **diagnostische logboeken**.
2. Elke gebeurtenis heeft standaard metagegevens die u leest het knooppunt, toepassing en service afkomstig is van de gebeurtenis. U kunt ook de lijst met gebeurtenissen filteren met behulp van de **gebeurtenissen filteren** vak aan de bovenkant van het venster gebeurtenissen. Bijvoorbeeld, u kunt filteren op **knooppuntnaam** of **servicenaam.** En wanneer u de details van gebeurtenis kijkt, kunt u ook onderbreken met behulp van de **onderbreken** knop aan de bovenkant van het venster gebeurtenissen en later hervatten zonder verlies van gebeurtenissen.
   
   ![Visual Studio diagnostische gegevens van de Gebeurtenissenviewer](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/DiagEventsExamples2.png)

## <a name="add-your-own-custom-traces-to-the-application-code"></a>Uw eigen aangepaste traceringen toevoegen aan de toepassingscode
De Service Fabric Visual Studio-projectsjablonen bevatten voorbeeldcode. De code staat het toevoegen van aangepaste code ETW toepassingstraceringen die worden weergegeven in de Visual Studio ETW-viewer naast system traceringen van Service Fabric. Het voordeel van deze methode is dat de metagegevens wordt automatisch toegevoegd aan traceringen en Visual Studio diagnostische gebeurtenissen Viewer al geconfigureerd is om deze weer te geven.

Voor projecten gemaakt op basis van de **servicesjablonen** (stateless of stateful) zoeken de `RunAsync` implementatie:

1. De aanroep van `ServiceEventSource.Current.ServiceMessage` in de `RunAsync` methode toont een voorbeeld van een aangepaste ETW-tracering van de toepassingscode.
2. In de **ServiceEventSource.cs** -bestand, vindt u een overbelasting voor de `ServiceEventSource.ServiceMessage` methode die moet worden gebruikt voor hoge frequentie gebeurtenissen om prestatieredenen.

Voor projecten gemaakt op basis van de **actor sjablonen** (stateless of stateful):

1. Open de **'ProjectName'.cs** waar het bestand *ProjectName* is de naam die u hebt gekozen voor Visual Studio-project.  
2. Zoek de code `ActorEventSource.Current.ActorMessage(this, "Doing Work");` in de *DoWorkAsync* methode.  Dit is een voorbeeld van een aangepaste ETW-tracering van toepassingscode geschreven.  
3. In het bestand **ActorEventSource.cs**, vindt u een overbelasting voor de `ActorEventSource.ActorMessage` methode die moet worden gebruikt voor hoge frequentie gebeurtenissen om prestatieredenen.

Na het toevoegen van aangepaste ETW-tracering toe aan uw servicecode, kunt u bouwen, implementeren en uitvoeren van de toepassing opnieuw om te zien van uw gebeurtenis(sen) in de diagnostische gebeurtenissen Viewer. Als u de toepassing met fouten opsporen **F5**, de diagnostische gebeurtenissen Viewer wordt automatisch geopend.

## <a name="next-steps"></a>Volgende stappen
Dezelfde tracering-code die u hebt toegevoegd aan uw toepassing hierboven voor lokale diagnostische gegevens werken met hulpprogramma's die u gebruiken kunt om deze gebeurtenissen wanneer uw toepassing wordt uitgevoerd op een Azure-cluster weer te geven. Bekijk deze artikelen die de verschillende opties voor de hulpprogramma's worden behandeld en wordt beschreven hoe u kunt deze instellen.

* [Het verzamelen van logboeken met diagnostische Azure-gegevens](service-fabric-diagnostics-how-to-setup-wad.md)
* [Gebeurtenis-aggregatie en verzameling op basis van EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md)

