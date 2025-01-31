---
title: Azure-quickstart - Een back-up van een VM maken met Azure CLI
description: Lees hoe u een back-up van virtuele machines maakt met Azure CLI
services: backup
author: rayne-wiselman
manager: carmonm
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 01/31/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: d3ed9370726d35f67edfbcf32dfd25e74d7865e5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "66127677"
---
# <a name="back-up-a-virtual-machine-in-azure-with-the-cli"></a>Een back-up van een virtuele machine maken in Azure met de CLI
De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. U kunt uw gegevens beschermen door regelmatig back-ups te maken. Gebruik Azure Backup om herstelpunten te maken die kunnen worden opgeslagen in geografisch redundante kluizen van Recovery Services. In dit artikel wordt uitgelegd hoe u een back-up van een virtuele machine (VM) maakt in Azure met Azure CLI. U kunt deze stappen ook uitvoeren met [Azure PowerShell](quick-backup-vm-powershell.md) of in [Azure Portal](quick-backup-vm-portal.md).

Deze Quick Start is bedoeld voor een back-up van een bestaande VM in Azure. Als u nog een VM moet maken, kan dat ook [met Azure CLI](../virtual-machines/linux/quick-create-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u de CLI lokaal wilt installeren en gebruiken, moet u Azure CLI versie 2.0.18 of hoger gebruiken. Voer `az --version` uit om de CLI-versie te bepalen. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). 


## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken
Een Recovery Services-kluis is een logische container waarin de back-upgegevens voor elke beveiligde resource worden opgeslagen, zoals virtuele Azure-machines. Wanneer de back-uptaak voor een beveiligde resource wordt uitgevoerd, wordt er binnen de Recovery Services-kluis een herstelpunt gemaakt. U kunt vervolgens een van deze herstelpunten gebruiken om gegevens voor dat tijdstip te herstellen.

Maak een Recovery Services-kluis met [az backup vault create](https://docs.microsoft.com/cli/azure/backup/vault#az-backup-vault-create). Geef de resourcegroep en locatie op van de VM die u wilt beveiligen. Als u de [Snelstartgids van Virtual Machines](../virtual-machines/linux/quick-create-cli.md) hebt gebruikt, hebt u het volgende gemaakt:

- een resourcegroep met de naam *myResourceGroup*,
- een virtuele machine met de naam *myVM*,
- resources op de locatie *VS - Oost*.

```azurecli-interactive 
az backup vault create --resource-group myResourceGroup \
    --name myRecoveryServicesVault \
    --location eastus
```

De Recovery Services-kluis is standaard ingesteld voor geografisch redundante opslag. In geval van geografisch redundante opslag worden uw back-upgegevens gerepliceerd naar een secundaire Azure-regio die honderden kilometers van de primaire regio is verwijderd. Als de instelling van de redundantie opslag worden gewijzigd moet, gebruikt u [az Backup-kluis back-up-eigenschappen instellen](https://docs.microsoft.com/cli/azure/backup/vault/backup-properties?view=azure-cli-latest#az-backup-vault-backup-properties-set) cmdlet.

```azurecli
az backup vault backup-properties set \
    --name myRecoveryServicesVault  \
    --resource-group myResourceGroup \
    --backup-storage-redundancy "LocallyRedundant/GeoRedundant" 
```


## <a name="enable-backup-for-an-azure-vm"></a>Back-up voor een virtuele Azure-machine inschakelen
Maak een beleidsregels om te definiëren wanneer een back-uptaak wordt uitgevoerd en hoe lang de herstelpunten worden opgeslagen. Met het standaardbeleid voor beveiliging wordt elke dag een back-uptaak uitgevoerd en worden herstelpunten gedurende 30 dagen bewaard. U kunt deze standaardwaarden gebruiken om uw VM snel te beveiligen. Als u back-upbeveiliging wilt inschakelen voor een VM, gebruikt u de opdracht [az backup protection enable-for-vm](https://docs.microsoft.com/cli/azure/backup/protection#az-backup-protection-enable-for-vm). Geef de resourcegroep en de VM op die u wilt beveiligen, en vervolgens het beleid dat u wilt gebruiken:

```azurecli-interactive 
az backup protection enable-for-vm \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --vm myVM \
    --policy-name DefaultPolicy
```

> [!NOTE]
> Als de VM zich niet in dezelfde resourcegroep bevindt als de kluis, verwijst myResourceGroup naar de resourcegroep waarin de kluis is gemaakt. Geef de VM-id op (in plaats van de naam van de VM), zoals hieronder wordt aangegeven.

```azurecli-interactive 
az backup protection enable-for-vm \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --vm $(az vm show -g VMResourceGroup -n MyVm --query id | tr -d '"') \
    --policy-name DefaultPolicy
```

## <a name="start-a-backup-job"></a>Een back-uptaak starten
Als u een back-up direct wilt starten in plaats van te wachten totdat de back-up volgens het standaardbeleid op het geplande tijdstip wordt uitgevoerd, gebruikt u de opdracht [az backup protection backup-now](https://docs.microsoft.com/cli/azure/backup/protection#az-backup-protection-backup-now). Met deze eerste back-uptaak wordt een volledig herstelpunt gemaakt. Bij elke volgende back-uptaak worden incrementele herstelpunten gemaakt. Incrementele herstelpunten zijn efficiënt qua opslag en tijd aangezien ze alleen wijzigingen bevatten die sinds de laatste back-up zijn doorgevoerd.

De volgende parameters worden gebruikt om een oor back-up van de VM te maken:

- `--container-name` is de naam van de VM
- `--item-name` is de naam van de VM
- De waarde voor `--retain-until` moet worden ingesteld op de laatst beschikbare datum, in UTC-tijdnotatie (**dd-mm-jjjj**), waarop het herstelpunt beschikbaar moet zijn

In het volgende voorbeeld wordt een back-up gemaakt van de VM met de naam *myVM* en wordt de vervaldatum van het herstelpunt ingesteld op 18 oktober 2017:

```azurecli-interactive 
az backup protection backup-now \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --container-name myVM \
    --item-name myVM \
    --retain-until 18-10-2017
```


## <a name="monitor-the-backup-job"></a>Uitvoering van back-uptaak volgen
U kunt de status van back-uptaken controleren met [az backup job list](https://docs.microsoft.com/cli/azure/backup/job#az-backup-job-list):

```azurecli-interactive 
az backup job list \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --output table
```

De uitvoer is vergelijkbaar met het volgende voorbeeld, waarin u kunt zien dat de back-uptaak wordt uitgevoerd (*InProgress*):

```
Name      Operation        Status      Item Name    Start Time UTC       Duration
--------  ---------------  ----------  -----------  -------------------  --------------
a0a8e5e6  Backup           InProgress  myvm         2017-09-19T03:09:21  0:00:48.718366
fe5d0414  ConfigureBackup  Completed   myvm         2017-09-19T03:03:57  0:00:31.191807
```

Wanneer de *status* van de back-uptaak verandert in *Completed*, is de VM beveiligd met Recovery Services en is er een volledig herstelpunt opgeslagen.


## <a name="clean-up-deployment"></a>Opschonen van implementatie
Wanneer een back-up niet meer nodig is, kunt u de beveiliging van de VM uitschakelen, de herstelpunten en de Recovery Services-kluis verwijderen, en vervolgens de resourcegroep en de bijbehorende VM-resources verwijderen. Als u een bestaande virtuele machine hebt gebruikt, kunt u de laatste opdracht ([az group delete](/cli/azure/group?view=azure-cli-latest#az-group-delete)) overslaan om de resourcegroep en VM te behouden.

Ga naar [Vervolgstappen](#next-steps) voor een zelfstudie over Microsoft Azure Backup, waarin wordt uitgelegd hoe u gegevens voor uw virtuele machine terugzet. 

```azurecli-interactive 
az backup protection disable \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --container-name myVM \
    --item-name myVM \
    --delete-backup-data true
az backup vault delete \
    --resource-group myResourceGroup \
    --name myRecoveryServicesVault \
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Volgende stappen
In deze Quick Start hebt u een Recovery Services-kluis gemaakt, de beveiliging op een VM ingeschakeld en het eerste herstelpunt gemaakt. Volg de andere zelfstudies als u nog meer wilt weten over Azure Backup en Recovery Services.

> [!div class="nextstepaction"]
> [Back-up maken van meerdere virtuele Azure-machines](./tutorial-backup-vm-at-scale.md)
