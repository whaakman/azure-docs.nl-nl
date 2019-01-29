---
title: Azure Service Fabric zelfstandige cluster schalen | Microsoft Docs
description: Meer informatie over zelfstandige Service Fabric-clusters in of uit en omhoog of omlaag schalen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: aljo
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/13/2018
ms.author: ryanwi
ms.openlocfilehash: 0c211c2bb9dc07e705679b5d4079b85de9d72d8c
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55100458"
---
# <a name="scaling-service-fabric-standalone-clusters"></a>Service Fabric zelfstandige clusters schalen
Een Service Fabric-cluster is een netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Een machine of virtuele machine die deel uitmaakt van een cluster, heet een knooppunt. Clusters kunnen mogelijk duizenden knooppunten bevatten. Na het maken van een Service Fabric-cluster, kunt u het cluster horizontaal schalen (het aantal knooppunten wijzigen) of verticaal (de resources van de knooppunten wijzigen).  U kunt het cluster schalen op elk gewenst moment, zelfs wanneer workloads worden uitgevoerd op het cluster.  Als het cluster wordt geschaald, wordt uw toepassingen automatisch ook schalen.

Waarom het cluster schalen? Toepassingsbehoeften veranderen verloop van tijd.  Mogelijk moet u verhogen clusterbronnen om te voldoen aan een grotere werkbelasting of netwerk verkeer of verkleinen van clusterbronnen wanneer de vraag afneemt.

## <a name="scaling-in-and-out-or-horizontal-scaling"></a>In en uit te schalen of horizontaal schalen
Het aantal knooppunten in het cluster wordt gewijzigd.  Zodra de nieuwe knooppunten die zijn toegevoegd aan het cluster, de [Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md) verplaatst van services toe, waardoor er minder belasting van de bestaande knooppunten.  Als de resources van het cluster niet efficiënt worden gebruikt, kunt u ook het aantal knooppunten verminderen.  Als knooppunten het cluster laat, worden de services verplaatsen uit deze knooppunten en toeneemt op de resterende knooppunten.  Verlaag het aantal knooppunten in een cluster worden uitgevoerd in Azure kunt u geld besparen, omdat u voor het aantal VM's u gebruikt en niet de werkbelasting op deze virtuele machines betaalt.  

- Voordelen: Oneindige schaal, in theorie.  Als uw toepassing is ontworpen voor schaalbaarheid, kunt u onbeperkte groei inschakelen door meer knooppunten toe te voegen.  De hulpmiddelen in cloudomgevingen kunt eenvoudig toevoegen of verwijderen van knooppunten, zodat het eenvoudig om aan te passen capaciteit is en u betaalt alleen voor de resources die u gebruikt.  
- Nadelen: Toepassingen worden gebruikt, [ontworpen voor schaalbaarheid](service-fabric-concepts-scalability.md).  Databases van toepassingen en persistentie moet mogelijk extra architectuur werk zo goed te kunnen schalen.  [Betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md) in stateful Service Fabric-services, maken het echter veel gemakkelijker te schalen van uw toepassingsgegevens.

Zelfstandige clusters kunnen u Service Fabric-cluster on-premises implementeren of in de cloud-provider van uw keuze.  Knooppunttypen bestaan uit fysieke of virtuele machines, afhankelijk van uw implementatie. Het proces van een zelfstandige cluster schalen wordt vergeleken met de clusters die worden uitgevoerd in Azure, iets bewerkelijker.  U moet handmatig wijzigen van het aantal knooppunten in het cluster en voer vervolgens een upgrade voor de configuratie van een cluster.

Het verwijderen van knooppunten kan meerdere upgrades initiëren. Sommige knooppunten zijn gemarkeerd met `IsSeedNode=”true”` markeren en kunnen worden geïdentificeerd door het opvragen van het cluster met behulp van het manifest [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest). Het verwijderen van deze knooppunten duurt mogelijk langer dan andere omdat het seed-knooppunten worden verplaatst moeten om in dergelijke scenario's. Het cluster moet minimaal drie primaire knooppunt type knooppunten behouden.

> [!WARNING]
> Het is raadzaam dat u het aantal knooppunten hieronder niet doen verlagen de [clustergrootte van de Betrouwbaarheidslaag](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) voor het cluster. Dit heeft invloed op de mogelijkheid van de Service Fabric-systeemservices worden gerepliceerd in het cluster, en wordt instabiel of mogelijk Vernietig het cluster.
>

Wanneer u een zelfstandige cluster, houd rekening met de volgende richtlijnen:
- De vervanging van de primaire knooppunten moet één knooppunt uitgevoerd na de andere, in plaats van te verwijderen en vervolgens toe te voegen in batches.
- Voordat u een knooppunttype is verwijderd, moet u controleren of er zijn geen knooppunten die verwijst naar het knooppunttype. Verwijder deze knooppunten voordat u het bijbehorende knooppunttype verwijdert. Wanneer alle bijbehorende knooppunten zijn verwijderd, kunt u de NodeType verwijderen uit de configuratie van het cluster en beginnen met een configuratie met behulp van een upgrade uitvoert [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade).

Zie voor meer informatie, [een zelfstandige cluster schalen](service-fabric-cluster-windows-server-add-remove-nodes.md).

## <a name="scaling-up-and-down-or-vertical-scaling"></a>Omhoog en omlaag schalen of verticaal schalen 
Hiermee wijzigt u de resources (CPU, geheugen of opslag) van knooppunten in het cluster.
- Voordelen: Architectuur van software en de toepassing blijft hetzelfde.
- Nadelen: Beperkte schaal, omdat er een limiet aan hoeveel u resources op afzonderlijke knooppunten kunt verhogen. Downtime, omdat u nemen fysieke of virtuele machines offline moet wilt toevoegen of verwijderen van resources.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [toepassing schaalbaarheid](service-fabric-concepts-scalability.md).
* [Schalen van een Azure-cluster in- of uitgeschaald](service-fabric-tutorial-scale-cluster.md).
* [Een Azure-cluster programmatisch schalen](service-fabric-cluster-programmatic-scaling.md) SDK met de fluent Azure compute.
* [Een zelfstandige-cluster in- of uitschalen](service-fabric-cluster-windows-server-add-remove-nodes.md).

