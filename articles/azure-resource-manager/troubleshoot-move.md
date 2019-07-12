---
title: Problemen oplossen bij het Azure-resources verplaatsen naar nieuwe groep voor abonnement of resourcegroep
description: Azure Resource Manager gebruiken voor resources verplaatsen naar een nieuwe resourcegroep of abonnement.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: e23d7c571a010e5bfb42e5f15368e0194272ed53
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723465"
---
# <a name="troubleshoot-moving-azure-resources-to-new-resource-group-or-subscription"></a>Oplossen van Azure-resources verplaatsen naar nieuwe resourcegroep of abonnement

In dit artikel bevat suggesties voor het oplossen van problemen bij het verplaatsen van resources.

## <a name="upgrade-a-subscription"></a>Een abonnement

Als u wilt upgraden van uw Azure-abonnement (zoals het overschakelen van gratis naar betalen per gebruik), moet u uw abonnement te zetten.

* Zie het upgraden van een gratis proefversie [uw gratis proefversie of Azure voor Microsoft Imagine-abonnement upgraden naar betalen per gebruik](../billing/billing-upgrade-azure-subscription.md).
* Als u wilt wijzigen in een betalen per gebruik-account, Zie [wijzigen van uw Azure-betalen per gebruik-abonnement naar een andere aanbieding](../billing/billing-how-to-switch-azure-offer.md).

Als u het abonnement niet kan omzetten [maken van een Azure-ondersteuningsaanvraag](../azure-supportability/how-to-create-azure-support-request.md). Selecteer **Abonnementsbeheer** voor het probleemtype.

## <a name="service-limitations"></a>Beperkingen voor service

Sommige services moet aanvullende overwegingen bij het verplaatsen van resources. Als u de volgende services verplaatst, zorg er dan voor dat u de richtlijnen en beperkingen controleren.

* [App Services](./move-limitations/app-service-move-limitations.md)
* [Azure DevOps Services](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
* [Klassiek implementatiemodel](./move-limitations/classic-model-move-limitations.md)
* [Recovery Services](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
* [Virtuele machines](./move-limitations/virtual-machines-move-limitations.md)
* [Virtuele netwerken](./move-limitations/virtual-network-move-limitations.md)

## <a name="large-requests"></a>Grote aanvragen

Indien mogelijk is grote break verplaatst naar afzonderlijke verplaatsingsbewerkingen. Resource Manager retourneert onmiddellijk een fout op wanneer er meer dan 800 resources in één bewerking. Verplaatsen van minder dan 800 resources kan echter ook mislukken door een time-out optreedt.

## <a name="next-steps"></a>Volgende stappen

Zie voor de opdrachten om resources te verplaatsen, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](resource-group-move-resources.md).
