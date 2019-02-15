---
title: Een knooppunttype toevoegen aan een Azure Service Fabric-cluster | Microsoft Docs
description: Leer hoe u een Service Fabric-cluster uitbreiden door het toevoegen van een virtuele-Machineschaalset opgehaald.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/13/2019
ms.author: aljo
ms.openlocfilehash: 01d4af8349d3f5a0f58c4c3fa56b489d739c7b42
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301703"
---
# <a name="scale-a-service-fabric-cluster-out-by-adding-a-virtual-machine-scale-set"></a>Schalen van een Service Fabric-cluster af door het toevoegen van een virtuele-machineschaalset
In dit artikel wordt beschreven hoe u een Azure Service Fabric-cluster schalen door een nieuw knooppunttype toe te voegen aan een bestaand cluster. Een Service Fabric-cluster is een netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Een machine of virtuele machine die deel uitmaakt van een cluster, heet een knooppunt. Virtuele-machineschaalsets vormen een Azure compute-resource die u gebruikt om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunttype die is gedefinieerd in een Azure-cluster is [instellen als een afzonderlijke schaalset](service-fabric-cluster-nodetypes.md). Vervolgens kan elk knooppunttype afzonderlijk worden beheerd. Na het maken van een Service Fabric-cluster, kunt u een cluster horizontaal schalen door een nieuwe knooppunttype (virtuele-machineschaalset) toe te voegen aan een bestaand cluster.  U kunt het cluster schalen op elk gewenst moment, zelfs wanneer workloads worden uitgevoerd op het cluster.  Als het cluster wordt geschaald, wordt uw toepassingen automatisch ook schalen.

## <a name="add-an-additional-scale-set-to-an-existing-cluster"></a>Voeg een extra schaalset aan een bestaand cluster
Een nieuwe versie van het knooppunttype (die wordt ondersteund door een virtuele-machineschaalset) toevoegen aan een bestaand cluster is vergelijkbaar met [upgraden van het primaire knooppunttype](service-fabric-scale-up-node-type.md), tenzij u de dezelfde NodeTypeRef; niet gebruikt natuurlijk wordt niet worden uitgeschakeld een actief gebruikt schaalsets voor virtuele machines en u niet verliezen beschikbaarheid van het cluster als het primaire knooppunttype niet is bijgewerkt. 

NodeTypeRef eigenschap in de virtuele machine is gedeclareerd extensie-eigenschappen van Service Fabric-schaalset:
```json
<snip>
"publisher": "Microsoft.Azure.ServiceFabric",
     "settings": {
     "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
     "nodeTypeRef": "[parameters('vmNodeType2Name')]",
     "dataPath": "D:\\\\SvcFab",
     "durabilityLevel": "Silver",
<snip>
```

Daarnaast moet u dit nieuwe knooppunttype toevoegen aan uw Service Fabric-cluster-bron:

```json
<snip>
"nodeTypes": [
      {
      "name": "[parameters('vmNodeType2Name')]",
      "applicationPorts": {
                "endPort": "[parameters('nt2applicationEndPort')]",
                "startPort": "[parameters('nt2applicationStartPort')]"
      },
      "clientConnectionEndpointPort": "[parameters('nt2fabricTcpGatewayPort')]",
      "durabilityLevel": "Silver",
       "ephemeralPorts": {
                "endPort": "[parameters('nt2ephemeralEndPort')]",
                "startPort": "[parameters('nt2ephemeralStartPort')]"
      },
      "httpGatewayEndpointPort": "[parameters('nt2fabricHttpGatewayPort')]",
      "isPrimary": false,
      "vmInstanceCount": "[parameters('nt2InstanceCount')]"
},
<snip>
```

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [opschalen van het primaire knooppunttype](service-fabric-scale-up-node-type.md)
* Meer informatie over [toepassing schaalbaarheid](service-fabric-concepts-scalability.md).
* [Schalen van een Azure-cluster in- of uitgeschaald](service-fabric-tutorial-scale-cluster.md).
* [Een Azure-cluster programmatisch schalen](service-fabric-cluster-programmatic-scaling.md) SDK met de fluent Azure compute.
* [Een zelfstandige-cluster in- of uitschalen](service-fabric-cluster-windows-server-add-remove-nodes.md).

