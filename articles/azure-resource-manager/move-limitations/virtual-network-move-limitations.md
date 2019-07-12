---
title: Virtuele netwerken van Azure-resources verplaatsen naar een nieuw abonnement of resourcegroep groep | Microsoft Docs
description: Azure Resource Manager gebruiken voor het verplaatsen van virtuele netwerken aan een nieuwe resourcegroep of abonnement.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: 809d403a463048910a284e7f8fcdc18cf7c65b69
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723491"
---
# <a name="move-guidance-for-virtual-networks"></a>Richtlijnen voor virtuele netwerken verplaatsen

In dit artikel wordt beschreven hoe u virtuele netwerken voor specifieke scenario's te verplaatsen.

## <a name="dependent-resources"></a>Afhankelijke resources

Bij het verplaatsen van een virtueel netwerk, moet u ook de afhankelijke resources verplaatsen. Voor VPN-Gateways, moet u IP-adressen, virtuele netwerkgateways en alle bijbehorende verbindingsresources verplaatsen. Lokale netwerkgateways kunnen zich in een andere resourcegroep.

Voor het verplaatsen van een virtuele machine met een netwerkinterfacekaart, moet u alle afhankelijke resources verplaatsen. Het virtuele netwerk voor de netwerkkaart, andere netwerkinterfacekaarten voor het virtuele netwerk en de VPN-gateways verplaatsen.

## <a name="peered-virtual-network"></a>Gekoppelde virtuele netwerk

Voor het verplaatsen van een gekoppeld virtueel netwerk, moet u eerst de virtueel-netwerkpeering uitschakelen. Als uitgeschakeld, kunt u het virtuele netwerk kunt verplaatsen. Na de verplaatsing opnieuw de peering op virtueel netwerk.

## <a name="subnet-links"></a>Subnetkoppelingen

U kunt een virtueel netwerk niet verplaatsen naar een ander abonnement, als het virtuele netwerk een subnet met resourcenavigatiekoppelingen bevat. Bijvoorbeeld, als een Azure-Cache voor Redis-resource wordt geïmplementeerd in een subnet, heeft dat subnet een resourcenavigatiekoppeling.

## <a name="next-steps"></a>Volgende stappen

Zie voor de opdrachten om resources te verplaatsen, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../resource-group-move-resources.md).
