---
title: Azure Service Fabric-knooppunttypen en de virtuele machine schalen sets | Microsoft Docs
description: Meer informatie over hoe Azure Service Fabric-knooppunt typen hebben betrekking op de virtuele-machineschaalset wordt ingesteld en op afstand verbinding maken met een schaal exemplaar of clusterknooppunt.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: chackdan
ms.openlocfilehash: ce3b3e6f3c7ee0c7198c27ed0ca0e3610d26f143
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Azure Service Fabric-knooppunttypen en de virtuele machine sets schalen
[Virtuele-machineschaalsets](/azure/virtual-machine-scale-sets) zijn van een Azure compute resource. U kunt schaalsets gebruiken om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunttype dat u in een Azure Service Fabric-cluster opgeeft, stelt u een afzonderlijke schaal.  De Service Fabric-runtime geïnstalleerd op elke virtuele machine in de schaal is ingesteld. U kunt onafhankelijk elk knooppunttype omhoog of omlaag schalen, wijzigen van de OS-SKU uitgevoerd op elk clusterknooppunt, hebben verschillende sets van poorten openen en gebruiken van andere capaciteitsmetrieken.

De volgende afbeelding ziet een cluster met twee knooppunttypen met de naam FrontEnd en BackEnd. Elk knooppunttype heeft vijf knooppunten.

![Een cluster met twee knooppunttypen][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>VM-scale set instanties worden toegewezen aan knooppunten
Zoals u in de voorgaande afbeelding, scale set instanties op exemplaar 0 start en vervolgens te verhogen door 1. De nummering wordt doorgevoerd in de knooppuntnamen. Bijvoorbeeld, is knooppunt BackEnd_0 exemplaar 0 van de back-end-scale set. Deze bepaalde schaalset heeft vijf exemplaren, met de naam BackEnd_0, BackEnd_1 BackEnd_2, BackEnd_3 en BackEnd_4.

Wanneer u een schaalset opschalen, wordt een nieuw exemplaar gemaakt. De nieuwe scale set-exemplaarnaam is meestal dat de schaal naam plus het volgende instantienummer ingesteld. In ons voorbeeld is het BackEnd_5.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Scale set netwerktaakverdelers worden toegewezen aan knooppunttypen en sets schalen
Als u uw cluster in de Azure portal geïmplementeerd of de steekproef Azure Resource Manager-sjabloon gebruikt, worden alle bronnen van een resourcegroep worden weergegeven. Hier ziet u de load balancers voor elk scale set of een knooppunt. De naam van de load balancer gebruikt de volgende indeling: **LB -&lt;naam knooppunttype&gt;**. Een voorbeeld is de Load Balancer-sfcluster4doc-0, zoals wordt weergegeven in de volgende afbeelding:

![Resources][Resources]


## <a name="next-steps"></a>Volgende stappen
* Zie de [overzicht van de functie 'Overal implementeren' en een vergelijking met Azure beheerde clusters](service-fabric-deploy-anywhere.md).
* Meer informatie over [beveiliging cluster](service-fabric-cluster-security.md).
* [Externe verbinding](service-fabric-cluster-remote-connect-to-azure-cluster-node.md) naar een specifieke scale set-exemplaar
* [Bijwerken van de waarden voor het RDP-poort](./scripts/service-fabric-powershell-change-rdp-port-range.md) op cluster virtuele machines na implementatie
* [De beheerdersgebruikersnaam en wachtwoord wijzigen](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) voor cluster virtuele machines

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
