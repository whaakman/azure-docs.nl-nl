---
title: Koppelpunt Azure File storage in Linux VM's met behulp van SMB met Azure CLI 1.0 | Microsoft Docs
description: Het koppelen van Azure File storage op Linux VM's via SMB
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
ms.date: 12/07/2016
ms.author: v-livech
ms.openlocfilehash: 4951860630f0aad107d0846d52ebe4423ee0b91c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-by-using-smb-with-azure-cli-10"></a>Koppelpunt Azure File storage in Linux VM's met behulp van SMB met Azure CLI 1.0

In dit artikel laat zien hoe Azure File storage koppelen aan een Linux-VM met behulp van het protocol Server Message Block (SMB). File storage biedt bestandsshares in de cloud via het standaard SMB-protocol. De vereisten zijn:

* Een [Azure-account](https://azure.microsoft.com/pricing/free-trial/)
* [Secure Shell (SSH) openbare en persoonlijke sleutelbestanden](mac-create-ssh-keys.md)

## <a name="cli-versions-to-use"></a>CLI-versies gebruiken
U kunt de taak uitvoeren met behulp van een van de volgende versies van de opdrachtregelinterface (CLI):

- [Azure CLI 1.0](#quick-commands) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)-onze volgende generatie CLI voor de resource management-implementatiemodel


## <a name="quick-commands"></a>Snelle opdrachten
U kunt de taak snel doen, de stappen in deze sectie uit te voeren. Voor meer informatie en context gedetailleerde, beginnen bij de ['Gedetailleerd overzicht'](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) sectie.

### <a name="prerequisites"></a>Vereisten
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
De volgende regel toe te voegen `/etc/fstab`:

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht

File storage biedt bestandsshares in de cloud die het standaard SMB-protocol gebruiken. Met de meest recente versie van File storage, kunt u een bestandsshare ook koppelen vanuit elk besturingssysteem dat ondersteuning biedt voor SMB 3.0. Als u een SMB-koppelen op Linux gebruikt, krijgt u eenvoudig back-ups naar een robuuste, permanente archiveren opslaglocatie die wordt ondersteund door een SLA.

Verplaatsen van bestanden van een virtuele machine naar een SMB-koppelpunt die wordt gehost op File storage is een uitstekende manier om de logboeken voor foutopsporing. Dat komt doordat de dezelfde SMB-share op de lokaal naar uw werkstation Mac, Linux of Windows worden gekoppeld. SMB is de beste oplossing voor het streamen van Linux of toepassingslogboeken in real-time, omdat het SMB-protocol niet is gebouwd voor het afhandelen van deze rechten zware logboekregistratie. Een speciale, uniforme logboekregistratie laag hulpprogramma zoals Fluentd zou een betere keuze dan SMB zijn voor het verzamelen van Linux- en uitvoer logboekregistratie toepassing.

Voor deze gedetailleerde procedure we maken van de vereisten die nodig zijn voor het eerst de bestandsshare voor opslag maken en vervolgens via SMB koppelen op een Linux-VM.

1. Een Azure storage-account maken met behulp van de volgende code:

    ```azurecli
    azure storage account create myStorageAccount \
    --sku-name lrs \
    --kind storage \
    -l westus \
    -g myResourceGroup
    ```

2. De opslagaccountsleutels weergeven.

    Wanneer u een opslagaccount maakt, worden de sleutels in paren worden gemaakt zodat ze kunnen worden gedraaid zonder onderbreking van de service. Wanneer u naar de tweede sleutel in het paar overschakelt, maakt u een nieuw sleutelpaar. Nieuwe toegangscodes voor opslag worden altijd gemaakt in paren, hebt u altijd ten minste één niet-gebruikte opslagsleutel gereed om te activeren. Als u wilt de toegangscodes voor opslag weergeven, moet u de volgende code gebruiken:

    ```azurecli
    azure storage account keys list myStorageAccount \
    --resource-group myResourceGroup
    ```
3. De bestandsshare voor opslag maken.

    De File storage-share bevat de SMB-share. Het quotum wordt altijd weergegeven in GB (Gigabyte). Voor het maken van de bestandsshare voor opslag, moet u de volgende code gebruiken:

    ```azurecli
    azure storage share create mystorageshare \
    --quota 10 \
    --account-name myStorageAccount \
    --account-key nPOgPR<--snip-->4Q==
    ```

4. Maak de map koppelpunt.

    U moet een lokale map maken in het Linux-bestandssysteem naar de SMB-share te koppelen. Geschreven of gelezen uit de lokale Koppelmap wordt doorgestuurd naar de SMB-share die wordt gehost op de opslag van bestanden. Als u wilt maken van de map de volgende code gebruiken:

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

5. Koppel de SMB-share met behulp van de volgende code:

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=myStorageAccount,password=myStorageAccountkey,dir_mode=0777,file_mode=0777
    ```

6. Behouden van de SMB-koppelpunt via opnieuw wordt opgestart.

    Wanneer u de Linux-VM opnieuw opstart, wordt tijdens het afsluiten van de gekoppelde SMB-share ontkoppeld. Als u wilt opnieuw koppelen van de SMB-share op opstarten, moet u een regel toevoegen aan de /etc/fstab Linux. Linux gebruikt het fstab-bestand voor een lijst met de bestandssystemen die nodig is om te koppelen tijdens het opstarten. Het toevoegen van de SMB-share zorgt ervoor dat de File storage-share een permanent gekoppelde bestandssysteem voor de Linux-VM. De File storage SMB-share toe te voegen aan een nieuwe virtuele machine is mogelijk wanneer u cloud-init gebruiken.

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a>Volgende stappen

- [Gebruik van cloud-init voor het aanpassen van een Linux-VM tijdens het maken van](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Een schijf toevoegen aan een virtuele Linux-machine](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Versleutelen van schijven op een Linux-VM met behulp van de Azure CLI](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
