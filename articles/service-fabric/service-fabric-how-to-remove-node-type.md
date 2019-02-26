---
title: Verwijderen van een knooppunttype in Azure Service Fabric | Microsoft Docs
description: Informatie over het verwijderen van een knooppunttype uit een Service Fabric-cluster in Azure uitvoert.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chakdan
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/14/2019
ms.author: aljo
ms.openlocfilehash: 2bde95b744ac136e8ba5c0517e0f749a6dce8a1e
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56805270"
---
# <a name="remove-a-service-fabric-node-type"></a>Verwijderen van de soort van een Service Fabric-knooppunt
In dit artikel wordt beschreven hoe u een Azure Service Fabric-cluster schalen door het verwijderen van een bestaande knooppunttype uit een cluster. Een Service Fabric-cluster is een netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Een machine of virtuele machine die deel uitmaakt van een cluster, heet een knooppunt. Virtuele-machineschaalsets vormen een Azure compute-resource die u gebruikt om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunttype die is gedefinieerd in een Azure-cluster is [instellen als een afzonderlijke schaalset](service-fabric-cluster-nodetypes.md). Vervolgens kan elk knooppunttype afzonderlijk worden beheerd. Na het maken van een Service Fabric-cluster, kunt u een cluster horizontaal schalen door een knooppunttype (virtuele-machineschaalset) en alle bijbehorende knooppunten te verwijderen.  U kunt het cluster schalen op elk gewenst moment, zelfs wanneer workloads worden uitgevoerd op het cluster.  Als het cluster wordt geschaald, wordt uw toepassingen automatisch ook schalen.

Gebruik [Remove-AzureRmServiceFabricNodeType](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/remove-azurermservicefabricnodetype) te verwijderen van de soort van een Service Fabric-knooppunt.

De drie bewerkingen die zich voordoen wanneer Remove-AzureRmServiceFabricNodeType heet zijn:
1.  De virtuele-machineschaalset achter het knooppunttype is verwijderd.
2.  Het knooppunttype is verwijderd uit het cluster.
3.  Voor elk knooppunt binnen dat knooppunttype, wordt de status van de gehele voor dat knooppunt uit het systeem verwijderd. Als er services op dat knooppunt zijn, klikt u vervolgens de services eerst verplaatst uit naar een ander knooppunt. Als de clustermanager een knooppunt niet voor de replica/service vinden kan, klikt u vervolgens is de bewerking vertraagd/geblokkeerd.

> [!WARNING]
> Remove-AzureRmServiceFabricNodeType gebruiken om te verwijderen van een knooppunttype uit een productiecluster wordt niet aanbevolen moet regelmatig worden gebruikt. Het is een opdracht als de resource VM scale set achter het knooppunttype worden verwijderd. 

## <a name="durability-characteristics"></a>Kenmerken van duurzaamheid
Bij het gebruik van de Remove-AzureRmServiceFabricNodeType veiligheid prioriteit boven snelheid. Het knooppunttype moet Silver- of Gold [duurzaamheidsniveau](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster), omdat:
- Brons geeft u geen garanties over het opslaan van informatie over de status.
- Silver en Gold duurzaamheid trap eventuele wijzigingen in de schaalset.
- Goud biedt u ook controle over de Azure-updates onder de schaalset.

Service Fabric 'regelt' onderliggende wijzigingen en updates zodat de gegevens zijn niet verloren gaan. Wanneer u een knooppunt met de duurzaamheid Brons verwijdert, kunt u informatie over de status verliezen. Als u een primaire knooppunttype verwijderen wilt en uw toepassing staatloos is, is Brons acceptabel. Wanneer u stateful werkbelastingen in de productieomgeving uitvoert, moet de minimale configuratie Silver. Op dezelfde manier voor productiescenario's moet het primaire knooppunttype altijd worden Silver- of Gold.

### <a name="more-about-bronze-durability"></a>Meer informatie over duurzaamheid Brons

Wanneer u een knooppunttype dat is Brons verwijdert, gaan alle knooppunten in het knooppunttype onmiddellijk omlaag. Service Fabric Brons knooppunten scale set updates niet overlappen, alle virtuele machines dus u kunt onmiddellijk. Als u iets stateful op deze knooppunten hebt, gaat de gegevens verloren. Nu, zelfs als u stateless was, alle knooppunten in de Service Fabric deel uitmaken van de ring, zodat u een volledige groep mogelijk verloren gaan, die het cluster zelf kan destabiliseren.

## <a name="recommended-node-type-removal-process"></a>Aanbevolen procedure voor het verwijderen van type knooppunt

Als u wilt verwijderen van het knooppunttype, voer de [Remove-AzureRmServiceFabricNodeType](/powershell/module/azurerm.servicefabric/remove-azurermservicefabricnodetype) cmdlet.  De cmdlet duurt enige tijd om te voltooien.  Voer [Remove-ServiceFabricNodeState](/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) op elk van de knooppunten die moeten worden verwijderd.

```powershell
$groupname = "mynodetype"
$nodetype = "nt2vm"
$clustername = "mytestcluster"

Remove-AzureRmServiceFabricNodeType -Name $clustername  -NodeType $nodetype -ResourceGroupName $groupname

Connect-ServiceFabricCluster -ConnectionEndpoint mytestcluster.eastus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <thumbprint> `
          -FindType FindByThumbprint -FindValue <thumbprint> `
          -StoreLocation CurrentUser -StoreName My

$nodes = Get-ServiceFabricNode | Where-Object {$_.NodeType -eq $nodetype} | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending

Foreach($node in $nodes)
{
    Remove-ServiceFabricNodeState -NodeName $node.NodeName -TimeoutSec 300 -Force 
}
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over cluster [duurzaamheid kenmerken](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).
- Meer informatie over [knooppunttypen en Virtual Machine Scale Sets](service-fabric-cluster-nodetypes.md).
- Meer informatie over [Service Fabric-cluster schalen](service-fabric-cluster-scaling.md).
