---
title: Beschikbaarheid van Azure Container Instances-resource
description: Beschikbaarheid van resources voor Computing en geheugen voor de Azure Container Instances-service in verschillende Azure-regio's.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 03/01/2019
ms.author: danlep
ms.openlocfilehash: 1ca23a95c746139963aa70ed20bb888152fd5cd8
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554848"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>De beschikbaarheid van Azure Container Instances in Azure-regio 's

Dit artikel worden de beschikbaarheid van Azure Container Instances-Computing en geheugen resources in Azure-regio's. 

Waarden die worden gepresenteerd zijn de maximale resources beschikbaar per implementatie van een [containergroep](container-instances-container-groups.md). Waarden zijn actueel op het moment van publicatie. Gebruik de API [List Capabilities](/rest/api/container-instances/listcapabilities/listcapabilities) voor actuele informatie. 

> [!NOTE]
> Containergroepen gemaakt binnen deze resourcelimieten zijn afhankelijk van beschikbaarheid in de implementatieregio. Wanneer een regio zwaar wordt belast, kan er een fout optreden bij het implementeren van instanties. Beperken van dergelijke implementatiefouten, implementeert u instanties met lagere broninstellingen of voert u de implementatie op een later tijdstip.

Zie voor meer informatie over quota's en andere limieten in uw implementaties [quota en limieten voor Azure Container Instances](container-instances-quotas.md).

## <a name="availability---general"></a>Beschikbaarheid - algemeen

| Locatie | OS | CPU | Geheugen (GB) |
| -------- | -- | :---: | :-----------: |
| Canada - centraal, US - centraal, US - oost 2, US - zuid-centraal | Linux | 4 | 16 |
| US - oost, Europa - noord, Europa - west, US - west, US - west 2 | Linux | 4 | 14 |
| Japan - oost | Linux | 2 | 8 |
| Australië - oost, Azië - zuidoost | Linux | 2 | 7 |
| India - centraal, Azië - oost, US - noord-centraal, India - zuid | Linux | 2 | 3,5 |
| US - oost, Europa - west, US - west | Windows | 4 | 14 |
| Australië - oost, Canada - centraal, India - centraal, US - centraal, Azië - oost, US - oost 2, Japan - oost, US - noord-centraal, Europa - noord, US - zuid-centraal, India - zuid, Azië - zuidoost, US - west 2 | Windows | 2 | 3,5 |

## <a name="availability---virtual-network-deployment-preview"></a>Beschikbaarheid - implementatie voor virtuele netwerken (preview)

De volgende regio's en bronnen beschikbaar zijn voor een containergroep geïmplementeerd in een [virtueel Azure-netwerk](container-instances-vnet.md) (preview)

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

## <a name="availability---gpu-resources-preview"></a>Beschikbaarheid - GPU-resources (preview)

De volgende regio's en bronnen beschikbaar zijn voor een containergroep geïmplementeerd met [GPU resources](container-instances-gpu.md) (preview)

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]
[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

## <a name="next-steps"></a>Volgende stappen

Brengt het team weten als u graag meer regio's of betere beschikbaarheid op [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Zie voor meer informatie over het oplossen van de implementatie van de containers-instantie [probleemoplossing voor implementatieproblemen met Azure Container Instances](container-instances-troubleshooting.md).
