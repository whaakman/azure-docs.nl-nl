---
title: Azure Service Fabric-gebeurtenissen | Microsoft Docs
description: Meer informatie over de Service Fabric-gebeurtenissen die buiten het vak om u te helpen bij het controleren van uw Azure Service Fabric-cluster.
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
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: c4ce8e01b1dc819453610f68d044cc268e27eed7
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57242748"
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

Hier volgen enkele voorbeelden van scenario's dat u gebeurtenissen voor in het cluster moet zien. 
* Levenscyclusgebeurtenissen voor het knooppunt: als knooppunten zijn beschikbaar, gaat u naar beneden, in/uit te schalen, opnieuw opstarten en ingeschakeld/uitgeschakeld zijn, deze gebeurtenissen weergegeven van wat is er gebeurd en helpt u identificeren als er iets mis is met de computer of als er is een API die is aangeroepen via SF te wijzigen van de status van een knooppunt.
* Upgrade voor clusters: de upgrade van uw cluster (SF versie of de configuratie wijzigen), ziet u de upgrade starten en voltooien draaien door elk van uw Upgrade-domeinen (of Rollback is opgegeven). 
* Upgrades van toepassingen: net als het cluster, upgrades, er is een uitgebreide set met gebeurtenissen als de upgrade doorloopt. Deze gebeurtenissen kunnen zijn handig om te begrijpen wanneer een upgrade is gepland, de huidige status van een upgrade, en de algemene volgorde van gebeurtenissen. Dit is handig voor het zoeken naar terug om te zien wat upgrades zijn teruggedraaid uit is of of terugdraaien is geactiveerd.
* Implementatie van de toepassing/Service / verwijderen: Er zijn gebeurtenissen voor elke toepassing, service en -container, die wordt gemaakt of verwijderd en handig wanneer u in- of uitgeschaald bijvoorbeeld verhoging van het aantal replica's
* Partitie worden verplaatst (herconfiguratie): wanneer een stateful partitie gaat via een herconfiguratie (een wijziging in de replicaset), een gebeurtenis wordt geregistreerd. Dit is handig als u probeert te begrijpen hoe vaak uw replicaset partitie is gewijzigd of Failover-overschakeling uitvoeren, of welk knooppunt de primaire replica werd uitgevoerd op elk gewenst moment in de tijd bijhouden.
* Chaos-gebeurtenissen: bij het gebruik van Service Fabric [Chaos](service-fabric-controlled-chaos.md) service, ziet u gebeurtenissen telkens wanneer de service is gestart of gestopt, of wanneer het injects een storing in het systeem.
* Health-gebeurtenissen: Service Fabric wordt aangegeven dat statusgebeurtenissen telkens wanneer een waarschuwing of een statusrapport van de fout is gemaakt, of een entiteit terug naar de status OK status gaat of een statusrapport is verlopen. Deze gebeurtenissen zijn zeer nuttig zijn voor het bijhouden van statistieken van de historische status voor een entiteit. 

## <a name="how-to-access-events"></a>Hoe u toegang tot gebeurtenissen

Er zijn een aantal verschillende manieren waarmee de Service Fabric-gebeurtenissen kunnen worden geopend:
* De gebeurtenissen worden geregistreerd via standaard kanalen zoals ETW/Windows-gebeurtenislogboeken en kunnen worden gevisualiseerd door een controleprogramma die ondersteuning biedt voor deze zoals Azure Monitor-Logboeken. Standaard clusters die zijn gemaakt in de portal voor diagnostische gegevens zijn ingeschakeld en de Windows Azure diagnostics-agent de gebeurtenissen verzenden naar Azure-tabelopslag hebben, maar u moet nog steeds dit integreren in uw log analytics-resource. Meer informatie over het configureren van de [Azure Diagnostics-agent](service-fabric-diagnostics-event-aggregation-wad.md) de configuratie van diagnostische gegevens van uw cluster om op te halen meer logboeken of prestatiemeteritems wijzigen en de [integratie van Azure Monitor-Logboeken](service-fabric-diagnostics-event-analysis-oms.md)
* EventStore service Rest API's waarmee u kunt query uitvoeren op het cluster rechtstreeks of via de Service Fabric-clientbibliotheek. Zie [Query EventStore APIs voor Clustergebeurtenissen](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie op het cluster - bewaking [bewaking van het cluster en het platform](service-fabric-diagnostics-event-generation-infra.md).
* Meer informatie over de service EventStore - [overzicht van EventStore-service](service-fabric-diagnostics-eventstore.md)
