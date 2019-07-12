---
title: Quota's, -SKU's en beschikbaarheid in regio's in Azure Kubernetes Service (AKS)
description: Meer informatie over de standaardquota, beperkte grootte van VM-SKU en beschikbaarheid in regio's van de Azure Kubernetes Service (AKS).
services: container-service
author: mlearned
ms.service: container-service
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: mlearned
ms.openlocfilehash: 318846cddecdf020e2e751d3a0b9e05fc83bba73
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614556"
---
# <a name="quotas-virtual-machine-size-restrictions-and-region-availability-in-azure-kubernetes-service-aks"></a>Quota en beperkingen voor VM-grootte beschikbaarheid in regio's in Azure Kubernetes Service (AKS)

Alle Azure-services instellen standaardlimieten en quota voor resources en functies. Bepaalde virtuele machine (VM) SKU's zijn ook voor gebruik met beperkte toegang.

Dit artikel worden de standaard resourcelimieten voor Azure Kubernetes Service (AKS)-resources en de beschikbaarheid van AKS in de Azure-regio's.

## <a name="service-quotas-and-limits"></a>Servicequota en -limieten

[!INCLUDE [container-service-limits](../../includes/container-service-limits.md)]

## <a name="provisioned-infrastructure"></a>Ingerichte infrastructuur

Alle andere beperkingen voor netwerk, berekening en opslag zijn van toepassing op de ingerichte infrastructuur. Zie voor de relevante limieten [Azure-abonnement en Servicelimieten](../azure-subscription-service-limits.md).

> [!IMPORTANT]
> Wanneer u een AKS-cluster bijwerkt, worden tijdelijk aanvullende bronnen verbruikt. Deze bronnen omvatten beschikbare IP-adressen in een subnet van het virtuele netwerk of vCPU-quotum voor virtuele machines. Als u Windows Server-containers (momenteel in preview in AKS), is de enige onderschreven aanpak van de meest recente updates van toepassing op de knooppunten een upgrade bewerking uit te voeren. Een mislukte cluster-upgradeproces kan erop wijzen dat u de beschikbare IP-adres ruimte of vCPU quotum voor het afhandelen van deze resources tijdelijk geen hebt. Zie voor meer informatie over het upgradeproces voor Windows Server-knooppunt [een knooppuntgroep in AKS Upgrade][nodepool-upgrade].

## <a name="restricted-vm-sizes"></a>Beperkte VM-grootten

Elk knooppunt in een AKS-cluster bevat een vaste hoeveelheid rekenresources, zoals vCPU en geheugen. Als een AKS-knooppunten onvoldoende compute-resources bevat, mislukken schillen correct uit te voeren. Om ervoor te zorgen dat de vereiste *kube-systeem* schillen en uw toepassingen betrouwbaar kunnen worden gepland, gebruik de volgende VM-SKU's niet in AKS:

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

Bepaalde standaardlimieten en -quota kunnen worden verhoogd. Als uw resource een toename ondersteunt, verhoging zijn via een [Azure-ondersteuningsaanvraag][azure-support] (voor **type probleem**, selecteer **quotum**).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
[region-availability]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service

<!-- LINKS - Internal -->
[vm-skus]: ../virtual-machines/linux/sizes.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
