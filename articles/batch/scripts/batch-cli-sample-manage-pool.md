---
title: Azure CLI - voorbeeldscript voor het beheren van opslaggroepen in Batch | Microsoft Docs
description: Azure CLI-voorbeeldscript - opslaggroepen in Batch beheren
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: ae7eab97c1da1113b0248b74a9dd67de8ce49e36
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="managing-azure-batch-pools-with-azure-cli"></a>Het beheren van Azure Batch-pools met Azure CLI

Deze script laat zien van de hulpprogramma's die beschikbaar zijn in de Azure CLI maken en beheren van pools van rekenknooppunten in de Azure Batch-service.

> [!NOTE]
> De opdrachten in dit voorbeeld maken Azure virtuele machines. VM's worden uitgevoerd, wordt de kosten voor je account doorlopen. Verwijder de virtuele machines om te beperken deze kosten, zodra u klaar bent met het voorbeeld. Zie [pools opschonen](#clean-up-pools).

Batch-pools kunnen worden geconfigureerd op twee manieren aan een Cloud Services-configuratie (alleen Windows) of de configuratie van een virtuele Machine (Windows en Linux). De onderstaande voorbeeldscripts laten zien hoe groepen met beide configuraties te maken.

## <a name="prerequisites"></a>Vereisten

- Installeer de Azure CLI met behulp van de instructies in de [Azure CLI installatiehandleiding](https://docs.microsoft.com/cli/azure/install-azure-cli), als u dit nog niet hebt gedaan.
- Maak een Batch-account als u er nog geen hebt. Zie [een Batch-account maken met de Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) voor een voorbeeld van een script dat een account maakt.
- Configureer een toepassing voor uitvoering van een begintaak als u dit nog niet hebt gedaan. Zie [toepassingen toevoegen aan Azure Batch met Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) voor een voorbeeld van een script dat een toepassing maakt en toepassingspakketten uploadt naar Azure.

## <a name="pool-with-cloud-service-configuration-sample-script"></a>Groep met voorbeeldscript voor cloud service-configuratie

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Cloud Services Pools")]

## <a name="pool-with-virtual-machine-configuration-sample-script"></a>Groep met de virtuele machine configuratie voorbeeldscript

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Virtual Machine Pools")]

## <a name="clean-up-pools"></a>Opschonen van groepen

Voer de volgende opdracht om de groepen verwijderen na het uitvoeren van script in het bovenstaande voorbeeld.
```azurecli
az batch pool delete --pool-id mypool-windows
az batch pool delete --pool-id mypool-linux
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten om te maken en manipuleren van Batch-pools.
Elke opdracht in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [aanmeldingsnaam AZ batch-account](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Verificatie uitvoeren tegen een Batch-account.  |
| [overzicht van de AZ batch-toepassing](https://docs.microsoft.com/cli/azure/batch/application/summary#az_batch_application_summary_list) | Lijst van de beschikbare toepassingen in de Batch-account.  |
| [AZ batch-pool maken](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_create) | Een pool van virtuele machines maken.  |
| [AZ batch pool instellen](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_set) | Eigenschappen van een groep bijwerken.  |
| [lijst van AZ batch pool knooppunt-agent-SKU 's](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#az_batch_pool_node_agent_skus_list) | Lijst met beschikbare knooppunt agent SKU's en informatie over de afbeelding.  |
| [AZ batch-pool formaat wijzigen](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_resize) | De grootte van het aantal VM's worden uitgevoerd in de opgegeven groep.  |
| [AZ batch pool weergeven](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_show) | De eigenschappen van een groep weergeven.  |
| [AZ batch pool verwijderen](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_delete) | Verwijder de opgegeven groep.  |
| [AZ batch-pool automatisch schalen inschakelen](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_enable) | Automatische schaling inschakelen op een pool en toepassen van een formule.  |
| [AZ batch-pool automatisch schalen uitschakelen](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_disable) | Schakel automatisch schalen op een groep.  |
| [lijst met knooppunten AZ-batch](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_list) | Lijst van het rekenknooppunt in de opgegeven groep.  |
| [AZ batch knooppunt opnieuw opstarten](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_reboot) | Het opgegeven computerknooppunt opnieuw opstarten.  |
| [AZ batch knooppunt verwijderen](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_delete) | Verwijder de vermelde knooppunten van de opgegeven groep.  |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Aanvullende Batch CLI scriptvoorbeelden vindt u in de [documentatie van Azure Batch CLI](../batch-cli-samples.md).

