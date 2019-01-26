---
title: Azure Service Fabric best practices voor netwerken | Microsoft Docs
description: Aanbevolen procedures voor het beheren van Service Fabric-netwerken.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: jeanpaul.connock
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: 4118e52e8fac34e860f05e17effb01e9375582ec
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54913831"
---
# <a name="networking"></a>Netwerken

Bij het maken en beheren van Azure Service Fabric-clusters, biedt u verbinding met het netwerk voor uw knooppunten en toepassingen. De netwerkresources zijn IP-adresbereiken, virtuele netwerken, load balancers en netwerkbeveiligingsgroepen. In dit artikel leert u aanbevolen procedures voor deze resources.

Bekijk Azure [Service Fabric netwerken patronen](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking) voor meer informatie over het maken van clusters die gebruikmaken van de volgende functies: Bestaand virtueel netwerk of subnet, statische openbare IP-adres, alleen-interne load balancer, of interne en externe load balancer.

## <a name="infrastructure-networking"></a>Netwerken voor infrastructuur
Optimale prestaties van de virtuele Machine met versnelde netwerken, is door op te geven enableAcceleratedNetworking-eigenschap in de Resource Manager-sjabloon, het volgende fragment van een virtuele Machine schaal ingesteld NetworkInterfaceConfigurations die u kunt versnelde netwerken:

```json
"networkInterfaceConfigurations": [
  {
    "name": "[concat(variables('nicName'), '-0')]",
    "properties": {
      "enableAcceleratedNetworking": true,
      "ipConfigurations": [
        {
        <snip>
        }
      ],
      "primary": true
    }
  }
]
```
Service Fabric-cluster kan worden ingericht op [Linux met versnelde netwerken](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli), en [Windows met versnelde netwerken](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell).

Versneld netwerken wordt ondersteund voor Azure Virtual Machine-serie-SKU's: D/DSv2-, D/DSv3, E/ESv3, F/FS, FSv2 en Ms-/ Mms. Versneld netwerken is getest met de SKU Standard_DS8_v3 23-1/2019.

Om in te schakelen versnelde netwerken op een bestaande Service Fabric-cluster, moet u eerst [schalen van een Service Fabric-cluster af door het toevoegen van een virtuele-Machineschaalset](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out), voer de volgende stappen uit:
1. Inrichten van een NodeType met versnelde netwerken ingeschakeld
2. Migreren van uw services en hun status naar de ingerichte NodeType met versnelde netwerken ingeschakeld

Uitschalen infrastructuur is vereist om in te schakelen versnelde netwerken in een bestaand cluster, omdat downtime, waardoor Accelerated Networking in plaats veroorzaken zou omdat hiervoor alle virtuele machines in een beschikbaarheidsset worden [stoppen en toewijzing ongedaan maken voordat u inschakelt versnelde netwerken op een bestaande NIC](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms).

## <a name="cluster-networking"></a>Clusternetwerk

* Service Fabric-clusters kunnen worden geïmplementeerd in een bestaand virtueel netwerk met de volgende stappen wordt beschreven in [Service Fabric netwerken patronen](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking).

* Netwerkbeveiligingsgroepen (nsg's) worden aanbevolen voor knooppunttypen dat binnenkomend en uitgaand verkeer tot het cluster beperken. Zorg ervoor dat de vereiste poorten zijn geopend in de NSG. Bijvoorbeeld: ![Service Fabric NSG-regels][NSGSetup]

* Het primaire knooppunttype, waarin de Service Fabric-systeemservices hoeft niet te worden blootgesteld via de externe load balancer en kunnen worden weergegeven door een [interne load balancer](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking#internal-only-load-balancer)

* Gebruik een [statisch openbaar IP-adres](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking#static-public-ip-address-1) voor uw cluster.

## <a name="application-networking"></a>Toepassing netwerken

* Gebruiken om uit te voeren op Windows-containerwerkbelastingen, [open netwerken modus](https://docs.microsoft.com/azure/service-fabric/service-fabric-networking-modes#set-up-open-networking-mode) service-naar-servicecommunicatie om gemakkelijker te maken.

* Een omgekeerde proxy gebruiken zoals [Traefik](https://docs.traefik.io/configuration/backends/servicefabric/) of de [Service Fabric reverse proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) om algemene toepassingspoorten, zoals 80 of 443 zichtbaar te maken.

## <a name="next-steps"></a>Volgende stappen

* Een cluster maken op virtuele machines of op computers met Windows Server: [Service Fabric-cluster maken voor Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Een cluster maken op VM's of computers waarop Linux wordt uitgevoerd: [Een Linux-cluster maken](service-fabric-cluster-creation-via-portal.md)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)

[NSGSetup]: ./media/service-fabric-best-practices/service-fabric-nsg-rules.png
