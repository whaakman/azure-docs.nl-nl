---
title: Herstellen van een VM-schijf met Azure Backup | Microsoft Docs
description: Informatie over het herstellen van een schijf en een herstellen van een virtuele machine in Azure met back-up en Recovery Services maken.
services: backup, virtual-machines
documentationcenter: virtual-machines
author: markgalioto
manager: carmonm
editor: 
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: 
ms.service: backup, virtual-machines
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/28/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 9bc6da13786eb9eb6186ceadf0432b3a3ec2c941
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="restore-a-disk-and-create-a-recovered-vm-in-azure"></a>Herstellen van een schijf en het maken van een herstelde virtuele machine in Azure
Azure Backup maakt herstelpunten die zijn opgeslagen in een geografisch redundante recovery kluizen. Wanneer u vanaf een herstelpunt herstelt, kunt u de hele virtuele machine of de afzonderlijke bestanden terugzetten. In dit artikel wordt uitgelegd hoe u een volledige virtuele machine herstellen. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Lijst en selecteer herstelpunten
> * Een schijf herstellen vanaf een herstelpunt
> * Een virtuele machine van de herstelde schijf maken

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.18 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 


## <a name="prerequisites"></a>Vereisten
Deze zelfstudie vereist een Linux VM die is beveiligd met Azure Backup. Om te simuleren een per ongeluk verwijderen van de VM- en herstelproces, kunt u een virtuele machine maken van een schijf in een herstelpunt. Als u een Linux VM die is beveiligd met Azure Backup moet, Zie [Back-up van een virtuele machine in Azure met de CLI](quick-backup-vm-cli.md).


## <a name="backup-overview"></a>Overzicht van Backup
Wanneer Azure een back-up begint, maakt de Backup-extensie op de virtuele machine een momentopname punt in tijd. De Backup-extensie is geïnstalleerd op de virtuele machine wanneer de eerste back-up wordt aangevraagd. Azure Backup kan ook een momentopname van de onderliggende opslag als de virtuele machine niet wordt uitgevoerd wanneer de back-up plaatsvindt.

Standaard heeft Azure Backup een bestand system consistente back-up. Zodra de Azure Backup maakt de momentopname, worden de gegevens worden overgedragen naar de Recovery Services-kluis. Als u wilt optimaliseren, Azure Backup identificeert en alleen de blokken met gegevens die zijn gewijzigd sinds de vorige back-up overdraagt.

Wanneer de overdracht van gegevens voltooid is, wordt de momentopname is verwijderd en een herstelpunt wordt gemaakt.


## <a name="list-available-recovery-points"></a>Lijst met beschikbare herstelpunten
Voor het herstellen van een schijf, selecteert u een herstelpunt wordt gemaakt als de bron voor de gegevens voor herstel. Wanneer het standaardbeleid voor elke dag van een herstelpunt maakt en deze gedurende 30 dagen behoudt, kunt u een aantal herstelpunten dat kunt u het selecteren van een bepaald punt in tijd voor herstel houden. 

Gebruiken om een lijst met beschikbare herstelpunten weer, [az back-recoverypoint lijst](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az_backup_recoverypoint_list). Het herstelpunt **naam** wordt gebruikt voor het herstellen van schijven. In deze zelfstudie willen we de meest recente beschikbare herstelpunt. De `--query [0].name` parameter selecteert u de naam van de meest recente herstelpunt punt als volgt:

```azurecli-interactive
az backup recoverypoint list \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --container-name myVM \
    --item-name myVM \
    --query [0].name \
    --output tsv
```


## <a name="restore-a-vm-disk"></a>De schijf van een virtuele machine herstellen
Voor het herstellen van de schijf van het herstelpunt, moet u eerst een Azure storage-account maken. Dit opslagaccount wordt gebruikt voor het opslaan van de herstelde schijf. In de extra stappen de herstelde schijf gebruikt voor het maken van een virtuele machine.

1. Gebruik voor het maken van een opslagaccount [az storage-account maken](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create). De opslagaccountnaam moet worden alleen kleine letters bevatten en globaal uniek zijn. Vervang *mystorageaccount* met uw eigen unieke naam:

    ```azurecli-interactive
    az storage account create \
        --resource-group myResourceGroup \
        --name mystorageaccount \
        --sku Standard_LRS
    ```

2. Herstellen van de schijf uit uw herstelpunt met [az back-up terugzetten terugzetten schijven](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az_backup_restore_restore_disks). Vervang *mystorageaccount* met de naam van het opslagaccount dat u hebt gemaakt in de voorgaande opdracht. Vervang *myRecoveryPointName* met de recovery point-naam die u hebt verkregen in de uitvoer van de vorige [az back-recoverypoint lijst](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az_backup_recoverypoint_list) opdracht:

    ```azurecli-interactive
    az backup restore restore-disks \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --storage-account mystorageaccount \
        --rp-name myRecoveryPointName
    ```


## <a name="monitor-the-restore-job"></a>De hersteltaak bewaken
Gebruik voor het controleren van de status van de hersteltaak [az back-uptaak lijst](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az_backup_job_list):

```azurecli-interactive 
az backup job list \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --output table
```

De uitvoer is vergelijkbaar met het volgende voorbeeld, waarin de hersteltaak is *InProgress*:

```
Name      Operation        Status      Item Name    Start Time UTC       Duration
--------  ---------------  ----------  -----------  -------------------  --------------
7f2ad916  Restore          InProgress  myvm         2017-09-19T19:39:52  0:00:34.520850
a0a8e5e6  Backup           Completed   myvm         2017-09-19T03:09:21  0:15:26.155212
fe5d0414  ConfigureBackup  Completed   myvm         2017-09-19T03:03:57  0:00:31.191807
```

Wanneer de *Status* van de taak terugzetten rapporten *voltooid*, de schijf is hersteld naar het opslagaccount.


## <a name="convert-the-restored-disk-to-a-managed-disk"></a>De herstelde schijf converteren naar een schijf beheerd
De hersteltaak wordt een niet-beheerde schijf gemaakt. Om een virtuele machine van de schijf maken, moet u deze eerst geconverteerd naar een beheerde schijf.

1. Verkrijgen van de verbindingsgegevens voor uw opslagaccount met [az storage-account weergeven verbindingsreeks](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_show_connection_string). Vervang *mystorageaccount* met de naam van uw storage-account als volgt:
    
    ```azurecli-interactive
    export AZURE_STORAGE_CONNECTION_STRING=$( az storage account show-connection-string \
        --resource-group myResourceGroup \
        --output tsv \
        --name mystorageaccount )
    ```

2. De onbeheerde schijf is in het opslagaccount beveiligd. De volgende opdrachten informatie ophalen over de onbeheerde schijf en maak een variabele met de naam *uri* die wordt gebruikt in de volgende stap bij het maken van de schijf worden beheerd.

    ```azurecli-interactive
    container=$(az storage container list --query [0].name -o tsv)
    blob=$(az storage blob list --container-name $container --query [0].name -o tsv)
    uri=$(az storage blob url --container-name $container --name $blob -o tsv)
    ```

3. Nu u een schijf beheerd van de herstelde schijf met maken kunt [az schijf maken](https://docs.microsoft.com/cli/azure/disk?view=azure-cli-latest#az_disk_create). De *uri* variabele uit de vorige stap wordt gebruikt als bron voor de schijf worden beheerd.

    ```azurecli-interactive
    az disk create \
        --resource-group myResourceGroup \
        --name myRestoredDisk \
        --source $uri
    ```

4. Als u nu een beheerd schijf van de herstelde schijf hebt, opschonen van de niet-beheerde schijf- en opslagbeheer rekening met [az storage-account verwijderen](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_delete). Vervang *mystorageaccount* met de naam van uw storage-account als volgt:

    ```azurecli-interactive
    az storage account delete \
        --resource-group myResourceGroup \
        --name mystorageaccount
    ```


## <a name="create-a-vm-from-the-restored-disk"></a>Een virtuele machine van de herstelde schijf maken
De laatste stap is het maken van een virtuele machine van de schijf worden beheerd.

1. Een virtuele machine maken van de schijf worden beheerd met [az vm maken](/cli/azure/vm?view=azure-cli-latest#az_vm_create) als volgt:

    ```azurecli-interactive
    az vm create \
        --resource-group myResourceGroup \
        --name myRestoredVM \
        --attach-os-disk myRestoredDisk \
        --os-type linux
    ```

2. Lijst om te bevestigen dat de virtuele machine is gemaakt van de herstelde schijf, de virtuele machines in de resourcegroep met [az vm lijst](/cli/azure/vm?view=azure-cli-latest#az_vm_list) als volgt:

    ```azurecli-interactive
    az vm list --resource-group myResourceGroup --output table
    ```


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt u een schijf hersteld vanaf een herstelpunt en wordt gemaakt van een virtuele machine van de schijf. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Lijst en selecteer herstelpunten
> * Een schijf herstellen vanaf een herstelpunt
> * Een virtuele machine van de herstelde schijf maken

Ga naar de volgende zelfstudie voor meer informatie over het herstellen van afzonderlijke bestanden van een herstelpunt wordt gemaakt.

> [!div class="nextstepaction"]
> [Bestanden herstellen met een virtuele machine in Azure](tutorial-restore-files.md)

