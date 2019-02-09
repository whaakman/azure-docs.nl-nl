---
title: Overzicht van Azure als zelfstandige Service Fabric-clusters | Microsoft Docs
description: U kunt Service Fabric-clusters maken op VM's of computers waarop Windows Server of Linux wordt uitgevoerd. Dit betekent dat u zich kunt implementeren en uitvoeren van Service Fabric-toepassingen in een omgeving waarin u werkt met een set van Windows Server of Linux-computers die verbonden on-premises, Microsoft Azure zijn, of met elke andere cloudprovider.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/01/2019
ms.author: dekapur
ms.openlocfilehash: cf16a01ef1003b73740c05ff45f703a65aaf0acc
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55961178"
---
# <a name="comparing-azure-and-standalone-service-fabric-clusters-on-windows-server-and-linux"></a>Vergelijking van Azure en zelfstandige Service Fabric-clusters in Windows Server en Linux
Een Service Fabric-cluster is een netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Een machine of virtuele machine die deel uitmaakt van een cluster wordt een clusterknooppunt genoemd. Clusters kunnen worden geschaald naar duizenden knooppunten. Als u nieuwe knooppunten aan het cluster toevoegt, rebalances Service Fabric de partitiereplica's service en de exemplaren in het grotere aantal knooppunten. Algemene verbetert de prestaties van toepassingen en conflicten voor toegang tot het geheugen vermindert. Als de knooppunten in het cluster niet efficiënt worden gebruikt, kunt u het aantal knooppunten in het cluster verminderen. Service Fabric rebalances opnieuw de partitiereplica's en exemplaren voor het kleinere aantal knooppunten om beter gebruik van de hardware op elk knooppunt.

Service Fabric kunt u het maken van Service Fabric-clusters op alle virtuele machines of computers waarop Windows Server of Linux wordt uitgevoerd. Dit betekent dat u kunt implementeren en uitvoeren van Service Fabric-toepassingen in een omgeving waar u een set van Windows Server of Linux-computers onderling hebt, on-premises, Microsoft Azure, worden, of met elke andere cloudprovider.

## <a name="benefits-of-clusters-on-azure"></a>Voordelen van clusters op Azure
In Azure bieden we integratie met andere Azure-functies en services, waarmee bewerkingen en het beheer van het cluster eenvoudiger en betrouwbaarder.

* **Azure-portal:** Azure-portal kunt eenvoudig maken en beheren van clusters.
* **Azure Resource Manager:** Gebruik van Azure Resource Manager kunt eenvoudig beheer van alle resources die door het cluster worden gebruikt als een eenheid en kosten bijhouden en facturering vereenvoudigt.
* **Service Fabric-Cluster als een Azure-Resource** A Service Fabric-cluster is een Azure-resource, dus u kunt het model, zoals u ook andere resources in Azure.
* **Integratie met Azure-infrastructuur** coördineert de Service Fabric met de onderliggende Azure-infrastructuur voor OS-, netwerk en andere upgrades worden uitgevoerd voor betere beschikbaarheid en betrouwbaarheid van uw toepassingen.  
* **Diagnostische gegevens:** In Azure bieden we integratie met Azure diagnostics en Log Analytics.
* **Automatisch schalen:** Voor clusters op Azure bieden we ingebouwde functionaliteit voor automatisch schalen vanwege een virtuele-machineschaalsets. In on-premises en andere cloudomgevingen hebt u uw eigen automatisch schalen functie of schaal handmatig met behulp van de API's die Service Fabric beschikbaar maakt voor het schalen van clusters.

## <a name="benefits-of-standalone-clusters"></a>Voordelen van zelfstandige clusters
* U staat op vrije keuze van alle cloudproviders voor het hosten van uw cluster.
* Service Fabric-toepassingen, één keer worden geschreven, kunnen worden uitgevoerd in omgevingen met meerdere hosting met minimale geen wijzigingen.
* Kennis van het bouwen van Service Fabric-toepassingen die zijn doorgevoerd in van de ene host-omgeving naar een andere.
* Operationele ervaring uitvoeren en beheren van Service Fabric clusters uitvoert via van de ene omgeving naar een andere.
* Brede klant bereik is het niet-gebonden hostingbeperkingen omgeving.
* Er bestaat een extra laag voor betrouwbaarheid en de bescherming tegen storingen wijdverbreid omdat u de services verplaatsen kunt via naar een andere implementatieomgeving als een gegevensprovider center of in de cloud een onleesbaar heeft.

## <a name="next-steps"></a>Volgende stappen

* Lees het overzicht van [Service Fabric-clusters op Azure](service-fabric-azure-clusters-overview.md)
* Lees het overzicht van [Service Fabric zelfstandige clusters](service-fabric-standalone-clusters-overview.md)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)