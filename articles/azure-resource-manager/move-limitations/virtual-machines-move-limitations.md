---
title: Azure virtuele Machines naar nieuwe abonnement of resourcegroep groep verplaatsen | Microsoft Docs
description: Azure Resource Manager gebruiken voor virtuele machines verplaatsen naar een nieuwe resourcegroep of abonnement.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: 095ed1c8d2328b1eb391042125526696ba8cda49
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723543"
---
# <a name="move-guidance-for-virtual-machines"></a>Richtlijnen voor virtuele machines verplaatsen

Dit artikel beschrijft de scenario's die worden momenteel niet ondersteund en de stappen voor het verplaatsen van virtuele machines met back-up.

## <a name="scenarios-not-supported"></a>Scenario's niet ondersteund

De volgende scenario's nog niet ondersteund:

* Beheerde schijven in Beschikbaarheidszones kunnen niet worden verplaatst naar een ander abonnement.
* Virtuele Machines met een certificaat dat is opgeslagen in Key Vault kan worden verplaatst naar een nieuwe resourcegroep in hetzelfde abonnement, maar niet tussen meerdere abonnementen.
* Virtual Machine Scale Sets met standaard SKU Load Balancer of een standaard SKU en openbare IP kan niet worden verplaatst.
* Virtuele machines die zijn gemaakt op basis van Marketplace-resources met een abonnement dat is gekoppeld kan niet worden verplaatst tussen resourcegroepen of abonnementen. Inrichting ongedaan maken van de virtuele machine in het huidige abonnement en opnieuw implementeren in het nieuwe abonnement.
* Virtuele machines in een bestaand virtueel netwerk, maar worden niet alle resources in het virtuele netwerk verplaatsen.

## <a name="virtual-machines-with-azure-backup"></a>Virtuele machines met Azure Backup

Voor het verplaatsen van virtuele machines die zijn geconfigureerd met Azure Backup, gebruikt u de volgende tijdelijke oplossing:

* Zoek de locatie van uw virtuele Machine.
* Vinden van een resourcegroep met de volgende naamgevingspatroon: `AzureBackupRG_<location of your VM>_1` bijvoorbeeld AzureBackupRG_westus2_1
* Als in Azure portal, klikt u vervolgens selectievakje ' verborgen typen weergeven"
* Als in PowerShell, gebruikt u de `Get-AzResource -ResourceGroupName AzureBackupRG_<location of your VM>_1` cmdlet
* Als u in de CLI, gebruikt u de `az resource list -g AzureBackupRG_<location of your VM>_1`
* Het vinden van de resource met het type `Microsoft.Compute/restorePointCollections` waarvoor het naamgevingspatroon `AzureBackup_<name of your VM that you're trying to move>_###########`
* Deze resource verwijderen. Deze bewerking wordt alleen de directe herstelpunten, niet de gegevens waarvan een back-up is gemaakt in de kluis verwijderd.
* Nadat het verwijderen is voltooid, kunt u de kluis en de virtuele machine verplaatsen naar het doelabonnement. Na het verplaatsen, kunt u back-ups zonder verlies van gegevens blijven.
* Zie voor meer informatie over het verplaatsen Recovery Services-kluizen voor back-up [beperkingen voor Recovery Services](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="next-steps"></a>Volgende stappen

Zie voor de opdrachten om resources te verplaatsen, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../resource-group-move-resources.md).
