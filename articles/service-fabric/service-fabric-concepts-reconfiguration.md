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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60882194"
---
# <a name="reconfiguration-in-azure-service-fabric"></a>Herconfiguratie in Azure Service Fabric
Een *configuratie* wordt gedefinieerd als de replica's en hun rollen voor een partitie van een stateful service.

Een *herconfiguratie* is het proces van het verplaatsen van één configuratie naar een andere configuratie. Wordt een wijziging aan de replicaset voor een partitie van een stateful service verzonden. De oude configuratie heet de *vorige configuratie (PC)* , en de nieuwe configuratie heet de *huidige configuratie (CC)* . Het protocol herconfiguratie in Azure Service Fabric behoudt consistentie en beschikbaarheid tijdens alle wijzigingen aan de replicaset onderhoudt.

Failover Manager initieert wanneer in reactie op verschillende gebeurtenissen in het systeem. Bijvoorbeeld, als de primaire vervolgens een nieuwe configuratie mislukt wordt gestart als u wilt promoveren van een actieve secundaire aan een primaire. Een ander voorbeeld is in reactie op upgrades van toepassingen kan het nodig zijn om te verplaatsen van de primaire naar een ander knooppunt om bij te werken van het knooppunt zijn.

## <a name="reconfiguration-types"></a>Herconfiguratie van typen
Wanneer kunnen worden ingedeeld in twee typen:

- Wanneer waar de primaire wordt gewijzigd:
    - **Failover**: Failovers zijn wanneer in reactie op het mislukken van een primaire die wordt uitgevoerd.
    - **SwapPrimary**: Swaps zijn wanneer waar Service Fabric moeten zijn om te verplaatsen van een actief primaire van het ene knooppunt naar een andere, gewoonlijk in reactie op de taakverdeling of een upgrade.

- Wanneer waar wordt de primaire niet is gewijzigd.

## <a name="reconfiguration-phases"></a>Herconfiguratie fasen
Een nieuwe configuratie wordt uitgevoerd in verschillende fasen:

- **Phase0**: Deze fase gebeurt in swap-primaire wanneer, waar de huidige primaire de status naar de nieuwe primaire en de overgang naar het actieve secundaire overgebracht.

- **Phase1**: Deze fase er gebeurt tijdens het rekening gebracht wanneer de primaire wordt gewijzigd. Tijdens deze fase identificeert de Service Fabric de juiste primaire tussen de huidige replica's. Deze fase is niet tijdens het wisselen van primaire wanneer nodig, omdat de nieuwe primaire al is gekozen. 

- **Phase2**: Tijdens deze fase, Service Fabric zorgt ervoor dat alle gegevens beschikbaar zijn in een meerderheid van de replica's van de huidige configuratie.

Er zijn verschillende andere fasen die alleen voor intern gebruik.

## <a name="stuck-reconfigurations"></a>Vastgelopen wanneer
Wanneer krijg *vastgelopen* voor tal van redenen. Enkele van de veelvoorkomende oorzaken zijn:

- **Replica's omlaag**: Sommige fasen herconfiguratie vereisen een meerderheid van de replica's in de configuratie uit.
- **Problemen met netwerk of communicatie**: Wanneer de netwerkverbinding tussen de verschillende knooppunten nodig hebt.
- **API-fouten**: Het protocol herconfiguratie is vereist dat de service-implementaties bepaalde API's voltooien. Bijvoorbeeld, niet naleven van de annulering token in een betrouwbare service zorgt ervoor dat wanneer SwapPrimary te zitten.

Statusrapporten betrekking op systeemonderdelen gebruiken, zoals System.FM, System.RA en System.RAP, als u wilt vaststellen waar een herconfiguratie is vastgelopen. De [system health rapportpagina](service-fabric-understand-and-troubleshoot-with-system-health-reports.md) beschrijft deze health-rapporten.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over Service Fabric-concepten:

- [Levenscyclus van Reliable Services - C#](service-fabric-reliable-services-lifecycle.md)
- [Systeemstatusrapporten](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [Replica's en exemplaren](service-fabric-concepts-replica-lifecycle.md)
