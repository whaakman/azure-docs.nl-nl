---
title: Gebeurtenissen van Azure Service Fabric | Microsoft Docs
description: Meer informatie over de Service Fabric-gebeurtenissen die buiten het vak om u te helpen bij het bewaken van uw Azure Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: 6379f463d5c2e9027e1d8d4dd90db61cb72cdd44
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="service-fabric-events"></a>Service Fabric-gebeurtenissen 

Het Service Fabric-platform, schrijft enkele gestructureerde gebeurtenissen voor belangrijke operationele activiteiten plaatsvinden in uw cluster. Deze bereik van de cluster-upgrades replica plaatsing beslissingen te nemen. Elke gebeurtenis dat Service Fabric is toegewezen aan een van de volgende entiteiten in het cluster beschrijft:
* Cluster
* Toepassing
* Service
* Partitie
* Replica 
* Container

Voor een volledig overzicht van gebeurtenissen die worden weergegeven door het platform - [lijst van Service Fabric-gebeurtenissen](service-fabric-diagnostics-event-generation-operational.md).

Hier volgen enkele voorbeelden van belangrijke scenario's dat u gebeurtenissen voor in uw cluster ziet. 
1. Knooppunt lifecycle gebeurtenissen: als knooppunten komen, uitvallen, worden geactiveerd/gedeactiveerd of opnieuw gestart, gebeurtenissen worden weergegeven met wat is er gebeurd en te identificeren als er iets mis met de machine zelf of als er een API die is aangeroepen via SF naar de status van een knooppunt wijzigen.
1. Upgrade van het cluster: de upgrade van uw cluster (SF versie of configuratie wijzigen), ziet u de upgrade initiëren, door elk van uw UDs draaien en voltooien (of ongedaan maken). 
1. Toepassingsupgrades: op dezelfde manier in een cluster moeten worden bijgewerkt, wordt er een uitgebreide reeks gebeurtenissen als de upgrade wordt via. Deze gebeurtenissen is handig om te begrijpen wanneer een upgrade is gepland, de huidige status van een upgrade en de totale volgorde van gebeurtenissen. Dit is handig voor het opzoeken terug om te zien wat upgrades zijn teruggedraaid uit is.
1. Implementatie van de toepassing/Service / verwijdering: Er zijn van gebeurtenissen voor elke toepassing, de service en de container, wordt gemaakt of verwijderd.
1. Partitie worden verplaatst (herconfiguratie): wanneer een stateful partitie gaat via een herconfiguratie (een wijziging in de replicaset), een gebeurtenis wordt vastgelegd. Dit is handig als u probeert te begrijpen hoe vaak de replicaset partitie wordt gewijzigd, of welk knooppunt de primaire replica werd uitgevoerd op elk punt in tijd bijhouden.
1. Gebeurtenissen van chaos: bij gebruik van Service Fabric [Chaos](service-fabric-controlled-chaos.md) service, ziet u gebeurtenissen telkens wanneer de service is gestart of gestopt, of wanneer er een storing injects in het systeem.
1. Health-gebeurtenissen: Service Fabric beschrijft health gebeurtenissen telkens wanneer een waarschuwing of een foutrapport health is gemaakt, of een entiteit teruggaan naar een OK-status of een statusrapport is verlopen. Deze gebeurtenissen zijn handig voor het bijhouden van historische health statistieken voor een entiteit. 

## <a name="how-to-access-events"></a>Over het openen van gebeurtenissen

Er zijn een aantal verschillende manieren toegankelijk via welke Service Fabric-gebeurtenissen:
* via het operationele kanaal. Deze kunnen worden verzameld via de Azure Diagnostics-uitbreiding en verzonden naar een tabel van de opslag voor consumptie of voor opname in een hulpprogramma zoals OMS Log Analytics. Als ' diagnostische gegevens ' is ingeschakeld voor een cluster, de Azure Diagnostics-agent op het cluster is geïmplementeerd en is standaard geconfigureerd om te lezen in de logboeken van het operationele kanaal. Meer informatie over het configureren van de [Azure Diagnostics agent](service-fabric-diagnostics-event-aggregation-wad.md) de configuratie van diagnostische gegevens van het cluster om op te halen meer logboeken of prestatiemeteritems wijzigen. 
* via de EventStore-service Rest-API's waarmee u kunt een query uitvoeren op het knooppunt rechtstreeks of via de Service Fabric-clientbibliotheek. Zie [Query EventStore APIs voor Clustergebeurtenissen](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie op het cluster - bewaking [bewaking van het cluster en het platform](service-fabric-diagnostics-event-generation-infra.md).
* Meer informatie over de service EventStore - [EventStore service-overzicht](service-fabric-diagnostics-eventstore.md)
