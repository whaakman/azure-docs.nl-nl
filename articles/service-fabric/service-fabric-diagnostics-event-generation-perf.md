---
title: Azure Service Fabric-prestaties controleren | Microsoft Docs
description: Meer informatie over prestatiemeteritems voor controle en diagnose van Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: ee1608c40801f568b38ace4670b0d5ea7f73003c
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663074"
---
# <a name="performance-metrics"></a>Metrische gegevens voor prestaties

Metrische gegevens moeten worden verzameld voor meer informatie over de prestaties van uw cluster, evenals de toepassingen die erin worden uitgevoerd. Service Fabric-clusters, wordt u aangeraden de volgende prestatiemeteritems verzamelen.

## <a name="nodes"></a>Knooppunten

Voor de computers in uw cluster, kunt u het verzamelen van de volgende prestatiemeteritems om beter te begrijpen van de belasting op elke machine en juiste cluster schalen beslissingen te maken.

| Categorie | Naam van teller |
| --- | --- |
| Logische schijf | Logical Disk Free Space |
| PhysicalDisk(per Disk) | Gem. Wachtrijlengte voor schijf lezen |
| PhysicalDisk(per Disk) | Gem. Wachtrijlengte voor schijf schrijven |
| PhysicalDisk(per Disk) | Gem. Schijf sec/lezen |
| PhysicalDisk(per Disk) | Gem. Schijf sec/schrijven |
| PhysicalDisk(per Disk) | Schijf lezen per seconde |
| PhysicalDisk(per Disk) | Bytes gelezen op schijf/sec |
| PhysicalDisk(per Disk) | Schijf schrijven per seconde |
| PhysicalDisk(per Disk) | Bytes geschreven naar schijf/sec |
| Geheugen | Beschikbare megabytes (MB) |
| PagingFile | % Gebruik |
| Processor(Total) | % Processortijd |
| Proces (per service) | % Processortijd |
| Proces (per service) | Proces-ID |
| Proces (per service) | Privébytes |
| Proces (per service) | Aantal threads |
| Proces (per service) | Virtuele Bytes |
| Proces (per service) | Werkset |
| Proces (per service) | Werkset - privé |
| Netwerk Interface(all-instances) | Bytes recd |
| Netwerk Interface(all-instances) | Verzonden bytes |
| Netwerk Interface(all-instances) | Totaal aantal bytes |
| Netwerk Interface(all-instances) | Wachtrijlengte uitvoer |
| Netwerk Interface(all-instances) | Uitgaande pakketten dat is verwijderd |
| Netwerk Interface(all-instances) | Genegeerde ontvangen pakketten |
| Netwerk Interface(all-instances) | Uitgaande pakketten met fouten |
| Netwerk Interface(all-instances) | Ontvangen pakketten met fouten |

## <a name="net-applications-and-services"></a>.NET-toepassingen en services

De volgende prestatiemeteritems verzamelen als u .NET-services met uw cluster implementeert. 

| Categorie | Naam van teller |
| --- | --- |
| .NET CLR-geheugen (per service) | Proces-id |
| .NET CLR-geheugen (per service) | # Totaal toegewezen Bytes |
| .NET CLR-geheugen (per service) | # Totaal aantal Bytes gereserveerd |
| .NET CLR-geheugen (per service) | Aantal bytes in alle Heaps |
| .NET CLR-geheugen (per service) | Groot Object-Heap-grootte |
| .NET CLR-geheugen (per service) | # GC verwerkt |
| .NET CLR-geheugen (per service) | # Verzamelingen van generatie 0 |
| .NET CLR-geheugen (per service) | # Verzamelingen van generatie 1 |
| .NET CLR-geheugen (per service) | # Verzamelingen van generatie 2 |
| .NET CLR-geheugen (per service) | % GC-tijd in |

### <a name="service-fabrics-custom-performance-counters"></a>De aangepaste prestatiemeteritems van service Fabric

Service Fabric genereert een aanzienlijke hoeveelheid van het aangepaste prestatiemeteritems. Als u de SDK is geïnstalleerd hebt, kunt u de uitgebreide lijst zien op uw Windows-computer in uw toepassing Prestatiemeter (Start > Prestatiemeter). 

In de toepassingen die u implementeert voor uw cluster, als u van Reliable Actors gebruikmaakt toevoegen tellers van `Service Fabric Actor` en `Service Fabric Actor Method` categorieën (Zie [Service Fabric Reliable Actors-Diagnostics](service-fabric-reliable-actors-diagnostics.md)).

Als u betrouwbare Services of Service voor externe toegang gebruikt, hebben we op dezelfde manier `Service Fabric Service` en `Service Fabric Service Method` teller categorieën dat u over items, Zie verzamelen moet [bewaking met service remoting](service-fabric-reliable-serviceremoting-diagnostics.md) en [betrouwbare Services-prestatiemeteritems](service-fabric-reliable-services-diagnostics.md#performance-counters). 

Als u betrouwbare verzamelingen, raden wij aan toe te voegen de `Avg. Transaction ms/Commit` uit de `Service Fabric Transactional Replicator` voor het verzamelen van de gemiddelde doorvoervertraging per transactie metrisch gegeven.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [gebeurtenisgeneratie op het niveau van het platform](service-fabric-diagnostics-event-generation-infra.md) in Service Fabric
* Verzamelen van metrische gegevens voor prestaties via [Log Analytics-agent](service-fabric-diagnostics-oms-agent.md)
