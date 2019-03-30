---
title: Diagnostische gegevens van Azure Service Fabric Stateful Reliable Services | Microsoft Docs
description: Diagnosefunctionaliteit voor Stateful Reliable Services in Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: ae0e8f99-69ab-4d45-896d-1fa80ed45659
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/24/2018
ms.author: dekapur
ms.openlocfilehash: 96fc58ca8456e9a2e539e432f0ed85f6edc1a54f
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58664112"
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnosefunctionaliteit voor Stateful Reliable Services
De Azure Service Fabric Stateful Reliable Services StatefulServiceBase-klasse verzendt [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) gebeurtenissen die kunnen worden gebruikt voor fouten opsporen in de service bieden inzicht in hoe de runtime is functioneren en kunnen oplossen.

## <a name="eventsource-events"></a>Gebeurtenisbron gebeurtenissen
De naam van de gebeurtenisbron voor de klasse Stateful Reliable Services StatefulServiceBase is 'Microsoft-service fabric-Services'. Gebeurtenissen uit deze gebeurtenisbron worden weergegeven in de [diagnostische gebeurtenissen](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) venster wanneer de service wordt [foutopsporing in Visual Studio](service-fabric-debugging-your-application.md).

Voorbeelden van hulpprogramma's en technologieën die hulp bieden bij het verzamelen en/of EventSource gebeurtenissen bekijken zijn [voorbeeld](https://www.microsoft.com/download/details.aspx?id=28567), [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md), en de [Microsoft TraceEvent Library](https://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Gebeurtenissen
| Gebeurtenisnaam | Gebeurtenis-id | Niveau | Beschrijving van gebeurtenis |
| --- | --- | --- | --- |
| StatefulRunAsyncInvocation |1 |Informatief |Verzonden wanneer de service RunAsync-taak wordt gestart |
| StatefulRunAsyncCancellation |2 |Informatief |Wanneer de service RunAsync-taak is geannuleerd |
| StatefulRunAsyncCompletion |3 |Informatief |Verzonden wanneer de service RunAsync-taak is voltooid |
| StatefulRunAsyncSlowCancellation |4 |Waarschuwing |Wanneer de service RunAsync-taak te lang duren om uit te voeren annulering |
| StatefulRunAsyncFailure |5 |Fout |Wanneer de taak service RunAsync een uitzondering genereert |

## <a name="interpret-events"></a>Gebeurtenissen interpreteren
StatefulRunAsyncInvocation StatefulRunAsyncCompletion en StatefulRunAsyncCancellation gebeurtenissen zijn handig voor de schrijver van de service om te begrijpen van de levenscyclus van een service, evenals de timing van als een service wordt gestart, wordt geannuleerd of is voltooid. Deze informatie kan nuttig zijn bij foutopsporing in problemen met de service of informatie over de levenscyclus van de service.

Schrijvers van de service moeten aandacht besteedt aan gebeurtenissen StatefulRunAsyncSlowCancellation en StatefulRunAsyncFailure omdat ze duiden op problemen met de service.

StatefulRunAsyncFailure wordt verzonden wanneer de taak service RunAsync() een uitzondering genereert. Een uitzondering geeft meestal aan fout of bug in de service. Bovendien wordt de uitzondering met de service mislukt, zodat deze wordt verplaatst naar een ander knooppunt. Deze bewerking kan duur zijn en inkomende aanvragen kan vertraging terwijl de service is verplaatst. Schrijvers van de service moeten de oorzaak van de uitzondering en deze indien mogelijk, te beperken.

StatefulRunAsyncSlowCancellation wordt verzonden wanneer een aanvraag voor annulering voor de taak RunAsync langer duurt dan vier seconden. Wanneer een service te lang annulering duurt, het is van invloed op de mogelijkheid van de service om snel opnieuw worden gestart op een ander knooppunt. Dit scenario mogelijk invloed op de algemene beschikbaarheid van de service.

## <a name="performance-counters"></a>Prestatiemeteritems
De runtime van Reliable Services definieert de volgende categorieën voor prestatiemeteritems:

| Categorie | Description |
| --- | --- |
| Transactionele replicator voor Service Fabric |Prestatiemeteritems specifiek voor de transactionele Replicator voor Azure Service Fabric |
| Service Fabric TStore |Prestatiemeteritems specifiek voor de Azure Service Fabric TStore |

De transactionele Replicator voor Service Fabric wordt gebruikt door de [betrouwbare status Manager](service-fabric-reliable-services-reliable-collections-internals.md) voor het repliceren van transacties binnen een bepaalde set [replica's](service-fabric-concepts-replica-lifecycle.md).

De Service Fabric TStore is een onderdeel dat wordt gebruikt [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections-internals.md) voor het opslaan en ophalen van sleutel / waarde-paren.

De [Windows Performance Monitor](https://technet.microsoft.com/library/cc749249.aspx) toepassing die is standaard beschikbaar in het Windows-besturingssysteem kan worden gebruikt voor het verzamelen en weergeven van gegevens van prestatiemeteritems. [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) is een andere optie voor het verzamelen van gegevens van prestatiemeteritems en uploaden naar Azure-tabellen.

### <a name="performance-counter-instance-names"></a>Namen van prestatiemeteritems exemplaar
Een cluster met een groot aantal betrouwbare services of partities van de betrouwbare service heeft een groot aantal transactionele replicator prestaties teller exemplaren. Dit is ook het geval is bij TStore-prestatiemeteritems, maar ook wordt vermenigvuldigd met het aantal betrouwbare woordenboeken en betrouwbare wachtrijen gebruikt. De namen van prestatiemeteritems exemplaar kunnen helpen bij het identificeren van de specifieke [partitie](service-fabric-concepts-partitioning.md), service-replica en state-provider in het geval van TStore, waaraan het exemplaar van prestatiemeteritem is gekoppeld.

#### <a name="service-fabric-transactional-replicator-category"></a>Transactionele Replicator voor service Fabric-categorie
Voor de categorie `Service Fabric Transactional Replicator`, de namen van de teller-exemplaar zijn in de volgende indeling:

`ServiceFabricPartitionId:ServiceFabricReplicaId`

*ServiceFabricPartitionId* is van de tekenreeksvoorstelling van de Service Fabric-partitie-ID die het exemplaar van prestatiemeteritem is gekoppeld. De partitie-ID is een GUID en de tekenreeks is die worden gegenereerd via [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) met indelingsopgave "D".

*ServiceFabricReplicaId* is de ID die is gekoppeld aan een bepaalde replica van een betrouwbare service. Replica-ID is opgenomen in de naam van het exemplaar om te zorgen dat deze zijn uniek en voorkomen van conflicten met andere prestaties teller exemplaren die worden gegenereerd door de dezelfde partitie. Meer informatie over de replica's en hun rol in betrouwbare services vindt u [hier](service-fabric-concepts-replica-lifecycle.md).

De volgende naam van het exemplaar is normaal dat voor een item onder de `Service Fabric Transactional Replicator` categorie:

`00d0126d-3e36-4d68-98da-cc4f7195d85e:131652217797162571`

In het voorgaande voorbeeld `00d0126d-3e36-4d68-98da-cc4f7195d85e` is van de tekenreeksvoorstelling van de Service Fabric-partitie-ID, en `131652217797162571` is de replica-ID.

#### <a name="service-fabric-tstore-category"></a>Service Fabric TStore categorie
Voor de categorie `Service Fabric TStore`, de namen van de teller-exemplaar zijn in de volgende indeling:

`ServiceFabricPartitionId:ServiceFabricReplicaId:ServiceFabricStateProviderId_PerformanceCounterInstanceDifferentiator`

*ServiceFabricPartitionId* is van de tekenreeksvoorstelling van de Service Fabric-partitie-ID die het exemplaar van prestatiemeteritem is gekoppeld. De partitie-ID is een GUID en de tekenreeks is die worden gegenereerd via [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) met indelingsopgave "D".

*ServiceFabricReplicaId* is de ID die is gekoppeld aan een bepaalde replica van een betrouwbare service. Replica-ID is opgenomen in de naam van het exemplaar om te zorgen dat deze zijn uniek en voorkomen van conflicten met andere prestaties teller exemplaren die worden gegenereerd door de dezelfde partitie. Meer informatie over de replica's en hun rol in betrouwbare services vindt u [hier](service-fabric-concepts-replica-lifecycle.md).

*ServiceFabricStateProviderId* is de ID die is gekoppeld aan een state-provider in een betrouwbare service. Status-ID van Provider is opgenomen in de naam van het exemplaar naar een TStore onderscheiden van andere.

*PerformanceCounterInstanceDifferentiator* is een onderscheidende-ID die is gekoppeld aan een exemplaar van prestatiemeteritem binnen een state-provider. Dit kenmerk is opgenomen in de naam van het exemplaar om te zorgen dat deze zijn uniek en voorkomen van conflicten met andere prestaties teller exemplaren die worden gegenereerd door de dezelfde state-provider.

De volgende naam van het exemplaar is normaal dat voor een item onder de `Service Fabric TStore` categorie:

`00d0126d-3e36-4d68-98da-cc4f7195d85e:131652217797162571:142652217797162571_1337`

In het voorgaande voorbeeld `00d0126d-3e36-4d68-98da-cc4f7195d85e` is van de tekenreeksvoorstelling van de Service Fabric-partitie-ID, `131652217797162571` is de replica-ID, `142652217797162571` wordt de foutstatus-ID-provider en `1337` is van de prestaties teller exemplaar gegevensbehoud.

### <a name="transactional-replicator-performance-counters"></a>Transactionele Replicator-prestatiemeteritems

De Reliable Services-runtime verzendt de volgende gebeurtenissen onder de `Service Fabric Transactional Replicator` categorie

 Naam van het prestatiemeteritem | Description |
| --- | --- |
| Transactiebewerkingen starten/sec | Het aantal nieuwe schrijven transacties per seconde worden gemaakt.|
| Transactiebewerkingen/sec | Het aantal bewerkingen toevoegen/bijwerken/verwijderen die worden uitgevoerd op een betrouwbare verzamelingen per seconde.|
| Gem. Latentie van leegmaken (ms) | Het aantal bytes dat wordt leeggemaakt op de schijf met de transactionele replicatie per seconde |
| Beperkte bewerkingen/sec | Het aantal bewerkingen per seconde door de transactionele Replicator vanwege een beperking afgewezen. |
| Gem. Transactie ms en doorvoeren | Gemiddelde doorvoervertraging per transactie in milliseconden |
| Gem. Latentie van leegmaken (ms) | Gemiddelde duur van de schijf leegmaken bewerkingen, gestart door de transactionele Replicator in milliseconden |

### <a name="tstore-performance-counters"></a>Prestatiemeteritems TStore

De Reliable Services-runtime verzendt de volgende gebeurtenissen onder de `Service Fabric TStore` categorie

 Naam van het prestatiemeteritem | Description |
| --- | --- |
| Aantal items | Het aantal items in de store.|
| Schijfgrootte | De totale schijfgrootte in bytes, van controlepuntbestanden voor de store.|
| Geschreven bytes per seconde voor controlepuntbestand | Het aantal bytes per seconde voor het meest recente controlepunt geschreven.|
| Overgedragen bytes per seconde voor kopiëren van schijf | Het aantal bytes op de schijf (op de primaire replica) gelezen of geschreven (op een secundaire replica) per seconde tijdens een kopie van de store.|

## <a name="next-steps"></a>Volgende stappen
[Gebeurtenisbron providers in het voorbeeld](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
