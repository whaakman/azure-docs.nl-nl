---
title: Koppelpunt Azure File storage in virtuele Linux-machines met SMB | Microsoft Docs
description: Het koppelen van Azure File storage op Linux VM's met behulp van SMB met de Azure CLI 2.0
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/13/2017
ms.author: v-livech
ms.openlocfilehash: 9eae17b304f8a987b44ebed8906dabd8ff3a36a8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a>Koppelpunt Azure File storage in virtuele Linux-machines met SMB

In dit artikel leest u hoe de Azure File storage-service op een Linux-VM te gebruiken met behulp van een SMB-koppelen met de Azure CLI 2.0. Azure File storage biedt bestandsshares in de cloud met behulp van het standaard SMB-protocol. U kunt deze stappen ook uitvoeren met de [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). De vereisten zijn:

- [een Azure-account.](https://azure.microsoft.com/pricing/free-trial/)
- [bestanden voor openbare en persoonlijke SSH-sleutels](mac-create-ssh-keys.md)

## <a name="quick-commands"></a>Snelle opdrachten

* Een resourcegroep
* Een Azure-netwerk
* Een netwerkbeveiligingsgroep met een SSH inkomende
* Een subnet
* Een Azure storage-account
* Sleutels voor Azure-opslagaccount
* Een Azure File storage-share
* Een virtuele Linux-machine

Geen voorbeelden vervangen door uw eigen instellingen.

### <a name="create-a-directory-for-the-local-mount"></a>Maak een map voor de lokale koppeling

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-the-file-storage-smb-share-to-the-mount-point"></a>De opslag van bestanden naar het koppelpunt de SMB-share koppelen

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-the-mount-after-a-reboot"></a>De koppeling behouden na opnieuw opstarten
Om dit te doen, voeg de volgende regel om de `/etc/fstab`:

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht

File storage biedt bestandsshares in de cloud die het standaard SMB-protocol gebruiken. Met de meest recente versie van File storage, kunt u een bestandsshare ook koppelen vanuit elk besturingssysteem dat ondersteuning biedt voor SMB 3.0. Als u een SMB-koppelen op Linux gebruikt, krijgt u eenvoudig back-ups naar een robuuste, permanente archiveren opslaglocatie die wordt ondersteund door een SLA.

Verplaatsen van bestanden van een virtuele machine naar een SMB-koppelpunt die wordt gehost op File storage is een uitstekende manier om de logboeken voor foutopsporing. Dat komt doordat de dezelfde SMB-share op de lokaal naar uw werkstation Mac, Linux of Windows worden gekoppeld. SMB is de beste oplossing voor het streamen van Linux of toepassingslogboeken in real-time, omdat het SMB-protocol niet is gebouwd voor het afhandelen van deze rechten zware logboekregistratie. Een speciale, uniforme logboekregistratie laag hulpprogramma zoals Fluentd zou een betere keuze dan SMB zijn voor het verzamelen van Linux- en uitvoer logboekregistratie toepassing.

Voor deze gedetailleerde procedure we maken van de vereisten die nodig zijn voor het eerst de bestandsshare voor opslag maken en vervolgens via SMB koppelen op een Linux-VM.

1. Maak een resourcegroep met [az groep maken](/cli/azure/group#create) voor het opslaan van de bestandsshare.

    Maken van een resourcegroep met de naam `myResourceGroup` op de locatie 'VS-West', gebruikt u het volgende voorbeeld:

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

2. Maken van een Azure storage-account met [az storage-account maken](/cli/azure/storage/account#create) voor het opslaan van de bestanden.

    Voor het maken van een opslagaccount met de naam mystorageaccount met behulp van de opslag Standard_LRS SKU, gebruikt u het volgende voorbeeld:

    ```azurecli
    az storage account create --resource-group myResourceGroup \
        --name mystorageaccount \
        --location westus \
        --sku Standard_LRS
    ```

3. De opslagaccountsleutels weergeven.

    Wanneer u een opslagaccount maakt, worden de sleutels in paren worden gemaakt zodat ze kunnen worden gedraaid zonder onderbreking van de service. Wanneer u naar de tweede sleutel in het paar overschakelt, maakt u een nieuw sleutelpaar. Nieuwe toegangscodes voor opslag worden altijd gemaakt in paren, hebt u altijd ten minste één niet-gebruikte opslagaccountsleutel gereed om te activeren.

    Weergeven van de sleutels van opslagaccount met de [lijst met opslagaccounts die sleutels az](/cli/azure/storage/account/keys#list). Het opslagaccount sleutels voor de benoemde `mystorageaccount` worden vermeld in het volgende voorbeeld:

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount
    ```

    Als u wilt één sleutel extraheren, gebruikt u de `--query` vlag. Het volgende voorbeeld worden de eerste sleutel (`[0]`):

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount \
        --query '[0].{Key:value}' --output tsv
    ```

4. De bestandsshare voor opslag maken.

    De File storage-share bevat de SMB-share met [az storage-share maken](/cli/azure/storage/share#create). Het quotum wordt altijd weergegeven in GB (Gigabyte). Geeft u een van de sleutels van de voorgaande `az storage account keys list` opdracht. Maak een share met de naam mystorageshare met een target 10 GB met behulp van het volgende voorbeeld:

    ```azurecli
    az storage share create --name mystorageshare \
        --quota 10 \
        --account-name mystorageaccount \
        --account-key nPOgPR<--snip-->4Q==
    ```

5. Maak een map koppelpunt.

    Een lokale map maken in het Linux-bestandssysteem naar de SMB-share te koppelen. Geschreven of gelezen uit de lokale Koppelmap wordt doorgestuurd naar de SMB-share die wordt gehost op de opslag van bestanden. Voor het maken van een lokale map op /mnt/mymountdirectory, gebruikt u het volgende voorbeeld:

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

6. Koppel de SMB-share naar een lokale map.

    Geef uw eigen storage account gebruikersnaam en het opslagaccountsleutel referenties voor de koppeling als volgt:

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=mystorageaccount,password=mystorageaccountkey,dir_mode=0777,file_mode=0777
    ```

7. Behouden van de SMB-koppelpunt via opnieuw wordt opgestart.

    Wanneer u de Linux-VM opnieuw opstart, wordt tijdens het afsluiten van de gekoppelde SMB-share ontkoppeld. Als u wilt opnieuw koppelen van de SMB-share op opstarten, moet u een regel toegevoegd aan de /etc/fstab Linux. Linux gebruikt het fstab-bestand voor een lijst met de bestandssystemen die nodig is om te koppelen tijdens het opstarten. Het toevoegen van de SMB-share zorgt ervoor dat de File storage-share een permanent gekoppelde bestandssysteem voor de Linux-VM. De File storage SMB-share toe te voegen aan een nieuwe virtuele machine is mogelijk wanneer u cloud-init gebruiken.

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a>Volgende stappen

- [Gebruik van cloud-init voor het aanpassen van een Linux-VM tijdens het maken van](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Een schijf toevoegen aan een virtuele Linux-machine](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Versleutelen van schijven op een Linux-VM met behulp van de Azure CLI](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
