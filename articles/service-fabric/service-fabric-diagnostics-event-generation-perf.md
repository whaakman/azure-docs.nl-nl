---
title: Azure Service Fabric-prestaties controleren | Microsoft Docs
description: Meer informatie over prestatiemeteritems voor controle en diagnose van Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/16/2018
ms.author: srrengar
ms.openlocfilehash: 1e6ea5d6ae321a0443631ec928912611a68346c6
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49408010"
---
# <a name="performance-metrics"></a>Metrische gegevens voor prestaties

Metrische gegevens moeten worden verzameld voor meer informatie over de prestaties van uw cluster, evenals de toepassingen die erin worden uitgevoerd. Service Fabric-clusters, wordt u aangeraden de volgende prestatiemeteritems verzamelen.

## <a name="nodes"></a>Knooppunten

Voor de computers in uw cluster, kunt u het verzamelen van de volgende prestatiemeteritems om beter te begrijpen van de belasting op elke machine en juiste cluster schalen beslissingen te maken.

| Categorie | Naam van teller |
| --- | --- |
| Fysieke schijf (per schijf) | Gem. Wachtrijlengte voor schijf lezen |
| Fysieke schijf (per schijf) | Gem. Wachtrijlengte voor schijf schrijven |
| Fysieke schijf (per schijf) | Gem. Schijf sec/lezen |
| Fysieke schijf (per schijf) | Gem. Schijf sec/schrijven |
| Fysieke schijf (per schijf) | Schijf lezen per seconde |
| Fysieke schijf (per schijf) | Bytes gelezen op schijf/sec |
| Fysieke schijf (per schijf) | Schijf schrijven per seconde |
| Fysieke schijf (per schijf) | Bytes geschreven naar schijf/sec |
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
| .NET CLR-geheugen (per service) | # Verzamelingen van generatie 0 |
| .NET CLR-geheugen (per service) | # Verzamelingen van generatie 1 |
| .NET CLR-geheugen (per service) | # Verzamelingen van generatie 2 |
| .NET CLR-geheugen (per service) | % GC-tijd in |

### <a name="service-fabrics-custom-performance-counters"></a>De aangepaste prestatiemeteritems van service Fabric

Service Fabric genereert een aanzienlijke hoeveelheid van het aangepaste prestatiemeteritems. Als u de SDK is geïnstalleerd hebt, kunt u de uitgebreide lijst zien op uw Windows-computer in uw toepassing Prestatiemeter (Start > Prestatiemeter). 

In de toepassingen die u implementeert met uw cluster, als u gebruikmaakt van Reliable Actors, toe te voegen countes van `Service Fabric Actor` en `Service Fabric Actor Method` categorieën (Zie [Service Fabric Reliable Actors-Diagnostics](service-fabric-reliable-actors-diagnostics.md)).

Als u betrouwbare Services gebruikt, hebben we op dezelfde manier `Service Fabric Service` en `Service Fabric Service Method` categorieën voor prestatiemeteritems die u moet verzamelen van prestatiemeteritems. 

Als u betrouwbare verzamelingen, raden wij aan toe te voegen de `Avg. Transaction ms/Commit` uit de `Service Fabric Transactional Replicator` voor het verzamelen van de gemiddelde doorvoervertraging per transactie metrisch gegeven.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [gebeurtenisgeneratie op het niveau van het platform](service-fabric-diagnostics-event-generation-infra.md) in Service Fabric
* Verzamelen van metrische gegevens voor prestaties via [Log Analytics-agent](service-fabric-diagnostics-oms-agent.md)
