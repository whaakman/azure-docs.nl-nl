---
title: Herconfiguratie in Azure Service Fabric | Microsoft Docs
description: Herconfiguratie van partities in Service Fabric begrijpen
services: service-fabric
documentationcenter: .net
author: appi101
manager: anuragg
editor: ''
ms.assetid: d5ab75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2018
ms.author: aprameyr
ms.openlocfilehash: a24aa6aa1695a3d1166816b7960bdd7b551e1a37
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34212803"
---
# <a name="reconfiguration-in-azure-service-fabric"></a>Herconfiguratie in Azure Service Fabric
Een *configuratie* is gedefinieerd als de replica's en hun rollen voor een partitie van een stateful service.

Een *herconfiguratie* is het proces van één configuratie verplaatsen naar een andere configuratie. Maakt een wijziging aan de replicaset voor een partitie van een stateful service. De configuratie van de oude heet de *vorige configuratie (PC)*, en de naam van de nieuwe configuratie de *huidige configuratie (CC)*. Het protocol herconfiguratie in Azure Service Fabric consistentie bewaart en onderhoudt beschikbaarheid tijdens eventuele wijzigingen aan de replicaset.

Failover Manager initieert herconfiguraties in reactie op verschillende gebeurtenissen in het systeem. Bijvoorbeeld, als de primaire vervolgens een nieuwe configuratie mislukt wordt gestart om het promoveren van een actieve secundaire aan een primaire. Een ander voorbeeld is in reactie op toepassingsupgrades wanneer mogelijk de primaire naar een ander knooppunt verplaatsen om de upgrade van het knooppunt.

## <a name="reconfiguration-types"></a>Herconfiguratie van typen
Herconfiguraties worden ingedeeld in twee typen:

- Herconfiguraties waar het wijzigen van de primaire:
    - **Failover**: Failovers herconfiguraties in reactie op het mislukken van een primaire actief zijn.
    - **SwapPrimary**: worden verwisseld zijn herconfiguraties waarin Service Fabric verplaatsen van een actief primaire van het ene knooppunt naar een andere, gewoonlijk in reactie moet op taakverdeling of een upgrade.

- Herconfiguraties waarbij de primaire zelf niet worden gewijzigd.

## <a name="reconfiguration-phases"></a>Herconfiguratie fasen
Een nieuwe configuratie wordt uitgevoerd in verschillende fasen:

- **Phase0**: deze fase in swap-primaire herconfiguraties waar de huidige primaire de status naar de nieuwe primaire en de overgang naar het actieve secundaire overgebracht gebeurt.

- **Phase1**: deze fase er gebeurt tijdens herconfiguraties waarbij de primaire wordt gewijzigd. Tijdens deze fase identificeert Service Fabric de juiste primaire van de huidige replica's. In deze fase is niet nodig tijdens swap-primaire herconfiguraties omdat de nieuwe primaire zijn al geselecteerd. 

- **Phase2**: tijdens deze fase Service Fabric zorgt ervoor dat alle gegevens beschikbaar zijn in een meerderheid van de replica's van de huidige configuratie.

Er zijn verschillende andere fasen uitsluitend voor intern gebruik worden.

## <a name="stuck-reconfigurations"></a>Vastgelopen herconfiguraties
Vind herconfiguraties *vastgelopen* voor een aantal redenen. Enkele veelvoorkomende redenen:

- **Replica's omlaag**: een aantal fasen herconfiguratie vereisen meerderheid van de replica's in de configuratie moet.
- **Netwerk-of**: herconfiguraties vereisen netwerkverbinding tussen de verschillende knooppunten.
- **API-fouten**: het protocol herconfiguratie vereist dat-serverimplementaties bepaalde API's eindigt. Bijvoorbeeld niet naleven van de annulering token in een betrouwbare service zorgt ervoor dat SwapPrimary herconfiguraties niet.

Statusrapporten van onderdelen van het systeem worden gebruikt, zoals System.FM System.RA en System.RAP, om op te sporen wanneer een nieuwe configuratie is vastgelopen. De [system health rapportpagina](service-fabric-understand-and-troubleshoot-with-system-health-reports.md) beschrijft deze health-rapporten.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over Service Fabric-concepten:

- [Levenscyclus van Reliable Services - C#](service-fabric-reliable-services-lifecycle.md)
- [Systeemstatusrapporten](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [Replica's en exemplaren](service-fabric-concepts-replica-lifecycle.md)
