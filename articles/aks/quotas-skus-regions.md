---
title: Quota, SKU en beschikbaarheid in regio's in Azure Kubernetes Service (AKS)
description: Meer informatie over de standaardquota, beperkte grootte van VM-SKU en beschikbaarheid in regio's van de Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: iainfou
ms.openlocfilehash: abeb9ef6e467b62cf7332e01e1b77c710b9ba4f4
ms.sourcegitcommit: 524625dd12e0537173616a991802075e2dc9da12
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2019
ms.locfileid: "64413061"
---
# <a name="quotas-virtual-machine-size-restrictions-and-region-availability-in-azure-kubernetes-service-aks"></a>Quota en beperkingen voor VM-grootte beschikbaarheid in regio's in Azure Kubernetes Service (AKS)

Alle Azure-services bevatten bepaalde standaardlimieten en -quota voor resources en functies. Voor de grootte van het knooppunt, bepaalde virtuele machine (VM) SKU's zijn vervolgens beperkt voor gebruik.

Dit artikel worden de standaardlimieten voor de resource voor resources in Azure Kubernetes Service (AKS), evenals de beschikbaarheid van de AKS-service in Azure-regio's.

## <a name="service-quotas-and-limits"></a>Servicequota en -limieten

[!INCLUDE [container-service-limits](../../includes/container-service-limits.md)]

## <a name="provisioned-infrastructure"></a>Ingerichte infrastructuur

Alle andere beperkingen voor netwerk, berekening en opslag zijn van toepassing op de ingerichte infrastructuur. Zie [Azure-abonnement en servicelimieten](../azure-subscription-service-limits.md) voor de relevante limieten.

## <a name="restricted-vm-sizes"></a>Beperkte VM-grootten

Elk knooppunt in een AKS-cluster bevat een vaste hoeveelheid rekenresources, zoals vCPU en geheugen. Als een AKS-knooppunten onvoldoende compute-resources bevat, mislukken schillen correct uit te voeren. Om ervoor te zorgen dat de vereiste *kube-systeem* schillen en uw toepassingen betrouwbaar kunnen worden gepland, de volgende VM-SKU's in AKS kan niet worden gebruikt:

- Standard_A0
- Standard_A1
- Standard_A1_v2
- Standard_B1s
- Standard_B1ms
- Standard_F1
- Standard_F1s

Zie voor meer informatie over VM-typen en hun rekenresources [grootten voor virtuele machines in Azure][vm-skus].

## <a name="region-availability"></a>Beschikbaarheid in regio’s

Voor de meest recente lijst van de locatie waar u kunt implementeren en clusters worden uitgevoerd, Zie [beschikbaarheid in regio's AKS][region-availability].

## <a name="next-steps"></a>Volgende stappen

Bepaalde standaardlimieten en -quota kunnen worden verhoogd. Als u een verhoging wilt aanvragen voor een of meer resources die een dergelijke verhoging ondersteunen, dient u een [Azure-ondersteuningsaanvraag][azure-support] in (selecteer Quota bij **Type probleem**).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
[region-availability]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service

<!-- LINKS - Internal -->
[vm-skus]: ../virtual-machines/linux/sizes.md
