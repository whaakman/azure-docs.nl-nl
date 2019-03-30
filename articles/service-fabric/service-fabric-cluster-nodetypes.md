---
title: Azure Service Fabric-knooppunttypen en VM-schaalsets | Microsoft Docs
description: Informatie over hoe Azure Service Fabric-knooppunt typen hebben betrekking op de virtuele-machineschaalset wordt ingesteld en op afstand verbinding maken met een schaal ingesteld exemplaar of elk clusterknooppunt.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: chackdan
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: chackdan
ms.openlocfilehash: 7f9397ee21f74fe6a776881940e5721264216b0f
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58660611"
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Azure Service Fabric-knooppunttypen en VM-schaalsets
[Virtuele-machineschaalsets](/azure/virtual-machine-scale-sets) worden een Azure compute-resource. U kunt schaalsets gebruiken om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunttype die u in een Azure Service Fabric-cluster definieert, stelt u een afzonderlijke schaal.  De Service Fabric-runtime geïnstalleerd op elke virtuele machine in de schaalset is ingesteld. U kunt onafhankelijk omhoog of omlaag schalen van elk knooppunttype, wijzigen van de SKU van het besturingssysteem die worden uitgevoerd op elk clusterknooppunt, verschillende open poorten bevatten en verschillende capaciteitsstatistieken gebruikt.

De volgende afbeelding ziet u een cluster met twee typen van de knooppunten, met de naam FrontEnd en BackEnd. Elk knooppunttype heeft vijf knooppunten.

![Een cluster met twee knooppunttypen][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>Virtuele machine schaalsetinstanties worden toegewezen aan knooppunten
Zoals u in de afbeelding hierboven, schaalsetinstanties begint bij het exemplaar 0 en vervolgens worden verhoogd met 1. De nummering wordt weergegeven in de knooppuntnamen van het. Knooppunt BackEnd_0 is bijvoorbeeld het exemplaar 0 van de back-end-schaalset. Deze specifieke schaalset heeft vijf exemplaren, met de naam BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 en BackEnd_4.

Wanneer u een schaalset opschalen, wordt een nieuw exemplaar gemaakt. De nieuwe scale set-exemplaarnaam is meestal dat de schaalset naam plus de volgende instantienummer. In ons voorbeeld is het BackEnd_5.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Scale set load balancers worden toegewezen aan knooppunttypen en schaalsets
Als u uw cluster in Azure portal is geïmplementeerd of het voorbeeld van Azure Resource Manager-sjabloon gebruikt, worden alle resources onder een resourcegroep worden weergegeven. Hier ziet u de load balancers voor elk scale set of knooppunt. De naam van de load balancer maakt gebruik van de volgende indeling: **LB -&lt;de naam van knooppunttype&gt;**. Een voorbeeld is de LB-sfcluster4doc-0, zoals wordt weergegeven in de volgende afbeelding:

![Resources][Resources]


## <a name="next-steps"></a>Volgende stappen
* Zie de [overzicht van de functie 'Overal implementeren' en een vergelijking met Azure beheerde clusters](service-fabric-deploy-anywhere.md).
* Meer informatie over [cluster security](service-fabric-cluster-security.md).
* [Extern verbinding maken met](service-fabric-cluster-remote-connect-to-azure-cluster-node.md) naar een specifieke scale set-exemplaar
* [De waarden RDP-poortbereik bijwerken](./scripts/service-fabric-powershell-change-rdp-port-range.md) op cluster-VM's na de implementatie
* [De gebruikersnaam van beheerder en het wachtwoord wijzigen](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) voor cluster-VM's

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
