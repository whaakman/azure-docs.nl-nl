---
title: Azure Service Fabric gebeurtenis Store | Microsoft Docs
description: Meer informatie over de Azure Service Fabric-EventStore
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: 1d235d5a75975c8d58cad1bbde0f16df2b1b3e7a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="eventstore-service-overview"></a>Overzicht van de service EventStore

>[!NOTE]
>Vanaf Service Fabric versie 6.2. de EventStore APIs zijn momenteel in preview voor Windows-clusters die alleen wordt uitgevoerd op Azure. We werken over het overdragen van deze functionaliteit op Linux, evenals onze zelfstandige-clusters.

## <a name="overview"></a>Overzicht

De EventStore-service is geïntroduceerd in versie 6.2, is een controleoptie in Service Fabric die biedt een manier om u inzicht in de status van uw cluster of werkbelastingen op een bepaald tijdstip. De service EventStore beschrijft Service Fabric-gebeurtenissen via API's kunt u oproepen maken. Deze EventStore APIs kunt u query uitvoeren op het knooppunt rechtstreeks voor diagnostische gegevens over een entiteit in het cluster en moet worden gebruikt om u te helpen:
* Problemen vaststellen op ontwikkeling of tests, of waarop u een pijplijn bewaking mogelijk gebruiken
* Bevestig dat beheeracties die u op het cluster onderneemt correct wordt verwerkt door het cluster

Zie voor een volledige lijst van gebeurtenissen die beschikbaar zijn in de EventStore [Service Fabric-gebeurtenissen](service-fabric-diagnostics-event-generation-operational.md).

De service EventStore kan worden opgevraagd voor gebeurtenissen die beschikbaar voor elke entiteit en dit entiteitstype in het cluster zijn. Dit betekent dat u kunt een query voor gebeurtenissen op de volgende niveaus;
* Cluster: alle cluster niveau gebeurtenissen
* Knooppunten: alle knooppunt niveau gebeurtenissen
* Knooppunt: gebeurtenissen die specifiek zijn voor één knooppunt, op basis van `nodeName`
* Toepassingen: alle niveau toepassingsgebeurtenissen
* Toepassing: gebeurtenissen die specifiek zijn voor één toepassing
* Services: gebeurtenissen van alle services in uw clusters
* -Service: gebeurtenissen van een specifieke service
* Partities: gebeurtenissen van alle partities
* -Partitie: gebeurtenissen van een specifieke partitie
* Replica's: gebeurtenissen van alle replica's / exemplaren
* De replica: gebeurtenissen van een specifieke replica / exemplaar


De service EventStore heeft ook de mogelijkheid gebeurtenissen in het cluster met elkaar correleren. De EventStore-service is door te kijken gebeurtenissen die van andere entiteiten die mogelijk zijn van invloed op elkaar op hetzelfde moment zijn geschreven, kunnen deze gebeurtenissen om te helpen bij het identificeren van mogelijke oorzaken voor activiteiten in uw cluster koppelen. Bijvoorbeeld, als een van uw toepassingen worden niet in orde zonder geïnduceerde wijzigingen gebeurt, de EventStore wordt ook blik op andere gebeurtenissen die worden weergegeven door het platform en kan correleren dit met een `NodeDown` gebeurtenis. Dit helpt bij het sneller foutdetectie en hoofdoorzaken analyse.

Om te beginnen met het gebruik van de service EventStore Zie [Query EventStore APIs voor Clustergebeurtenissen](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Volgende stappen
* Overzicht van controle en diagnostische gegevens in Service Fabric - [controle en diagnostische gegevens voor Service Fabric](service-fabric-diagnostics-overview.md)
* Meer informatie over het bewaken van uw cluster- [bewaking van het cluster en het platform](service-fabric-diagnostics-event-generation-infra.md).