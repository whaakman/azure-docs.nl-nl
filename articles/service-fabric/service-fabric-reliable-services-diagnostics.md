---
title: Diagnostische gegevens van Azure Service Fabric betrouwbare Stateful Services | Microsoft Docs
description: Diagnostische functionaliteit voor betrouwbare Stateful Services in Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: ae0e8f99-69ab-4d45-896d-1fa80ed45659
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: edcaaaf8f1619082b33195aedf1fb1abf32e85b1
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnosefunctionaliteit voor Stateful Reliable Services
De klasse Azure Service Fabric Stateful betrouwbare Services StatefulServiceBase verzendt [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) gebeurtenissen die kunnen worden gebruikt voor fouten opsporen in de service biedt mogelijk inzicht in hoe de runtime is functioneren en oplossen van problemen met.

## <a name="eventsource-events"></a>EventSource gebeurtenissen
De naam van de EventSource voor de klasse Stateful betrouwbare Services StatefulServiceBase is 'Microsoft-ServiceFabric-Services.' Gebeurtenissen van de bron van deze gebeurtenissen worden weergegeven in de [diagnostische gebeurtenissen](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) venster wanneer de service wordt [fouten worden opgespoord in Visual Studio](service-fabric-debugging-your-application.md).

Voorbeelden van hulpprogramma's en -technologieën die helpen bij het verzamelen en/of het bekijken van EventSource gebeurtenissen zijn [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md), en de [Microsoft TraceEvent Library](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Gebeurtenissen
| De naam van gebeurtenis | Gebeurtenis-ID | Niveau | Beschrijving van gebeurtenis |
| --- | --- | --- | --- |
| StatefulRunAsyncInvocation |1 |Informatief |Verzonden wanneer de taak service RunAsync wordt gestart |
| StatefulRunAsyncCancellation |2 |Informatief |Verzonden wanneer de taak service RunAsync wordt geannuleerd |
| StatefulRunAsyncCompletion |3 |Informatief |Verzonden wanneer de service RunAsync-taak is voltooid |
| StatefulRunAsyncSlowCancellation |4 |Waarschuwing |Verzonden wanneer de taak service RunAsync duurt te lang annulering voltooien |
| StatefulRunAsyncFailure |5 |Fout |Wanneer de taak service RunAsync een uitzondering wordt verzonden |

## <a name="interpret-events"></a>Gebeurtenissen interpreteren
Gebeurtenissen StatefulRunAsyncInvocation, StatefulRunAsyncCompletion en StatefulRunAsyncCancellation zijn handig voor de schrijver van de service om te begrijpen van de levenscyclus van een service, evenals de tijd voor wanneer een service wordt gestart, wordt geannuleerd of is voltooid. Deze informatie kan nuttig zijn wanneer foutopsporing van problemen met de service of het begrijpen van de levenscyclus van de service.

Schrijvers van de service moeten aandacht besteedt aan gebeurtenissen StatefulRunAsyncSlowCancellation en StatefulRunAsyncFailure omdat ze problemen met de service aangeeft.

StatefulRunAsyncFailure wordt verzonden wanneer de taak service RunAsync() er een uitzondering gegenereerd. Een uitzondering veroorzaakt geeft meestal aan een fout of een fout in de service. De uitzondering wordt bovendien de service mislukt, zodat deze is verplaatst naar een ander knooppunt. Deze bewerking kan duur zijn en binnenkomende aanvragen kan worden vertraagd terwijl de service wordt verplaatst. Schrijvers van de service moeten de oorzaak van de uitzondering en deze indien mogelijk te beperken.

StatefulRunAsyncSlowCancellation wordt verzonden wanneer een annuleringsaanvraag voor de RunAsync-taak langer dan vier seconden duurt. Wanneer een service te lang annulering duurt, het is van invloed op de mogelijkheid van de service te snel opnieuw worden gestart op een ander knooppunt. Dit scenario kan invloed hebben op de algemene beschikbaarheid van de service.

## <a name="next-steps"></a>Volgende stappen
[EventSource providers op PerfView](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
