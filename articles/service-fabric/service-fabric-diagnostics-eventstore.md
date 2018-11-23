---
title: Azure Service Fabric Event Store | Microsoft Docs
description: Meer informatie over Azure Service Fabric van EventStore
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: cd58e24a51b153d6bf217f7d32a82e882183ed73
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2018
ms.locfileid: "52290668"
---
# <a name="eventstore-service-overview"></a>Overzicht van EventStore-service

## <a name="overview"></a>Overzicht

Die is geïntroduceerd in versie 6.2, is de EventStore-service een optie voor de bewaking in Service Fabric, waarmee u een manier om u inzicht krijgt in de status van uw cluster of de werkbelastingen op een bepaald tijdstip. Service Fabric-gebeurtenissen via een reeks API's (toegankelijk via REST-eindpunten of via de clientbibliotheek) wordt aangegeven dat de EventStore-service. Deze APIs EventStore kunt u query uitvoeren op het cluster rechtstreeks gebruikt om toegang te krijgen van diagnostische gegevens voor elke entiteit in het cluster en moet worden gebruikt om u te helpen:

* Problemen vaststellen op de ontwikkeling en testen, of waar mogelijk gebruikt u een pijplijn voor bewaking
* Bevestig dat beheeracties die u op het cluster onderneemt correct worden verwerkt door uw cluster

Zie voor een volledige lijst van gebeurtenissen die beschikbaar zijn in de EventStore [Service Fabric-gebeurtenissen](service-fabric-diagnostics-event-generation-operational.md).

>[!NOTE]
>Vanaf het Service Fabric versie 6.2. De APIs EventStore zijn momenteel in Preview-versie van Windows-clusters wordt alleen uitgevoerd op Azure. Er wordt gewerkt aan het overzetten van deze functionaliteit voor zowel Linux als onze zelfstandige clusters.

De EventStore-service kan worden opgevraagd voor gebeurtenissen die beschikbaar voor elke entiteit en het type van de entiteit in het cluster zijn. Dit betekent dat u kunt een query voor gebeurtenissen op de volgende niveaus;
* Cluster: alle gebeurtenissen op toepassingsniveau cluster
* Knooppunten: alle knooppunt op gebeurtenissen
* Knooppunt: gebeurtenissen die specifiek zijn voor één knooppunt, op basis van `nodeName`
* Toepassingen: alle gebeurtenissen op toepassingsniveau
* Toepassing: gebeurtenissen die specifiek zijn voor één toepassing
* Services: gebeurtenissen van alle services in uw clusters
* -Service: gebeurtenissen naar een specifieke service
* Partities: gebeurtenissen van alle partities
* -Partitie: gebeurtenissen van een specifieke partitie
* Replica's: gebeurtenissen van alle replica's / instanties
* De replica is: gebeurtenissen van een specifieke replica / exemplaar


De EventStore-service biedt ook de mogelijkheid te correleren van gebeurtenissen in uw cluster. De EventStore-service is door te kijken op gebeurtenissen die zijn geschreven op hetzelfde moment uit verschillende entiteiten die mogelijk van invloed is elkaar worden verbonden, niet koppelen van deze gebeurtenissen om te helpen bij het identificeren van mogelijke oorzaken voor activiteiten in uw cluster. Bijvoorbeeld, als een van uw toepassingen met een slechte status zonder wijzigingen in de geïnduceerde gebeurt, de EventStore wordt ook een overzicht van andere gebeurtenissen die worden weergegeven door het platform en kan correleren van dit met een `NodeDown` gebeurtenis. Dit helpt met snellere foutdetectie en analyse van hoofdoorzaken.

We raden u aan de EventStore gebruiken voor snelle analyse en voor een momentopname beter beeld van hoe uw cluster wordt uitgevoerd, en als dingen gebeuren als er verwacht.

Om te beginnen met het gebruik van de service EventStore, Zie [Query EventStore APIs voor Clustergebeurtenissen](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Volgende stappen
* Overzicht van controle en diagnostische gegevens in Service Fabric - [bewaking en diagnose voor Service Fabric](service-fabric-diagnostics-overview.md)
* Meer informatie over het controleren van uw cluster- [bewaking van het cluster en het platform](service-fabric-diagnostics-event-generation-infra.md).