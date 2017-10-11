---
title: Prestaties van Azure Service Fabric Monitoring | Microsoft Docs
description: Meer informatie over prestatiemeteritems voor controle en diagnostische gegevens van Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 9d63148c182c705b6b49733c59ed8fdd13872d72
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="performance-metrics"></a>Maatstaven voor prestaties

Metrische gegevens moeten worden verzameld voor informatie over de prestaties van uw cluster, evenals de toepassingen die erin worden uitgevoerd. Voor Service Fabric-clusters, wordt u aangeraden de volgende prestatiemeteritems verzamelen.

## <a name="nodes"></a>Knooppunten

Voor de computers in uw cluster, kunt u het verzamelen van de volgende prestatiemeteritems om beter te begrijpen van de belasting op elke machine en relevante cluster schalen beslissingen maken.

| Prestatiemeteritem-categorie | Naam van het meteritem |
| --- | --- |
| Fysieke schijf (per schijf) | Gem. Wachtrijlengte voor schijf lezen |
| Fysieke schijf (per schijf) | Gem. Wachtrijlengte voor schijf schrijven |
| Fysieke schijf (per schijf) | Gem. Leestijd schijf |
| Fysieke schijf (per schijf) | Gem. Schrijftijd schijf |
| Fysieke schijf (per schijf) | Schijf lezen per seconde |
| Fysieke schijf (per schijf) | Gelezen Bytes per seconde |
| Fysieke schijf (per schijf) | Schijf schrijven per seconde |
| Fysieke schijf (per schijf) | Geschreven Bytes per seconde |
| Geheugen | Beschikbare megabytes (MB) |
| PagingFile | % Gebruik |
| Process(Total) | Percentage processortijd |
| Proces (per-service) | Percentage processortijd |
| Proces (per-service) | Proces-ID |
| Proces (per-service) | Eigen Bytes |
| Proces (per-service) | Aantal threads |
| Proces (per-service) | Virtuele Bytes |
| Proces (per-service) | Werkset |
| Proces (per-service) | Werkset - privé |

## <a name="net-applications-and-services"></a>.NET-toepassingen en services

De volgende prestatiemeteritems verzamelen als u .NET-services in uw cluster implementeert. 

| Prestatiemeteritem-categorie | Naam van het meteritem |
| --- | --- |
| .NET CLR-geheugen (per-service) | Proces-ID |
| .NET CLR-geheugen (per-service) | # Totaal aantal toegewezen Bytes |
| .NET CLR-geheugen (per-service) | # Totaal aantal Bytes gereserveerd |
| .NET CLR-geheugen (per-service) | Aantal bytes in alle Heaps |
| .NET CLR-geheugen (per-service) | # Verzamelingen van generatie 0 |
| .NET CLR-geheugen (per-service) | # Generatie 1-verzamelingen |
| .NET CLR-geheugen (per-service) | # Gen 2-verzamelingen |
| .NET CLR-geheugen (per-service) | % Tijd in % |

### <a name="service-fabrics-custom-performance-counters"></a>Aangepaste service-Fabric-prestatiemeteritems

Service Fabric genereert een aanzienlijke hoeveelheid aangepaste prestatiemeteritems. Als u de SDK is geïnstalleerd hebt, ziet u de uitgebreide lijst op uw Windows-computer in uw toepassing Prestatiemeter (Start > Prestatiemeter). 

In de toepassingen die u implementeert met uw cluster, als u gebruikmaakt van Reliable Actors, het toevoegen van countes van `Service Fabric Actor` en `Service Fabric Actor Method` categorieën (Zie [Service Fabric betrouwbare actoren Diagnostics](service-fabric-reliable-actors-diagnostics.md)).

Als u Reliable Services gebruikt, hebben we op dezelfde manier `Service Fabric Service` en `Service Fabric Service Method` categorieën voor prestatiemeteritems die u moet verzamelen van prestatiemeteritems. 

Als u betrouwbare verzamelingen gebruikt, raden wij aan toe te voegen de `Avg. Transaction ms/Commit` van de `Service Fabric Transactional Replicator` voor het verzamelen van de gemiddelde doorvoervertraging per transactie metriek.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [genereren van gebeurtenissen op het niveau van het platform](service-fabric-diagnostics-event-generation-infra.md) in Service Fabric
* Verzamelen van prestatiegegevens via [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md)
