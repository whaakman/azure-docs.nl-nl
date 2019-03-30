---
title: Beschikbaarheid van Service Fabric-services | Microsoft Docs
description: Hierin wordt beschreven fout met betrekking tot detectie, failover en herstel van services
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: 279ba4a4-f2ef-4e4e-b164-daefd10582e4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: dd10af0d3c8a57168a27a039286ea0ec4c1dad02
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662739"
---
# <a name="availability-of-service-fabric-services"></a>Beschikbaarheid van Service Fabric-services
In dit artikel biedt een overzicht van hoe Azure Service Fabric de beschikbaarheid van een service onderhoudt.

## <a name="availability-of-service-fabric-stateless-services"></a>Beschikbaarheid van Service Fabric stateless services
Service Fabric-services kunnen worden stateless of stateful. Een stateless service is een toepassingsservice die u niet beschikt over een [lokale status](service-fabric-concepts-state.md) die moet maximaal beschikbaar of betrouwbaar.

Het maken van een stateless service moet definiëren van een `InstanceCount`. Het aantal instanties bepaalt het aantal exemplaren van de stateless service-toepassingslogica waarmee moet worden uitgevoerd in het cluster. Het aantal instanties verhogen, is de aanbevolen manier om een stateless service uitbreiden.

Wanneer een exemplaar van een staatloze met de naam-service is mislukt, wordt een nieuw exemplaar gemaakt op een in aanmerking komende knooppunt in het cluster. Bijvoorbeeld, een stateless service-exemplaar op knooppunt1 kan mislukken en worden opnieuw gemaakt op Knooppunt5.

## <a name="availability-of-service-fabric-stateful-services"></a>Beschikbaarheid van stateful Service Fabric-services
Een stateful service heeft een status die ermee verbonden zijn. Een stateful service is in Service Fabric gemodelleerd als een set van replica's. Elke replica is een actief exemplaar van de code van de service. De replica heeft ook een kopie van de status voor de service. Lezen en schrijven bewerkingen worden uitgevoerd op een van de replica, met de naam de *primaire*. Wijzigingen in de status van schrijven bewerkingen zijn *gerepliceerd* aan de andere replica's in de replicaset, met de naam *actieve secundaire databases*, en worden toegepast. 

Er mag slechts één primaire replica, maar kunnen er meerdere actieve secundaire replica's. Het aantal actieve secundaire replica's kan worden geconfigureerd en een groter aantal replica's een groter aantal gelijktijdige software en problemen met de hardware kan tolereren.

Als de primaire replica uitgeschakeld wordt, wordt in Service Fabric een van de actieve secundaire replica's de nieuwe primaire replica. Deze actieve secundaire replica heeft al de bijgewerkte versie van de status, via *replicatie*, en u kunt doorgaan met het verwerken van meer bewerkingen voor lezen/schrijven. Dit proces staat bekend als *herconfiguratie* en wordt beschreven in de [herconfiguratie](service-fabric-concepts-reconfiguration.md) artikel.

Het concept van een replica wordt een primaire of secundaire actieve staat bekend als de *replicarol*. Deze replica's worden beschreven in de [replica's en exemplaren](service-fabric-concepts-replica-lifecycle.md) artikel. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over Service Fabric-concepten:

- [Schalen van Service Fabric-services](service-fabric-concepts-scalability.md)
- [Service Fabric-services partitioneren](service-fabric-concepts-partitioning.md)
- [Status definiëren en beheren](service-fabric-concepts-state.md)
- [Reliable Services](service-fabric-reliable-services-introduction.md)

