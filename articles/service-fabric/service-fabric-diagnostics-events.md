---
title: Azure Service Fabric-gebeurtenissen | Microsoft Docs
description: Meer informatie over de Service Fabric-gebeurtenissen die buiten het vak om u te helpen bij het controleren van uw Azure Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: ca63d67f6d7c19b4ca6928c4cc0f9ccb06eace2b
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49402978"
---
# <a name="service-fabric-events"></a>Service Fabric-gebeurtenissen 

De Service Fabric-platform schrijft verschillende gestructureerde gebeurtenissen voor belangrijke operationele activiteiten die plaatsvinden binnen uw cluster. Deze variëren van cluster-upgrades tot replica plaatsing beslissingen. Elke gebeurtenis die Service Fabric wordt aangegeven dat is toegewezen aan een van de volgende entiteiten in het cluster:
* Cluster
* Toepassing
* Service
* Partitie
* Replica 
* Container

Een volledige lijst van gebeurtenissen die worden weergegeven door het platform - [lijst van Service Fabric-gebeurtenissen](service-fabric-diagnostics-event-generation-operational.md).

Hier volgen enkele voorbeelden van belangrijke scenario's dat u gebeurtenissen voor in het cluster moet zien. 
1. Levenscyclusgebeurtenissen voor het knooppunt: als er knooppunten zijn beschikbaar, gaat u naar beneden, worden ingeschakeld/uitgeschakeld of opnieuw opgestart, gebeurtenissen weergegeven van wat is er gebeurd en te herkennen als er iets mis is met de computer of als er is een API die is aangeroepen via SF naar de status van een knooppunt wijzigen.
1. Upgrade voor clusters: de upgrade van uw cluster (SF versie of de configuratie wijzigen), ziet u de upgrade initiëren, selecteer met elk van de ud's en voltooid (of Rollback is opgegeven). 
1. Upgrades van toepassingen: op dezelfde manier aan het cluster wordt bijgewerkt, wordt er een uitgebreide set met gebeurtenissen als de upgrade doorloopt. Deze gebeurtenissen kunnen zijn handig om te begrijpen wanneer een upgrade is gepland, de huidige status van een upgrade, en de algemene volgorde van gebeurtenissen. Dit is handig voor het zoeken naar terug om te zien wat upgrades zijn teruggedraaid van is.
1. Implementatie van de toepassing/Service / verwijderen: Er zijn gebeurtenissen voor elke toepassing, service en -container wordt gemaakt of verwijderd.
1. Partitie worden verplaatst (herconfiguratie): wanneer een stateful partitie gaat via een herconfiguratie (een wijziging in de replicaset), een gebeurtenis wordt geregistreerd. Dit is handig als u probeert om te begrijpen hoe vaak uw replicaset partitie wordt gewijzigd, of welk knooppunt de primaire replica werd uitgevoerd op elk gewenst moment in de tijd bijhouden.
1. Chaos-gebeurtenissen: bij het gebruik van Service Fabric [Chaos](service-fabric-controlled-chaos.md) service, ziet u gebeurtenissen telkens wanneer de service is gestart of gestopt, of wanneer het injects een storing in het systeem.
1. Statusgebeurtenissen: Service Fabric beschrijft statusgebeurtenissen telkens wanneer een waarschuwing of een statusrapport van de fout is gemaakt, of een entiteit terug naar de status OK status gaat of een statusrapport is verlopen. Deze gebeurtenissen zijn zeer nuttig zijn voor het bijhouden van statistieken van de historische status voor een entiteit. 

## <a name="how-to-access-events"></a>Hoe u toegang tot gebeurtenissen

Er zijn een aantal verschillende manieren waarmee de Service Fabric-gebeurtenissen kunnen worden geopend:
* via het operationele kanaal. Deze kunnen worden verzameld via de Azure Diagnostics-extensie en verzonden aan een opslagtabel voor consumptie of voor opname in een hulpprogramma zoals Azure Log Analytics. Als ' diagnostische gegevens ' is ingeschakeld voor een cluster, wordt de Azure Diagnostics-agent op uw cluster is geïmplementeerd en is standaard geconfigureerd om te lezen in Logboeken van het operationele kanaal. Meer informatie over het configureren van de [Azure Diagnostics-agent](service-fabric-diagnostics-event-aggregation-wad.md) de configuratie van diagnostische gegevens van uw cluster om op te halen meer logboeken of prestatiemeteritems wijzigen. 
* via de EventStore-service Rest API's waarmee u kunt query uitvoeren op het cluster rechtstreeks of via de Service Fabric-clientbibliotheek. Zie [Query EventStore APIs voor Clustergebeurtenissen](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie op het cluster - bewaking [bewaking van het cluster en het platform](service-fabric-diagnostics-event-generation-infra.md).
* Meer informatie over de service EventStore - [overzicht van EventStore-service](service-fabric-diagnostics-eventstore.md)
