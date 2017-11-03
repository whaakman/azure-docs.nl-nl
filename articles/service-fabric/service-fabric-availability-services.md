---
title: Beschikbaarheid van Service Fabric-services | Microsoft Docs
description: Hierin wordt beschreven foutenopsporing, failovers en herstel van services
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 279ba4a4-f2ef-4e4e-b164-daefd10582e4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 6c23a56df48434db3b82bce70cbd3a23941a077a
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2017
---
# <a name="availability-of-service-fabric-services"></a>Beschikbaarheid van Service Fabric-services
In dit artikel biedt een overzicht van hoe Azure Service Fabric de beschikbaarheid van een service houdt.

## <a name="availability-of-service-fabric-stateless-services"></a>Beschikbaarheid van Service Fabric stateless services
Stateful of stateless, kunnen de service Fabric-services zijn. Een stateless is een toepassingsservice beschikt niet over een [lokale status](service-fabric-concepts-state.md) dat moet worden maximaal beschikbaar of betrouwbaar.

Het maken van een stateless service moet definiëren van een `InstanceCount`. Het aantal exemplaren definieert het aantal exemplaren van de staatloze service toepassingslogica die moet worden uitgevoerd in het cluster. Het aantal exemplaren is de aanbevolen manier van een staatloze service uitbreiden.

Wanneer een exemplaar van een staatloze met de naam-service is mislukt, wordt een nieuw exemplaar gemaakt op een in aanmerking komende knooppunt in het cluster. Bijvoorbeeld, een stateless service-exemplaar op knooppunt1 mislukken en opnieuw worden gemaakt op Knooppunt5.

## <a name="availability-of-service-fabric-stateful-services"></a>Beschikbaarheid van Service Fabric stateful services
Een stateful service heeft een status die is gekoppeld. Een stateful service is in Service Fabric gemodelleerd als een set van replica's. Elke replica is een actief exemplaar van de code van de service. De replica heeft ook een kopie van de status voor de service. Lezen en schrijven bewerkingen worden uitgevoerd op één replica, aangeroepen de *primaire*. Schrijven van wijzigingen in de status van bewerkingen zijn *gerepliceerd* aan de andere replica's in de replicaset, aangeroepen *actieve secundaire replica's*, en kunnen worden toegepast. 

Er mag slechts één primaire replica kunnen, maar er meerdere actieve secundaire replica's zijn. Het aantal actieve secundaire replica's kan worden geconfigureerd en een hoger aantal replica's tolereert een groter aantal gelijktijdige software en hardwarefouten.

Als de primaire replica uitgeschakeld wordt, Service Fabric kunt u een van de actieve secundaire replica's de nieuwe primaire replica. Deze actieve secundaire replica is al de bijgewerkte versie van de status *replicatie*, en kan worden voortgezet verwerkingen verder lezen/schrijven. Dit proces wordt ook wel *herconfiguratie* en wordt nader beschreven in de [herconfiguratie](service-fabric-concepts-reconfiguration.md) artikel.

Het concept van een replica wordt een primaire of secundaire, actieve, wordt ook wel de *replicarol*. Deze replica's worden beschreven verder in de [replica's en -exemplaren](service-fabric-concepts-replica-lifecycle.md) artikel. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over Service Fabric-concepten:

- [Schalen van Service Fabric-services](service-fabric-concepts-scalability.md)
- [Partitionering Service Fabric-services](service-fabric-concepts-partitioning.md)
- [Definiëren en beheren van status](service-fabric-concepts-state.md)
- [Reliable Services](service-fabric-reliable-services-introduction.md)

