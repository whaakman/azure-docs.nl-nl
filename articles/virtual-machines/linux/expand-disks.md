---
title: Vouw de virtuele harde schijven op een Linux VM in Azure | Microsoft Docs
description: Meer informatie over het uitbreiden van de virtuele harde schijven op een Linux-VM met de Azure CLI 2.0
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: b82cc0473c003da767ee230ab485c69b233977d1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-expand-virtual-hard-disks-on-a-linux-vm-with-the-azure-cli"></a>Het uitbreiden van de virtuele harde schijven op een Linux-VM met de Azure CLI
Grootte van de virtuele harde schijf voor het besturingssysteem (OS) is doorgaans 30 GB op een Linux virtuele machine (VM) in Azure. U kunt [gegevensschijven toevoegen](add-disk.md) te voorzien in extra opslagruimte, maar u kunnen ook desgewenst een bestaande gegevensschijf wilt uitbreiden. Dit artikel wordt uitgelegd hoe u beheerde schijven voor een Linux-VM met de Azure CLI 2.0 uitbreiden. U kunt ook uitbreiden met de niet-beheerde OS-schijf met de [Azure CLI 1.0](expand-disks-nodejs.md).

> [!WARNING]
> Zorg er altijd dat u maakt u een back-up van uw gegevens voordat u de schijf uitvoert de grootte van bewerkingen. Zie voor meer informatie [Back-up van Linux virtuele machines in Azure](tutorial-backup-vms.md).

## <a name="expand-disk"></a>Schijf uitbreiden
Zorg ervoor dat u de meest recente hebt [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/#login).

In dit artikel is een bestaande virtuele machine in Azure met ten minste één schijf met gegevens die zijn gekoppeld en voorbereid vereist. Als u nog geen een virtuele machine die u kunt gebruiken, raadpleegt u [maken en voorbereiden van een virtuele machine met gegevensschijven](tutorial-manage-disks.md#create-and-attach-disks).

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. De namen van de voorbeeld-parameter *myResourceGroup* en *myVM*.

1. Bewerkingen op virtuele harde schijven kunnen niet worden uitgevoerd met de virtuele machine uitgevoerd. Toewijzing van uw virtuele machine met [az vm ongedaan](/cli/azure/vm#deallocate). Het volgende voorbeeld de virtuele machine met de naam deallocates *myVM* in de resourcegroep met de naam *myResourceGroup*:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > `az vm stop`Geeft de rekenresources niet vrij. Gebruik om rekenresources release `az vm deallocate`. De virtuele machine moet ongedaan als u de virtuele harde schijf wilt uitbreiden.

2. Een lijst met beheerde schijven weergeven in een resourcegroep met [az Schijflijst](/cli/azure/disk#list). Het volgende voorbeeld wordt een lijst met beheerde schijven in de resourcegroep met de naam *myResourceGroup*:

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    Vouw de benodigde schijfruimte met [az schijf update](/cli/azure/disk#update). Het volgende voorbeeld wordt de beheerde schijf met de naam *myDataDisk* worden *200*Gb in grootte:

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > Wanneer u een beheerde schijf uitbreidt, worden de bijgewerkte grootte is toegewezen aan de dichtstbijzijnde beheerde schijfgrootte. Zie voor een tabel van de beschikbare beheerde schijfgrootten en lagen [beheerd schijven overzicht van Azure - prijzen en facturering](../windows/managed-disks-overview.md#pricing-and-billing).

3. Start de virtuele machine met [az vm start](/cli/azure/vm#start). Het volgende voorbeeld wordt de virtuele machine met de naam *myVM* in de resourcegroep met de naam *myResourceGroup*:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

4. SSH met uw virtuele machine met de juiste referenties. U kunt het openbare IP-adres van uw virtuele machine met verkrijgen [az vm weergeven](/cli/azure/vm#show):

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

5. Voor het gebruik van de uitgevouwen schijf, moet u de onderliggende partitie en bestandssysteem uitvouwen.

    a. Als al is gekoppeld, ontkoppelt u de schijf:

    ```bash
    sudo umount /dev/sdc1
    ```

    b. Gebruik `parted` schijfgegevens wilt bekijken en grootte van de partitie:

    ```bash
    sudo parted /dev/sdc
    ```

    Informatie weergeven over de bestaande partitie-indeling met `print`. De uitvoer is vergelijkbaar met het volgende voorbeeld, waarin dat de onderliggende schijf 215Gb groot is:

    ```bash
    GNU Parted 3.2
    Using /dev/sdc1
    Welcome to GNU Parted! Type 'help' to view a list of commands.
    (parted) print
    Model: Unknown Msft Virtual Disk (scsi)
    Disk /dev/sdc1: 215GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: loop
    Disk Flags:
    
    Number  Start  End    Size   File system  Flags
        1      0.00B  107GB  107GB  ext4
    ```

    c. Vouw de partitie met `resizepart`. Voer het partitienummer *1*, en een grootte voor de nieuwe partitie:

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    d. Voer om af te sluiten`quit`

5. Bij de partitie is gewijzigd, Controleer de consistentie van de partitie met `e2fsck`:

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

6. Nu het formaat van het bestandssysteem met `resize2fs`:

    ```bash
    sudo resize2fs /dev/sdc1
    ```

7. Koppelen van de partitie naar de gewenste locatie, zoals `/datadrive`:

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

8. Gebruiken om te controleren of de grootte van de besturingssysteemschijf is gewijzigd, `df -h`. De volgende voorbeelduitvoer wordt weergegeven voor het gegevensstation */dev/sdc1*, is nu 200 GB:

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a>Volgende stappen
Als u extra opslagruimte, moet u ook [gegevensschijven toevoegen aan een Linux-VM](add-disk.md). Zie voor meer informatie over schijfversleuteling [schijven op een Linux-VM met de Azure CLI versleutelen](encrypt-disks.md).
