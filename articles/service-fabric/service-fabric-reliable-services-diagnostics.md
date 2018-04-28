---
title: Diagnostische gegevens van Azure Service Fabric betrouwbare Stateful Services | Microsoft Docs
description: Diagnostische functionaliteit voor betrouwbare Stateful Services in Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ae0e8f99-69ab-4d45-896d-1fa80ed45659
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 3ed03194ca095d539d10081578fa71c748ba1d23
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnosefunctionaliteit voor Stateful Reliable Services
De klasse Azure Service Fabric Stateful betrouwbare Services StatefulServiceBase verzendt [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) gebeurtenissen die kunnen worden gebruikt voor fouten opsporen in de service biedt mogelijk inzicht in hoe de runtime is functioneren en oplossen van problemen met.

## <a name="eventsource-events"></a>EventSource gebeurtenissen
De naam van de EventSource voor de klasse Stateful betrouwbare Services StatefulServiceBase is 'Microsoft-ServiceFabric-Services.' Gebeurtenissen van de bron van deze gebeurtenissen worden weergegeven in de [diagnostische gebeurtenissen](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) venster wanneer de service wordt [fouten worden opgespoord in Visual Studio](service-fabric-debugging-your-application.md).

Voorbeelden van hulpprogramma's en -technologieën die helpen bij het verzamelen en/of het bekijken van EventSource gebeurtenissen zijn [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md), en de [Microsoft TraceEvent Library](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Gebeurtenissen
| De naam van gebeurtenis | Gebeurtenis-id | Niveau | Beschrijving van gebeurtenis |
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

## <a name="performance-counters"></a>Prestatiemeteritems
De runtime Reliable Services definieert de volgende categorieën voor prestatiemeteritems:

| Category | Beschrijving |
| --- | --- |
| Transactionele replicator voor Service Fabric |Items die specifiek bij de Azure Service Fabric Transactional Replicator |

Service Fabric Transactional Replicator wordt gebruikt door de [betrouwbare statusbeheer](service-fabric-reliable-services-reliable-collections-internals.md) voor replicatie van transacties binnen een bepaalde set [replica's](service-fabric-concepts-replica-lifecycle.md). 

De [Windows Prestatiemeter](https://technet.microsoft.com/library/cc749249.aspx) toepassing die standaard beschikbaar in de Windows-besturingssysteem kan worden gebruikt voor het verzamelen en weergeven van gegevens van prestatiemeteritems. [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) is een andere optie voor het verzamelen van gegevens van prestatiemeteritems en uploaden naar Azure-tabellen.

### <a name="performance-counter-instance-names"></a>Namen van prestatiemeteritems exemplaar
Een cluster met een groot aantal betrouwbare services of servicepartities betrouwbare heeft een groot aantal exemplaren van transactional replicator prestaties teller. De namen van prestatiemeteritems exemplaar kunnen helpen bij het identificeren van de specifieke [partitie](service-fabric-concepts-partitioning.md) en service-replica die het exemplaar van prestatiemeteritem is gekoppeld.

#### <a name="service-fabric-transactional-replicator-category"></a>Service Fabric Transactional Replicator categorie
Voor de categorie `Service Fabric Transactional Replicator`, de teller exemplaarnamen worden in de volgende indeling:

`ServiceFabricPartitionId:ServiceFabricReplicaId`

*ServiceFabricPartitionId* de tekenreeksweergave van de Service Fabric-partitie-ID die het exemplaar van prestatiemeteritem is gekoppeld. De partitie-ID is een GUID en de tekenreeksvoorstelling wordt gegenereerd via [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) met indelingsopgave "D".

*ServiceFabricReplicaId* is de ID die is gekoppeld aan een bepaalde replica van een betrouwbare service. Replica-ID is opgenomen in de naam van het exemplaar om te controleren van de uniekheid en het voorkomen van conflicten met andere prestaties teller-exemplaren die worden gegenereerd door dezelfde partitie. Meer informatie over de replica's en hun rol binnen betrouwbare services vindt [hier](service-fabric-concepts-replica-lifecycle.md).

De volgende naam van het exemplaar is Typerend voor een item onder de `Service Fabric Transactional Replicator` categorie:

`00d0126d-3e36-4d68-98da-cc4f7195d85e:131652217797162571`

In het voorgaande voorbeeld `00d0126d-3e36-4d68-98da-cc4f7195d85e` de tekenreeksweergave van de Service Fabric partitie-ID en `131652217797162571` is van de replica-ID.

### <a name="transactional-replicator-performance-counters"></a>Prestatiemeteritems voor transactionele replicatie

De runtime Reliable Services verzendt de volgende gebeurtenissen onder de `Service Fabric Transactional Replicator` categorie

 Naam van het meteritem | Beschrijving |
| --- | --- |
| Transactiebewerkingen starten/sec | Het aantal nieuwe schrijven transacties per seconde worden gemaakt.|
| Transactiebewerkingen/sec | Het aantal bewerkingen voor toevoegen, bijwerken, verwijderen is uitgevoerd op betrouwbare verzamelingen per seconde.|
| Gem. Leegmaken latentie (ms) | Het aantal bytes dat wordt leeggemaakt op de schijf met de transactionele replicatie per seconde |
| Beperkte bewerkingen/sec | Het aantal bewerkingen per seconde met de transactionele replicatie vanwege een beperking geweigerd. |
| Gem. Transactie-ms/Commit | Gemiddelde doorvoervertraging per transactie in milliseconden |
| Gem. Leegmaken latentie (ms) | Gemiddelde duur van het leegmaken schijfbewerkingen geïnitieerd door de transactionele replicatie in milliseconden |

## <a name="next-steps"></a>Volgende stappen
[EventSource providers op PerfView](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
