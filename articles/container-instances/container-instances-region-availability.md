---
title: Beschikbaarheid van Azure Container Instances-resource
description: Beschikbaarheid van resources voor Computing en geheugen voor de Azure Container Instances-service in verschillende Azure-regio's.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 05/14/2019
ms.author: danlep
ms.openlocfilehash: 64b60178413e470cc7fe9b3991c6fc29b5a0f860
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65794300"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>De beschikbaarheid van Azure Container Instances in Azure-regio 's

Dit artikel worden de beschikbaarheid van Azure Container Instances-Computing en geheugen resources in Azure-regio's. 

Waarden die worden gepresenteerd zijn de maximale resources beschikbaar per implementatie van een [containergroep](container-instances-container-groups.md). Waarden zijn actueel op het moment van publicatie. 

> [!NOTE]
> Containergroepen gemaakt binnen deze resourcelimieten zijn afhankelijk van beschikbaarheid in de implementatieregio. Wanneer een regio zwaar wordt belast, kan er een fout optreden bij het implementeren van instanties. Beperken van dergelijke implementatiefouten, implementeert u instanties met lagere broninstellingen of voert u de implementatie op een later tijdstip.

Zie voor meer informatie over quota's en andere limieten in uw implementaties [quota en limieten voor Azure Container Instances](container-instances-quotas.md).

## <a name="availability---general"></a>Beschikbaarheid - algemeen

De volgende regio's en resources zijn beschikbaar voor containergroepen met Linux en [ondersteund](container-instances-faq.md#what-windows-base-os-images-are-supported) containers op basis van Windows Server 2016.

| Locatie | OS | CPU | Geheugen (GB) |
| -------- | -- | :---: | :-----------: |
| Canada centraal, centraal-India, VS-midden, Oost-Azië, VS-Oost, VS-Oost 2, Noord-Europa, Zuid-centraal VS, Zuidoost-Azië, UK-Zuid, VS-West | Linux | 4 | 16 |
| West-Europa, VS-West 2 | Linux | 4 | 14 |
| Australië-Oost, Japan-Oost | Linux | 2 | 8 |
| Noord-centraal VS, Zuid-India | Linux | 2 | 3,5 |
| Europa -west | Windows | 4 | 16 |
| US - oost, US - west | Windows | 4 | 14 |
| Australië-Oost, Canada centraal, centraal-India, VS-midden, Oost-Azië, VS-Oost 2, Japan-Oost, Noord-centraal VS, Noord-Europa, Zuid-centraal VS, Zuidoost-Azië, Zuid-India, UK-Zuid, VS-West 2 | Windows | 2 | 3,5 |

## <a name="availability---windows-server-2019-ltsc-1809-deployments-preview"></a>Beschikbaarheid - Windows Server 2019 LTSC 1809 implementaties (preview)

De volgende regio's en resources zijn beschikbaar voor containergroepen met containers op basis van Windows Server 2019 (preview).

| Locatie | OS | CPU | Geheugen (GB) |
| -------- | -- | :---: | :-----------: |
| Zuidoost-Azië, Noord-Europa, West-Europa, VS-midden, VS-Oost, VS-West, VS-West 2 | Windows | 4 | 16 |
| US - oost 2 | Windows | 2 | 3,5 |


## <a name="availability---virtual-network-deployment-preview"></a>Beschikbaarheid - implementatie voor virtuele netwerken (preview)

De volgende regio's en bronnen beschikbaar zijn voor een containergroep geïmplementeerd in een [virtueel Azure-netwerk](container-instances-vnet.md) (preview).

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

## <a name="availability---gpu-resources-preview"></a>Beschikbaarheid - GPU-resources (preview)

De volgende regio's en bronnen beschikbaar zijn voor een containergroep geïmplementeerd met [GPU resources](container-instances-gpu.md) (preview).

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]
[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

## <a name="next-steps"></a>Volgende stappen

Brengt het team weten als u graag meer regio's of betere beschikbaarheid op [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Zie voor meer informatie over het oplossen van de implementatie van de containers-instantie [probleemoplossing voor implementatieproblemen met Azure Container Instances](container-instances-troubleshooting.md).
